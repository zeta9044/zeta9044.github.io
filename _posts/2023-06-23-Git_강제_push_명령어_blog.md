---
layout: post
title: "Git 강제 push 명령어"
tags: [강제, push, 명령어, Git]
---

## 사용자 입력:
> git 로컬에서 원격에 강제로 push하는 명령어.

## ChatGPT 응답:
> 로컬 저장소에서 원격 저장소로 강제로 push하는 명령어는 다음과 같습니다:

{% raw %}```shell
git push -f origin <branch_name>
```{% endraw %}

위 명령어에서 {% raw %}`<branch_name>`{% endraw %}은 원하는 브랜치의 이름입니다. 이 명령어는 로컬 저장소의 변경 사항을 원격 저장소로 강제로 밀어넣습니다. 하지만 주의해야 할 점은 이 작업은 원격 저장소에 있는 변경 사항을 덮어쓸 수 있으므로 신중하게 사용해야 합니다.

