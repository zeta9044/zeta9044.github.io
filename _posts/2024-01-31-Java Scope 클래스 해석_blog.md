---
layout: post
title: "Java Scope 클래스 해석"
date: 2024-01-31
tags: [Scope, 해석, Java, 클래스]
categories: blog
---

## 사용자 입력:
> 
/** An abstract base class that houses common functionality for scopes. */
public abstract class BaseScope implements Scope {

	protected int nodeId;
	protected ScopeType scopeType;
	protected Scope parentScope; // null if this scope is the root of the scope tree

	/** All symbols defined in this scope; can include classes, functions,
	 *  variables, or anything else that is a Symbol impl. It does NOT
	 *  include non-Symbol-based things like LocalScope. See nestedScopes.
	 */
	protected Map<String, Symbol> symbols = new LinkedHashMap<>();

	/** All directly contained scopes, typically LocalScopes within a
	 *  LocalScope or a LocalScope within a FunctionSymbol. This does not
	 *  include SymbolWithScope objects.
	 */
	protected List<Scope> nestedScopesNotSymbols = new ArrayList<>();

	public BaseScope() { }

	public BaseScope(ParserRuleContext ctx, Scope parentScope) {
		setNodeId(ctx);
		setParentScope(parentScope);
	}

	public int getNodeId() {
		return nodeId;
	}

	public void setNodeId(ParserRuleContext ctx) {
		this.nodeId = ctx.hashCode();
	}
	public Map<String, ? extends Symbol> getMembers() {
		return symbols;
	}

	@Override
	public Symbol getSymbol(String name) {
		return symbols.get(name);
	}

	@Override
	public void setParentScope(Scope enclosingScope) {
		this.parentScope = enclosingScope;
	}

	public List<Scope> getAllNestedScopedSymbols() {
		List<Scope> scopes = new ArrayList<Scope>();
		Utils.getAllNestedScopedSymbols(this, scopes);
		return scopes;
	}

	@Override
	public List<Scope> getNestedScopedSymbols() {
		List<? extends Symbol> scopes = Utils.filter(getSymbols(), s -> s instanceof Scope);
		return (List)scopes; // force it to cast
	}

	@Override
	public List<Scope> getNestedScopes() {
		ArrayList<Scope> all = new ArrayList<>();
		all.addAll(getNestedScopedSymbols());
		all.addAll(nestedScopesNotSymbols);
		return all;
	}

	@Override
	public Symbol resolve(String name) {
		Symbol s = symbols.get(name);
		if ( s!=null ) {
			return s;
		}
		// if not here, check any enclosing scope
		Scope parent = getParentScope();
		if ( parent != null ) return parent.resolve(name);
		return null; // not found
	}

	public void define(Symbol sym) throws IllegalArgumentException {
		if ( symbols.containsKey(sym.getName()) ) {
			throw new IllegalArgumentException("duplicate symbol "+sym.getName());
		}
		sym.setScope(this);
		sym.setInsertionOrderNumber(symbols.size()); // set to insertion position from 0
		symbols.put(sym.getName(), sym);
	}

	public Scope getParentScope() { return parentScope; }

	/** Walk up enclosingScope until we find topmost. Note this is
	 *  enclosing scope not necessarily parent. This will usually be
	 *  a global scope or something, depending on your scope tree.
	 */
	public Scope getOuterMostEnclosingScope() {
		Scope s = this;
		while ( s.getParentScope()!=null ) {
			s = s.getParentScope();
		}
		return s;
	}

	@Override
	public List<Scope> getParentPathToRoot() {
		List<Scope> scopes = new ArrayList<>();
		Scope s = this;
		while ( s!=null ) {
			scopes.add(s);
			s = s.getParentScope();
		}
		return scopes;
	}

	@Override
	public List<? extends Symbol> getSymbols() {
		Collection<Symbol> values = symbols.values();
		if ( values instanceof List ) {
			return (List<Symbol>)values;
		}
		return new ArrayList<>(values);
	}

