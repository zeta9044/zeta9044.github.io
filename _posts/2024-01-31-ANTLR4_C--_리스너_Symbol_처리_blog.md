---
layout: post
title: "ANTLR4 C++ 리스너 Symbol 처리"
date: 2024-01-31
tags: [Symbol, C, 리스너, 처리]
categories: blog
---

## 사용자 입력:
> ANTLR4로 C++소스를 분석하는 리스너를 만드는데, scope를 정의해서 각 symbol을 추출하려고해.  scope에서  symbol이 Map<String, Symbol> symbols = new LinkedHashMap<>(); 이렇게 정의하고 있는데, 함수명하고, 변수명은 같을 수 있고, 같은 함수명에 파라미터만 다를 수 있는데, 이렇게 하면, 예를 들어, 같은 이름이 3개인데, 각각 다른 함수,변수야. 그러면, map이니까, 나중에 들어간 symbol만 찾을 수 있는 거 아녀?

## ChatGPT 응답:
> ANTLR4를 사용하여 C++ 소스 코드를 분석하고 리스너(listener)를 만들 때, 같은 이름을 가진 다양한 심볼(예: 함수, 변수 등)을 처리하는 것은 일반적인 문제입니다. `Map<String, Symbol>` 구조를 사용하면, 말씀하신 것처럼, 같은 이름을 가진 심볼이 여러 개 있을 때 마지막에 추가된 심볼만 저장되고 이전 것들은 덮어쓰게 됩니다. 이는 함수 오버로딩이나 변수와 함수가 같은 이름을 공유할 수 있는 언어의 특성을 제대로 다루지 못합니다.

이 문제를 해결하기 위해 다음과 같은 접근 방법을 고려할 수 있습니다:

### 1. 심볼 이름과 유형을 키로 사용하기

심볼의 이름뿐만 아니라 유형(예: 변수, 함수, 클래스 등) 또는 함수의 경우 파라미터 타입 등을 조합하여 맵의 키로 사용할 수 있습니다. 이 방법은 변수와 함수가 같은 이름을 가질 수 있는 경우에 유용합니다.

```java
Map<String, Symbol> symbols = new LinkedHashMap<>();
```

### 2. 이름을 키로 사용하고 값으로 리스트 사용하기

같은 이름을 가진 심볼을 처리하기 위해 `Map<String, List<Symbol>>` 구조를 사용할 수 있습니다. 이렇게 하면 같은 이름을 가진 여러 심볼을 리스트에 저장할 수 있고, 각각에 대해 접근할 수 있습니다.

```java
Map<String, List<Symbol>> symbols = new LinkedHashMap<>();
```

### 3. 멀티키를 사용하는 맵 사용하기

멀티키를 사용할 수 있는 맵 구조를 사용하여, 이름과 심볼의 유형(또는 함수의 경우 파라미터 시그니처)을 조합한 키로 각 심볼을 구별할 수 있습니다. Apache Commons Collections 같은 라이브러리에서 제공하는 `MultiKeyMap`을 사용할 수 있습니다.

```java
MultiKeyMap multiKeySymbols = new MultiKeyMap();
```

### 4. Custom Key Object 사용하기

함수명과 파라미터 타입(또는 개수)을 포함하는 커스텀 키 객체를 정의하고, 이를 맵의 키로 사용할 수 있습니다. 이 방법은 함수 오버로딩을 처리할 때 특히 유용합니다.

```java
class SymbolKey {
    String name;
    List<String> parameterTypes; // 또는 간단한 개수 정보, 타입 등

    // equals()와 hashCode() 구현
}

Map<SymbolKey, Symbol> symbols = new HashMap<>();
```

함수 오버로딩을 정확히 처리하기 위해서는 함수 시그니처(함수 이름과 파라미터 타입 목록)를 고려해야 합니다. 이렇게 하면 동일한 이름을 가진 여러 함수를 올바르게 구별하고 저장할 수 있습니다.

## 사용자 입력:
> C++에서 구분용도로 구체적으로 제시해줘.

## ChatGPT 응답:
> C++에서 함수 오버로딩과 같은 이름을 가진 심볼들을 구분하기 위해 ANTLR4 리스너를 사용하여 심볼 테이블을 구성할 때, 각 심볼을 구별하기 위한 구체적인 접근 방법을 제공하겠습니다. C++의 경우, 함수는 이름뿐만 아니라 매개변수의 타입과 개수에 따라서도 구별됩니다. 따라서 심볼을 구분하기 위해, 함수의 이름과 매개변수의 타입 및 개수를 포함하는 복합 키를 사용할 수 있습니다.

### 심볼 키를 위한 구조 정의

C++에서 함수 오버로딩을 처리하기 위해, 함수 이름과 매개변수 리스트(타입 및 개수)를 포함하는 복합 키를 사용하는 커스텀 객체를 정의할 수 있습니다. 이 객체는 맵의 키로 사용되며, `equals()`와 `hashCode()` 메소드를 오버라이드하여 객체 비교가 올바르게 수행되도록 해야 합니다.

