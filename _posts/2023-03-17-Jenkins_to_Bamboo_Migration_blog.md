---
layout: post
title: "Jenkins to Bamboo Migration"
tags: [Jenkins, to, Bamboo, Migration]
---

## 사용자 입력:
> jenkins 빌드 작업을 bamboo로 전환하려는데 도와줄 수 있나?

## ChatGPT 응답:
> 물론이죠! Jenkins와 Bamboo 모두 CI/CD 도구이기 때문에 빌드 작업을 전환하는 것은 비교적 쉬운 작업입니다. 

다음은 Jenkins에서 Bamboo로 빌드 작업을 전환하는 일반적인 단계입니다.

1. Jenkins에서 사용하는 빌드 스크립트를 확인합니다.
2. Bamboo에서 해당 빌드 스크립트를 작성합니다. Bamboo에서는 스크립트를 Groovy, Java, Python 등 다양한 언어로 작성할 수 있습니다.
3. 빌드에 필요한 모든 파일과 라이브러리를 Bamboo로 이전합니다.
4. Jenkins에서 사용하는 모든 환경 변수, 빌드 매개 변수 및 세부 정보를 Bamboo로 이전합니다.
5. Bamboo에서 빌드를 실행하고 결과를 확인합니다.

이러한 단계를 수행하면 Jenkins에서 Bamboo로 빌드 작업을 성공적으로 전환할 수 있습니다. 

하지만 이러한 작업은 모든 경우에 쉽지는 않습니다. 각 도구에는 고유한 구조, 구성 및 기능이 있기 때문입니다. 따라서 빌드 작업을 전환하기 전에 모든 요구 사항을 이해하고 계획을 수립하는 것이 좋습니다. 또한 Jenkins와 Bamboo를 모두 경험한 개발자나 전문가의 지원을 받는 것이 좋습니다.

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
}  위 코드를 bamboo로 전환해줘

## ChatGPT 응답:
> 해당 Jenkins 파일을 Bamboo로 전환하려면 다음과 같은 작업을 수행할 수 있습니다.

1. Bamboo에서 새로운 Plan을 생성합니다.
2. Plan 설정에서 소스 코드 선택 후, Git 저장소 URL 및 자격 증명을 입력합니다.
3. Stage를 추가하고, "Script" 타입으로 변환한 뒤, 아래 스크립트를 붙여넣기합니다. 

{% raw %}``{% endraw %}`
// git parameter plugin을 썼을 때, origin/develop에서 develop만 추출하도록 하는 로직
// 태그 패턴 구분
if (bamboo.GIT_SOURCE =~ /\d.\d.\d/) {
    println "TAG"
    bamboo.TOMCAT_DEPLOY_PATH = '/home/lia/LIA_MASTER/LIAWAS/tomcat/webapps'
    bamboo.TOMCAT_BIN_PATH = '/home/lia/LIA_MASTER/LIAWAS/tomcat/bin'
} else {
    println "BRANCH"
    if (bamboo.GIT_SOURCE.startsWith("origin")) {
        bamboo.GIT_SOURCE = bamboo.GIT_SOURCE.replace("origin/", "")
    }

    if (bamboo.GIT_SOURCE.startsWith("develop")) {
        bamboo.TOMCAT_DEPLOY_PATH = '/home/lia/LIA_DEVELOP/LIAWAS/tomcat/webapps'
        bamboo.TOMCAT_BIN_PATH = '/home/lia/LIA_DEVELOP/LIAWAS/tomcat/bin'
     } else if (bamboo.GIT_SOURCE.startsWith("release")) {
        bamboo.TOMCAT_DEPLOY_PATH = '/home/lia/LIA_RELEASE/LIAWAS/tomcat/webapps'
        bamboo.TOMCAT_BIN_PATH = '/home/lia/LIA_RELEASE/LIAWAS/tomcat/bin'
    } else if (bamboo.GIT_SOURCE.startsWith("hotfix")) {
        bamboo.TOMCAT_DEPLOY_PATH = '/home/lia/LIA_RELEASE/LIAWAS/tomcat/webapps'
        bamboo.TOMCAT_BIN_PATH = '/home/lia/LIA_RELEASE/LIAWAS/tomcat/bin'
    } else {
        bamboo.TOMCAT_DEPLOY_PATH = '/home/lia/LIA_DEVELOP/LIAWAS/tomcat/webapps'
        bamboo.TOMCAT_BIN_PATH = '/home/lia/LIA_DEVELOP/LIAWAS/tomcat/bin'
    }
}

// clone source
def cloneSource() {
    echo bamboo.GIT_SOURCE
    return {
        if (bamboo.GIT_SOURCE.startsWith("develop") || bamboo.GIT_SOURCE.startsWith("release") || bamboo.GIT_SOURCE.startsWith("feature") || bamboo.GIT_SOURCE.startsWith("hotfix")) {
            stage("소스\n준비\n${bamboo.GIT_SOURCE}") {
                // branch로 시작하는 개발 또는 스테이징일때
                echo "[git소스 준비]"
                checkout([
                    bamboo.gitRepository(bamboo.GIT_SOURCE),
                    bamboo.submoduleCheckout(),
                ])
            }
        } else {
            stage("소스\n준비\n${bamboo.GIT_SOURCE}") {
                echo "[git소스 준비]"
                checkout([
                    bamboo.gitRepository('refs/tags/'+bamboo.GIT_SOURCE),
                    bamboo.submoduleCheckout(),
                ])
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
               

