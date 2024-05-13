---
layout: post
title: "Jenkins to Bamboo Conversion"
tags: [Jenkins, to, Bamboo, Conversion]
---

## 사용자 입력:
> jenkins 빌드 작업을 bamboo로 전환하려는데 도와줄 수 있나?

## ChatGPT 응답:
> 물론 도와드릴 수 있습니다. Jenkins에서 Bamboo로 빌드 작업을 전환하려면 다음 단계를 따라주세요.

1. Bamboo 설치 및 설정:
   Jenkins와 마찬가지로 Bamboo를 호스트할 서버에 설치해야 합니다. Atlassian 웹사이트에서 Bamboo 설치 파일을 다운로드 받고, 설치를 완료한 후 라이선스 키를 입력하고 Bamboo를 설정하세요.

2. Bamboo 사용자 및 권한 설정:
   Bamboo에서 프로젝트에 액세스할 수 있는 사용자와 그룹을 생성하고 권한을 설정하세요. Jenkins에서 사용하던 권한 설정을 참조하여 Bamboo에서 비슷한 권한 구조를 설정할 수 있습니다.

3. 프로젝트 및 저장소 설정:
   Bamboo에서 새로운 프로젝트를 생성하고 코드 저장소를 연결하세요. Jenkins에서 사용한 코드 저장소 설정을 참조하여 Bamboo에 동일한 저장소를 연결할 수 있습니다.

4. 빌드 계획 및 작업 구성:
   Bamboo에서 빌드 계획을 생성하고, Jenkins에서 사용하던 빌드 작업을 참조하여 계획 내에 작업을 구성하세요. 이 작업에는 다음과 같은 단계가 포함됩니다.

   - 소스 코드 체크아웃
   - 빌드 작업 실행 (예: Maven, Gradle 등)
   - 테스트 실행 및 결과 수집
   - 빌드 아티팩트 저장 및 배포

5. 트리거 및 빌드 환경 설정:
   빌드 계획에 트리거를 추가하여 Jenkins에서와 같은 방식으로 빌드를 자동화할 수 있습니다. 예를 들어, 코드 커밋 시 빌드를 시작하거나 일정 시간에 자동 빌드를 수행할 수 있습니다. 또한, 필요한 경우 여러 빌드 환경을 설정하여 서로 다른 환경에서 빌드를 실행할 수 있습니다.

6. 통합 및 알림 설정:
   Bamboo에서 통합 플러그인을 사용하여 Jira, Bitbucket 등의 다른 Atlassian 제품 및 서드파티 도구와 통합할 수 있습니다. 또한, 빌드 결과와 관련된 알림을 받기 위해 이메일, 슬랙 등의 알림 채널을 설정할 수 있습니다.

7. Jenkins에서 Bamboo로 이전:
   모든 설정이 완료되면, Jenkins에서 빌드 작업을 중지하고 Bamboo에서 빌드를 시작하세요. Bamboo 빌드가 정상적으로 작동하는지 확인한 후, Jenkins를 완전히 중단할 수 있습니다.

이 단계들을

## 사용자 입력:
> bamboo서버와 배포할 서버가 달라도 되나?

## ChatGPT 응답:
> 네, Bamboo 서버와 배포 대상 서버가 서로 다른 서버여도 문제가 없습니다. 실제로 대부분의 경우, CI/CD 도구와 배포 대상 서버는 별도의 서버에 위치하게 됩니다. Bamboo에서 원격 서버에 빌드 아티팩트를 배포하는 방법은 다음과 같습니다.

1. SSH 키 생성 및 배포 대상 서버에 등록:
   배포 대상 서버에 접근하기 위해 SSH 키를 생성해야 합니다. 생성된 공개 키를 배포 대상 서버의 {% raw %}`authorized_keys`{% endraw %} 파일에 추가하세요. 이렇게 하면 Bamboo 서버가 배포 대상 서버에 안전하게 접근할 수 있습니다.

2. Bamboo에서 배포 프로젝트 생성:
   Bamboo에서 배포 프로젝트를 생성하고 연관된 빌드 프로젝트를 선택하세요. 이렇게 하면 빌드 프로젝트의 결과물을 배포 프로젝트에서 사용할 수 있습니다.