```cpp
class FunctionSignature {
public:
    std::string name;
    std::vector<std::string> parameterTypes;

    FunctionSignature(const std::string& name, const std::vector<std::string>& parameterTypes)
        : name(name), parameterTypes(parameterTypes) {}

    // 비교 연산자 오버로드
    bool operator==(const FunctionSignature& other) const {
        return name == other.name && parameterTypes == other.parameterTypes;
    }

    // 해시 함수 정의
    friend std::size_t hash_value(const FunctionSignature& fs) {
        std::size_t seed = 0;
        boost::hash_combine(seed, boost::hash_value(fs.name));
        for (const auto& type : fs.parameterTypes) {
            boost::hash_combine(seed, boost::hash_value(type));
        }
        return seed;
    }
};

// 맵을 위한 해시 함수 오버로드
namespace std {
    template <>
    struct hash<FunctionSignature> {
        std::size_t operator()(const FunctionSignature& fs) const {
            return hash_value(fs);
        }
    };
}
```

### 심볼 테이블 구조

위에서 정의한 `FunctionSignature`를 사용하여 심볼 테이블을 구성합니다. 여기서는 `std::unordered_map`을 사용하며, 키로는 `FunctionSignature`를, 값으로는 심볼에 대한 정보(예: 함수의 반환 타입, 변수 타입 등)를 저장하는 구조체를 사용합니다.

```cpp
class SymbolInfo {
public:
    // 심볼 정보, 예: 타입, 스코프 레벨 등
    std::string type;
    // 추가 정보 필드
};

std::unordered_map<FunctionSignature, SymbolInfo> symbolTable;
```

### 사용 예시

함수 심볼을 심볼 테이블에 추가하는 예제입니다:

```cpp
// 함수 심볼 추가
std::string functionName = "myFunction";
std::vector<std::string> paramTypes = {"int", "double"};
FunctionSignature signature(functionName, paramTypes);
SymbolInfo info{"void"}; // 예: 반환 타입이 void

symbolTable[signature] = info;
```

이 방법을 사용하면, 이름이 같지만 매개변수 타입이나 개수가 다른 함수들(오버로딩된 함수들)을 심볼 테이블 내에서 정확히 구별하여 관리할 수 있습니다. C++의 오버로딩 규칙을 적용하여 심볼의 유일성을 보장하며, 이러한 구조는 ANTLR4 리스너 내에서 심볼 정보를 추출하고 저장하는 데 사용될 수 있습니다.

## 사용자 입력:
> C++소스분석을 위해서 자바로 개발하고 있다는데, 꼭 C++로 제시하더라.

## ChatGPT 응답:
> C++ 소스 코드 분석을 위한 도구를 Java로 개발하고 있고, C++에서 함수 오버로딩과 같은 이름을 가진 심볼들을 구분하는 방법을 자바 코드로 제공하는 것이 목적이라면, 이전에 C++ 코드로 제시한 예시를 Java에 맞게 변환해 보겠습니다. Java에서는 같은 이름을 가진 여러 함수를 구분하기 위해 함수의 시그니처(함수 이름과 파라미터 타입)를 고려해야 합니다. 이를 위해 Java에서는 클래스와 해시맵을 사용하여 이러한 정보를 관리할 수 있습니다.

### 심볼 키를 위한 Java 클래스 정의

함수의 이름과 매개변수 타입을 포함하는 복합 키를 위한 Java 클래스를 정의합니다. 이 클래스는 맵의 키로 사용될 것이며, `equals()`와 `hashCode()` 메소드를 오버라이드하여 객체 비교를 올바르게 수행합니다.

```java
import java.util.List;
import java.util.Objects;

public class FunctionSignature {
    private String name;
    private List<String> parameterTypes;

    public FunctionSignature(String name, List<String> parameterTypes) {
        this.name = name;
        this.parameterTypes = parameterTypes;
    }

    // Getters and setters

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        FunctionSignature that = (FunctionSignature) o;
        return Objects.equals(name, that.name) &&
               Objects.equals(parameterTypes, that.parameterTypes);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, parameterTypes);
    }
}
```

### 심볼 테이블 구조

`FunctionSignature` 클래스를 사용하여 심볼 테이블을 구성합니다. 여기서는 `HashMap`을 사용하며, 키로는 `FunctionSignature`를, 값으로는 심볼에 대한 정보를 저장하는 구조체나 클래스를 사용합니다.

```java
import java.util.HashMap;
import java.util.Map;

public class SymbolTable {
    private Map<FunctionSignature, SymbolInfo> symbols = new HashMap<>();

    // 심볼 추가 및 조회 메소드
}
```

### 심볼 정보를 위한 클래스

심볼 정보(예: 함수의 반환 타입, 변수 타입 등)를 저장하기 위한 `SymbolInfo` 클래스를 정의합니다.

