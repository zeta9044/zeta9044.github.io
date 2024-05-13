---
layout: post
title: "Artifact 생성 패턴 설정"
tags: [생, Artifact, 설정, 패턴]
---

## 사용자 입력:
> bamboo에서 artifact 생성할 때, Specify the name (or Ant file copy pattern) of the artifact(s) you want to keep. e.g. **/*.jar. 이런 설명이 있는데, **/aiworks/lib/system/  이런 디렉토리 밑에 *.jar와 review-log4j.xml을 가져오려고 하면?

## ChatGPT 응답:
> Bamboo에서 artifact를 생성할 때, 특정 파일 패턴을 지정하여 원하는 파일을 선택할 수 있습니다. 예를 들어, {% raw %}`**/aiworks/lib/system/`{% endraw %} 디렉토리 밑에 있는 모든 {% raw %}`.jar`{% endraw %} 파일과 {% raw %}`review-log4j.xml`{% endraw %} 파일을 가져오고자 한다면, 다음과 같은 패턴을 사용할 수 있습니다:

{% raw %}```
**/aiworks/lib/system/*.jar
**/aiworks/lib/system/review-log4j.xml
```{% endraw %}

여기서 {% raw %}`**/`{% endraw %}는 모든 하위 디렉토리를 의미하며, {% raw %}`*.jar`{% endraw %}는 해당 경로에 있는 모든 {% raw %}`.jar`{% endraw %} 파일을 선택합니다. {% raw %}`review-log4j.xml`{% endraw %}은 지정된 경로에 있는 해당 파일을 명시적으로 선택합니다.

이렇게 설정하면 Bamboo는 지정된 패턴에 일치하는 모든 파일을 artifact로 취급하여 보관합니다.

## 사용자 입력:
> 너는 2줄이잖아.

## ChatGPT 응답:
> 아, 이해했습니다. Bamboo에서 하나의 패턴으로 여러 파일 타입을 포함시키고 싶다면, 다음과 같이 작성할 수 있습니다:

{% raw %}```
**/aiworks/lib/system/*.jar, **/aiworks/lib/system/review-log4j.xml
```{% endraw %}

이 패턴은 {% raw %}`**/aiworks/lib/system/`{% endraw %} 디렉토리 밑에 있는 모든 {% raw %}`.jar`{% endraw %} 파일과 {% raw %}`review-log4j.xml`{% endraw %} 파일을 하나의 패턴으로 지정합니다. Bamboo는 이 패턴에 해당하는 모든 파일을 artifact로 취급합니다.