3. 배포 환경 생성 및 구성:
   배포 프로젝트 내에서 배포 환경을 생성하고 구성하세요. 배포 환경은 일반적으로 개발, 스테이징, 프로덕션 등과 같은 다양한 환경을 나타냅니다.

4. 배포 작업 추가:
   배포 환경에 배포 작업을 추가하고 필요한 스크립트와 설정을 구성하세요. 배포 작업에는 다음과 같은 작업이 포함될 수 있습니다.

   - 원격 서버로 아티팩트 전송
   - 원격 서버에서 필요한 서비스 재시작
   - 원격 서버에서 데이터베이스 마이그레이션 수행
   - 원격 서버에서 테스트 및 검증 작업 실행

5. 배포 트리거 설정:
   빌드 프로젝트와 연동하여 배포를 자동화하거나 수동으로 시작할 수 있도록 배포 트리거를 설정하세요. 예를 들어, 빌드가 성공적으로 완료되면 자동으로 배포를 시작하거나, 수동으로 배포를 시작하는 등의 설정을 할 수 있습니다.

이 과정을 완료하면 Bamboo 서버와 별도의 서버에 배포하는 것이 가능해집니다.

## 사용자 입력:
> Bamboo specs가 뭐야?

## ChatGPT 응답:
> Bamboo Specs은 Bamboo의 설정과 프로젝트를 코드 형식으로 관리할 수 있는 기능입니다. 이를 사용하면, 프로젝트 설정을 버전 관리 시스템에 저장하고, 변경 사항을 추적하며 협업을 더 쉽게 할 수 있습니다. Bamboo Specs는 Java와 YAML 두 가지 형식을 지원합니다.

Bamboo Specs를 사용하면 다음과 같은 이점이 있습니다:

1. 버전 관리: 설정을 코드 형식으로 관리하면 변경 사항을 추적하고 버전 관리 시스템에서 관리할 수 있습니다. 이를 통해 팀원들이 설정 변경 사항을 검토하고 문제를 신속하게 해결할 수 있습니다.

2. 협업 개선: 설정을 코드로 관리하면 프로젝트 설정에 대한 협업이 더욱 쉬워집니다. 팀원들은 설정 변경 사항에 대해 토론하고, 코드 리뷰를 통해 최선의 구성을 결정할 수 있습니다.

3. 자동화 및 일관성: Bamboo Specs을 사용하면 프로젝트 설정을 자동화하고 일관성을 유지할 수 있습니다. 또한, 새로운 프로젝트를 생성할 때 기존 프로젝트의 설정을 쉽게 복사하거나 재사용할 수 있습니다.

4. 오류 감소: 코드 형식으로 설정을 관리하면 구성 오류를 줄일 수 있습니다. 이는 Bamboo에서 설정을 검증하고 적용하기 전에 버전 관리 시스템에서 검토할 수 있기 때문입니다.

Bamboo Specs를 사용하려면 다음 단계를 따라주세요:

1. 코드 저장소 생성: Bamboo Specs 파일을 저장할 코드 저장소를 생성하거나 기존 저장소를 사용합니다.

2. Bamboo Specs 파일 생성: 프로젝트 설정을 저장할 Java 또는 YAML 파일을 작성합니다. Bamboo에서 제공하는 도구와 예제를 사용하여 파일을 작성할 수 있습니다.

3. 저장소 연결: Bamboo와 코드 저장소를 연결하고 Bamboo Specs 파일이 저장된 디렉토리를 설정합니다.

4. Bamboo Specs 실행: Bamboo에서 Specs 파일을 실행하여 프로젝트 설정을 적용하고, 변경 사항이 있을 때마다 Bamboo에서 자동으로 이를 감지하고 적용하도록 설정합니다.

이제 Bamboo Specs을 사용하여 프로젝트 설정을 코드로 관리하고 협업을 쉽게 할 수 있습니다.

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
} 이 jenkins pipeline script를 bamboo로 전환해줘. 배포도 포함되어 있는데 분리해줘

