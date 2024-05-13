---
layout: post
title: "Convert Oracle Query to PostgreSQL"
tags: [PostgreSQL, Convert, to, Oracle, Query]
---

## 사용자 입력:
> SELECT
					CASE
						WHEN instr(x.ATTR_VALUE, 'ConnectorPX') > 0 THEN 'tbl'
						ELSE 'obj'
					END
				FROM
					ais0039 x  이 쿼리는 오라클인데, postgresql로 바꾸면?

## ChatGPT 응답:
> 

아래와 같이 PostgreSQL에서 해당 쿼리를 작성할 수 있습니다.

{% raw %}```
SELECT 
	CASE
		WHEN strpos(x.ATTR_VALUE, 'ConnectorPX') > 0 THEN 'tbl'
		ELSE 'obj'
	END
FROM
	ais0039 x
```{% endraw %} 

주의할 점은 PostgreSQL에서 문자열 함수 {% raw %}`instr`{% endraw %}은 사용할 수 없으며, 대신 {% raw %}`strpos`{% endraw %} 함수를 사용해야 한다는 점입니다. {% raw %}`strpos`{% endraw %} 함수는 인자 순서가 다르기 때문에, 문자열이 첫 번째 인자에 위치해야 합니다.

## 사용자 입력:
> 						select
							substr(
								second_part
								, 1
								, instr(
									second_part
									, '.'
								) - 1
							) as column1
						from
							(
								select
									reverse(
										split_part(
											reverse(s4.ATTR_VALUE)
											, '\\'
											, 1
										)
									) as second_part
								from
									dual
							)  오라클 쿼리인데, postgresql로 변경해줘.

## ChatGPT 응답:
> 다음은 PostgreSQL에서 해당 쿼리를 작성한 예입니다.

{% raw %}```
SELECT 
	substring(
		second_part
		, 1
		, strpos(
			second_part
			, '.'
		) - 1
	) AS column1
FROM (
	SELECT 
		reverse(
			split_part(
				reverse(s4.ATTR_VALUE)
				, E'\\'
				, 1
			)
		) AS second_part
	FROM 
		(SELECT 1) AS dummy_table
) AS subquery_alias;
```{% endraw %}

PostgreSQL에서는 {% raw %}`dual`{% endraw %}이라는 가상 테이블이 없기 때문에, {% raw %}`SELECT 1`{% endraw %}과 같이 임시적인 값을 반환하는 서브쿼리를 사용하여 대체할 수 있습니다. 또한 PostgreSQL에서 이스케이프 시퀀스를 사용하기 위해서는 문자열 앞에 {% raw %}`E`{% endraw %}를 붙여야 합니다. 마지막으로, {% raw %}`substr`{% endraw %} 함수는 PostgreSQL에서 {% raw %}`substring`{% endraw %} 함수로 대체되어 사용됩니다.

## 사용자 입력:
> 								select
									substr(
										second_part
										, instr(
											second_part
											, '.'
										) + 1
									) as column2
								from
									(
										select
											reverse(
												split_part(
													reverse(s4.ATTR_VALUE)
													, '\\'
													, 1
												)
											) as second_part
										from
											dual
									) 오라클 쿼리인데, postgresql로 변경해줘.

## ChatGPT 응답:
> 다음은 PostgreSQL에서 해당 쿼리를 작성한 예입니다.

{% raw %}```
SELECT 
	substring(
		second_part
		, strpos(
			second_part
			, '.'
		) + 1
	) AS column2
FROM (
	SELECT 
		reverse(
			split_part(
				reverse(s4.ATTR_VALUE)
				, E'\\'
				, 1
			)
		) AS second_part
	FROM 
		(SELECT 1) AS dummy_table
) AS subquery_alias;
```{% endraw %}

위와 마찬가지로, {% raw %}`dual`{% endraw %} 테이블 대신 {% raw %}`SELECT 1`{% endraw %}과 같은 임시 테이블을 사용합니다. 그리고 문자열 앞에 {% raw %}`E`{% endraw %}를 붙여 이스케이프 시퀀스를 지정해주어야 합니다. 마지막으로, {% raw %}`substr`{% endraw %} 함수 대신 PostgreSQL에서는 {% raw %}`substring`{% endraw %} 함수를 사용합니다.

