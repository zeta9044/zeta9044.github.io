---
layout: post
title: "MyBatis 동적 쿼리"
date: 2023-05-03
tags: [적, MyBatis, 쿼리]
categories: blog
---

## 사용자 입력:
> mybatis에서 동적파라미터는 $를 쓰는 걸로 알고 있는데..

## ChatGPT 응답:
> MyBatis에서 동적 쿼리를 작성할 때는 "$"와 "#" 두 가지 종류의 파라미터 표현식을 사용할 수 있습니다.

"$" 표현식을 사용하는 경우, 파라미터 값을 문자열 치환하여 SQL 문을 생성합니다. 예를 들어, 다음과 같은 쿼리를 작성한다고 가정해 봅시다.

```xml
<select id="selectUsers" parameterType="map" resultType="User">
  SELECT * FROM users
  WHERE
  <if test="name != null">
    name = '${name}'
  </if>
  <if test="email != null">
    AND email = '${email}'
  </if>
</select>
```

위의 코드에서 "name" 파라미터의 값이 "John"인 경우, MyBatis는 다음과 같은 SQL을 생성합니다.

```sql
SELECT * FROM users WHERE name = 'John'
```

반면에, "#" 표현식을 사용하는 경우, 파라미터 값을 SQL 파라미터로 바인딩합니다. 이렇게 함으로써 SQL 쿼리를 더욱 안전하게 만들 수 있습니다. 예를 들어, 다음과 같은 코드를 작성할 수 있습니다.

```xml
<select id="selectUsers" parameterType="map" resultType="User">
  SELECT * FROM users
  WHERE
  <if test="name != null">
    name = #{name}
  </if>
  <if test="email != null">
    AND email = #{email}
  </if>
</select>
```

위의 코드에서 "name" 파라미터의 값이 "John"인 경우, MyBatis는 다음과 같은 SQL을 생성합니다.

```sql
SELECT * FROM users WHERE name = ?
```

이렇게 작성된 SQL에서 "?"는 파라미터 값을 바인딩할 위치를 나타냅니다. 파라미터 값을 실제 SQL 문으로 대체하기 위해서는 JDBC API를 사용하여 파라미터 값을 설정해야 합니다.

## 사용자 입력:
> <select id="selectUsers" parameterType="map" resultType="User">
  SELECT * FROM users
  WHERE
  <if test="name != null">
    name = ${name}
  </if>
  <if test="email != null">
    AND email = #{email}
  </if>
</select>

## ChatGPT 응답:
> 위의 코드에서는 파라미터로 전달된 "name" 값이 "$" 기호로 치환되어 SQL 문이 생성됩니다. 이 경우에는 입력된 파라미터 값을 직접 SQL 문에 포함시키므로, SQL Injection 공격에 취약합니다. 따라서 보안적인 이유로 "$" 대신 "#" 기호를 사용하여 파라미터 값을 SQL 파라미터로 바인딩하는 것이 좋습니다.

따라서, 아래와 같이 수정하는 것이 좋습니다.

```xml
<select id="selectUsers" parameterType="map" resultType="User">
  SELECT * FROM users
  WHERE
  <if test="name != null">
    name = #{name}
  </if>
  <if test="email != null">
    AND email = #{email}
  </if>
</select>
```

위의 코드에서는 파라미터로 전달된 "name" 값이 "#" 기호로 치환되어 SQL 문이 생성됩니다. 이 경우에는 입력된 파라미터 값을 SQL 파라미터로 바인딩하기 때문에, SQL Injection 공격에 대한 보안성이 향상됩니다.

## 사용자 입력:
> <select id="selectUsers" parameterType="map" resultType="User">
  SELECT * FROM users
  WHERE
  <if test="name != null">
    name = ${name}
  </if>
  <if test="email != null">
    AND email = #{email}
  </if>
</select>   이런 xml mapper가 있는데. selectUser(name, hashmap) 이렇게 보내면 name이 바인딩 되지 않나?


## ChatGPT 응답:
> `<if>` 요소에서 `name` 파라미터를 `$` 기호를 사용하여 문자열 치환 방식으로 바인딩하고 있기 때문에, `name` 파라미터는 null이 아닌 경우에만 해당 SQL 조건문에 포함됩니다. 따라서 `name` 파라미터가 null인 경우에는 해당 SQL 쿼리에서 `name` 조건이 생략되므로 결과 집합에는 해당 조건을 만족하는 데이터가 포함되지 않습니다.