```java
public class SymbolInfo {
    private String type; // 예: 반환 타입

    // 생성자, getters, setters
}
```

### 사용 예시

함수 심볼을 심볼 테이블에 추가하는 예제입니다:

```java
import java.util.Arrays;

public class SymbolTableExample {
    public static void main(String[] args) {
        SymbolTable symbolTable = new SymbolTable();

        // 함수 심볼 추가
        String functionName = "myFunction";
        List<String> paramTypes = Arrays.asList("int", "double");
        FunctionSignature signature = new FunctionSignature(functionName, paramTypes);
        SymbolInfo info = new SymbolInfo("void"); // 예: 반환 타입이 void

        symbolTable.getSymbols().put(signature, info);
    }
}
```

이 방법을 사용하면, Java에서 C++ 소스 코드 분석 도구를 개발할 때, 이름이 같지만 매개변수 타입이나 개수가 다른 함수들(오버로딩된 함수들)을 정확히 구별하여 관리할 수 있습니다. 이 구조는 ANTLR4 리스너 내에서 C++ 코드의 심볼 정보를 추출하고 저장하는 데 사용될 수 있습니다.

## 사용자 입력:
> 같은 scope에 함수만 있는게 아니라 변수도 있어.

## ChatGPT 응답:
> 같은 스코프 내에 함수와 변수가 모두 존재하며, 이들을 구분하여 관리해야 할 경우, Java에서는 조금 더 복잡한 자료 구조를 사용하여 이를 처리할 수 있습니다. 심볼의 종류(함수, 변수 등)와 식별자(이름), 그리고 함수의 경우는 매개변수의 타입까지 고려하여 심볼을 구별할 필요가 있습니다. 이를 위해 심볼 정보를 저장하는 `SymbolInfo` 클래스를 확장하고, 심볼을 저장할 때 식별자와 심볼의 종류를 모두 고려하는 방식으로 구현할 수 있습니다.

### SymbolInfo 클래스 확장

심볼 정보 클래스에 심볼의 종류(예: 함수, 변수)를 나타내는 필드를 추가하여, 함수와 변수를 구분할 수 있도록 합니다.

```java
public class SymbolInfo {
    private String name; // 심볼의 이름
    private String type; // 변수의 타입 혹은 함수의 반환 타입
    private SymbolKind kind; // 심볼의 종류 (함수, 변수 등)
    private List<String> parameterTypes; // 함수의 경우 매개변수 타입 리스트

    public SymbolInfo(String name, String type, SymbolKind kind, List<String> parameterTypes) {
        this.name = name;
        this.type = type;
        this.kind = kind;
        this.parameterTypes = parameterTypes;
    }

    // Getters and setters

    public enum SymbolKind {
        FUNCTION,
        VARIABLE
    }
}
```

### 심볼 테이블 구조 개선

심볼 테이블에서는 `FunctionSignature` 대신 `SymbolInfo` 객체를 직접 키로 사용하거나, 심볼의 이름과 종류를 기반으로 하는 복합 키를 사용하여 심볼을 저장하고 검색할 수 있습니다. 이 경우, `equals()`와 `hashCode()` 메소드를 심볼 이름과 종류, 그리고 함수의 경우 매개변수 타입까지 고려하여 구현해야 합니다.

### Symbol 테이블에 다양한 심볼 추가

심볼 테이블에 함수와 변수를 모두 추가할 수 있도록, 심볼을 추가하는 로직을 개선합니다.

```java
import java.util.HashMap;
import java.util.Map;
import java.util.ArrayList;

public class SymbolTable {
    private Map<String, List<SymbolInfo>> symbols = new HashMap<>();

    public void addSymbol(String name, String type, SymbolInfo.SymbolKind kind, List<String> parameterTypes) {
        SymbolInfo newSymbol = new SymbolInfo(name, type, kind, parameterTypes);
        
        // 동일한 이름을 가진 심볼 리스트를 찾거나 새 리스트를 생성
        symbols.computeIfAbsent(name, k -> new ArrayList<>()).add(newSymbol);
    }

    // 심볼 조회 메소드 등
}
```

### 사용 예시

변수와 함수를 심볼 테이블에 추가하는 예시입니다:

```java
public class SymbolTableExample {
    public static void main(String[] args) {
        SymbolTable symbolTable = new SymbolTable();

        // 변수 심볼 추가
        symbolTable.addSymbol("myVar", "int", SymbolInfo.SymbolKind.VARIABLE, null);

        // 함수 심볼 추가
        ArrayList<String> paramTypes = new ArrayList<>();
        paramTypes.add("int");
        paramTypes.add("double");
        symbolTable.addSymbol("myFunction", "void", SymbolInfo.SymbolKind.FUNCTION, paramTypes);
    }
}
```