## ChatGPT 응답:
> Jenkins 파이프라인 스크립트를 Bamboo 설정으로 전환하려면, 먼저 Bamboo 빌드 계획과 배포 프로젝트를 생성해야 합니다. 아래에 Bamboo 설정을 위한 주요 단계를 설명합니다.

1. Bamboo에서 새로운 빌드 계획을 생성하십시오.
2. 소스 저장소를 연결하고 Git 저장소를 설정합니다.
3. 아래 코드를 참조하여 빌드 계획의 각 단계와 작업을 구성하십시오.

빌드 계획 구성:

- **스테이지 1: 소스 준비**
    - 작업 1: 소스 코드 체크아웃 (Git 저장소 설정 사용)

- **스테이지 2: 환경 설정**
    - 작업 1: Script 작업 (아래 스크립트 사용)
{% raw %}```bash
cp ./qtrack-web/config/application-server.yml ./qtrack-web/src/main/resources/application.yml
```{% endraw %}

- **스테이지 3: UI 빌드**
    - 작업 1: Script 작업 (아래 스크립트 사용)
{% raw %}```bash
cd ./qtrack-web/src/main/js
yarn
yarn build
cd ../../../../
./gradlew uploadArchives
```{% endraw %}

- **스테이지 4: WAR 빌드**
    - 작업 1: Script 작업 (아래 스크립트 사용)
{% raw %}```bash
./gradlew clean build -x test --refresh-dependencies
```{% endraw %}

이제 배포 프로젝트를 생성하고 배포 작업을 분리하십시오.

1. Bamboo에서 새로운 배포 프로젝트를 생성합니다.
2. 빌드 프로젝트와 연결하십시오.
3. 새 배포 환경을 생성하고 필요한 배포 작업을 추가하십시오.

배포 작업 구성:

- **배포 작업 1: WAR 파일 복사**
    - 작업 1: Artifact Download 작업 (WAR 파일 다운로드)
    - 작업 2: SCP 작업 또는 SSH 작업 (WAR 파일을 원격 서버로 복사)

- **배포 작업 2: 서버 중지**
    - 작업 1: SSH 작업 (아래 스크립트 사용)
{% raw %}```bash
${TOMCAT_BIN_PATH}/shutdown.sh
```{% endraw %}

- **배포 작업 3: 서버 시작**
    - 작업 1: SSH 작업 (아래 스크립트 사용)
{% raw %}```bash
${TOMCAT_BIN_PATH}/startup.sh
```{% endraw %}

Jenkins 파이프라인 스크립트가 Bamboo로 전환되었습니다. 이제 Bamboo에서 빌드 계획을 실행하고 배포 프로젝트를 사용하여 원격

## 사용자 입력:
> 17-3월-2023 17:48:54	'/data1/bamboo-home/xml-data/build-dir/QTRAC-DEV-CHKO/jgraphx'에 복제합니다...
17-3월-2023 17:48:54	Warning: Permanently added '[192.168.0.119]:7999' (RSA) to the list of known hosts.
17-3월-2023 17:48:54	Repository not found
17-3월-2023 17:48:54	The requested repository does not exist, or you do not have permission to access it.

## ChatGPT 응답:
> 위의 오류 메시지는 Git 저장소를 찾을 수 없거나, 접근 권한이 없음을 나타냅니다. 이 문제를 해결하려면 다음 단계를 수행해 주세요.

1. Git 저장소 URL 확인: 저장소 URL이 정확한지 확인하십시오. Bamboo 설정에서 저장소 URL이 올바르게 구성되어 있는지 확인합니다. 저장소 URL은 일반적으로 다음 형식 중 하나를 사용합니다.

   {% raw %}```
   https://username@bitbucket.org/username/repository.git
   ssh://git@bitbucket.org/username/repository.git
   ```{% endraw %}

2. 권한 확인: 현재 Bamboo 사용자 또는 Bamboo 서버가 Git 저장소에 대한 액세스 권한이 있는지 확인하십시오. Git 서비스 (예: GitHub, GitLab, Bitbucket 등)에서 해당 사용자 또는 서버에 대한 액세스 권한을 부여해야 할 수 있습니다.

