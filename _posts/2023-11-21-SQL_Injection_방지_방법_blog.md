---
layout: post
title: "SQL Injection 방지 방법"
date: 2023-11-21
tags: [SQL, Injection, 방지, 방법]
categories: blog
---

## 사용자 입력:
> for ( Object o : parserList ){
	if ( !""XRW"".equals((String)o) && !""XML"".equals((String)o) ){
		cs = conn.prepareCall(""{call NEW_""+(String)o+""RELINFO_MULTI.PROCESS(?)}""); 
		cs.setString(1,prjId);
		cs.execute();
		try {
이 자바코드에서 	cs = conn.prepareCall(""{call NEW_""+(String)o+""RELINFO_MULTI.PROCESS(?)}"");  이 부분이 SQL INJECTION 취약점이 있다고 보고됐는데, 어떻게 조치해야 될까?

## ChatGPT 응답:
> 이 자바 코드에서 `cs = conn.prepareCall("{call NEW_"+(String)o+"RELINFO_MULTI.PROCESS(?)}");` 부분이 SQL 인젝션 취약점으로 보고된 것은, `o` 객체가 사용자 입력 또는 외부 소스에서 제어될 수 있기 때문입니다. 이 경우, 사용자가 입력한 값이 SQL 쿼리의 일부가 되어 데이터베이스 시스템에 해를 끼칠 수 있는 SQL 명령을 주입할 수 있습니다.

이 문제를 해결하기 위한 몇 가지 방법이 있습니다:

1. **입력 값 검증**: `o` 객체가 사용자 입력을 기반으로 할 때, 이 값을 검증해야 합니다. 특정 패턴이나 값 집합에 대해 검증을 수행하여 예상치 못한 입력을 차단합니다.

2. **사전 정의된 리스트 사용**: `o` 값이 될 수 있는 옵션들을 사전에 정의된 리스트로 제한합니다. 이 리스트에 없는 값은 사용할 수 없도록 하여, SQL 인젝션의 위험을 줄일 수 있습니다.

3. **명령어 및 절차 분리**: 가능하다면, 다이나믹한 SQL 구문 대신에 사전에 정의된 스토어드 프로시저나 함수를 사용합니다. 이렇게 하면, SQL 명령어를 주입하는 것이 더 어려워집니다.

4. **파라미터화된 쿼리 사용**: 가능한 경우, SQL 쿼리에서 파라미터를 사용합니다. 이 방법은 SQL 인젝션 공격을 막는 가장 효과적인 방법 중 하나입니다. 하지만, 이 경우에는 스토어드 프로시저의 이름이 동적이기 때문에 직접적으로 적용하기 어려울 수 있습니다.

5. **최소 권한 원칙 적용**: 데이터베이스 연결에 사용되는 사용자 계정은 필요한 최소한의 권한만을 가져야 합니다. 이렇게 하면, 만약 SQL 인젝션이 발생하더라도 그 피해를 최소화할 수 있습니다.

이러한 조치들을 적절히 혼합하여 사용하면, SQL 인젝션 취약점을 효과적으로 줄일 수 있습니다.

## 사용자 입력:
> 해당 블록 다시 보여줄게. 앞에 내용에는 붙여넣기 하다가 잘못된 문자가 들어가 있어. 			for ( Object o : parserList ){
				if ( !"XRW".equals((String)o) && !"XML".equals((String)o) ){
					cs = conn.prepareCall("{call NEW_"+(String)o+"RELINFO_MULTI.PROCESS(?)}"); 
					cs.setString(1,prjId);
					cs.execute();
					try {
						if ( cs != null ) cs.close();
					}catch(Exception e1){}
				}
			}

## ChatGPT 응답:
> 이 코드 블록을 다시 살펴보면, `cs = conn.prepareCall("{call NEW_"+(String)o+"RELINFO_MULTI.PROCESS(?)}");` 부분에서 여전히 SQL 인젝션의 위험이 있습니다. 여기서 `(String)o`가 사용자 입력 또는 외부에서 조작될 수 있는 값이라면, 악의적인 사용자가 SQL 쿼리를 조작할 수 있습니다.

이 문제를 해결하기 위한 방법들은 다음과 같습니다:

1. **입력 값 검증 및 제한**: `o` 객체에 대한 입력 값을 엄격하게 검증하고, 허용된 특정 값들로 제한합니다. 예를 들어, `o`가 특정 명령어 또는 알려진 스토어드 프로시저 이름 집합 중 하나에 속해야 한다고 정의할 수 있습니다.

2. **하드코딩 된 맵핑 사용**: `o` 값에 따라 다른 스토어드 프로시저를 호출하려면, `o` 값과 스토어드 프로시저 이름 간의 하드코딩 된 맵핑을 사용할 수 있습니다. 이렇게 하면 동적 문자열 연결을 피하고, 입력 값에 따라 안전하게 스토어드 프로시저를 선택할 수 있습니다.

3. **파라미터화된 스토어드 프로시저 사용**: 가능하면, 스토어드 프로시저의 이름을 파라미터로 넘기는 방법을 고려합니다. 하지만, 이는 일반적으로 SQL 쿼리에서는 지원되지 않으므로, 다른 방법을 적용해야 할 수도 있습니다.

4. **최소 권한 원칙 적용**: 데이터베이스 연결에 사용되는 계정은 필요한 최소한의 권한을 가지도록 설정합니다. 이렇게 하면, SQL 인젝션 공격이 발생하더라도 피해를 최소화할 수 있습니다.

5. **코드 리뷰 및 보안 감사**: 정기적인 코드 리뷰와 보안 감사를 통해 이러한 취약점을 발견하고 수정할 수 있습니다.

이러한 방법들을 적절히 혼합하여 사용하면, SQL 인젝션 취약점을 효과적으로 줄일 수 있습니다.

## 사용자 입력:
> 일단, 위 코드에서 적용할 수 있는 방법을 보여줘.

## ChatGPT 응답:
> 위 코드에 적용할 수 있는 방법은 다음과 같습니다:

1. **입력 값 검증 및 제한**: 
   - `o` 값이 허용된 특정 값들로만 제한되도록 합니다. 예를 들어, 특정한 스토어드 프로시저 이름의 리스트를 만들고, `o`가 이 리스트에 있는 값 중 하나인지 확인합니다.
   - 다음과 같은 코드 스니펫을 사용할 수 있습니다:

     ```java
     List<String> allowedProcedures = Arrays.asList("Procedure1", "Procedure2", "Procedure3"); // 허용된 스토어드 프로시저 이름
     for (Object o : parserList) {
         if (!"XRW".equals(o) && !"XML".equals(o) && allowedProcedures.contains(o.toString())) {
             cs = conn.prepareCall("{call NEW_" + o + "RELINFO_MULTI.PROCESS(?)}");
             cs.setString(1, prjId);
             cs.execute();
             // 나머지 코드...
         }
     }
     ```

2. **하드코딩된 맵핑 사용**:
   - `o` 값에 따라 호출할 스토어드 프로시저의 이름을 하드코딩된 맵핑을 통해 결정합니다.
   - 예를 들어, 다음과 같이 구현할 수 있습니다:

     ```java
     Map<String, String> procedureMap = new HashMap<>();
     procedureMap.put("Option1", "NEW_Option1RELINFO_MULTI.PROCESS");
     procedureMap.put("Option2", "NEW_Option2RELINFO_MULTI.PROCESS");
     // 기타 맵핑 추가...

     for (Object o : parserList) {
         if (!"XRW".equals(o) && !"XML".equals(o)) {
             String procedureName = procedureMap.get(o.toString());
             if (procedureName != null) {
                 cs = conn.prepareCall("{call " + procedureName + "(?)}");
                 cs.setString(1, prjId);
                 cs.execute();
                 // 나머지 코드...
             }
         }
     }
     ```

이러한 접근 방식은 동적 SQL 구성에 대한 위험을 크게 줄여주며, 코드의 유지보수성과 보안성을 개선합니다.