	public List<? extends Symbol> getAllSymbols() {
		List<Symbol> syms = new ArrayList<>();
		syms.addAll(getSymbols());
		for (Symbol s : symbols.values()) {
			if ( s instanceof Scope) {
				Scope scope = (Scope)s;
				syms.addAll(scope.getAllSymbols());
			}
		}
		return syms;
	}

	@Override
	public int getNumberOfSymbols() {
		return symbols.size();
	}

	@Override
	public Set<String> getSymbolNames() {
		return symbols.keySet();
	}

	public String toString() { return symbols.keySet().toString(); }

	public String toScopeStackString(String separator) {
		return Utils.toScopeStackString(this, separator);
	}

	public String toQualifierString(String separator) {
		return Utils.toQualifierString(this, separator);
	}
}
 이 소스 해석 좀 해줘.

## ChatGPT 응답:
> 이 소스 코드는 스코프(scope) 관리를 위한 추상 기반 클래스를 정의하는 Java 코드입니다. 스코프는 프로그래밍 언어에서 식별자(변수, 함수 등)의 유효 범위를 정의합니다. 이 클래스는 `Scope` 인터페이스를 구현하며, 다양한 유형의 스코프에 공통적인 기능을 제공합니다.

### 클래스 구조와 주요 컴포넌트

- **필드**:
  - `nodeId`: 스코프를 식별하는 데 사용되는 고유 ID입니다. `ParserRuleContext`의 해시코드를 사용하여 설정됩니다.
  - `scopeType`: 스코프의 유형(예: 클래스, 함수, 변수 스코프 등)을 저장합니다.
  - `parentScope`: 현재 스코프의 부모 스코프를 참조합니다. 루트 스코프인 경우 `null`일 수 있습니다.
  - `symbols`: 현재 스코프 내에 정의된 모든 심볼(클래스, 함수, 변수 등)을 저장하는 맵입니다.
  - `nestedScopesNotSymbols`: 현재 스코프에 직접 포함된 다른 스코프들을 저장하는 리스트입니다. 이는 `SymbolWithScope` 객체를 포함하지 않습니다.

- **생성자**:
  - 기본 생성자와 부모 스코프 및 `ParserRuleContext`를 매개변수로 받는 생성자가 있습니다. 이를 통해 스코프의 노드 ID와 부모 스코프를 설정할 수 있습니다.

- **메서드**:
  - `getSymbol`, `define`, `resolve` 등 심볼을 관리하기 위한 메서드들이 포함되어 있습니다. 이들은 각각 심볼을 조회, 정의, 해결(식별자에 해당하는 심볼을 찾음)하는 역할을 합니다.
  - `getNestedScopes`, `getAllNestedScopedSymbols`, `getNestedScopedSymbols` 등은 현재 스코프와 중첩된 스코프들을 관리합니다.
  - `getParentScope`, `getOuterMostEnclosingScope`, `getParentPathToRoot` 등은 부모 스코프와 관련된 정보를 제공하는 메서드입니다.
  - `getSymbols`, `getAllSymbols`, `getNumberOfSymbols`, `getSymbolNames` 등은 스코프 내 심볼들에 대한 정보를 제공합니다.
  - `toString`, `toScopeStackString`, `toQualifierString` 등은 스코프와 그 내용을 문자열로 표현하기 위한 메서드입니다.

### 핵심 기능

- **스코프 식별과 관리**: 각 스코프는 고유한 ID를 가지며, 부모 스코프와의 관계를 통해 스코프 트리를 형성합니다.
- **심볼 관리**: 스코프 내에 정의된 심볼들을 관리하고, 심볼 이름으로 심볼을 조회하거나 추가할 수 있습니다.
- **스코프 해석**: 이름을 기반으로 심볼을 찾으며, 현재 스코프에서 찾지 못한 경우 부모 스코프로 올라가면서 심볼을 찾습니다.
- **중첩 스코프 관리**: 스코프 내에 중첩된 스코프를 관리하고, 이를 통해 스코프 계층을 탐색할 수 있습니다.

이 클래스는 프로그래밍 언어의 구문 분석 및 심볼 테이블 구축 과정에서 중요한