이 접근 방식을 사용하면, 같은 스코프 내에 있는 함수와 변수를 구분하여 저장하고 관리할 수 있습니다. 각 심볼은 이름뿐만 아니라 종류(변수, 함수)와, 함수의 경우 매개변수 타입까지 고려하여 구별됩니다. 이렇게 하면 오버로딩된 함수와 같은 이름을 가진 변수를 정확하게 처리할

## 사용자 입력:
> 변수 이름과 함수 이름이 같은게 있을 때 추출하는 예로 보여줘야.

## ChatGPT 응답:
> 변수 이름과 함수 이름이 같을 수 있는 상황을 처리하기 위해, Java로 구현된 심볼 테이블에서 이러한 심볼들을 정확하게 추출하는 예시를 다루겠습니다. 이 예시에서는 심볼의 이름, 타입, 종류(함수 또는 변수), 그리고 함수의 경우 매개변수 타입 리스트를 고려하여 심볼 정보를 저장하고 조회하는 방법을 보여줍니다.

### SymbolInfo 클래스와 SymbolTable 클래스

먼저, 심볼 정보(`SymbolInfo`) 클래스와 심볼 테이블(`SymbolTable`) 클래스를 정의합니다. `SymbolInfo` 클래스는 심볼의 종류에 따라 다른 정보를 저장할 수 있도록 구성되며, `SymbolTable` 클래스는 이러한 심볼들을 관리합니다.

```java
import java.util.*;

public class SymbolInfo {
    private String name; // 심볼 이름
    private String type; // 타입 (변수 타입 또는 함수 반환 타입)
    private SymbolKind kind; // 심볼 종류
    private List<String> parameterTypes; // 함수의 매개변수 타입 리스트

    // Constructor, getters and setters

    public enum SymbolKind {
        FUNCTION,
        VARIABLE
    }

    public SymbolInfo(String name, String type, SymbolKind kind, List<String> parameterTypes) {
        this.name = name;
        this.type = type;
        this.kind = kind;
        this.parameterTypes = parameterTypes;
    }
}

public class SymbolTable {
    private Map<String, List<SymbolInfo>> symbols = new HashMap<>();

    public void addSymbol(String name, String type, SymbolInfo.SymbolKind kind, List<String> parameterTypes) {
        SymbolInfo newSymbol = new SymbolInfo(name, type, kind, parameterTypes);
        symbols.computeIfAbsent(name, k -> new ArrayList<>()).add(newSymbol);
    }

    public List<SymbolInfo> getSymbolsByName(String name) {
        return symbols.getOrDefault(name, Collections.emptyList());
    }
}
```

### 변수와 함수의 이름이 같을 때의 처리

변수와 함수가 같은 이름을 가질 때, 두 심볼을 `SymbolTable`에 추가하고, 이를 조회하는 방법을 예시로 드리겠습니다.

```java
public class SymbolTableExample {
    public static void main(String[] args) {
        SymbolTable symbolTable = new SymbolTable();

        // 변수 추가
        symbolTable.addSymbol("mySymbol", "int", SymbolInfo.SymbolKind.VARIABLE, null);

        // 이름이 같지만 매개변수 타입이 다른 함수 추가
        symbolTable.addSymbol("mySymbol", "void", SymbolInfo.SymbolKind.FUNCTION, Arrays.asList("int"));

        // 이름이 같은 심볼 조회
        List<SymbolInfo> symbols = symbolTable.getSymbolsByName("mySymbol");

        for (SymbolInfo symbol : symbols) {
            System.out.println("Name: " + symbol.getName() + ", Type: " + symbol.getType() +
                    ", Kind: " + symbol.getKind() +
                    ", Parameters: " + (symbol.getParameterTypes() != null ? symbol.getParameterTypes() : "N/A"));
        }
    }
}
```

이 예시에서 `mySymbol`이라는 이름을 가진 변수와 함수를 `SymbolTable`에 추가했습니다. 변수는 매개변수 타입 리스트 없이 추가되며, 함수는 매개변수 타입 리스트(`"int"`)를 포함하여 추가됩니다. `getSymbolsByName` 메소드를 사용하여 같은 이름을 가진 모든 심볼을 조회할 때, 변수와 함수가 구분되어 나타납니다.

이 방식을 통해, 같은 스코프 내에서 이름이 동일한 변수와 함수를 구분하여 관리하고, 필요에 따라 적절히 추출할 수 있습니다. 이러한 접근 방식은 C++ 소스 코드 분석 도구 개발에 있어서 중요한 구성 요소가 될 수 있습니다.

## 사용자 입력:
> 같은 symbol 조회 같은 건 필요없어. 정확하게  instance 변수, 선언 변수, 함수를 이름으로 찾고 싶은 거야.

