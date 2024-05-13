---
layout: post
title: "Jenkins와 Node.js"
date: 2023-03-14
tags: [Node, js]
categories: blog
---

## 사용자 입력:
> jenkins 구동하는데 node.js가 필요한가?

## ChatGPT 응답:
> Jenkins는 Java로 작성된 오픈 소스 지속적인 통합(CI) 및 지속적인 배포(CD) 도구입니다. 따라서 Jenkins 자체는 Node.js를 실행하기 위해 필요하지 않습니다.

그러나 Jenkins 플러그인 중 일부는 Node.js를 사용합니다. 예를 들어, Jenkins에서 JavaScript 프로젝트를 빌드하려면 Node.js 설치가 필요할 수 있습니다. Jenkins에서 Node.js를 사용하는 경우, Node.js 플러그인을 설치해야 할 수도 있습니다.

따라서 Jenkins를 사용하는데 Node.js가 필요한지 여부는 사용자의 프로젝트 요구 사항과 사용 중인 플러그인에 따라 다릅니다.

## 사용자 입력:
> pipe line은  node.js가 필요한가?

## ChatGPT 응답:
> Jenkins Pipeline은 Jenkins에서 지원하는 지속적인 통합(CI) 및 지속적인 배포(CD) 파이프라인을 정의하고 실행하는 방법을 제공하는 도구입니다. 이것은 Groovy 기반의 DSL(Domain Specific Language)을 사용하여 구성됩니다.

Pipeline은 일반적으로 빌드, 테스트, 패키징, 배포 등의 일련의 단계를 실행하고 그 결과를 분석하며 이를 자동화하는 작업 흐름을 정의합니다.

따라서 Pipeline을 사용하려면 Node.js가 필수적이지는 않습니다. 그러나 Pipeline에서 Node.js를 사용하려면 Node.js를 실행할 수 있는 환경이 필요할 수 있습니다. 예를 들어, 프론트엔드 웹 애플리케이션을 빌드하려면 Node.js와 npm(Node Package Manager)이 필요합니다. 이 경우 Pipeline에서 Node.js를 사용하기 위해 Node.js 플러그인을 설치해야 할 수 있습니다.

결론적으로, Pipeline에서 Node.js가 필요한지 여부는 사용자의 프로젝트 요구 사항에 따라 다릅니다.

## 사용자 입력:
> pipline에서 node.js가 필요없는데.

## ChatGPT 응답:
> 예, Jenkins Pipeline에서 Node.js가 필요하지 않을 수 있습니다. Pipeline은 다양한 언어와 프레임워크를 지원하며, 이를 사용하여 빌드, 테스트, 패키징, 배포 등의 작업을 자동화 할 수 있습니다.

예를 들어, Java 언어를 사용하여 작성된 웹 애플리케이션을 빌드하고 테스트하려면 Maven 또는 Gradle과 같은 Java 빌드 도구를 사용할 수 있습니다. 이 경우 Node.js가 필요하지 않습니다.

또한, Python, Ruby, Go, C# 등의 언어를 사용하여 작성된 애플리케이션도 Pipeline에서 지원됩니다. 이러한 경우에도 Node.js는 필요하지 않습니다.

따라서 Jenkins Pipeline에서 Node.js가 필요한지 여부는 사용자의 프로젝트 요구 사항에 따라 다르며, 필요에 따라 필요한 도구를 설치하여 사용하면 됩니다.

## 사용자 입력:
> jenkins에서 node.js가 필요한 플러그인 알려줘

## ChatGPT 응답:
> Jenkins에서 Node.js가 필요한 플러그인 중 일부는 다음과 같습니다.

1. NodeJS Plugin: 이 플러그인을 사용하면 Jenkins에서 Node.js를 설치하고 사용할 수 있습니다. Node.js를 설치하면 npm(Node Package Manager)도 함께 설치됩니다.

2. npm Plugin: 이 플러그인을 사용하면 Jenkins에서 Node.js 프로젝트를 빌드 할 때 npm을 실행할 수 있습니다. 이를 사용하여 프로젝트에 필요한 Node.js 패키지를 설치하거나 업데이트할 수 있습니다.

3. Grunt Plugin: 이 플러그인을 사용하면 Jenkins에서 Grunt 빌드를 실행할 수 있습니다. Grunt는 JavaScript 프로젝트의 빌드 자동화를 위한 Task Runner입니다.