## ChatGPT 응답:
>  역할을 합니다. 코드를 통해 식별자의 유효 범위를 관리하고, 이름 충돌을 방지하며, 코드의 구조를 명확하게 파악할 수 있습니다.

## 사용자 입력:
> 난 이 소스 중에 	protected Map<String, Symbol> symbols = new LinkedHashMap<>(); 이 부분이 제일 이해가 안가. 전역 스코프 같은 경우,   변수명과 함수명이 같을 수 있고, 매개변수가 다르지만 이름은 같은게 존재 하잖아.

## ChatGPT 응답:
> `protected Map<String, Symbol> symbols = new LinkedHashMap<>();` 이 부분은 스코프 내에 정의된 모든 심볼(변수, 함수 등)을 저장하는데 사용됩니다. `Map`의 키는 심볼의 이름을 나타내고, 값은 `Symbol` 인터페이스를 구현하는 객체입니다. 이 객체는 해당 심볼의 세부 정보(타입, 위치 등)를 포함합니다. 여기서 사용된 `LinkedHashMap`은 삽입 순서를 유지하는 `Map` 구현입니다.

### 문제 상황: 이름 충돌

이 구현에서는 동일한 이름을 가진 심볼(예: 변수명과 함수명이 같은 경우, 이름은 같지만 매개변수가 다른 함수 오버로딩)이 동일한 스코프 내에 존재할 수 있는 문제를 언급하셨습니다. 실제로, 여러 프로그래밍 언어에서는 함수 오버로딩을 지원하며, 이는 같은 이름을 가진 여러 함수가 같은 스코프 내에 공존할 수 있음을 의미합니다. 변수와 함수가 같은 이름을 가질 수는 있지만, 일반적으로 같은 스코프 내에서는 이를 구분하여 관리합니다.

### 해결 방법

1. **심볼 분류**: 이 코드에서는 `Symbol` 인터페이스의 구현을 통해 다양한 유형의 심볼(변수, 함수 등)을 다룰 수 있습니다. 각 심볼 유형은 추가적인 정보를 가질 수 있으며, 이를 통해 동일한 이름을 가진 심볼을 구분할 수 있습니다.

2. **함수 오버로딩**: 함수 오버로딩의 경우, 각 함수는 매개변수의 타입이나 개수에 따라 구별됩니다. 이러한 구별은 `Symbol` 객체 내에 매개변수 리스트 등의 추가 정보를 포함시켜 관리함으로써 이루어질 수 있습니다. 그러나 위 코드만으로는 함수 오버로딩을 직접적으로 지원하지 않습니다. 실제 구현에서는 함수 이름과 매개변수 타입을 조합하여 고유한 키를 생성하거나, `Symbol` 구현 내에서 오버로딩을 처리하는 로직을 추가해야 할 수 있습니다.

3. **별도의 구조 사용**: 실제 언어 구현에서는 함수 오버로딩을 지원하기 위해 `symbols` 맵 외에도 별도의 데이터 구조(예: 매개변수 타입을 고려한 다중 키 맵, 함수 심볼을 별도로 관리하는 리스트 등)를 사용할 수 있습니다. 이는 각 함수에 대해 고유한 식별자를 생성하거나, 매개변수 시그니처를 고려하여 함수를 구별하는 방법입니다.

### 결론

위 코드 조각은 기본적인 심볼 테이블 구현을 보여주며, 모든 심볼을 단일 맵에서 관리합니다. 함수 오버로딩이나 같은 이름의 변수와 함수 공존 같은 상황을 직접 처리하기 위해서는 추가적인 로직이 필요합니다. 이는 `Symbol` 클래스의 확장, 별도의 데이터 구조 사용, 또는 심볼 조회 및 삽입 메커니즘에 대한 세밀한 조정을 통해 이루어질 수 있습니다.

## 사용자 입력:
> Map은 기본적으로 key가 같으면, 나중에 들어온 값만 유지되는 걸로 알고 있는데, 전역스코프에 같은 변수명,함수명이 있으면 하나는 지워진다는 거아냐?

