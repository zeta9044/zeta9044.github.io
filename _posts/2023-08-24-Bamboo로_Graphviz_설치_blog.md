---
layout: post
title: "Bamboo로 Graphviz 설치"
date: 2023-08-24
tags: [설치, 로, Graphviz]
categories: blog
---

## 사용자 입력:
> conflence서버에 bamboo도 같이 설치되어 있는데, bamboo의 build기능을 이용해서, graphviz를 설치할 수는 없나?

## ChatGPT 응답:
> Bamboo와 Confluence는 Atlassian에서 개발한 제품으로, Bamboo는 CI/CD (Continuous Integration / Continuous Deployment)를 위한 도구이고, Confluence는 위키 및 협업 도구입니다.

Graphviz는 그래프 시각화 소프트웨어입니다. Bamboo의 빌드 기능을 사용하여 서버에 Graphviz를 설치하는 것은 가능합니다. 그렇게 하려면 Bamboo에서 작업 플랜을 생성하고, 이 플랜에 셸 스크립트나 배치 파일을 통해 Graphviz를 설치하는 명령어를 실행해야 합니다.

다음은 간단한 과정을 설명합니다:

1. **Bamboo 플랜 생성**: Bamboo에서 새로운 빌드 플랜을 만듭니다.
 
2. **작업 추가**: 해당 플랜 내에 새 작업을 추가합니다.

3. **명령어 실행**: 작업 유형을 "Script"으로 선택하고, 서버에 Graphviz를 설치하기 위한 적절한 셸 명령어나 배치 파일 스크립트를 입력합니다.

    예를 들어, Ubuntu 또는 Debian 기반의 리눅스 시스템에서는:
    ```bash
    sudo apt-get update
    sudo apt-get install -y graphviz
    ```

4. **플랜 실행**: 플랜을 저장하고 실행하여 Graphviz가 성공적으로 설치되었는지 확인합니다.

이렇게 Bamboo를 사용하여 서버에 소프트웨어를 설치하는 것은 권장되지 않는 경우도 있습니다. 서버 구성 관리를 위한 도구 (예: Ansible, Puppet, Chef)를 사용하는 것이 관리 및 유지보수 측면에서 더 효과적일 수 있습니다.

이 접근 방식의 주요 장점은 Bamboo를 통해 CI/CD 파이프라인 내에서 필요한 소프트웨어를 자동으로 설치하여, 인프라와 응용 프로그램 배포를 한 곳에서 관리할 수 있다는 것입니다. 하지만 Bamboo가 중단되거나 문제가 발생하면 Graphviz 설치도 영향을 받을 수 있습니다. 따라서 중요한 서버에서는 이러한 방법을 사용하기 전에 모든 리스크를 잘 평가하십시오.