3. 인증 정보 확인: Bamboo 설정에서 올바른 인증 정보가 사용되고 있는지 확인하십시오. 사용자 이름과 암호 또는 SSH 키를 사용하여 인증 정보를 설정할 수 있습니다. 또한, 저장소 URL이 올바른 인증 방법을 사용하고 있는지 확인하십시오 (예: {% raw %}`https`{% endraw %} 또는 {% raw %}`ssh`{% endraw %}).

4. SSH 키 확인: SSH를 사용하여 저장소에 연결하는 경우, Bamboo 서버의 SSH 키가 저장소에 추가되어 있는지 확인하십시오. 또한, SSH 호스트 지문이 변경되지 않았는지 확인하십시오. 변경된 경우, SSH 호스트 지문을 새로운 지문으로 업데이트해야 합니다.

위의 단계를 모두 확인한 후에도 문제가 해결되지 않는 경우, 추가 지원을 위해 Git 서비스 제공자에 문의하십시오.

## 사용자 입력:
> A new public key for the selected repository will be stored in your Bitbucket Server user profile because you don't have administrative permissions.

## ChatGPT 응답:
> 이 메시지는 선택한 저장소에 대한 새로운 공개 키가 Bitbucket Server 사용자 프로필에 저장될 것임을 나타냅니다. 이는 귀하가 관리 권한이 없기 때문에 발생합니다.

이 문제를 해결하려면 다음 단계를 수행해야 합니다.

1. 공개 키 등록: Bitbucket Server 사용자 프로필에 공개 키를 추가하십시오. 이렇게 하면 귀하의 계정이 Git 저장소에 대한 액세스 권한을 얻게 됩니다. 사용자 설정에서 SSH 키를 관리하는 옵션을 찾아 공개 키를 추가해야 합니다.

2. 관리자에게 문의: 관리자 권한이 필요한 경우 조직의 Git 관리자에게 문의하여 저장소 또는 프로젝트 수준에서 액세스 권한을 부여해야 할 수도 있습니다. 관리자가 귀하의 계정에 대한 액세스 권한을 조정할 수 있습니다.

공개 키를 등록하고 관리자에게 문의한 후에도 문제가 해결되지 않는 경우, 추가 지원을 위해 Bitbucket Server 관리자 또는 Git 서비스 제공자에 문의하십시오.

## 사용자 입력:
> bamboo에서 yarn build하려면?

## ChatGPT 응답:
> Bamboo에서 {% raw %}`yarn build`{% endraw %}를 실행하려면 다음 단계를 수행해야 합니다.

1. Node.js와 Yarn 설치: 먼저 Bamboo 서버에 Node.js와 Yarn이 설치되어 있는지 확인합니다. 설치되어 있지 않다면, [Node.js 공식 사이트](https://nodejs.org/)에서 Node.js를 다운로드 및 설치하고, [Yarn 공식 사이트](https://yarnpkg.com/)에서 Yarn을 다운로드 및 설치하세요.

2. Bamboo 빌드 계획 설정: Bamboo에서 빌드 계획을 생성하거나 기존 빌드 계획을 수정합니다. 빌드 계획에 대한 설정이 필요합니다.

3. Node.js 및 Yarn을 사용하는 작업 추가: 빌드 계획에서 작업을 추가하고, 작업 유형으로 "Node.js"를 선택합니다. 작업 이름과 설명을 입력한 후 "Node.js Executable" 필드에서 설치된 Node.js 실행 파일을 선택하거나 경로를 입력합니다.

4. 스크립트 작성: 스크립트 작성 영역에 다음 명령을 입력합니다.

   {% raw %}```
   yarn install
   yarn build
   ```{% endraw %}

   {% raw %}`yarn install`{% endraw %} 명령은 프로젝트에 필요한 모든 종속성을 설치하는 데 사용되며, {% raw %}`yarn build`{% endraw %} 명령은 프로젝트를 빌드하는 데 사용됩니다.

5. 작업 저장 및 빌드 계획 실행: 작업을 저장한 후 빌드 계획을 실행하여 {% raw %}`yarn build`{% endraw %}가 제대로 작동하는지 확인합니다.

위의 단계를 완료하면 Bamboo에서 {% raw %}`yarn build`{% endraw %}를 사용하여 프로젝트를 빌드할 수 있습니다.

