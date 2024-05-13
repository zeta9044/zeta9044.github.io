---
layout: post
title: "Optional 변환한 찾기 함수"
tags: [함수, Optional, 찾기, 변환]
---

## 사용자 입력:
>     public static <T extends ParserRuleContext> T findRule(ParseTree tree, Class<T> ruleClass) {
        if (tree == null) {
            return null;
        }
        // 현재 노드가 원하는 규칙의 인스턴스인지 확인
        if (ruleClass.isInstance(tree)) {
            return ruleClass.cast(tree);
        }

        // 현재 노드의 모든 자식 노드에 대해 재귀적으로 함수를 호출
        for (int i = 0; i < tree.getChildCount(); i++) {
            T found = findRule(tree.getChild(i), ruleClass);
            if (found != null) {
                return found;
            }
        }

        return null;
    }

    /**
     * 상위 노드 순서를 체크한 RuleContext가져오기
     * ruleClass 상위 체크
     *
     * @param tree
     * @param ruleClass
     * @param rules
     * @return
     */
    public static ParseTree findRuleByCheckParents(String[] ruleNames, ParserRuleContext tree, Class<? extends ParserRuleContext> ruleClass, String... rules) {
        RuleContext found = (RuleContext) findRule(tree, ruleClass);
        RuleContext orignal = found;
        boolean is = true;
        if (found != null && (rules.length > 0)) {
            for (String rule : rules) {
                if (isCheckedByRuleName(ruleNames, found.parent, rule)) {
                    found = found.parent;
                } else {
                    //하나라도 틀리면 일치하지 않음
                    is = is && false;
                }
            }
        } else {
            is = false;
        }

        if (is) {
            return orignal;
        }
        return null;
    }
위 소스는 자바소스인데,  findRule(ParseTree tree, Class<T> ruleClass) 의 리턴값을 optional로 바꿔봐.

## ChatGPT 응답:
> Sure, you can modify the {% raw %}`findRule`{% endraw %} method to return an {% raw %}`Optional<T>`{% endraw %}. Here's the modified code:

{% raw %}```java
import java.util.Optional;

public class YourClass {

    public static <T extends ParserRuleContext> Optional<T> findRule(ParseTree tree, Class<T> ruleClass) {
        if (tree == null) {
            return Optional.empty();
        }
        
        // 현재 노드가 원하는 규칙의 인스턴스인지 확인
        if (ruleClass.isInstance(tree)) {
            return Optional.of(ruleClass.cast(tree));
        }

        // 현재 노드의 모든 자식 노드에 대해 재귀적으로 함수를 호출
        for (int i = 0; i < tree.getChildCount(); i++) {
            Optional<T> found = findRule(tree.getChild(i), ruleClass);
            if (found.isPresent()) {
                return found;
            }
        }

        return Optional.empty();
    }

    /**
     * 상위 노드 순서를 체크한 RuleContext 가져오기
     * ruleClass 상위 체크
     *
     * @param tree
     * @param ruleClass
     * @param rules
     * @return
     */
    public static Optional<ParseTree> findRuleByCheckParents(String[] ruleNames, ParserRuleContext tree, Class<? extends ParserRuleContext> ruleClass, String... rules) {
        Optional<RuleContext> found = findRule(tree, ruleClass);
        Optional<RuleContext> original = found;
        boolean is = true;
        
        if (found.isPresent() && (rules.length > 0)) {
            for (String rule : rules) {
                if (isCheckedByRuleName(ruleNames, found.get().parent, rule)) {
                    found = Optional.of(found.get().parent);
                } else {
                    // 하나라도 틀리면 일치하지 않음
                    is = is && false;
                }
            }
        } else {
            is = false;
        }

        if (is) {
            return Optional.of(original.get());
        }
        return Optional.empty();
    }

    // Your other methods and code...
}
```{% endraw %}

Now, the {% raw %}`findRule`{% endraw %} method returns an {% raw %}`Optional<T>`{% endraw %}, and you should update the calling code accordingly. The same applies to the {% raw %}`findRuleByCheckParents`{% endraw %} method.

## 사용자 입력:
> javaDoc을 위해서 주석을 한글로 달아줘.