## ChatGPT 응답:
> 네, 맞습니다. Java의 `Map` 인터페이스는 키-값 쌍을 저장할 때 키의 유일성을 보장합니다. 즉, 같은 키에 대해 새로운 값을 `put` 연산으로 추가하면 이전 값은 대체되어 저장 공간에서 제거됩니다. 이는 전역 스코프나 어떤 스코프에서도 동일한 원칙이 적용됩니다. 따라서, 같은 이름을 가진 변수나 함수를 `Map`에 추가하려 하면, 나중에 추가된 심볼만이 `Map`에 남게 되어 이전에 추가된 심볼은 접근할 수 없게 됩니다.

이러한 특성은 전역 스코프에서 함수 오버로딩이나 변수와 함수가 동일한 이름을 공유할 수 있는 언어 특성과 충돌할 수 있습니다. 예를 들어, C++이나 Java 같은 언어에서는 함수 오버로딩을 통해 같은 이름의 함수가 다른 매개변수를 가질 수 있습니다. 또한, 변수와 함수가 동일한 이름을 사용할 수 있는 경우도 있습니다(언어에 따라 다름).

### 해결 방법

이러한 문제를 해결하기 위한 몇 가지 접근 방법은 다음과 같습니다:

1. **복합 키 사용**: 함수의 이름뿐만 아니라 매개변수의 타입과 같은 추가 정보를 조합하여 복합 키를 만들 수 있습니다. 이 방법은 함수 오버로딩을 지원하는 데 유용합니다. 예를 들어, 함수 이름과 매개변수의 시그니처를 조합하여 키를 생성할 수 있습니다.

2. **심볼 카테고리 분리**: 변수와 함수를 분리하여 저장하는 별도의 맵을 사용할 수 있습니다. 이는 변수와 함수가 같은 이름을 가질 수 있는 언어의 특성을 지원하는 데 도움이 됩니다.

3. **심볼 리스트 관리**: 같은 이름을 가진 여러 심볼을 지원하기 위해 각 이름에 대해 심볼 리스트를 저장하는 구조를 사용할 수 있습니다. 이 방식에서는 이름을 키로 사용하여 해당 이름에 대한 모든 심볼(변수, 함수 등)의 리스트를 관리합니다.

4. **고급 심볼 관리 시스템**: 함수 오버로딩이나 이름 충돌을 해결하기 위해 더 복잡한 심볼 테이블 관리 시스템을 구현할 수 있습니다. 이 시스템은 심볼의 유형, 시그니처, 범위 등을 고려하여 심볼을 정확히 식별하고 관리할 수 있습니다.

이런 접근 방법들은 프로그래밍 언어의 특성과 구현 목표에 따라 선택되며, 복잡성과 성능 요구 사항을 고려하여 적절한 해결책을 채택해야 합니다.

## 사용자 입력:
> 왜 위에서 내가 처음에 제시한 코드가 변수와 함수가 같은 이름을 가질 수는 있지만, 일반적으로 같은 스코프 내에서는 이를 구분하여 관리된다고 했지?

## ChatGPT 응답:
> 제가 이전 답변에서 "변수와 함수가 같은 이름을 가질 수는 있지만, 일반적으로 같은 스코프 내에서는 이를 구분하여 관리한다"고 언급한 것은 일반적인 프로그래밍 언어의 스코프와 심볼 관리 방법에 대한 설명이었습니다. 이는 특정 언어의 특성이나 복잡한 심볼 관리 시스템을 염두에 둔 설명이었으나, 주어진 Java 코드의 `Map` 구조와 직접적으로 관련이 있어 보이는 부분에 대해 혼동을 드린 것 같습니다. 이 부분을 명확히 하도록 하겠습니다.

Java 코드에서 사용된 `Map<String, Symbol> symbols = new LinkedHashMap<>();`는 실제로는 각 키(변수명, 함수명 등)에 대해 오직 하나의 `Symbol` 객체만을 저장할 수 있습니다. 즉, 동일한 이름을 가진 심볼이 여러 개 있을 경우(예를 들어, 함수 오버로딩이나 변수와 함수가 같은 이름을 가지는 경우), `Map` 자체는 이를 구분하여 저장할 수 없습니다. 같은 이름으로 새로운 심볼을 `Map`에 추가하면 이전 심볼은 대체되기 때문입니다.