`email` 파라미터는 `#` 기호를 사용하여 SQL 파라미터로 바인딩하고 있기 때문에, MyBatis는 해당 파라미터를 SQL 쿼리에서 바인딩하기 위해 JDBC의 `PreparedStatement`를 사용합니다. `email` 파라미터는 null이 아닌 경우에만 SQL 쿼리에서 해당 조건이 적용됩니다.

따라서 `selectUsers` 쿼리를 호출할 때 `name` 파라미터와 `email` 파라미터를 함께 전달하더라도, `name` 파라미터가 null이면 해당 조건이 생략되고 `email` 파라미터가 null이 아니면 해당 조건이 포함되는 SQL 쿼리가 생성됩니다. 이렇게 생성된 SQL 쿼리는 `PreparedStatement`의 파라미터로 `email` 파라미터의 값을 바인딩하여 실행됩니다.

즉, `selectUsers` 쿼리를 호출할 때는 다음과 같이 `name` 파라미터와 `email` 파라미터를 함께 전달하면 됩니다.

```java
Map<String, Object> paramMap = new HashMap<>();
paramMap.put("name", "John"); // null이 아닌 값
paramMap.put("email", "john@example.com"); // null이 아닌 값
List<User> userList = sqlSession.selectList("selectUsers", paramMap);
```

위와 같이 `paramMap` 객체에 `name` 파라미터와 `email` 파라미터의 값을 저장한 후, `sqlSession.selectList()` 메서드를 호출할 때 해당 객체를 전달하면 MyBatis는 이를 바탕으로 SQL 쿼리를 생성하고 실행합니다.