4. Gulp Plugin: 이 플러그인을 사용하면 Jenkins에서 Gulp 빌드를 실행할 수 있습니다. Gulp는 JavaScript 프로젝트의 빌드 자동화를 위한 Task Runner입니다.

5. Karma Plugin: 이 플러그인을 사용하면 Jenkins에서 Karma를 실행할 수 있습니다. Karma는 JavaScript 프로젝트의 유닛 테스트를 실행하는 도구입니다.

6. Protractor Plugin: 이 플러그인을 사용하면 Jenkins에서 Protractor를 실행할 수 있습니다. Protractor는 AngularJS 웹 애플리케이션의 E2E(end-to-end) 테스트를 수행하는 도구입니다.

이 외에도 Jenkins에서 Node.js가 필요한 다양한 플러그인이 있을 수 있습니다. 프로젝트 요구 사항에 맞게 필요한 플러그인을 선택하여 사용하시면 됩니다.

## 사용자 입력:
> jenkins에서 npm plugin을 실행할 때, node.js버전을 특정할 수 있나?

## ChatGPT 응답:
> 네, Jenkins에서 npm Plugin을 실행할 때 특정 Node.js 버전을 사용할 수 있습니다.

NodeJS Plugin을 사용하여 Jenkins에 여러 버전의 Node.js를 설치한 경우, npm Plugin은 Jenkins의 Global Tool Configuration 설정에서 특정 Node.js 버전을 선택하여 사용할 수 있습니다.

1. Jenkins 대시보드에서 'Manage Jenkins'를 클릭하고 'Global Tool Configuration'을 선택합니다.
2. 'NodeJS installations' 항목에서 사용 가능한 Node.js 버전을 확인합니다.
3. 'Jenkins Location' 항목에서 'NodeJS' 설치 루트 디렉토리를 설정합니다.
4. Jenkins 파이프라인에서 npm Plugin을 사용할 때, 'withNpm' 블록 안에 'nodejs' 옵션을 사용하여 사용할 Node.js 버전을 선택합니다.

예를 들어, 다음과 같이 'withNpm' 블록을 사용하여 Node.js 버전 14.x를 사용하는 경우:

```
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        withNpm(nodejs: '14.x') {
          sh 'npm install'
          sh 'npm run build'
        }
      }
    }
  }
}
```

이렇게 하면 Jenkins 파이프라인에서 npm Plugin을 실행할 때 특정 Node.js 버전을 선택할 수 있습니다.

## 사용자 입력:
> 그러면 yarn build를 pipe line으로 실행 시킬때, node.js가 필요한가?

## ChatGPT 응답:
> 네, yarn build를 Jenkins Pipeline으로 실행할 때에도 Node.js가 필요합니다. 

yarn은 JavaScript 프로젝트의 패키지 매니저이며, Node.js 환경에서 실행됩니다. 따라서 Jenkins에서 yarn build를 실행하려면 Jenkins 에이전트 또는 빌드 서버에 Node.js가 설치되어 있어야 합니다.

Node.js가 설치되어 있는 경우, Pipeline에서 Node.js를 사용하여 yarn을 실행할 수 있습니다. 예를 들어, 다음과 같이 withNodeJS 블록을 사용하여 Node.js 버전을 지정하고 yarn build 명령을 실행할 수 있습니다.

```
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        withNodeJS('14.x') {
          sh 'yarn install'
          sh 'yarn build'
        }
      }
    }
  }
}
```

위의 예제에서 'withNodeJS' 블록을 사용하여 Node.js 버전 14.x를 선택하고, 'sh' 스텝을 사용하여 yarn 명령어를 실행합니다. 따라서 Jenkins Pipeline에서 yarn build를 실행하려면 Node.js가 필요하며, Node.js가 설치된 빌드 에이전트를 사용해야 합니다.

## 사용자 입력:
> 
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
}

## ChatGPT 응답:
> 이 파이프라인 코드에서는 명시적으로 Node.js를 사용하는 부분이 없습니다. 하지만 파이프라인에서 빌드 및 배포 작업에 필요한 도구 및 명령어가 Node.js와 관련된 것일 수 있습니다.

예를 들어, 'buildForUi' 또는 'buildForAll' 단계에서 JavaScript, TypeScript 또는 Vue.js와 같은 웹 프레임워크를 사용하는 경우, 이러한 프레임워크에서 Node.js를 사용하여 프로젝트를 빌드할 수 있습니다. 또한, 'deploy' 단계에서 배포에 사용되는 도구나 라이브러리가 Node.js 기반이 될 수 있습니다.