제가 언급한 "구분하여 관리한다"는 부분은 특정 언어의 구문 분석이나 심볼 테이블 관리에서 볼 수 있는 고급 기능을 가리키며, 이를 달성하기 위해서는 `Map` 이상의 복잡한 데이터 구조나 로직이 필요합니다. 예를 들어, 함수 오버로딩을 처리하기 위해서는 함수 이름 뿐만 아니라 매개변수의 시그니처를 포함한 복합 키를 사용하거나, 같은 이름을 가진 심볼을 구분할 수 있는 추가적인 메커니즘이 필요합니다.

따라서, 실제로 위에서 제공된 코드 조각은 동일한 스코프 내에서 같은 이름을 가진 다양한 유형의 심볼(예: 변수와 함수)을 구분하여 저장하지 않습니다. 이를 위해서는 추가적인 구현이 필요하며, 제가 제공한 설명은 이러한 추가적인 구현 가능성에 대한 일반적인 설명이었습니다. 혼동을 드려 죄송합니다.

## 사용자 입력:
> 그러면, 가능하도록 내가 제시한 코드를 바꿔봐.

## ChatGPT 응답:
> 주어진 코드를 수정하여 동일한 스코프 내에서 같은 이름을 가진 다른 유형의 심볼(예: 변수와 함수)을 구분하여 저장할 수 있도록 하는 방법 중 하나는 각 심볼의 이름과 유형을 조합하여 고유한 키를 생성하는 것입니다. 이를 위해 `Symbol` 클래스에 심볼 유형을 나타내는 필드를 추가하고, 이 유형을 키 생성에 사용할 수 있습니다. 다음은 기존 코드를 이러한 방식으로 수정한 예시입니다.

### 수정된 코드

먼저, `Symbol` 인터페이스에 `getType()` 메서드를 추가하거나 `Symbol` 클래스에 유형을 나타내는 필드를 추가한다고 가정합니다:

```java
public interface Symbol {
    // ... 기존 메서드들 ...

    // 심볼의 유형을 반환하는 메서드
    String getType();
}
```

이제 `BaseScope` 클래스 내에서 심볼을 저장할 때 이름과 유형을 조합하여 키를 생성합니다:

```java
public abstract class BaseScope implements Scope {
    // ... 기존 필드와 메서드 ...

    protected Map<String, Symbol> symbols = new LinkedHashMap<>();

    public void define(Symbol sym) throws IllegalArgumentException {
        // 심볼 이름과 유형을 조합하여 고유한 키 생성
        String key = sym.getName() + ":" + sym.getType();

        if (symbols.containsKey(key)) {
            throw new IllegalArgumentException("Duplicate symbol: " + key);
        }
        sym.setScope(this);
        sym.setInsertionOrderNumber(symbols.size());
        symbols.put(key, sym);
    }

    @Override
    public Symbol getSymbol(String name, String type) {
        // 이름과 유형을 조합하여 심볼 조회
        return symbols.get(name + ":" + type);
    }

    // ... 나머지 메서드들 ...
}
```

이 변경을 통해 변수와 함수가 같은 이름을 공유하더라도, 그들의 유형(`"variable"`, `"function"` 등)이 다르면 다른 키로 저장되어 구분됩니다. 예를 들어, 이름이 `"x"`인 변수와 `"x"`라는 이름의 함수는 각각 `"x:variable"`과 `"x:function"`이라는 키로 저장됩니다.

### 주의 사항
- 이 예시에서는 간단하게 심볼 유형을 문자열로 처리했습니다. 실제 구현에서는 유형을 나타내는 보다 정교한 시스템이 필요할 수 있습니다.
- 함수 오버로딩을 처리하기 위해서는 함수의 매개변수 시그니처도 키 생성에 포함해야 합니다. 이는 본 예시보다 더 복잡한 로직을 필요로 합니다.
- 이 방식은 이름과 유형(또는 매개변수 시그니처)을 기반으로 심볼을 구별하는 단순한 예시입니다. 실제 프로그래밍 언어 구현에서는 더 복잡한 요구 사항과 성능 고려 사항이 있을 수 있습니다.