## ChatGPT 응답:
> 이름으로 인스턴스 변수, 선언 변수, 함수를 정확하게 찾기 위해, Java에서 심볼 테이블을 구성할 때 각 심볼의 종류와 이름을 기반으로 한 더 구체적인 접근 방법이 필요합니다. 이를 위해, 각 심볼 종류(인스턴스 변수, 선언 변수, 함수)에 대한 구분이 명확하게 이루어지도록 심볼 정보를 저장하고 조회하는 메커니즘을 구현해야 합니다. 여기서는 심볼의 종류를 나타내는 `enum`을 활용하고, 이를 기반으로 심볼을 저장하고 검색하는 방법을 제공하겠습니다.

### Symbol 종류 구분

심볼의 종류를 나타내기 위한 `enum`을 정의하고, 이를 `SymbolInfo` 클래스에 포함시킵니다. 이 예시에서는 인스턴스 변수(instance variable), 선언 변수(local variable), 함수(function)를 구분합니다.

```java
public enum SymbolKind {
    INSTANCE_VARIABLE,
    LOCAL_VARIABLE,
    FUNCTION
}

public class SymbolInfo {
    private String name;
    private String type; // 함수의 반환 타입 또는 변수의 타입
    private SymbolKind kind;
    private List<String> parameterTypes; // 함수의 경우만 사용

    // Constructor, getters, and setters...
}
```

### 심볼 테이블 구조

심볼 테이블에서는 `SymbolInfo` 객체를 저장하며, 심볼의 이름과 종류를 통해 정확하게 심볼을 찾을 수 있도록 합니다. 여기서는 간단한 조회 메소드를 추가하여, 주어진 이름과 종류에 해당하는 심볼을 찾을 수 있습니다.

```java
import java.util.*;

public class SymbolTable {
    private List<SymbolInfo> symbols = new ArrayList<>();

    public void addSymbol(SymbolInfo symbol) {
        symbols.add(symbol);
    }

    public SymbolInfo findSymbol(String name, SymbolKind kind) {
        for (SymbolInfo symbol : symbols) {
            if (symbol.getName().equals(name) && symbol.getKind() == kind) {
                return symbol;
            }
        }
        return null; // 심볼을 찾지 못한 경우 null 반환
    }
}
```

### 심볼 추가 및 조회 예시

인스턴스 변수, 선언 변수, 함수를 심볼 테이블에 추가하고, 이름으로 이들을 조회하는 방법을 보여주는 예시입니다.

```java
public class SymbolTableExample {
    public static void main(String[] args) {
        SymbolTable symbolTable = new SymbolTable();

        // 인스턴스 변수 추가
        symbolTable.addSymbol(new SymbolInfo("myVar", "int", SymbolKind.INSTANCE_VARIABLE, null));

        // 선언 변수 추가
        symbolTable.addSymbol(new SymbolInfo("myVar", "int", SymbolKind.LOCAL_VARIABLE, null));

        // 함수 추가
        symbolTable.addSymbol(new SymbolInfo("myFunction", "void", SymbolKind.FUNCTION, Arrays.asList("int")));

        // 이름으로 인스턴스 변수 조회
        SymbolInfo instanceVar = symbolTable.findSymbol("myVar", SymbolKind.INSTANCE_VARIABLE);
        System.out.println("Found: " + instanceVar.getName() + ", Type: " + instanceVar.getType() + ", Kind: " + instanceVar.getKind());

        // 이름으로 함수 조회
        SymbolInfo function = symbolTable.findSymbol("myFunction", SymbolKind.FUNCTION);
        System.out.println("Found: " + function.getName() + ", Type: " + function.getType() + ", Kind: " + function.getKind());
    }
}
```

이 예시를 통해, 주어진 이름과 종류(`SymbolKind`)에 따라 인스턴스 변수, 선언 변수, 함수를 정확하게 찾고 구분할 수 있습니다. `findSymbol` 메소드는 주어진 이름과 종류에 해당하는 심볼이 심볼 테이블 내에 존재하는지 검사하고, 해당하는 심볼 정보를 반환합니다. 이러한 접근 방식은 C++ 소스 코드

## 사용자 입력:
> int x;  x = 1; 이르면 int x는 선언변수, x =1 , x는 인스턴스변수라고 칭한 거야...

## ChatGPT 응답:
> 알겠습니다. C++ 소스 코드 분석에서 변수 선언(`int x;`)과 변수 사용(`x = 1;`)을 구분하려는 의도를 바탕으로, "선언 변수"와 "인스턴스 변수"라는 용어에 대한 오해가 있었던 것 같습니다. 일반적으로 프로그래밍 언어에서 "인스턴스 변수"는 객체 지향 프로그래밍에서 클래스의 인스턴스에 속한 변수를 가리킵니다. 여기서는 다음과 같이 정정하겠습니다:

- **변수 선언(Variable Declaration):** 변수가 선언되는 위치를 의미합니다 (`int x;`).
- **변수 사용(Variable Usage):** 변수가 사용되는 위치를 의미합니다 (`x = 1;`).

