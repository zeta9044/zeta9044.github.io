---
layout: post
title: "Jira 필터에서 비활성화된 assignee 제외"
date: 2023-03-24
tags: [활성화, Jira, 필터, 비, 제외, assignee]
categories: blog
---

## 사용자 입력:
> project = DFT AND assignee in (membersOf(DF)) ORDER BY created DESC 은 jira 필터인데, 비활성화된 assiginee를 제외하려면 ?

## ChatGPT 응답:
> Jira에서 비활성화된 사용자를 제외하려면, "assignee not in inactiveUsers()" 를 필터에 추가해야 합니다. 따라서, 주어진 필터를 다음과 같이 변경할 수 있습니다:

```
project = DFT AND assignee in (membersOf(DF)) AND assignee not in inactiveUsers() ORDER BY created DESC
```

이렇게 하면, 비활성화된 assignee가 필터에서 제외되어, 활성화된 assignee만 포함된 결과를 볼 수 있습니다.

## 사용자 입력:
> JQL 함수 'inactiveUsers()'(을)를 찾을 수 없습니다.

## ChatGPT 응답:
> 죄송합니다. 제가 알려드린 'inactiveUsers()' 함수는 Jira에 기본으로 포함된 함수가 아닙니다. 사용자 정의 함수를 사용하여 비활성 사용자를 필터링하는 방법에 대해 설명하겠습니다. Jira에서 비활성 사용자를 제외하려면, 먼저 Jira Query Language (JQL)을 사용하여 필터를 만들고, 해당 필터를 이용하여 사용자를 제외해야 합니다.

다음은 필터를 작성하는 방법을 설명하는 예입니다.

1. 먼저, 사용자 지정 함수를 사용할 수 있는 Scriptrunner 또는 유사한 Jira 플러그인을 설치해야 합니다.

2. 플러그인이 설치된 후, 다음과 같이 JQL을 작성하여 비활성 사용자를 제외할 수 있습니다.

```
project = DFT AND assignee in (membersOf(DF)) AND assignee not in ("비활성 사용자 1", "비활성 사용자 2", ...) ORDER BY created DESC
```

이 방법은 비활성 사용자를 명시적으로 나열해야 한다는 단점이 있습니다.

더 간편한 방법을 원한다면, ScriptRunner와 같은 플러그인을 사용하여 사용자 정의 JQL 함수를 만들어 사용할 수 있습니다. 사용자 정의 JQL 함수를 작성하려면 다음 링크를 참조하세요: https://scriptrunner.adaptavist.com/latest/jira/jql-functions.html

이렇게하면 "inactiveUsers()"와 같은 사용자 정의 함수를 작성하여 비활성 사용자를 쉽게 필터링 할 수 있습니다.

## 사용자 입력:
> 슬랙탈퇴방법

## ChatGPT 응답:
> 슬랙(Slack)에서 탈퇴하는 방법은 사용자의 권한과 원하는 결과에 따라 다릅니다. 여기에서는 기본적인 슬랙 탈퇴 방법을 설명합니다.

1. 개별 채널에서 나가기:
- 채널에서 나가고 싶다면, 채널명 옆에 있는 아이콘을 클릭하고 'Leave channel'을 선택하면 됩니다. 이렇게 하면 해당 채널에서만 나갈 수 있습니다.

2. 슬랙 워크스페이스에서 나가기:
- 전체 슬랙 워크스페이스에서 탈퇴하려면 다음 단계를 따르세요.
  1. 워크스페이스 이름을 클릭하세요.
  2. 'Profile & account'을 선택하세요.
  3. 'Profile' 탭에서 'More actions'을 클릭하세요.
  4. 'Leave [Your Workspace Name]'을 선택하면 해당 워크스페이스에서 나갈 수 있습니다.

3. 슬랙 계정 삭제하기:
- 슬랙 계정을 완전히 삭제하려면, 계정 설정 페이지에서 삭제할 수 있습니다. 단, 이 작업은 워크스페이스 관리자의 승인이 필요할 수 있습니다. 계정 삭제를 원할 경우, 다음과 같이 진행하세요.
  1. 워크스페이스 이름을 클릭하세요.
  2. 'Profile & account'을 선택하세요.
  3. 'Account' 탭으로 이동하세요.
  4. 'Deactivate your account'을 클릭하세요.
  5. 안내에 따라 계정 삭제를 완료하세요.

위에서 설명한 방법은 일반적인 슬랙 사용자를 위한 것입니다. 워크스페이스 관리자나 슬랙 조직의 멤버인 경우, 탈퇴 방법이 다를 수 있으며, 슬랙 고객 지원에 문의해야 할 수도 있습니다.

## 사용자 입력:
> https://hooks.slack.com/services/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 이런 건 어디서 만들어?

## ChatGPT 응답:
> 슬랙 웹훅(Webhook) URL은 슬랙 앱에서 사용되는 통합 기능 중 하나입니다. 이를 사용하면 외부 서비스에서 슬랙 채널로 메시지를 보낼 수 있습니다. 슬랙 웹훅 URL을 만드는 방법은 다음과 같습니다.

1. 먼저, 슬랙 워크스페이스에 로그인하고, 웹 브라우저에서 슬랙 앱 디렉토리로 이동하세요: https://your_workspace_name.slack.com/apps (여기서 'your_workspace_name'을 실제 워크스페이스 이름으로 바꾸세요)

2. 검색 창에서 "Incoming WebHooks"를 검색하고, 결과 목록에서 "Incoming WebHooks" 앱을 클릭하세요.

3. "Add to Slack" 버튼을 클릭하세요.

4. "Post to Channel" 드롭다운에서 메시지를 받을 슬랙 채널을 선택하세요. 필요한 경우, 새 채널을 만들 수도 있습니다.

5. "Add Incoming WebHooks integration" 버튼을 클릭하세요.

6. 설정 페이지에서 웹훅 URL이 생성됩니다. 이 URL은 "Webhook URL" 섹션에 표시되며, `https://hooks.slack.com/services/...`와 같은 형식입니다.

7. 웹훅 설정을 사용자 지정하려면 해당 페이지에서 추가 옵션을 구성하세요. 예를 들어, 기본 사용자 이름, 아이콘, 레이블 등을 변경할 수 있습니다.

생성된 웹훅 URL을 사용하여 외부 서비스에서 메시지를 슬랙 채널로 보낼 수 있습니다. 이 URL은 외부 서비스에 추가하거나, 프로그래밍 언어와 라이브러리를 사용하여 메시지를 보내는 데 사용할 수 있습니다. 웹훅 URL은 공개적으로 노출되지 않도록 주의하세요. 제3자가 이 URL을 획득하면 해당 슬랙 채널에 메시지를 보낼 수 있습니다.