## 사용자 입력:
> 좀 더 복잡한 로직을 구현해줘.

## ChatGPT 응답:
> 더 복잡한 로직으로는 함수 오버로딩을 지원하는 로직을 구현할 수 있습니다. 함수 오버로딩은 동일한 이름을 가진 여러 함수가 다른 매개변수 세트를 가질 수 있게 하는 기능입니다. 이를 위해서는 함수 이름과 함께 매개변수의 타입이나 개수를 고려하여 고유한 키를 생성해야 합니다.

다음은 `BaseScope` 클래스를 수정하여 함수 오버로딩을 지원하도록 하는 예시입니다. 이 예시에서는 각 함수에 대한 고유한 식별자를 생성하기 위해 함수 이름과 매개변수의 타입을 사용합니다.

### 수정된 `Symbol` 인터페이스와 `FunctionSymbol` 클래스

먼저, `Symbol` 인터페이스와 함수 심볼을 위한 `FunctionSymbol` 클래스를 정의합니다. `FunctionSymbol`은 매개변수의 타입 목록을 포함합니다:

```java
public interface Symbol {
    String getName();
    String getType();
    // ... 기타 필요한 메서드들 ...
}

public class FunctionSymbol implements Symbol {
    private String name;
    private List<String> parameterTypes;

    public FunctionSymbol(String name, List<String> parameterTypes) {
        this.name = name;
        this.parameterTypes = parameterTypes;
    }

    // ... Symbol 인터페이스 구현 ...

    public String getSignature() {
        return name + "(" + String.join(",", parameterTypes) + ")";
    }
}
```

### 수정된 `BaseScope` 클래스

이제 `BaseScope` 클래스에서 심볼을 추가하고 조회하는 방법을 수정합니다:

```java
public abstract class BaseScope implements Scope {
    // ... 기존 필드와 메서드 ...

    protected Map<String, Symbol> symbols = new LinkedHashMap<>();

    public void define(Symbol sym) throws IllegalArgumentException {
        String key = sym instanceof FunctionSymbol ? ((FunctionSymbol) sym).getSignature() : sym.getName();
        
        if (symbols.containsKey(key)) {
            throw new IllegalArgumentException("Duplicate symbol: " + key);
        }
        sym.setScope(this);
        sym.setInsertionOrderNumber(symbols.size());
        symbols.put(key, sym);
    }

    @Override
    public Symbol getSymbol(String name) {
        // 이름만으로 심볼을 조회하는 경우
        return symbols.values().stream()
                .filter(s -> s.getName().equals(name))
                .findFirst()
                .orElse(null);
    }

    public Symbol getFunctionSymbol(String name, List<String> parameterTypes) {
        // 함수 이름과 매개변수 타입을 사용하여 함수 심볼을 조회
        String key = name + "(" + String.join(",", parameterTypes) + ")";
        return symbols.get(key);
    }

    // ... 나머지 메서드들 ...
}
```

이 코드에서 `FunctionSymbol`의 고유한 키는 함수 이름과 매개변수 타입의 조합으로 생성됩니다. 예를 들어, `foo(int, int)`와 `foo(String)`은 각각 `"foo(int,int)"`와 `"foo(String)"`로 식별됩니다.

### 주의 사항
- 이 예시는 함수 오버로딩을 지원하는 방식의 하나일 뿐, 실제 언어 및 컴파일러 구현에서는 더 많은 고려 사항과 성능 최적화가 필요합니다.
- 함수 오버로딩을 지원하는 로직은 언어의 규칙과 특성에 따라 달라질 수 있으며, 매개변수 타입 매칭, 코어션, 디폴트 매개변수 처리 등 복잡한 문제들을 다루어야 할 수도 있습니다.
- 실제 구현에서는 함수의 리턴 타입, 접근 지정자, 예외 명세 등 다양한 요소를 고려할 수도 있습니다.

