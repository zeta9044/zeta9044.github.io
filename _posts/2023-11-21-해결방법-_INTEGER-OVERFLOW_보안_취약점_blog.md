---
layout: post
title: "해결방법: INTEGER_OVERFLOW 보안 취약점"
date: 2023-11-21
tags: [방법, 취약점, 해결, 보안]
categories: blog
---

## 사용자 입력:
> 	pstmt.setString( 3, rs.getString("OBJ_ID"  ) );

	pstmt.setString( 4, rs.getString("FUNC_ID" ) );

	pstmt.setString( 5, rs.getString("VAR_ID"  ) );

	execResult += pstmt.executeUpdate(); 

	pstmt.clearParameters();

	lastFileIdBAK = rs.getInt("FILE_ID" );	

} 이것은 자바소스 일부 코드인데, 보안 취약점으로 INTEGER_OVERFLOW 가 발견됐다. 해결방법은?

## ChatGPT 응답:
> 이 자바 코드에서 발견된 `INTEGER_OVERFLOW` 보안 취약점은 `lastFileIdBAK = rs.getInt("FILE_ID");` 부분에서 발생할 가능성이 높습니다. `getInt` 메서드는 결과 집합에서 지정된 열의 값을 `int` 형식으로 검색합니다. 만약 `"FILE_ID"` 열의 값이 `int` 범위를 초과하는 경우, 정수 오버플로우가 발생할 수 있습니다.

해결 방법은 다음과 같습니다:

1. **데이터 타입 변경**: 만약 `"FILE_ID"` 열의 값이 `int` 범위를 초과할 수 있다면, 데이터 타입을 `long`으로 변경하는 것이 좋습니다. `rs.getLong("FILE_ID")`을 사용하여 값을 검색하면 됩니다.

2. **범위 검사 추가**: `getInt`를 호출하기 전에 해당 값이 `int` 범위 내에 있는지 확인하는 로직을 추가할 수 있습니다. 예를 들어, `ResultSetMetaData`를 사용하여 최대값을 확인하고, 이를 기반으로 조건부 로직을 구현할 수 있습니다.

3. **예외 처리 강화**: `getInt` 호출 시 발생할 수 있는 `SQLException`을 적절히 처리하여, 오버플로우가 발생했을 때 프로그램이 안정적으로 동작하도록 합니다.

4. **데이터베이스 스키마 검토**: 데이터베이스 스키마를 검토하여 `"FILE_ID"` 열의 값이 예상 범위 내에 있는지 확인하고, 필요하다면 스키마를 조정합니다.

이러한 접근 방식은 보안 취약점을 해결하고, 유사한 문제가 다시 발생하지 않도록 하는 데 도움이 될 것입니다.

## 사용자 입력:
> 	execResult += pstmt.executeUpdate();  이 부분으로 검출됐어.

## ChatGPT 응답:
> `execResult += pstmt.executeUpdate();` 부분에서 `INTEGER_OVERFLOW` 취약점이 검출된 경우, 문제의 원인은 `execResult` 변수에 대한 누적 연산에서 발생할 수 있습니다. `executeUpdate()` 메서드는 실행된 SQL 문에 의해 영향을 받은 행의 수를 `int` 형태로 반환합니다. 만약 많은 수의 데이터 조작 연산이 이루어져 `execResult`의 값이 `int`의 최대 범위를 초과하게 되면 오버플로우가 발생할 수 있습니다.

이를 해결하기 위한 방법은 다음과 같습니다:

1. **데이터 타입 변경**: `execResult`의 데이터 타입을 `long`으로 변경하여 더 큰 범위의 정수를 처리할 수 있도록 합니다. 이렇게 하면 `int`의 범위를 초과하는 누적값을 안전하게 처리할 수 있습니다.

   ```java
   long execResult = 0;
   // ...
   execResult += pstmt.executeUpdate();
   ```

2. **범위 검사 추가**: 각 `executeUpdate()` 호출 후에 `execResult`의 값을 확인하여 `int`의 최대값에 근접하거나 초과하는지 검사합니다. 이를 통해 오버플로우 발생 전에 적절한 조치를 취할 수 있습니다.

   ```java
   int updateCount = pstmt.executeUpdate();
   if (Integer.MAX_VALUE - execResult < updateCount) {
       // 오버플로우 방지를 위한 조치 취하기
   }
   execResult += updateCount;
   ```

3. **예외 처리 강화**: `executeUpdate()`의 호출과 결과 처리 과정에 대한 예외 처리를 강화하여, 오버플로우나 다른 예외 상황에 대응할 수 있도록 합니다.

이러한 조치를 통해 `INTEGER_OVERFLOW` 취약점을 해결하고, 향후 비슷한 문제가 발생하는 것을 방지할 수 있습니다.

## 사용자 입력:
> executeUpdate 함수 반환타입이 int인데, 왜 오류가 검출돼지?

## ChatGPT 응답:
> `executeUpdate` 함수의 반환 타입이 `int`인 경우, `INTEGER_OVERFLOW` 오류가 검출되는 이유는 일반적으로 `executeUpdate` 함수가 반환하는 값 자체의 오버플로우보다는 해당 값을 사용하는 방식에서 발생할 가능성이 더 높습니다. 이 경우, 문제는 `execResult += pstmt.executeUpdate();` 이 부분에서 발생합니다.

문제가 될 수 있는 시나리오를 살펴보면:

1. **누적값의 오버플로우**: `execResult` 변수가 `int` 타입일 경우, 이 변수에 `executeUpdate`의 결과를 반복적으로 더하면 `execResult`의 값이 `int`의 최대 값을 초과할 수 있습니다. 이는 특히 루프 내에서 많은 수의 데이터 조작이 이루어질 때 발생할 수 있습니다.

2. **executeUpdate의 반환값**: `executeUpdate`는 영향을 받은 행의 수를 반환합니다. 이 값이 예상보다 매우 크거나, 예외적인 상황에서 예상치 못한 큰 값을 반환할 수 있습니다.

이 문제를 해결하기 위한 방법으로는:

- **변수 타입 변경**: `execResult`의 데이터 타입을 `long`으로 변경하여 오버플로우의 위험을 감소시킬 수 있습니다.

- **범위 검사**: `executeUpdate` 호출 후에 `execResult`의 범위를 검사하여 `int`의 최대값을 초과하지 않는지 확인합니다.

- **로직 재검토**: 이 코드가 실행되는 전체적인 컨텍스트를 재검토하여, `execResult`가 오버플로우될 가능성이 있는지, 또는 다른 방식으로 해당 값을 관리할 수 있는지 검토합니다.

이러한 접근 방법을 통해 `INTEGER_OVERFLOW` 문제를 해결할 수 있습니다.

