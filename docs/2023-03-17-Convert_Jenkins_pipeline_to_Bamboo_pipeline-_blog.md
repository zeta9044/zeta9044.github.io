---
layout: post
title: "Convert Jenkins pipeline to Bamboo pipeline."
date: 2023-03-17
tags: [Bamboo, to, Convert, Jenkins, pipeline]
categories: blog
---

## 사용자 입력:
> //젠킨스 화면 매개변수 4개
//GIT_SOURCE

// git parameter plugin을 썼을 때, origin/develop에서 develop만 추출하도록 하는 로직
// 태그 패턴 구분
if (GIT_SOURCE ==~ /\d.\d.\d/) {
    println "TAG"
    TOMCAT_DEPLOY_PATH = '/home/lia/LIA_MASTER/LIAWAS/tomcat/webapps'
    TOMCAT_BIN_PATH = '/home/lia/LIA_MASTER/LIAWAS/tomcat/bin'
} else {
    println "BRANCH"
    if (GIT_SOURCE.startsWith("origin")) {
        GIT_SOURCE = GIT_SOURCE.replace("origin/", "")
    }

    if (GIT_SOURCE.startsWith("develop")) {
        TOMCAT_DEPLOY_PATH = '/home/lia/LIA_DEVELOP/LIAWAS/tomcat/webapps'
        TOMCAT_BIN_PATH = '/home/lia/LIA_DEVELOP/LIAWAS/tomcat/bin'
     } else if (GIT_SOURCE.startsWith("release")) {
        TOMCAT_DEPLOY_PATH = '/home/lia/LIA_RELEASE/LIAWAS/tomcat/webapps'
        TOMCAT_BIN_PATH = '/home/lia/LIA_RELEASE/LIAWAS/tomcat/bin'
    } else if (GIT_SOURCE.startsWith("hotfix")) {
        TOMCAT_DEPLOY_PATH = '/home/lia/LIA_RELEASE/LIAWAS/tomcat/webapps'
        TOMCAT_BIN_PATH = '/home/lia/LIA_RELEASE/LIAWAS/tomcat/bin'
    } else {
        TOMCAT_DEPLOY_PATH = '/home/lia/LIA_DEVELOP/LIAWAS/tomcat/webapps'
        TOMCAT_BIN_PATH = '/home/lia/LIA_DEVELOP/LIAWAS/tomcat/bin'
    }
}


// clone source
def cloneSource() {
    echo GIT_SOURCE
    return {
        if (GIT_SOURCE.startsWith("develop") || GIT_SOURCE.startsWith("release") || GIT_SOURCE.startsWith("feature") || GIT_SOURCE.startsWith("hotfix")) {
            stage("소스\n준비\n${GIT_SOURCE}") {
                // branch로 시작하는 개발 또는 스테이징일때
                echo "[git소스 준비]"
                checkout([$class: 'GitSCM', branches: [[name: "${GIT_SOURCE}"]], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: false, recursiveSubmodules: false, reference: '', trackingSubmodules: false]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'kangyou.choi', url: 'ssh://git@192.168.0.119:7999/qtss/qtrack.git']]])
            }
        } else {
            stage("소스\n준비\n${GIT_SOURCE}") {
                echo "[git소스 준비]"
                checkout([$class: 'GitSCM', branches: [[name: "refs/tags/${GIT_SOURCE}"]], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: false, recursiveSubmodules: false, reference: '', trackingSubmodules: false]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'kangyou.choi', url: 'ssh://git@192.168.0.119:7999/qtss/qtrack.git']]])
            }

        }
    }
}

// 환경설정
def setEnv() {
    return {
        stage('환경설정') {
            try {
                //application.yml설정
                echo "[application.yml 설정]"
                sh 'cp ./qtrack-web/config/application-server.yml ./qtrack-web/src/main/resources/application.yml'
            } catch (err) {
                echo err.getMessage()
                currentBuild.result = "FAILURE"
                throw new Exception("Throw to stop pipeline")
            }
        }
    }
}

// build ui
def buildForUi() {
    return {
        stage('ui빌드') {
            dir('./qtrack-web/src/main/js') {
                  withEnv(['PATH+NODEJS=/usr/local/bin']) {
                      try {
                          echo "[ui빌드]"
                          sh 'rm -rf ./qtrack-web/src/main/webapp/dist'
                          sh 'yarn'
                          sh 'yarn build'
                          sh '../../../../gradlew uploadArchives'
                      } catch (err) {
                          echo err.getMessage()
                          currentBuild.result = "FAILURE"
                          throw new Exception("Throw to stop pipeline")
                      }
                }
            }
        }
    }
}

