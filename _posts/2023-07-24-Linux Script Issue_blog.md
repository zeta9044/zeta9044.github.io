---
layout: post
title: "Linux Script Issue"
date: 2023-07-24
tags: [Issue, Script, Linux]
categories: blog
---

## 사용자 입력:
> [ -d "pentaho-parser" ] && cd pentaho-parser && ../gradlew compileJjtree compileJavacc clean build -x test   이 리눅스 명령을 실행시키면, pentaho-parser가 없으면 false로 결과가 나오고 shell이 false를 반환한다. 그래서, 다음 명령어가 실행이 안된다. 

## ChatGPT 응답:
> 해당 리눅스 명령어는 `pentaho-parser` 디렉토리가 존재하는지 확인한 후, 디렉토리가 존재하는 경우 해당 디렉토리로 이동하여 `../gradlew compileJjtree compileJavacc clean build -x test` 명령어를 실행하는 스크립트입니다.

만약 `pentaho-parser` 디렉토리가 존재하지 않는 경우 스크립트는 조건문 `-d "pentaho-parser"`의 결과로 인해 false를 반환합니다. 이 경우, 스크립트가 그 이후의 명령어를 실행하지 않는 문제가 발생할 수 있습니다.

해당 스크립트를 수정하여 문제를 해결하려면, 존재하지 않는 경우에도 스크립트가 계속 실행되도록 조치해야 합니다. 예를 들어, 다음과 같이 수정할 수 있습니다:

```bash
if [ -d "pentaho-parser" ]; then
    cd pentaho-parser && ../gradlew compileJjtree compileJavacc clean build -x test
fi
```

이렇게 하면 `pentaho-parser` 디렉토리가 존재하지 않더라도 스크립트가 계속해서 실행됩니다. 조건문에 따라 해당 디렉토리가 존재하는 경우에만 해당 명령어들이 실행되게 됩니다.

## 사용자 입력:
> 위 명령어를 cmd상에서 실행이 되나?

## ChatGPT 응답:
> 제가 제공한 명령어는 리눅스/유닉스 운영체제의 쉘에서 사용할 수 있는 명령어입니다. 따라서, 위 명령어는 Windows 운영체제에서는 직접적으로 실행되지 않을 수 있습니다.

Windows 운영체제에서는 명령어 구문이 다르기 때문에 위의 스크립트를 그대로 실행하면 오류가 발생할 수 있습니다. Windows 환경에서는 다른 명령어 구문을 사용해야 합니다.

Windows의 경우 위 스크립트를 다음과 같이 변경해야 합니다:

```cmd
IF EXIST "pentaho-parser" (
    cd pentaho-parser
    ..\gradlew compileJjtree compileJavacc clean build -x test
)
```

위 스크립트는 `IF EXIST`를 사용하여 `pentaho-parser` 디렉토리가 존재하는지 확인하고, 존재하는 경우 해당 디렉토리로 이동한 후 `..\gradlew compileJjtree compileJavacc clean build -x test` 명령어를 실행합니다.

Windows의 cmd에서는 리눅스/유닉스의 쉘과 다른 문법과 명령어를 사용하기 때문에, 스크립트를 해당 운영체제에 맞게 적절히 수정해야 정상적으로 실행됩니다.

## 사용자 입력:
> 리눅스에서 실행할 거야.

## ChatGPT 응답:
> 알겠습니다. 리눅스에서 실행할 경우에는 제가 이전에 제시한 스크립트를 그대로 사용하시면 됩니다. 리눅스 운영체제에서는 해당 스크립트가 정상적으로 실행될 것입니다.

다시 한 번, 리눅스에서 실행할 스크립트는 아래와 같습니다:

```bash
if [ -d "pentaho-parser" ]; then
    cd pentaho-parser && ../gradlew compileJjtree compileJavacc clean build -x test
fi
```

위 스크립트를 리눅스 쉘에서 실행하면 `pentaho-parser` 디렉토리가 존재하는지 확인한 후, 디렉토리가 존재하는 경우 해당 디렉토리로 이동하여 `../gradlew compileJjtree compileJavacc clean build -x test` 명령어가 실행됩니다. `pentaho-parser` 디렉토리가 존재하지 않는 경우에는 아무런 동작도 하지 않게 됩니다.