변수 선언과 사용을 구분하기 위한 심볼 테이블 구성이 목표라면, 심볼 정보(`SymbolInfo`) 클래스와 심볼 테이블(`SymbolTable`)을 사용하여 이를 구현할 수 있습니다. 하지만 주어진 예에서 "변수 선언"과 "변수 사용"을 심볼의 종류로 나누는 대신, 심볼 사용의 문맥(context)을 식별할 수 있도록 정보를 저장하는 방식을 적용할 필요가 있습니다.

### 심볼 정보와 심볼 테이블 구조 조정

```java
public class SymbolInfo {
    private String name; // 심볼 이름
    private String type; // 변수 타입
    private SymbolUsage usage; // 심볼 사용 유형: 선언(DECLARATION) 또는 사용(USAGE)

    public SymbolInfo(String name, String type, SymbolUsage usage) {
        this.name = name;
        this.type = type;
        this.usage = usage;
    }

    // Getters and setters...

    public enum SymbolUsage {
        DECLARATION, USAGE
    }
}

public class SymbolTable {
    private List<SymbolInfo> symbols = new ArrayList<>();

    public void addSymbol(SymbolInfo symbol) {
        symbols.add(symbol);
    }

    public List<SymbolInfo> findSymbolsByName(String name) {
        List<SymbolInfo> foundSymbols = new ArrayList<>();
        for (SymbolInfo symbol : symbols) {
            if (symbol.getName().equals(name)) {
                foundSymbols.add(symbol);
            }
        }
        return foundSymbols; // 이름이 일치하는 모든 심볼 반환
    }
}
```

### 변수 선언과 사용 식별

변수 선언과 사용을 식별하는 예시 코드입니다:

```java
public class SymbolTableExample {
    public static void main(String[] args) {
        SymbolTable symbolTable = new SymbolTable();

        // 변수 선언 추가
        symbolTable.addSymbol(new SymbolInfo("x", "int", SymbolInfo.SymbolUsage.DECLARATION));

        // 변수 사용 추가 (이 예에서는 변수 사용을 별도로 표시하는 방법을 보여줍니다)
        symbolTable.addSymbol(new SymbolInfo("x", "int", SymbolInfo.SymbolUsage.USAGE));

        // 'x'에 대한 모든 심볼 조회
        List<SymbolInfo> xSymbols = symbolTable.findSymbolsByName("x");
        for (SymbolInfo symbol : xSymbols) {
            System.out.println("Name: " + symbol.getName() + ", Type: " + symbol.getType() + ", Usage: " + symbol.getUsage());
        }
    }
}
```

이 접근 방식을 통해, 동일한 이름('x')을 가진 심볼이 변수 선언인지, 아니면 변수 사용인지를 구분하여 처리할 수 있습니다. 이러한 정보는 C++ 소스 코드 분석 도구를 개발할 때 변수의 선언과 사용 위치를 파악하고, 소스 코드 내에서 변수의 사용 패턴을 분석하는 데 유용하게 사용될 수 있습니다.

## 사용자 입력:
> 선언하고 사용한 변수에 대해서 연관관계를 추가해줘.

## ChatGPT 응답:
> 변수의 선언과 사용에 대한 연관 관계를 추적하기 위해, 심볼 정보(`SymbolInfo`)와 심볼 테이블(`SymbolTable`)을 확장하여 변수의 선언과 그 변수가 사용된 모든 인스턴스를 연결할 수 있는 구조를 제공하겠습니다. 이를 통해 변수가 선언된 위치와 해당 변수가 사용된 모든 위치 사이의 연관 관계를 명확히 할 수 있습니다.

### 심볼 정보 확장

변수 선언과 사용 사이의 연관 관계를 나타내기 위해, `SymbolInfo` 클래스에 변수 사용 정보를 추가합니다. 변수 선언에 대한 참조를 포함하도록 하여, 각 사용 사례가 해당 변수의 선언을 참조할 수 있도록 합니다.

```java
public class SymbolInfo {
    private String name;
    private String type;
    private SymbolUsage usage;
    private SymbolInfo declaration; // 변수 사용의 경우 선언에 대한 참조

    public SymbolInfo(String name, String type, SymbolUsage usage) {
        this.name = name;
        this.type = type;
        this.usage = usage;
        this.declaration = null; // 기본값은 null
    }

    // 선언에 대한 참조를 설정하는 메소드
    public void setDeclaration(SymbolInfo declaration) {
        if (this.usage == SymbolUsage.USAGE) {
            this.declaration = declaration;
        }
    }

    // Getters and setters...

    public enum SymbolUsage {
        DECLARATION, USAGE
    }
}
```

### 심볼 테이블 수정

`SymbolTable` 클래스에서는 변수 선언을 찾고, 변수 사용 사례에 선언을 연결하는 메소드를 추가합니다. 변수 사용 사례를 추가할 때, 해당 변수의 선언을 찾아 `SymbolInfo` 객체에 연결합니다.