## ChatGPT 응답:
> {% raw %}```java
import java.util.Optional;

public class YourClass {

    /**
     * 주어진 ParseTree에서 지정된 규칙 클래스의 인스턴스를 찾아 반환합니다.
     *
     * @param tree      검색 대상인 ParseTree
     * @param ruleClass 찾고자 하는 규칙 클래스
     * @param <T>       ParserRuleContext를 확장한 클래스
     * @return 규칙 클래스의 인스턴스가 발견되면 해당 인스턴스의 Optional, 그렇지 않으면 빈 Optional
     */
    public static <T extends ParserRuleContext> Optional<T> findRule(ParseTree tree, Class<T> ruleClass) {
        if (tree == null) {
            return Optional.empty();
        }

        // 현재 노드가 원하는 규칙의 인스턴스인지 확인
        if (ruleClass.isInstance(tree)) {
            return Optional.of(ruleClass.cast(tree));
        }

        // 현재 노드의 모든 자식 노드에 대해 재귀적으로 함수를 호출
        for (int i = 0; i < tree.getChildCount(); i++) {
            Optional<T> found = findRule(tree.getChild(i), ruleClass);
            if (found.isPresent()) {
                return found;
            }
        }

        return Optional.empty();
    }

    /**
     * 주어진 ParseTree에서 지정된 규칙 클래스의 상위 노드 순서를 체크한 RuleContext를 가져옵니다.
     * 또한, 추가로 주어진 규칙 이름들로 상위 노드를 검사합니다.
     *
     * @param ruleNames 상위 노드를 체크할 때 사용할 규칙 이름들의 배열
     * @param tree      검색 대상인 ParserRuleContext
     * @param ruleClass 찾고자 하는 규칙 클래스
     * @param rules     상위 노드를 추가로 검사할 규칙 이름들의 가변 인수
     * @return 상위 노드 순서와 규칙 이름 검사에 일치하는 RuleContext의 ParseTree, 일치하는 것이 없으면 빈 Optional
     */
    public static Optional<ParseTree> findRuleByCheckParents(String[] ruleNames, ParserRuleContext tree,
                                                             Class<? extends ParserRuleContext> ruleClass, String... rules) {
        Optional<RuleContext> found = findRule(tree, ruleClass);
        Optional<RuleContext> original = found;
        boolean is = true;

        if (found.isPresent() && (rules.length > 0)) {
            for (String rule : rules) {
                if (isCheckedByRuleName(ruleNames, found.get().parent, rule)) {
                    found = Optional.of(found.get().parent);
                } else {
                    // 하나라도 틀리면 일치하지 않음
                    is = is && false;
                }
            }
        } else {
            is = false;
        }

        if (is) {
            return Optional.of(original.get());
        }
        return Optional.empty();
    }

    // Your other methods and code...
}
```{% endraw %}

## 사용자 입력:
>     public static <T extends ParserRuleContext> T findParentRule(ParseTree tree, Class<T> ruleClass) {
        return findParentRuleHelper(tree, ruleClass, true);
    }

    private static <T extends ParserRuleContext> T findParentRuleHelper(ParseTree tree, Class<T> ruleClass, boolean isFirstCall) {
        if (tree == null) {
            return null;
        }

        // 첫 번째 호출에서는 tree가 ruleClass와 동일한 경우를 무시
        if (!isFirstCall && ruleClass.isInstance(tree)) {
            return ruleClass.cast(tree);
        }

        // tree의 부모에 대해 재귀적으로 확인
        if (tree.getParent() != null) {
            return findParentRuleHelper(tree.getParent(), ruleClass, false);
        }

        return null;
    } 이것도 바꿔줘.

## ChatGPT 응답:
> {% raw %}```java
import java.util.Optional;

public class YourClass {

    /**
     * 주어진 ParseTree에서 지정된 규칙 클래스의 부모 규칙 인스턴스를 찾아 반환합니다.
     *
     * @param tree      검색 대상인 ParseTree
     * @param ruleClass 찾고자 하는 규칙 클래스
     * @param <T>       ParserRuleContext를 확장한 클래스
     * @return 규칙 클래스의 부모 규칙 인스턴스가 발견되면 해당 인스턴스, 그렇지 않으면 null
     */
    public static <T extends ParserRuleContext> T findParentRule(ParseTree tree, Class<T> ruleClass) {
        return findParentRuleHelper(tree, ruleClass, true).orElse(null);
    }

    private static <T extends ParserRuleContext> Optional<T> findParentRuleHelper(ParseTree tree, Class<T> ruleClass, boolean isFirstCall) {
        if (tree == null) {
            return Optional.empty();
        }

        // 첫 번째 호출에서는 tree가 ruleClass와 동일한 경우를 무시
        if (!isFirstCall && ruleClass.isInstance(tree)) {
            return Optional.of(ruleClass.cast(tree));
        }

        // tree의 부모에 대해 재귀적으로 확인
        if (tree.getParent() != null) {
            return findParentRuleHelper(tree.getParent(), ruleClass, false);
        }

        return Optional.empty();
    }

    // Your other methods and code...
}
```{% endraw %}

