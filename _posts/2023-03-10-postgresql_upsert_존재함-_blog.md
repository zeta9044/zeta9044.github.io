---
layout: post
title: "postgresql upsert 존재함."
tags: [함, 존재, postgresql, upsert]
---

## 사용자 입력:
> postgresql에서 upsert가 있나?

## ChatGPT 응답:
> 네, PostgreSQL은 UPSERT (INSERT ON CONFLICT)라는 기능을 제공합니다. 이를 사용하면 INSERT 문을 실행하면서 충돌이 발생하면 해당 행을 업데이트할 수 있습니다.

예를 들어, 다음 쿼리는 "users" 테이블에 새로운 사용자를 삽입하고, 이미 해당 이메일 주소를 가진 사용자가 있으면 해당 사용자의 이름을 업데이트합니다.

{% raw %}```sql
INSERT INTO users (email, name) VALUES ('test@example.com', 'John Doe')
  ON CONFLICT (email) DO UPDATE SET name = 'Jane Doe';
```{% endraw %}

이 쿼리에서 "email" 열에 대해 고유 제약 조건이 있어야 합니다. 이것은 UPSERT를 사용하려면 필수적인 조건입니다.