```java
import java.util.*;

public class SymbolTable {
    private List<SymbolInfo> symbols = new ArrayList<>();

    public void addSymbol(SymbolInfo symbol) {
        // 변수 사용인 경우, 선언을 찾아서 연결
        if (symbol.getUsage() == SymbolInfo.SymbolUsage.USAGE) {
            SymbolInfo declaration = findDeclaration(symbol.getName());
            if (declaration != null) {
                symbol.setDeclaration(declaration);
            }
        }
        symbols.add(symbol);
    }

    private SymbolInfo findDeclaration(String name) {
        for (SymbolInfo symbol : symbols) {
            if (symbol.getName().equals(name) && symbol.getUsage() == SymbolInfo.SymbolUsage.DECLARATION) {
                return symbol;
            }
        }
        return null;
    }

    // 기존 메소드...
}
```

### 사용 예시

변수 `x`가 선언되고 사용되는 경우를 추적하는 예시입니다.

```java
public class SymbolTableExample {
    public static void main(String[] args) {
        SymbolTable symbolTable = new SymbolTable();

        // 변수 'x' 선언 추가
        symbolTable.addSymbol(new SymbolInfo("x", "int", SymbolInfo.SymbolUsage.DECLARATION));

        // 변수 'x' 사용 추가
        SymbolInfo usage1 = new SymbolInfo("x", "int", SymbolInfo.SymbolUsage.USAGE);
        symbolTable.addSymbol(usage1);

        // 'x' 사용 사례 조회 및 선언과의 연결 확인
        List<SymbolInfo> xUsages = symbolTable.findSymbolsByName("x");
        for (SymbolInfo usage : xUsages) {
            if (usage.getUsage() == SymbolInfo.SymbolUsage.USAGE) {
                System.out.println("Usage of: " + usage.getName() + " is linked to declaration: " +
                        (usage.getDeclaration() != null ? "Yes" : "No"));
            }
        }
    }
}
```

이 구조를 사용하면, 변수가 선언된 위치와 해당 변수가 사용된 모든 위치 사이의 연관 관계를 명확히 추적할 수 있습니다. 이를 통해, 소스 코드 분석 도구에서 변수의 생명주기와 스코프를 더 잘 이해하고, 코드 내에서 변수 사용의 정확성을 검증하는 데 도움이 될 수 있습니다.

## 사용자 입력:
> 코드 블럭에 따라서  int x가 동일할 수 있잖아.

## ChatGPT 응답:
> 코드 블록에 따라 동일한 이름의 변수 `int x`가 다른 스코프에서 선언될 수 있는 상황을 처리하기 위해, 스코프 관리 기능을 심볼 테이블에 추가할 필요가 있습니다. 이를 위해, 각 심볼에 대한 스코프 정보를 추가하고, 현재 스코프를 추적할 수 있는 메커니즘을 구현해야 합니다. 스코프는 일반적으로 계층적 구조를 가지며, 심볼 테이블은 이러한 구조를 반영하여 각 심볼의 스코프를 정확하게 관리할 수 있어야 합니다.

### 스코프 관리를 위한 클래스 추가

스코프를 나타내는 `Scope` 클래스를 추가하고, 각 `SymbolInfo`에 스코프 정보를 포함시킵니다. 이를 통해 같은 이름의 변수가 다른 스코프에서 선언되었을 때 이를 구분할 수 있습니다.

```java
public class Scope {
    private Scope parentScope;
    private List<Scope> childScopes;
    private List<SymbolInfo> symbols;
    private String scopeName;

    public Scope(Scope parentScope, String scopeName) {
        this.parentScope = parentScope;
        this.scopeName = scopeName;
        this.childScopes = new ArrayList<>();
        this.symbols = new ArrayList<>();
        if (parentScope != null) {
            parentScope.addChildScope(this);
        }
    }

    public void addSymbol(SymbolInfo symbol) {
        symbols.add(symbol);
    }

    public void addChildScope(Scope childScope) {
        childScopes.add(childScope);
    }

    // Getters and setters...

    public Scope getParentScope() {
        return parentScope;
    }

    public List<SymbolInfo> getSymbols() {
        return symbols;
    }

    public String getScopeName() {
        return scopeName;
    }

    // 스코프 내에서 심볼 찾기
    public SymbolInfo findSymbolInScope(String name) {
        for (SymbolInfo symbol : symbols) {
            if (symbol.getName().equals(name)) {
                return symbol;
            }
        }
        return null;
    }
}

public class SymbolInfo {
    private String name;
    private String type;
    private SymbolUsage usage;
    private SymbolInfo declaration; // 사용 사례의 선언 참조
    private Scope scope; // 심볼이 속한 스코프

    // Constructor including scope, getters, and setters...
}
```

### 스코프 기반 심볼 관리

변수 선언 및 사용을 스코프에 따라 관리하는 예를 보여주는 방법입니다. 각 스코프에서 변수를 선언하거나 사용할 때, 현재 스코프 정보를 `SymbolInfo` 객체에 포함시켜서 스코프를 기반으로 심볼을 구분하게 됩니다.

