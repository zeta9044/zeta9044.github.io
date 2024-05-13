---
layout: post
title: "C++ Antlr4 유틸 함수"
date: 2023-11-20
tags: [C, 함수, 유틸]
categories: blog
---

## 사용자 입력:
> 현재,  Antlr4로 되어 있는 C++리스너에서 활용하기 위해서 rule을 찾는 util함수를 아래와 같이 만들었어.     public static <T extends ParserRuleContext> List<T> findAll(ParserRuleContext rootContext, Class<T> targetContextClass) {
        if (rootContext == null) {
            return null;
        }

        List<T> foundContexts = new ArrayList<>();
        findAllRecursive(rootContext, targetContextClass, foundContexts);
        return foundContexts;
    }

    // 재귀적으로 targetContextClass 찾는 메서드
    private static <T extends ParserRuleContext> void findAllRecursive(ParserRuleContext context, Class<T> targetContextClass, List<T> foundContexts) {
        if (targetContextClass.isAssignableFrom(context.getClass())) {
            foundContexts.add(targetContextClass.cast(context));
        }

        for (int i = 0; i < context.getChildCount(); i++) {
            ParseTree child = context.getChild(i);
            if (child instanceof ParserRuleContext) {
                findAllRecursive((ParserRuleContext) child, targetContextClass, foundContexts);
            }
        }
    }   그런데, 이것은 현재 노드 하위의 특정 룰들을 리스트로 반환하는 거야.   그런데, 내가 추가하고 싶은 유틸함수는      현재 노드를 제외하고,  현재 노드 하위로 현재 노드와 같은 룰이름을 부모 순서대로 리스트를 반환하는 함수와   현재 노드 상위로 현재 노드와 같은 룰이름을 부모 순서대로 리스트를 반환하는 함수야. 만들어줘.

## ChatGPT 응답:
> 네, Antlr4의 C++ 리스너에서 사용할 수 있는 두 가지 추가 유틸리티 함수를 만들어 드릴게요. 하나는 현재 노드를 제외하고 현재 노드 하위에 있는 동일한 룰 이름을 가진 노드들을 부모 순서대로 리스트로 반환하는 함수이고, 다른 하나는 현재 노드 상위에 있는 동일한 룰 이름을 가진 노드들을 부모 순서대로 리스트로 반환하는 함수입니다.

1. **현재 노드 하위의 동일한 룰 이름을 가진 노드 찾기:**

   이 함수는 현재 노드의 하위 노드들 중에서 현재 노드와 같은 타입의 노드들을 찾아 리스트로 반환합니다. 이 때, 현재 노드는 제외됩니다.

   ```java
   public static <T extends ParserRuleContext> List<T> findSubContextsOfSameType(ParserRuleContext currentContext, Class<T> targetContextClass) {
       List<T> foundContexts = new ArrayList<>();
       findSubContextsOfSameTypeRecursive(currentContext, targetContextClass, foundContexts, true);
       return foundContexts;
   }

   private static <T extends ParserRuleContext> void findSubContextsOfSameTypeRecursive(ParserRuleContext context, Class<T> targetContextClass, List<T> foundContexts, boolean skipCurrent) {
       if (!skipCurrent && targetContextClass.isAssignableFrom(context.getClass())) {
           foundContexts.add(targetContextClass.cast(context));
       }

       for (int i = 0; i < context.getChildCount(); i++) {
           ParseTree child = context.getChild(i);
           if (child instanceof ParserRuleContext) {
               findSubContextsOfSameTypeRecursive((ParserRuleContext) child, targetContextClass, foundContexts, false);
           }
       }
   }
   ```

2. **현재 노드 상위의 동일한 룰 이름을 가진 노드 찾기:**

   이 함수는 현재 노드의 상위 노드들 중에서 현재 노드와 같은 타입의 노드들을 찾아 리스트로 반환합니다. 이 때, 현재 노드는 제외됩니다.

   ```java
   public static <T extends ParserRuleContext> List<T> findParentContextsOfSameType(ParserRuleContext currentContext, Class<T> targetContextClass) {
       List<T> foundContexts = new ArrayList<>();
       ParserRuleContext parent = currentContext.getParent();
       while (parent != null) {
           if (targetContextClass.isAssignableFrom(parent.getClass())) {
               foundContexts.add(targetContextClass.cast(parent));
           }
           parent = parent.getParent();
       }
       return foundContexts;
   }
   ```

이 두 함수를 사용하면 현재 노드와 동일한 룰을 가진 하위 또는 상위 노드들을 쉽게 찾을 수 있습니다. 함수의 설계가 Antlr4의 C++ 버전에도 적합하게 구현되어 있어야 한다는 점을 유의하시기 바랍니다. C++ 구현에서는 Java의 제네릭 대신 템플릿을 사용할 수 있습니다.