// build all
def buildForAll() {
    return {
        stage('war빌드') {
            try {
                sh 'sh ./gradlew clean build -x test --refresh-dependencies'
            } catch (err) {
                echo err.getMessage()
                currentBuild.result = "FAILURE"
                throw new Exception("Throw to stop pipeline")
            }
        }
    }
}

// 배포
def deploy() {
    return {
        stage("배포") {
            try {
                echo TOMCAT_DEPLOY_PATH
                sh 'cp ./qtrack-web/build/libs/Qtrack*.war ' + TOMCAT_DEPLOY_PATH + '/qtrack.war'
            } catch (err) {
                echo err.getMessage()
                currentBuild.result = "FAILURE"
                throw new Exception("Throw to stop pipeline")
            }
        }
    }
}

// 중지
def stop() {
    return {
        stage("서버\n중지") {
            try {
                if (GIT_SOURCE ==~ /\d.\d.\d/) {
                    sh 'sudo ' + TOMCAT_BIN_PATH + '/shutdown.sh'
                } else {
                    sh TOMCAT_BIN_PATH + '/shutdown.sh'
                }
                echo "server shutdown finished."
            } catch (err) {
                echo err.getMessage()
                echo "ignore"
            }
        }
    }
}

// 시작
def start() {
    return {
        stage("서버\n시작") {
            try {
                keepRunning {
                    if (GIT_SOURCE ==~ /\d.\d.\d/) {
                        sh 'sudo ' + TOMCAT_BIN_PATH + '/startup.sh'
                    } else {
                        sh TOMCAT_BIN_PATH + '/startup.sh'
                    }
                }
                echo "server startup finished."
            } catch (err) {
                echo err.getMessage()
                currentBuild.result = "FAILURE"
                throw new Exception("Throw to stop pipeline")
            }
        }
    }
}


node {

    // 소스 준비
    // GIT_BRANCH는 젠킨스화면애서 받는 매개변수
    cloneSource().call()

    // 환경설정
    setEnv().call()

    //ui빌드
    buildForUi().call()

    // 빌드
    buildForAll().call()

    //배포
    deploy().call()

    //서버재시작
    stop().call()
    start().call()

    stage("완료") {
        echo "완료"
    }
} 이 스크립트를 bamboo로 전환해줘

## ChatGPT 응답:
> Bamboo는 Jenkins와 비슷한 기능을 가진 CI/CD 도구입니다. Bamboo에서도 파이프라인을 정의하여 작업을 진행할 수 있습니다. Jenkins에서 사용한 Groovy 스크립트를 Bamboo의 스크립트 작업으로 변환하기 위해, 기본 구조를 YAML 형식으로 변경하겠습니다.

```yaml
---
version: 2
plan:
  project-key: QT
  key: QTRACK
  name: QTrack

stages:
  - Source Preparation:
      - Clone Repository
  - Environment Setup:
      - Set Environment
  - UI Build:
      - Build UI
  - War Build:
      - Build War
  - Deployment:
      - Deploy
  - Server Control:
      - Stop Server
      - Start Server
  - Completion:
      - Finish

jobs:
  Clone Repository:
    tasks:
      - script:
          - git clone <repository_url>
          - git checkout ${bamboo.GIT_SOURCE}

  Set Environment:
    tasks:
      - script:
          - cp ./qtrack-web/config/application-server.yml ./qtrack-web/src/main/resources/application.yml

  Build UI:
    tasks:
      - script:
          - cd ./qtrack-web/src/main/js
          - yarn
          - yarn build
          - ../../../../gradlew uploadArchives

  Build War:
    tasks:
      - script:
          - ./gradlew clean build -x test --refresh-dependencies

  Deploy:
    tasks:
      - script:
          - cp ./qtrack-web/build/libs/Qtrack*.war ${bamboo.TOMCAT_DEPLOY_PATH}/qtrack.war

  Stop Server:
    tasks:
      - script:
          - ${bamboo.TOMCAT_BIN_PATH}/shutdown.sh

  Start Server:
    tasks:
      - script:
          - ${bamboo.TOMCAT_BIN_PATH}/startup.sh

  Finish:
    tasks:
      - script:
          - echo "완료"
```

위의 YAML 스크립트는 Bamboo의 기본 구조를 따르고 있습니다. 각 작업은 스테이지로 구분되어 있으며, 작업 내에는 스크립트 태스크가 포함되어 있습니다. 이 구조를 사용하여 Jenkins 파이프라인을 Bamboo에서 실행할 수 있습니다.

주의: 위의 예제에서는 일부 환경 변수와 경로가 변경되어야 할 수도 있습니다. 예를 들어, `<repository_url>`을 실제 Git 저장소 URL로 변경해야 합니다. 또한, Bamboo에서 사용되는 환경 변수를 정의하고 사용해야 할 수도 있습니다. 이를 적절하게 수정하여 작업을 진행해 주세요.