## 사용자 입력:
> void insertTempTable(String tempTableName, HashMap<String, String> args); 이렇게 선언 되어 있고, 이에 따른 매퍼는 <insert id="insertTempTable" parameterType="java.util.HashMap">/* ImpactStreamRelation.xml.insertTempTable */ /* ImpactStreamRelation.xml.insertTempTable */ 
        INSERT INTO ${tempTableName}
        VALUES
            ( #{SEQ_ID}
            , CAST(#{CAL_ID} AS NUMERIC )
            , #{BIZ_ID}
            , #{PRJ_ID}
            , CAST(#{OBJ_ID} AS NUMERIC )
            , CAST(#{FILE_ID} AS NUMERIC )
            , CAST(#{FUNC_ID} AS NUMERIC )
            , #{PRJ_PATH}
            , #{FILE_NAME}
            , #{FUNC_NAME}
            , #{FILE_PATH}
            , #{FILE_COMMENT}
            , #{OBJ_NAME#}
            , #{OBJ_TYPE}
            , #{DBC_CHECK}
            , CAST(#{REL_CAL_ID} AS NUMERIC )
            , #{REL_PRJ_ID}
            , CAST(#{REL_OBJ_ID} AS NUMERIC )
            , CAST(#{REL_FILE_ID} AS NUMERIC )
            , CAST(#{REL_FUNC_ID} AS NUMERIC )
            , #{REL_FILE_NAME}
            , #{REL_FUNC_NAME}
            , CAST(#{REL_DEPTH} AS NUMERIC ))
    </insert>
이렇게 정의 되어 있는데, 현재, ${tempTableName}이 바인딩되지 않는다는 ibatis오류가 나온다. 데이터베이스는 오라클이다. postgresql에서는 오류가 생성되지 않았는데, 원인이 무엇일까?

## ChatGPT 응답:
> `${tempTableName}` 표현식은 MyBatis에서 동적으로 테이블 이름을 바인딩하기 위해 사용되고 있습니다. 그러나 오라클 데이터베이스에서는 테이블 이름에 대한 바인딩을 `$` 기호를 사용하여 문자열 치환 방식으로 처리할 수 없기 때문에 해당 오류가 발생하게 됩니다.

오라클 데이터베이스에서는 동적으로 테이블 이름을 바인딩하기 위해서는 SQL 문을 동적으로 생성하여 실행해야 합니다. 이를 위해서는 `Dynamic SQL`을 사용하여 동적으로 SQL 쿼리를 생성하는 방식을 사용해야 합니다. `Dynamic SQL`은 SQL 쿼리를 문자열로 동적으로 생성하고, `EXECUTE IMMEDIATE` 명령문을 사용하여 해당 쿼리를 실행하는 방식입니다.

따라서, `insertTempTable` 매퍼에서는 다음과 같이 `Dynamic SQL`을 사용하여 테이블 이름을 동적으로 생성하도록 수정해야 합니다.

```xml
<insert id="insertTempTable" parameterType="java.util.HashMap">
  <![CDATA[
    DECLARE
      v_table_name VARCHAR2(100) := :tempTableName;
      v_sql VARCHAR2(1000);
    BEGIN
      v_sql := 'INSERT INTO ' || v_table_name || ' VALUES ( ' ||
               ':SEQ_ID, CAST(:CAL_ID AS NUMBER), :BIZ_ID, :PRJ_ID, ' ||
               'CAST(:OBJ_ID AS NUMBER), CAST(:FILE_ID AS NUMBER), ' ||
               'CAST(:FUNC_ID AS NUMBER), :PRJ_PATH, :FILE_NAME, ' ||
               ':FUNC_NAME, :FILE_PATH, :FILE_COMMENT, :OBJ_NAME#, ' ||
               ':OBJ_TYPE, :DBC_CHECK, CAST(:REL_CAL_ID AS NUMBER), ' ||
               ':REL_PRJ_ID, CAST(:REL_OBJ_ID AS NUMBER), ' ||
               'CAST(:REL_FILE_ID AS NUMBER), CAST(:REL_FUNC_ID AS NUMBER), ' ||
               ':REL_FILE_NAME, :REL_FUNC_NAME, CAST(:REL_DEPTH AS NUMBER))';
      EXECUTE IMMEDIATE v_sql
      USING :SEQ_ID, :CAL_ID, :BIZ_ID, :PRJ_ID, :OBJ_ID, :FILE_ID,
            :FUNC_ID, :PRJ_PATH, :FILE_NAME, :FUNC_NAME, :FILE_PATH,
            :FILE_COMMENT, :OBJ_NAME#, :OBJ_TYPE, :DBC_CHECK, :REL_CAL_ID,
            :REL_PRJ_ID, :REL_OBJ_ID, :REL_FILE_ID, :REL_FUNC_ID,
            :REL_FILE_NAME, :REL_FUNC_NAME, :REL_DEPTH;
    END;
  ]]>
</insert>
```

위의 코드에서 `v_table_name` 변수에 `tempTableName` 파라미터 값을 할당하고, `v_sql` 변수에 동적으로 생성된 SQL 쿼리를 할당한 후, `EXECUTE IMMEDIATE` 명령문을 사용하여 해당 쿼리를 실행하고 있습니다. 이렇게 변경된 코드를 사용하면 오라클 데이터베이스에서도 테이블 이름을 동적으로 바인딩할 수 있습니다.

## 사용자 입력:
> SQL: DECLARE         v_table_name VARCHAR2(100) := :tempTableName;       v_sql VARCHAR2(1000);         BEGIN       v_sql := 'INSERT INTO ' || v_table_name || ' VALUES ( ' ||                ':SEQ_ID, CAST(:CAL_ID AS NUMBER), :BIZ_ID, :PRJ_ID, ' ||                'CAST(:OBJ_ID AS NUMBER), CAST(:FILE_ID AS NUMBER), ' ||                'CAST(:FUNC_ID AS NUMBER), :PRJ_PATH, :FILE_NAME, ' ||                ':FUNC_NAME, :FILE_PATH, :FILE_COMMENT, :OBJ_NAME#, ' ||                ':OBJ_TYPE, :DBC_CHECK, CAST(:REL_CAL_ID AS NUMBER), ' ||                ':REL_PRJ_ID, CAST(:REL_OBJ_ID AS NUMBER), ' ||                'CAST(:REL_FILE_ID AS NUMBER), CAST(:REL_FUNC_ID AS NUMBER), ' ||                ':REL_FILE_NAME, :REL_FUNC_NAME, CAST(:REL_DEPTH AS NUMBER))';         EXECUTE IMMEDIATE v_sql             USING :SEQ_ID, :CAL_ID, :BIZ_ID, :PRJ_ID, :OBJ_ID, :FILE_ID,             :FUNC_ID, :PRJ_PATH, :FILE_NAME, :FUNC_NAME, :FILE_PATH,             :FILE_COMMENT, :OBJ_NAME#, :OBJ_TYPE, :DBC_CHECK, :REL_CAL_ID,             :REL_PRJ_ID, :REL_OBJ_ID, :REL_FILE_ID, :REL_FUNC_ID,             :REL_FILE_NAME, :REL_FUNC_NAME, :REL_DEPTH;         END;
### Cause: java.sql.SQLException: 인덱스에서 누락된 IN 또는 OUT 매개변수:: 1
; uncategorized SQLException for SQL []; SQL state [99999]; error code [17041]; 인덱스에서 누락된 IN 또는 OUT 매개변수:: 1; nested exception is java.sql.SQLException: 인덱스에서 누락된 IN 또는 OUT 매개변수:: 1


## ChatGPT 응답:
> 이 오류는 실행 시간에 바인딩되는 매개변수의 개수와 쿼리문에서 사용하는 매개변수의 개수가 일치하지 않을 때 발생합니다. 오류 메시지에 "인덱스에서 누락된 IN 또는 OUT 매개변수:: 1"라는 문구가 있으므로, 바인딩되지 않은 매개변수가 쿼리문에서 첫 번째 위치에 있다는 것을 의미합니다.

`insertTempTable` 매퍼에서는 22개의 파라미터를 사용하고 있지만, 동적으로 생성된 SQL 쿼리에는 23개의 바인딩 매개변수가 포함되어 있습니다. 이러한 이유로 오류가 발생하고 있습니다.

이 문제를 해결하려면 다음과 같이 SQL 쿼리에 사용되는 바인딩 매개변수의 개수와 매퍼에서 전달하는 파라미터의 개수가 일치하도록 수정해야 합니다.

```xml
<insert id="insertTempTable" parameterType="java.util.HashMap">
  <![CDATA[
    DECLARE
      v_table_name VARCHAR2(100) := :tempTableName;
      v_sql VARCHAR2(1000);
    BEGIN
      v_sql := 'INSERT INTO ' || v_table_name || ' VALUES ( ' ||
               ':SEQ_ID, CAST(:CAL_ID AS NUMBER), :BIZ_ID, :PRJ_ID, ' ||
               'CAST(:OBJ_ID AS NUMBER), CAST(:FILE_ID AS NUMBER), ' ||
               'CAST(:FUNC_ID AS NUMBER), :PRJ_PATH, :FILE_NAME, ' ||
               ':FUNC_NAME, :FILE_PATH, :FILE_COMMENT, :OBJ_NAME#, ' ||
               ':OBJ_TYPE, :DBC_CHECK, CAST(:REL_CAL_ID AS NUMBER), ' ||
               ':REL_PRJ_ID, CAST(:REL_OBJ_ID AS NUMBER), ' ||
               'CAST(:REL_FILE_ID AS NUMBER), CAST(:REL_FUNC_ID AS NUMBER), ' ||
               ':REL_FILE_NAME, :REL_FUNC_NAME, CAST(:REL_DEPTH AS NUMBER))';
      EXECUTE IMMEDIATE v_sql
      USING :SEQ_ID, :CAL_ID, :BIZ_ID, :PRJ_ID, :OBJ_ID, :FILE_ID,
            :FUNC_ID, :PRJ_PATH, :FILE_NAME, :FUNC_NAME, :FILE_PATH,
            :FILE_COMMENT, :OBJ_NAME#, :OBJ_TYPE, :DBC_CHECK, :REL_CAL_ID,
            :REL_PRJ_ID, :REL_OBJ_ID, :REL_FILE_ID, :REL_FUNC_ID,
            :REL_FILE_NAME, :REL_FUNC_NAME, :REL_DEPTH, null;
    END;
  ]]>
</insert>
```

위의 코드에서는 SQL 쿼리에 23개의 바인딩 매개변수를 사용하고 있지만, 매퍼에서 전달하는 파라미터는 22개입니다. 따라서, `EXECUTE IMMEDIATE` 명령문에서는 마지막 매개변수로 `null` 값을 추가하여 오류를 해결하였습니다. 해당 매개변수는 SQL 쿼리에서 사용되지 않으므로, 어떤 값을 넣어도 상관

## 사용자 입력:
> RelationControlDao.java안에  void insertTempTable(String tempTableName, HashMap<String, String> args); 선언되어 있고, 매퍼는  <insert id="insertTempTable" parameterType="java.util.HashMap">
        INSERT INTO ${tempTableName}
        VALUES
            ( #{SEQ_ID}
            , CAST(#{CAL_ID} AS NUMERIC )
            , #{BIZ_ID}
            , #{PRJ_ID}
            , CAST(#{OBJ_ID} AS NUMERIC )
            , CAST(#{FILE_ID} AS NUMERIC )
            , CAST(#{FUNC_ID} AS NUMERIC )
            , #{PRJ_PATH}
            , #{FILE_NAME}
            , #{FUNC_NAME}
            , #{FILE_PATH}
            , #{FILE_COMMENT}
            , #{OBJ_NAME#}
            , #{OBJ_TYPE}
            , #{DBC_CHECK}
            , CAST(#{REL_CAL_ID} AS NUMERIC )
            , #{REL_PRJ_ID}
            , CAST(#{REL_OBJ_ID} AS NUMERIC )
            , CAST(#{REL_FILE_ID} AS NUMERIC )
            , CAST(#{REL_FUNC_ID} AS NUMERIC )
            , #{REL_FILE_NAME}
            , #{REL_FUNC_NAME}
            , CAST(#{REL_DEPTH} AS NUMERIC ))
    </insert> 이렇게 되어 있다. 코드에서  String tempTableName="s";HashMap e = new Hashmap; e에는 필요한 모든 변수가 세팅되었다고 가정한다. relationControlDao.insertTempTable(tempTableName, e);을 호출하면, nested exception is org.apache.ibatis.binding.BindingException: Parameter 'tempTableName' not found. Available parameters are [arg1, arg0, param1, param2] 이런 메세지가 나와.
	postgresql에서는 잘 동작했는데, oracle에서는 오류메세지가 나오는데 어떻게 고쳐야 하나?

## ChatGPT 응답:
> 해당 오류는 MyBatis에서 `parameterType`으로 지정한 객체 내에 `tempTableName`이라는 이름의 프로퍼티가 없기 때문에 발생하고 있습니다. 이 오류는 PostgreSQL에서는 발생하지 않은 이유는 PostgreSQL에서는 `parameterType`으로 지정한 객체와 쿼리에서 사용하는 파라미터의 이름이 일치하지 않아도 자동으로 매핑되기 때문입니다.

오라클 데이터베이스에서는 이와 달리 `parameterType`으로 지정한 객체와 쿼리에서 사용하는 파라미터의 이름이 일치해야 자동으로 매핑됩니다. 따라서, `tempTableName` 프로퍼티를 객체에 추가하거나, MyBatis 매퍼에서 `tempTableName` 프로퍼티를 사용하는 방법을 변경해야 합니다.

1. `tempTableName` 프로퍼티를 객체에 추가하는 방법

```java
public class TempTable {
  private String tempTableName;
  private String SEQ_ID;
  private String CAL_ID;
  private String BIZ_ID;
  // 나머지 필드 생략

  // getters, setters 생략
}
```

```xml
<insert id="insertTempTable" parameterType="TempTable">
  INSERT INTO ${tempTableName}
  VALUES
    ( #{SEQ_ID}
    , CAST(#{CAL_ID} AS NUMERIC )
    , #{BIZ_ID}
    , #{PRJ_ID}
    , CAST(#{OBJ_ID} AS NUMERIC )
    , CAST(#{FILE_ID} AS NUMERIC )
    , CAST(#{FUNC_ID} AS NUMERIC )
    , #{PRJ_PATH}
    , #{FILE_NAME}
    , #{FUNC_NAME}
    , #{FILE_PATH}
    , #{FILE_COMMENT}
    , #{OBJ_NAME#}
    , #{OBJ_TYPE}
    , #{DBC_CHECK}
    , CAST(#{REL_CAL_ID} AS NUMERIC )
    , #{REL_PRJ_ID}
    , CAST(#{REL_OBJ_ID} AS NUMERIC )
    , CAST(#{REL_FILE_ID} AS NUMERIC )
    , CAST(#{REL_FUNC_ID} AS NUMERIC )
    , #{REL_FILE_NAME}
    , #{REL_FUNC_NAME}
    , CAST(#{REL_DEPTH} AS NUMERIC ))
</insert>
```

위의 코드에서는 `TempTable`이라는 클래스를 만들어 `tempTableName` 프로퍼티를 추가하고, `parameterType`을 `TempTable`로 변경하였습니다. 이렇게 하면 MyBatis에서 `tempTableName` 프로퍼티를 자동으로 매핑할 수 있습니다.

2. MyBatis 매퍼에서 `tempTableName` 프로퍼티를 사용하는 방법을 변경하는 방법

```xml
<insert id="insertTempTable" parameterType="java.util.HashMap">
  <![CDATA[
    INSERT INTO ${arg0}
    VALUES
      ( #{arg1.SEQ_ID}
      , CAST(#{arg1.CAL_ID} AS NUMERIC )
      , #{arg1.BIZ_ID}
      , #{arg1.PRJ_ID}
      , CAST(#{arg1.OBJ_ID} AS NUMERIC )
      , CAST(#{arg1.FILE_ID} AS NUMERIC )
      , CAST

## 사용자 입력:
> MyBatis 매퍼에서 tempTableName 프로퍼티를 사용하는 방법을 변경하는 방법이 설명에서 끊겼어. 완벽한 설명부탁해.

## ChatGPT 응답:
> 죄송합니다. 이어서 설명드리겠습니다.

2. MyBatis 매퍼에서 `tempTableName` 프로퍼티를 사용하는 방법을 변경하는 방법

```xml
<insert id="insertTempTable" parameterType="java.util.HashMap">
  <![CDATA[
    INSERT INTO ${arg0}
    VALUES
      ( #{arg1.SEQ_ID}
      , CAST(#{arg1.CAL_ID} AS NUMERIC )
      , #{arg1.BIZ_ID}
      , #{arg1.PRJ_ID}
      , CAST(#{arg1.OBJ_ID} AS NUMERIC )
      , CAST(#{arg1.FILE_ID} AS NUMERIC )
      , CAST(#{arg1.FUNC_ID} AS NUMERIC )
      , #{arg1.PRJ_PATH}
      , #{arg1.FILE_NAME}
      , #{arg1.FUNC_NAME}
      , #{arg1.FILE_PATH}
      , #{arg1.FILE_COMMENT}
      , #{arg1.OBJ_NAME#}
      , #{arg1.OBJ_TYPE}
      , #{arg1.DBC_CHECK}
      , CAST(#{arg1.REL_CAL_ID} AS NUMERIC )
      , #{arg1.REL_PRJ_ID}
      , CAST(#{arg1.REL_OBJ_ID} AS NUMERIC )
      , CAST(#{arg1.REL_FILE_ID} AS NUMERIC )
      , CAST(#{arg1.REL_FUNC_ID} AS NUMERIC )
      , #{arg1.REL_FILE_NAME}
      , #{arg1.REL_FUNC_NAME}
      , CAST(#{arg1.REL_DEPTH} AS NUMERIC )
      )
  ]]>
</insert>
```

위의 코드에서는 `tempTableName`을 `#{arg0}`으로 변경하고, `HashMap` 객체에서 사용하는 프로퍼티 이름을 `arg1`로 변경하였습니다. 이렇게 변경하면 MyBatis에서 `tempTableName` 프로퍼티를 찾을 수 있습니다.

따라서, `RelationControlDao` 클래스에서 아래와 같이 `insertTempTable` 메소드를 호출하면 됩니다.

```java
String tempTableName = "s";
HashMap<String, String> args = new HashMap<>();
args.put("SEQ_ID", "1");
args.put("CAL_ID", "2");
args.put("BIZ_ID", "3");
// 나머지 필드 생략

relationControlDao.insertTempTable(tempTableName, args);
```