## 사용자 입력:
> 위 내용을 담은 쉘스크립트 파일이 없어도 그냥 cmd상에서 실행된다는 거지?

## ChatGPT 응답:
> 아니요, 리눅스 운영체제에서 작성된 쉘 스크립트는 Windows cmd 상에서 직접 실행되지 않습니다. 리눅스 쉘 스크립트는 리눅스 환경에서 사용하기 위해 설계되어 특정 리눅스 쉘 (예: bash)에서 동작하도록 작성됩니다.

따라서, 위에 제공한 쉘 스크립트는 리눅스/유닉스 운영체제에서 실행해야 합니다. Windows에서는 cmd 명령어를 사용해야 하며, 쉘 스크립트 파일을 직접 실행할 수 없습니다.

만약 리눅스 환경이 아니라 Windows 환경에서 해당 스크립트를 실행하고자 한다면, 이전에 제시한 Windows cmd용 스크립트를 사용하셔야 합니다:

```cmd
IF EXIST "pentaho-parser" (
    cd pentaho-parser
    ..\gradlew compileJjtree compileJavacc clean build -x test
)
```

따라서, 스크립트를 실행하고자 하는 운영체제에 따라 적절한 스크립트를 사용해야 합니다. 리눅스 환경에서는 쉘 스크립트를 사용하고, Windows 환경에서는 cmd 스크립트를 사용해야 합니다.

## 사용자 입력:
> 내가 말하는 cmd는 리눅스야.

## ChatGPT 응답:
> 죄송합니다, 오해가 있었군요. Windows 운영체제에서 사용하는 명령 프롬프트(cmd)와 리눅스 운영체제에서 사용하는 쉘(shell)은 이름이 비슷하지만, 서로 다른 환경에서 동작합니다. 리눅스에서 사용하는 기본 쉘은 보통 bash(Bourne Again SHell)입니다.

따라서, 리눅스 환경에서 해당 스크립트를 실행하고자 한다면, 제가 처음에 제시한 리눅스 쉘 스크립트를 사용하시면 됩니다:

```bash
if [ -d "pentaho-parser" ]; then
    cd pentaho-parser && ../gradlew compileJjtree compileJavacc clean build -x test
fi
```

위의 스크립트는 리눅스 쉘에서 정상적으로 동작합니다. 리눅스 환경에서 이 스크립트를 사용하면, `pentaho-parser` 디렉토리가 존재하는지 확인한 후, 디렉토리가 존재하는 경우 해당 디렉토리로 이동하여 `../gradlew compileJjtree compileJavacc clean build -x test` 명령어가 실행됩니다. `pentaho-parser` 디렉토리가 존재하지 않는 경우에는 아무런 동작도 하지 않게 됩니다.

## 사용자 입력:
> 		        sbSQL.append("\n  INSERT INTO AIS_TEMP                                                                                                                                   ");
		        sbSQL.append("\n        (PRJ_ID, FILE_ID, OBJ_ID, FUNC_ID, OBJ_TYPE, OBJ_ACCS_TYPE, OBJ_NAME, OBJ_FULL_NAME, PKG_NAME, FUNC_ACCS_TYPE, FUNC_NAME, ARGS_CNT, LANG_CD)     ");
		        sbSQL.append("\n  SELECT /*+RULE INDEX ( A AIS0012_PK ) */                                                                                                               ");
		        sbSQL.append("\n         A.PRJ_ID, A.FILE_ID, NVL(A.OBJ_ID, 0) OBJ_ID, A.FUNC_ID, B.OBJ_TYPE, B.OBJ_ACCS_TYPE, B.OBJ_NAME, B.OBJ_FULL_NAME, B.PACKAGE,                   ");
		        sbSQL.append("\n         A.FUNC_ACCS_TYPE, A.FUNC_NAME, A.ARGS_CNT, 'XML'                                                                                                ");
		        sbSQL.append("\n    FROM AIS0011 B,                                                                                                                                      ");
		        sbSQL.append("\n         AIS0012 A                                                                                                                                       ");
		        sbSQL.append("\n   WHERE B.PRJ_ID  = RTRIM(A.PRJ_ID)                                                                                                                     ");
		        sbSQL.append("\n     AND B.FILE_ID = RTRIM(A.FILE_ID)                                                                                                                    ");
		        sbSQL.append("\n     AND B.OBJ_ID  = RTRIM(A.OBJ_ID)                                                                                                                     ");
		        sbSQL.append("\n     AND ( A.PRJ_ID, A.FILE_ID ) IN ( SELECT C.PRJ_ID, C.FILE_ID                                                                                         ");
		        sbSQL.append("\n                                        FROM VW_AIS0010 C                                                                                                ");
		        sbSQL.append("\n                                       WHERE C.PRJ_ID       = ?                                                                                          ");	// p_prj_id
		        sbSQL.append("\n                                         AND C.FILE_LST_YN  = 'Y'                                                                                        ");
		        sbSQL.append("\n                                         AND C.FILE_ANLY_YN = 'Y'                                                                                        ");
		        sbSQL.append("\n                                         AND C.EXT_CD       IN ('XML', 'XSQL')                                                                           ");
		        sbSQL.append("\n                                    )                                                                                                                    ");  여기서 sql문 뽑아줘.