```java
Scope globalScope = new Scope(null, "global");
Scope functionScope = new Scope(globalScope, "function");

// 글로벌 스코프에 변수 x 선언
SymbolInfo globalX = new SymbolInfo("x", "int", SymbolUsage.DECLARATION, globalScope);
globalScope.addSymbol(globalX);

// 함수 스코프에 동일한 이름의 변수 x 선언
SymbolInfo functionX = new SymbolInfo("x", "int", SymbolUsage.DECLARATION, functionScope);
functionScope.addSymbol(functionX);

// 각 스코프에서 x 찾기
SymbolInfo foundInGlobal = globalScope.findSymbolInScope("x");
SymbolInfo foundInFunction = functionScope.findSymbolInScope("x");
```

이러한 접근 방식을 사용함으로써, 동일한 이름(`x`)을 가진 변수가 있더라도, 각 변수가 속한 스코프에 따라 이를 정확하게 구분할 수 있습니다. 스코프 정보를 포함시킴으로써, 변수의

## ChatGPT 응답:
>  선언과 사용을 스코프별로 추적하고 관리하는 복잡한 상황을 효과적으로 처리할 수 있습니다. 이 방법은 특히 중첩된 함수나 조건문, 반복문 등에서 서로 다른 스코프에 동일한 이름의 변수가 선언되는 경우에 유용합니다.

## 사용자 입력:
> 그러면, 함수 구현 블럭 안에 중첩된 것 구분할려면?

## ChatGPT 응답:
> 함수 구현 블록 내에서 중첩된 스코프(예: 중첩된 블록, 조건문, 반복문 등)를 구분하기 위해서는 스코프 관리 시스템을 더 세밀하게 구현해야 합니다. 이를 위해 각 스코프를 나타내는 `Scope` 클래스를 활용하며, 스코프를 계층적으로 관리하여 함수 내부의 중첩된 스코프까지 정확하게 추적할 수 있도록 합니다.

### 계층적 스코프 관리

`Scope` 클래스는 이미 각 스코프가 부모 스코프를 참조하고 자식 스코프 목록을 가지도록 설계되었습니다. 이 구조를 활용하여 함수 내부에서 발생하는 중첩된 스코프를 관리합니다. 

각 스코프는 고유한 식별자(예: 이름) 또는 수준(예: 깊이)을 가질 수 있으며, 이를 통해 스코프를 구분합니다. 중첩된 스코프는 함수 스코프의 자식으로 추가되며, 더 깊은 중첩 스코프는 해당 부모 스코프의 자식으로 계속 추가됩니다.

### 예제: 함수 내 중첩된 스코프 처리

```java
public class ScopeExample {
    public static void main(String[] args) {
        // 전역 스코프 생성
        Scope globalScope = new Scope(null, "global");

        // 함수 스코프 생성 및 전역 스코프에 추가
        Scope functionScope = new Scope(globalScope, "function");

        // 함수 내부의 중첩된 스코프 생성 및 함수 스코프에 추가
        Scope nestedScope1 = new Scope(functionScope, "nested1");
        Scope nestedScope2 = new Scope(nestedScope1, "nested2"); // nested1 내부의 더 깊은 중첩

        // 스코프별 변수 선언
        functionScope.addSymbol(new SymbolInfo("x", "int", SymbolInfo.SymbolUsage.DECLARATION, functionScope));
        nestedScope1.addSymbol(new SymbolInfo("y", "int", SymbolInfo.SymbolUsage.DECLARATION, nestedScope1));
        nestedScope2.addSymbol(new SymbolInfo("z", "int", SymbolInfo.SymbolUsage.DECLARATION, nestedScope2));

        // 스코프와 변수 정보 출력
        printScopeInfo(globalScope);
    }

    public static void printScopeInfo(Scope scope) {
        System.out.println("Scope: " + scope.getScopeName());
        for (SymbolInfo symbol : scope.getSymbols()) {
            System.out.println("  Symbol: " + symbol.getName() + ", Type: " + symbol.getType());
        }
        for (Scope childScope : scope.getChildScopes()) {
            printScopeInfo(childScope); // 재귀적으로 자식 스코프 정보 출력
        }
    }
}
```

이 예제에서는 함수 스코프 내부에 중첩된 스코프를 두 단계(`nested1`, `nested2`)로 생성하고, 각 스코프에 변수를 선언합니다. `printScopeInfo` 메서드를 통해 스코프와 해당 스코프에 선언된 심볼 정보를 계층적으로 출력합니다.

이 방식을 통해, 함수 내부에서 발생하는 복잡한 중첩 스코프 구조를 정확하게 모델링하고 관리할 수 있습니다. 또한, 각 변수가 선언된 스코프를 기반으로 변수의 유효 범위를 결정하고, 동일한 이름을 가진 변수가 다른 스코프에서 선언되었을 때 이를 정

