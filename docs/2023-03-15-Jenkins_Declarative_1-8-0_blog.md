---
layout: post
title: "Jenkins Declarative 1.8.0"
date: 2023-03-15
tags: [Jenkins, Declarative]
categories: blog
---

## 사용자 입력:
> jenkins declaretive 최신 버전 알려줘

## ChatGPT 응답:
> 현재(2023년 3월 15일) 최신 버전은 Jenkins Declarative Extension 1.8.0입니다. 이 버전은 2022년 9월 14일에 릴리스되었습니다. 그러나 Jenkins와 Declarative Extension 모두 계속해서 업데이트되기 때문에, 이 글을 읽는 시점에는 더 높은 버전이 나와 있을 수 있습니다. 따라서 정확한 최신 버전을 확인하려면 Jenkins와 Declarative Extension의 공식 웹사이트를 참조하시기 바랍니다.

## 사용자 입력:
> dbeaver에서 한글이 안깨지도록 하려면

## ChatGPT 응답:
> dbeaver에서 한글이 깨지는 경우 대부분은 인코딩 설정 문제로 발생합니다. 따라서 다음과 같은 방법으로 한글이 깨지지 않도록 설정할 수 있습니다.

1. Dbeaver 설정에서 인코딩 설정 확인하기
   - Dbeaver 상단 메뉴바에서 [Window] - [Preferences] - [General] - [Workspace] 메뉴로 이동합니다.
   - Text file encoding 항목에서 인코딩 설정을 [UTF-8]로 변경합니다.

2. Dbeaver 연결 설정에서 인코딩 설정 확인하기
   - Dbeaver에서 연결 설정을 수정하고자 하는 데이터베이스를 우클릭하여 [Edit Connection]을 선택합니다.
   - [Driver properties] 탭에서 Connection properties 항목 중 "useUnicode"와 "characterEncoding" 값을 모두 "true"와 "UTF-8"로 변경합니다.
   
3. Dbeaver 실행 시 JVM 옵션 추가
   - Dbeaver를 실행할 때 JVM 옵션으로 -Dfile.encoding=UTF-8을 추가합니다.
     (예: dbeaver.exe -vmargs -Dfile.encoding=UTF-8)

위의 방법으로도 한글이 깨지는 경우, 데이터베이스와 연관된 인코딩 설정 문제일 가능성이 있으므로 데이터베이스 설정도 확인해 보시기 바랍니다.