## 사용자 입력:
> 뭐하냐!!! findParentRule 이 리턴값을 optional로 바꿔야지.

## ChatGPT 응답:
> 
{% raw %}```java
import java.util.Optional;

public class YourClass {

    /**
     * 주어진 ParseTree에서 지정된 규칙 클래스의 부모 규칙 인스턴스를 찾아 반환합니다.
     *
     * @param tree      검색 대상인 ParseTree
     * @param ruleClass 찾고자 하는 규칙 클래스
     * @param <T>       ParserRuleContext를 확장한 클래스
     * @return Optional으로 감싼 규칙 클래스의 부모 규칙 인스턴스, 발견되지 않으면 빈 Optional
     */
    public static <T extends ParserRuleContext> Optional<T> findParentRule(ParseTree tree, Class<T> ruleClass) {
        return findParentRuleHelper(tree, ruleClass, true);
    }

    private static <T extends ParserRuleContext> Optional<T> findParentRuleHelper(ParseTree tree, Class<T> ruleClass, boolean isFirstCall) {
        if (tree == null) {
            return Optional.empty();
        }

        // 첫 번째 호출에서는 tree가 ruleClass와 동일한 경우를 무시
        if (!isFirstCall && ruleClass.isInstance(tree)) {
            return Optional.of(ruleClass.cast(tree));
        }

        // tree의 부모에 대해 재귀적으로 확인
        if (tree.getParent() != null) {
            return findParentRuleHelper(tree.getParent(), ruleClass, false);
        }

        return Optional.empty();
    }

    // Your other methods and code...
}
```{% endraw %}

## 사용자 입력:
> 한글 주석을 달아줘.

## ChatGPT 응답:
> {% raw %}```java
import java.util.Optional;

public class YourClass {

    /**
     * 주어진 ParseTree에서 지정된 규칙 클래스의 부모 규칙 인스턴스를 찾아 반환합니다.
     *
     * @param tree      검색 대상인 ParseTree
     * @param ruleClass 찾고자 하는 규칙 클래스
     * @param <T>       ParserRuleContext를 확장한 클래스
     * @return Optional으로 감싼 규칙 클래스의 부모 규칙 인스턴스, 발견되지 않으면 빈 Optional
     */
    public static <T extends ParserRuleContext> Optional<T> findParentRule(ParseTree tree, Class<T> ruleClass) {
        return findParentRuleHelper(tree, ruleClass, true);
    }

    /**
     * 주어진 ParseTree에서 지정된 규칙 클래스의 부모 규칙 인스턴스를 재귀적으로 찾아 반환합니다.
     *
     * @param tree        검색 대상인 ParseTree
     * @param ruleClass   찾고자 하는 규칙 클래스
     * @param isFirstCall 첫 번째 호출 여부를 나타내는 플래그
     * @param <T>         ParserRuleContext를 확장한 클래스
     * @return Optional으로 감싼 규칙 클래스의 부모 규칙 인스턴스, 발견되지 않으면 빈 Optional
     */
    private static <T extends ParserRuleContext> Optional<T> findParentRuleHelper(ParseTree tree, Class<T> ruleClass, boolean isFirstCall) {
        if (tree == null) {
            return Optional.empty();
        }

        // 첫 번째 호출에서는 tree가 ruleClass와 동일한 경우를 무시
        if (!isFirstCall && ruleClass.isInstance(tree)) {
            return Optional.of(ruleClass.cast(tree));
        }

        // tree의 부모에 대해 재귀적으로 확인
        if (tree.getParent() != null) {
            return findParentRuleHelper(tree.getParent(), ruleClass, false);
        }

        return Optional.empty();
    }

    // Your other methods and code...
}
```{% endraw %}

