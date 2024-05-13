---
layout: post
title: "한글 Javadoc 주석"
tags: [한글, 주석, Javadoc]
---

## 사용자 입력:
>     private int getVarId(String callVarLeftVarName, Map<String, Object> keys, List<AISymbol> AIS0014) {
        int defaultVarId = -1;
        // 리스트 역순으로 재편성해서, 가장 나중에 추가된 변수를 찾는다.
        // 이유는 가장 나중에 추가된 변수가 scope에 맞기 때문
        // 리스트를 복제하여 새로운 리스트 생성(AIS0014가 변형되는 것을 방지,AIS0014는 불변객체가 되어야 함)
        List<AISymbol> copy = new ArrayList<>(List.copyOf(AIS0014));
        Collections.reverse(copy);

        for (AISymbol s : copy) {
            if (s.getPrjId().equals(keys.get(Keys.PRJ_ID))
                    && s.getFileId() == (Integer) keys.get(Keys.FILE_ID)
                    && s.getObjId() == (Integer) keys.get(Keys.OBJ_ID)
                    && s.getFuncId() == (Integer) keys.get(Keys.FUNC_ID)
                    && callVarLeftVarName.equals(s.getVarName())
            ) {
                return s.getVarId();
            }
        }

        return defaultVarId;
    }  이 함수에 javadoc용 주석을 한글로 만들어줘.

## ChatGPT 응답:
> {% raw %}```java
    /**
     * 주어진 변수명과 키 정보를 기반으로 변수 식별자를 검색하여 반환합니다.
     *
     * @param callVarLeftVarName 호출된 변수의 왼쪽 변수명
     * @param keys 변수를 식별하는 데 사용되는 키 정보 맵
     * @param AIS0014 검색 대상이 되는 AISymbol 목록
     * @return 변수 식별자. 해당하는 변수가 없을 경우 기본값인 -1을 반환합니다.
     */
    private int getVarId(String callVarLeftVarName, Map<String, Object> keys, List<AISymbol> AIS0014) {
        int defaultVarId = -1;

        // AIS0014가 변형되지 않도록 리스트를 복제하고 역순으로 재편성합니다.
        List<AISymbol> copy = new ArrayList<>(List.copyOf(AIS0014));
        Collections.reverse(copy);

        for (AISymbol s : copy) {
            // 주어진 키 정보와 일치하는 변수를 검색합니다.
            if (s.getPrjId().equals(keys.get(Keys.PRJ_ID))
                    && s.getFileId() == (Integer) keys.get(Keys.FILE_ID)
                    && s.getObjId() == (Integer) keys.get(Keys.OBJ_ID)
                    && s.getFuncId() == (Integer) keys.get(Keys.FUNC_ID)
                    && callVarLeftVarName.equals(s.getVarName())
            ) {
                return s.getVarId(); // 일치하는 변수가 발견되면 해당 변수의 식별자를 반환합니다.
            }
        }

        return defaultVarId; // 일치하는 변수가 없을 경우 기본값을 반환합니다.
    }
```{% endraw %}