## ChatGPT 응답:
> 주어진 코드는 Java 코드인 것으로 보이며, 문자열 변수 `sbSQL`에 SQL 쿼리를 구성하는 코드가 포함되어 있습니다. 주어진 코드에서 추출된 SQL 쿼리는 다음과 같습니다:

```sql
INSERT INTO AIS_TEMP
  (PRJ_ID, FILE_ID, OBJ_ID, FUNC_ID, OBJ_TYPE, OBJ_ACCS_TYPE, OBJ_NAME, OBJ_FULL_NAME, PKG_NAME, FUNC_ACCS_TYPE, FUNC_NAME, ARGS_CNT, LANG_CD)
SELECT /*+RULE INDEX ( A AIS0012_PK ) */
       A.PRJ_ID, A.FILE_ID, NVL(A.OBJ_ID, 0) OBJ_ID, A.FUNC_ID, B.OBJ_TYPE, B.OBJ_ACCS_TYPE, B.OBJ_NAME, B.OBJ_FULL_NAME, B.PACKAGE,
       A.FUNC_ACCS_TYPE, A.FUNC_NAME, A.ARGS_CNT, 'XML'
  FROM AIS0011 B, AIS0012 A
 WHERE B.PRJ_ID  = RTRIM(A.PRJ_ID)
   AND B.FILE_ID = RTRIM(A.FILE_ID)
   AND B.OBJ_ID  = RTRIM(A.OBJ_ID)
   AND ( A.PRJ_ID, A.FILE_ID ) IN ( SELECT C.PRJ_ID, C.FILE_ID
                                      FROM VW_AIS0010 C
                                     WHERE C.PRJ_ID       = ?
                                       AND C.FILE_LST_YN  = 'Y'
                                       AND C.FILE_ANLY_YN = 'Y'
                                       AND C.EXT_CD       IN ('XML', 'XSQL')
                                  )
```

이 SQL 쿼리는 `AIS0012` 테이블과 `AIS0011` 테이블을 조인하여 `AIS_TEMP` 테이블에 데이터를 삽입합니다. `AIS_TEMP` 테이블의 각 열에 값을 `AIS0012` 테이블과 `AIS0011` 테이블에서 추출한 데이터로 채웁니다. `AIS0010` 뷰를 사용하여 `AIS0012` 테이블의 데이터를 필터링하는데, 해당 뷰에서 `PRJ_ID`와 `FILE_ID`가 특정 값인 행들만 가져오도록 설정되어 있습니다. 이 특정 값을 설정하는 부분(`?`)은 실행 시에 변수로 대체되어야 할 부분입니다.

만약 SQL 쿼리를 자세히 이해하지 못하거나 오류가 발생한다면, 해당 코드가 실행되는 환경에 따라 데이터베이스 연결 및 실행 코드가 있어야 할 수도 있습니다. 코드 전체를 살펴보고 실행되는 환경에 대한 정보를 알아야 정확한 분석과 해석이 가능합니다.