따라서 파이프라인에서 Node.js가 필요한지 여부는 사용하는 도구, 라이브러리 또는 프레임워크에 따라 다르며, 빌드 에이전트에 Node.js가 설치되어 있는지 확인해야 합니다. Node.js가 필요한 경우, Node.js Plugin을 사용하여 Jenkins에서 Node.js를 설치하고 사용할 수 있습니다.

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
                checkout([$class: 'GitSCM', branches: [[name: "${GIT_SOURCE}"]], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: false, recursiveSubmodules: false, reference: '', trackingSubmodules: false]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0c2a8823-cc56-4cc7-9068-f487d2ae078b', url: 'ssh://git@192.168.0.119:7999/qtss/qtrack.git']]])
            }
        } else {
            stage("소스\n준비\n${GIT_SOURCE}") {
                echo "[git소스 준비]"
                checkout([$class: 'GitSCM', branches: [[name: "refs/tags/${GIT_SOURCE}"]], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: false, recursiveSubmodules: false, reference: '', trackingSubmodules: false]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0c2a8823-cc56-4cc7-9068-f487d2ae078b', url: 'ssh://git@192.168.0.119:7999/qtss/qtrack.git']]])
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

// build all
def buildForUi() {
    return {
        stage('ui빌드') {
            dir('./qtrack-web/src/main/js') {
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
}

## ChatGPT 응답:
> 이 파이프라인 코드에서는 Node.js를 명시적으로 사용하는 부분이 없습니다. 하지만 'buildForUi' 단계에서 yarn을 사용하여 UI를 빌드하고 있습니다. 따라서 Jenkins 빌드 에이전트에 Node.js가 설치되어 있어야 하며, yarn을 사용할 수 있어야 합니다.

파이프라인에서 Node.js 버전을 지정하지 않은 경우, Jenkins의 NodeJS Plugin에서 설정한 기본 Node.js 버전을 사용합니다. NodeJS Plugin을 사용하여 Jenkins에 Node.js를 설치하고 버전을 지정하면, 파이프라인에서 Node.js 버전을 지정하지 않아도 지정된 버전이 사용됩니다.

따라서 Node.js 및 yarn이 빌드 에이전트에 설치되어 있고, Jenkins에서 NodeJS Plugin을 사용하여 Node.js를 설치하고 지정한 경우, 이 파이프라인 코드에서는 문제 없이 yarn을 사용할 수 있습니다.

## 사용자 입력:
> docker에서 container 디렉토리를 host에 매핑하는 식 보여줘

## ChatGPT 응답:
> Docker 컨테이너 디렉토리를 호스트에 매핑하는 방법은 다음과 같습니다.

```
docker run -v /호스트/디렉토리:/컨테이너/디렉토리 이미지명
```

위의 명령어에서 `-v` 옵션을 사용하여 호스트의 디렉토리와 컨테이너의 디렉토리를 매핑합니다. `/호스트/디렉토리`는 호스트의 디렉토리 경로를, `/컨테이너/디렉토리`는 컨테이너의 디렉토리 경로를 나타냅니다.

예를 들어, 다음과 같이 호스트의 `/home/user/data` 디렉토리를 컨테이너의 `/data` 디렉토리와 매핑하는 경우:

```
docker run -v /home/user/data:/data ubuntu
```

이렇게 하면 호스트의 `/home/user/data` 디렉토리가 컨테이너의 `/data` 디렉토리와 매핑되며, 컨테이너에서 `/data` 디렉토리를 사용하여 호스트의 `/home/user/data` 디렉토리를 읽고 쓸 수 있습니다.

또한, `docker-compose.yml` 파일에서도 볼륨 매핑을 설정할 수 있습니다. 예를 들어, 다음과 같이 `docker-compose.yml` 파일에서 `volumes` 항목을 사용하여 호스트의 디렉토리와 컨테이너의 디렉토리를 매핑할 수 있습니다.

```
version: '3'
services:
  app:
    image: nginx
    volumes:
      - /home/user/data:/data
```

이렇게 하면 `docker-compose up` 명령을 실행할 때 호스트의 `/home/user/data` 디렉토리가 컨테이너의 `/data` 디렉토리와 매핑되며, 컨테이너에서 `/data` 디렉토리를 사용하여 호스트의 `/home/user/data` 디렉토리를 읽고 쓸 수 있습니다.

