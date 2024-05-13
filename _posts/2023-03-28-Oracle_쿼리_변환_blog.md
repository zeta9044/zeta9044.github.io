---
layout: post
title: "Oracle 쿼리 변환"
tags: [쿼리, Oracle, 변환]
---

## 사용자 입력:
>   <select id="findSystemClass" resultMap="findSystemClassMap">
    select row_number() over() as no
         , (
            select biz_eng_name
              from DUQTRKON01.vw_ais1000 a
             where biz_id = z.biz_id
           ) id
         , (
            select biz_eng_name
              from DUQTRKON01.vw_ais1000 a
             where biz_id = z.upper_biz_id
           ) parentid
         , biz_kor_name as name
      from
           <foreach close=")" collection="systems" item="item" open="(" separator=" union all ">
           (
            with recursive x as (
                                 select a.biz_id
                                      , a.upper_biz_id
                                      , a.biz_kor_name
                                   from DUQTRKON01.vw_ais1000 a
                                  where a.biz_id =(
                                                   select biz_id
                                                     from DUQTRKON01.vw_ais1000 a
                                                    where biz_eng_name = #{item}
                                                   )
                                  union all
                                 select b.biz_id
                                      , b.upper_biz_id
                                      , b.biz_kor_name
                                   from DUQTRKON01.vw_ais1000 b
                                   join x
                                     on x.biz_id = b.UPPER_BIZ_ID
                                 )
                   select x.biz_id
                        , x.upper_biz_id
                        , x.biz_kor_name
                     from x
           )
        </foreach>
         z
    </select> 이 mybatis 쿼리 xml 인데, postgresql쿼리야. 이것을 oracle 쿼리로 바꿔줘

## ChatGPT 응답:
> 

아래는 Oracle에서 동작하는 쿼리로 변환한 결과입니다.

{% raw %}```xml
<select id="findSystemClass" resultMap="findSystemClassMap">
  select rownum as no,
         (
           select biz_eng_name
           from DUQTRKON01.vw_ais1000 a
           where a.biz_id = z.biz_id
         ) as id,
         (
           select biz_eng_name
           from DUQTRKON01.vw_ais1000 a
           where a.biz_id = z.upper_biz_id
         ) as parentid,
         biz_kor_name as name
  from (
         <foreach close=")" collection="systems" item="item" open="(" separator=" union all ">
           with x (biz_id, upper_biz_id, biz_kor_name) as (
             select a.biz_id, a.upper_biz_id, a.biz_kor_name
             from DUQTRKON01.vw_ais1000 a
             where a.biz_id = (
                                select biz_id
                                from DUQTRKON01.vw_ais1000 a
                                where a.biz_eng_name = #{item}
                              )
             union all
             select b.biz_id, b.upper_biz_id, b.biz_kor_name
             from DUQTRKON01.vw_ais1000 b
             join x
             on x.biz_id = b.UPPER_BIZ_ID
           )
           select x.biz_id, x.upper_biz_id, x.biz_kor_name
           from x
         </foreach>
       ) z
</select>
```{% endraw %}

Oracle에서는 {% raw %}`row_number()`{% endraw %} 함수 대신 {% raw %}`rownum`{% endraw %}을 사용할 수 있습니다. 또한 Oracle에서는 {% raw %}`with recursive`{% endraw %} 구문을 지원하지 않으므로, {% raw %}`with`{% endraw %} 구문 안에 재귀적인 쿼리를 구현해야합니다. 마지막으로, Oracle에서는 {% raw %}`)`{% endraw %} 문자를 사용하기 전에 {% raw %}`with`{% endraw %} 구문의 이름을 반드시 명시해주어야합니다.

위의 변환된 쿼리는 Oracle에서 실행될 수 있도록 수정되었습니다.

## 사용자 입력:
> 위 쿼리는 계층형 쿼리가 아니어서 데이터가 안 나오는데.

## ChatGPT 응답:
> 저의 답변이 부적절하여 죄송합니다. 위 쿼리는 계층형 쿼리이기 때문에 Oracle에서 실행할 수 없습니다. Oracle에서는 {% raw %}`WITH RECURSIVE`{% endraw %} 구문을 지원하지 않기 때문입니다.

그러나 Oracle에서도 계층형 쿼리를 작성할 수 있습니다. 아래는 Oracle에서 실행 가능한 계층형 쿼리 예시입니다.

{% raw %}```xml
<select id="findSystemClass" resultMap="findSystemClassMap">
  SELECT LEVEL AS no,
         a.biz_eng_name AS id,
         b.biz_eng_name AS parentid,
         a.biz_kor_name AS name
  FROM DUQTRKON01.vw_ais1000 a
  LEFT OUTER JOIN DUQTRKON01.vw_ais1000 b ON b.biz_id = a.upper_biz_id
  START WITH a.biz_eng_name IN 
  <foreach close=")" collection="systems" item="item" open="(" separator=",">
    #{item}
  </foreach>
  CONNECT BY PRIOR a.biz_id = a.upper_biz_id;
</select>
```{% endraw %}

위 쿼리는 Oracle의 {% raw %}`START WITH`{% endraw %}와 {% raw %}`CONNECT BY PRIOR`{% endraw %} 구문을 사용하여 계층형 쿼리를 작성했습니다. 또한, {% raw %}`LEFT OUTER JOIN`{% endraw %}을 사용하여 부모 이름을 가져오도록 했습니다.

쿼리 실행 시 {% raw %}`systems`{% endraw %} 파라미터에는 찾고자 하는 {% raw %}`biz_eng_name`{% endraw %} 값을 배열 형태로 전달하면 됩니다.

