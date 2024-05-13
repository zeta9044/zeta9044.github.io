---
layout: post
title: "Docker 컨테이너 삭제의 영향"
tags: [영향, 삭제, 컨테이너, Docker]
---

## 사용자 입력:
> docker container를 삭제하면 host에 매핑된 디렉토리도 없어지나?

## ChatGPT 응답:
> 

기본적으로 Docker 컨테이너를 삭제하더라도 호스트 시스템에 매핑된 디렉토리는 그대로 남아있습니다. 즉, 호스트 시스템의 파일 시스템에 있는 파일은 컨테이너가 삭제되더라도 그대로 유지됩니다.

하지만 예외적인 경우도 있습니다. 

- 첫째, 동일한 이름을 가진 볼륨을 공유하는 여러 컨테이너가 있을 경우, 모든 컨테이너를 삭제하기 전에 볼륨을 제거해야합니다. 그렇지 않으면 볼륨이 여전히 호스트 시스템에 남아있을 수 있습니다.

- 둘째, 컨테이너 내부에서 작성한 파일 중에 호스트 시스템과 공유되는 파일이 있을 경우, 이 파일은 컨테이너가 삭제될 때 함께 삭제됩니다. 이는 컨테이너가 종료되면 해당 파일이 더 이상 필요하지 않기 때문입니다.

따라서, 컨테이너를 삭제하기 전에 컨테이너와 공유된 볼륨이나 파일이 있는지 확인하는 것이 좋습니다.

## 사용자 입력:
> Docker 재시작할 때, 이전의 host볼륨을 매핑했을 때, host볼륨이 container 내용으로 초기화되나?

## ChatGPT 응답:
> Docker는 컨테이너가 다시 시작될 때 이전에 사용한 볼륨을 그대로 유지합니다. 따라서 이전의 호스트 볼륨과의 매핑을 사용하여 컨테이너를 다시 시작하면 해당 볼륨에 저장된 데이터는 그대로 유지됩니다.

만약 컨테이너를 삭제하고 다시 생성한다면 볼륨이 초기화될 수 있습니다. 이때는 이전 데이터를 복원하려면 백업과 같은 추가 작업이 필요합니다.

추가로, 볼륨에 저장된 데이터가 덮어쓰이지 않도록 주의해야 합니다. 컨테이너에서 데이터를 수정하거나 삭제할 때에도 호스트 시스템에서도 동일한 작업이 수행되므로, 예상치 못한 결과가 발생할 수 있습니다. 따라서 볼륨을 사용할 때에는 권한 관리 및 데이터 무결성을 유지하기 위한 적절한 조치를 취하는 것이 좋습니다.

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
} 이 스크립트는 jenkins declaretive 버전이 얼마인가?

## ChatGPT 응답:
> 이 스크립트는 Jenkins Declarative Pipeline 문법을 사용하고 있습니다. 하지만 구체적인 Declarative Pipeline 버전은 스크립트에 포함되어 있지 않으므로 확인할 수 없습니다. 

Declarative Pipeline은 Jenkins 2.0 이후에 추가된 새로운 파이프라인 문법으로, 이전의 Scripted Pipeline과는 문법적으로 다릅니다. 따라서, 이 스크립트는 Jenkins 2.0 이상에서 실행될 수 있습니다.

## 사용자 입력:
> 위 코드가 jenkins declarative pipeline 1.8에서 동작할 수 있는가?

## ChatGPT 응답:
> Jenkins Declarative Pipeline 1.8은 존재하지 않습니다. 그러나 Jenkins 2.x 이상 버전에서 지원되는 Declarative Pipeline의 최신 버전인 1.8.x에서 이 스크립트는 동작할 수 있습니다.

Declarative Pipeline은 Jenkins 2.0 이후에 추가된 새로운 파이프라인 문법입니다. 따라서, Jenkins 2.x 이상 버전에서 이 스크립트가 정상적으로 실행됩니다. 그러나, 이 스크립트가 Jenkins 1.x 버전에서는 실행되지 않을 것입니다. 

따라서, 이 스크립트를 사용하려면 Jenkins 2.x 이상 버전을 사용해야 합니다.

## 사용자 입력:
> docker로 실행된 jenkins에서 host의 디렉토리나 명령을 접근하려면 방법이 없나?

## ChatGPT 응답:
> Docker로 실행된 Jenkins 컨테이너는 독립적인 환경으로 실행되기 때문에 호스트 시스템의 디렉토리나 명령을 직접적으로 접근할 수는 없습니다.

하지만, Docker의 Volume 기능을 사용하여 호스트 시스템의 디렉토리를 컨테이너 내부에 마운트할 수 있습니다. 이를 통해 컨테이너에서 호스트 시스템의 파일 시스템에 직접적으로 접근할 수 있습니다.

또한, Docker로 실행된 컨테이너 내부에서 호스트 시스템의 명령을 실행하려면, 호스트 시스템에서 실행되는 Docker CLI를 사용하여 컨테이너 내부로 명령을 전달할 수 있습니다. 이를 위해서는 Docker CLI가 호스트 시스템에서 실행되고 있어야 하며, 컨테이너 내부에서 사용되는 Docker 이미지가 호스트 시스템과 동일한 Docker 버전을 사용하고 있어야 합니다. 

하지만 이러한 방식은 보안상의 이유로 권장되지 않습니다. 대신, Jenkins 컨테이너 내에서 필요한 명령이나 작업을 수행할 수 있는 적절한 도구를 설치하고 사용하는 것이 좋습니다.

## 사용자 입력:
> job을 수행시킬려면 host의 내용을 실행시키는 건데, 방법이 없나?

## ChatGPT 응답:
> Jenkins Job에서 호스트 시스템의 명령을 실행하는 것은 보안상의 이유로 권장되지 않습니다. 대신, Jenkins 컨테이너 내에서 필요한 명령이나 작업을 수행할 수 있는 적절한 도구를 설치하고 사용하는 것이 좋습니다.

그러나, Jenkins에서 호스트 시스템의 명령을 실행해야 하는 경우에는 Docker Volume 기능을 사용하여 호스트 시스템의 디렉토리를 컨테이너 내부에 마운트하고, 그 디렉토리에 필요한 명령 파일이나 스크립트를 저장하여 Jenkins Job에서 이를 실행할 수 있습니다.

예를 들어, 호스트 시스템의 {% raw %}`/usr/local/bin`{% endraw %} 디렉토리를 컨테이너 내부의 {% raw %}`/host/usr/local/bin`{% endraw %} 디렉토리에 마운트하고, {% raw %}`my-script.sh`{% endraw %} 파일을 {% raw %}`/host/usr/local/bin`{% endraw %} 디렉토리에 저장하였다면, Jenkins Job에서는 다음과 같이 해당 스크립트를 실행할 수 있습니다.

{% raw %}```
sh '/host/usr/local/bin/my-script.sh'
```{% endraw %}

하지만, 이러한 방식은 보안상의 이유로 권장되지 않습니다. 대신, Jenkins 컨테이너 내에서 필요한 명령이나 작업을 수행할 수 있는 적절한 도구를 설치하고 사용하는 것이 좋습니다.

## 사용자 입력:
> docker의 Jenkins Job에서 호스트 시스템을 실행할 수 있는 방법은 없나?

## ChatGPT 응답:
> Jenkins Job에서 Docker 컨테이너 외부에 있는 호스트 시스템의 명령을 실행하는 것은 권장되지 않습니다. 이는 보안상의 이유와 Docker의 컨테이너화된 환경에서의 호스트 시스템과의 격리를 보장하기 위함입니다.

그러나, 호스트 시스템에서 Docker CLI를 통해 Jenkins 컨테이너 내부로 명령을 전달하고 실행하는 방법이 있습니다. 이를 위해서는 호스트 시스템에 Docker CLI를 설치하고, Jenkins 컨테이너를 실행할 때 호스트 시스템의 Docker 소켓을 마운트하면 됩니다.

다음은 호스트 시스템에서 Docker CLI를 사용하여 Jenkins 컨테이너 내부에서 Docker 명령을 실행하는 방법의 예시입니다.

1. 호스트 시스템에 Docker CLI를 설치합니다.

2. Jenkins 컨테이너를 실행할 때 호스트 시스템의 Docker 소켓을 마운트합니다.

   {% raw %}```
   docker run -d -v /var/run/docker.sock:/var/run/docker.sock jenkins/jenkins
   ```{% endraw %}

3. Jenkins Job에서 {% raw %}`sh`{% endraw %} 스크립트를 사용하여 Docker CLI를 사용하여 호스트 시스템에서 Docker 명령을 실행합니다.

   {% raw %}```
   sh 'docker ps'
   ```{% endraw %}

그러나, 이러한 방식은 보안상의 이유로 권장되지 않습니다. 대신, Jenkins 컨테이너 내에서 필요한 명령이나 작업을 수행할 수 있는 적절한 도구를 설치하고 사용하는 것이 좋습니다.

## 사용자 입력:
> centos에서 service 설정을 하는 위치는?

## ChatGPT 응답:
> CentOS에서 서비스(service) 설정 파일은 {% raw %}`/etc/init.d`{% endraw %} 디렉토리에 위치합니다. 이 디렉토리에는 시스템 부팅 시 자동으로 시작되는 서비스들의 스크립트 파일이 위치합니다. 서비스 설정 파일은 일반적으로 해당 서비스의 이름으로 생성되며, 스크립트 파일은 {% raw %}`/etc/init.d/<service_name>`{% endraw %} 형식으로 구성됩니다.

서비스 설정 파일은 {% raw %}`chkconfig`{% endraw %} 명령을 사용하여 시스템 부팅 시 자동으로 시작되도록 설정할 수 있습니다. {% raw %}`chkconfig`{% endraw %} 명령을 사용하여 서비스를 켜거나 끄는 것도 가능합니다. 또한, 서비스의 상태를 확인하거나 실행 중인 서비스를 중지하거나 다시 시작하는 등의 작업도 가능합니다.

CentOS 7 이상에서는 {% raw %}`systemd`{% endraw %} 서비스 매니저를 사용하여 서비스를 관리합니다. {% raw %}`systemctl`{% endraw %} 명령을 사용하여 서비스의 상태를 확인하거나 시작, 중지, 다시 시작 등의 작업을 수행할 수 있습니다. {% raw %}`systemctl`{% endraw %} 명령을 사용하여 시스템 부팅 시 자동으로 시작되도록 설정할 수도 있습니다. CentOS 7 이상에서는 {% raw %}`/etc/systemd/system`{% endraw %} 디렉토리에 서비스 설정 파일을 생성합니다.

## 사용자 입력:
> ● jenkins.service - Jenkins Continuous Integration Server
   Loaded: loaded (/usr/lib/systemd/system/jenkins.service; enabled; vendor preset: disabled)
   Active: failed (Result: start-limit) since 수 2023-03-15 15:24:48 KST; 5s ago
  Process: 9497 ExecStart=/usr/bin/jenkins (code=exited, status=1/FAILURE)
 Main PID: 9497 (code=exited, status=1/FAILURE)

 3월 15 15:24:48 liabuild systemd[1]: jenkins.service: main process exited, code=exited, status=1/FAILURE
 3월 15 15:24:48 liabuild systemd[1]: Failed to start Jenkins Continuous Integration Server.
 3월 15 15:24:48 liabuild systemd[1]: Unit jenkins.service entered failed state.
 3월 15 15:24:48 liabuild systemd[1]: jenkins.service failed.
 3월 15 15:24:48 liabuild systemd[1]: jenkins.service holdoff time over, scheduling restart.
 3월 15 15:24:48 liabuild systemd[1]: Stopped Jenkins Continuous Integration Server.
 3월 15 15:24:48 liabuild systemd[1]: start request repeated too quickly for jenkins.service
 3월 15 15:24:48 liabuild systemd[1]: Failed to start Jenkins Continuous Integration Server.
 3월 15 15:24:48 liabuild systemd[1]: Unit jenkins.service entered failed state.
 3월 15 15:24:48 liabuild systemd[1]: jenkins.service failed.


## ChatGPT 응답:
> 이 로그는 Jenkins 서비스가 시작에 실패하고, {% raw %}`start-limit`{% endraw %}으로 인해 재시작을 중단한 것을 나타냅니다. 이 에러 메시지는 일반적으로 Jenkins 서비스 구성 문제와 관련이 있습니다.

문제를 해결하려면 다음 단계를 따라야 합니다.

1. Jenkins 서비스 구성 파일인 {% raw %}`/usr/lib/systemd/system/jenkins.service`{% endraw %}를 엽니다.

2. 구성 파일의 {% raw %}`ExecStart`{% endraw %} 줄을 확인하여 Jenkins 실행 파일이 올바른 경로에 있는지 확인합니다.

3. Jenkins 실행 파일이 올바른 위치에 있으면, 구성 파일에서 {% raw %}`ExecStart`{% endraw %} 줄을 확인하여 Jenkins 실행 명령이 올바른지 확인합니다.

4. Jenkins 실행 명령이 올바르면, 로그 파일을 확인하여 Jenkins 서비스 시작 시 발생한 다른 오류를 찾습니다. 일반적으로 로그 파일은 {% raw %}`/var/log/jenkins/jenkins.log`{% endraw %}에 위치합니다.

5. 오류를 수정한 후에는 {% raw %}`systemctl daemon-reload`{% endraw %} 명령을 사용하여 시스템 데몬을 다시 로드합니다.

6. 마지막으로, {% raw %}`systemctl restart jenkins`{% endraw %} 명령을 사용하여 Jenkins 서비스를 다시 시작합니다.

이 단계를 수행한 후에도 문제가 해결되지 않으면, 추가적인 문제 해결을 위해 Jenkins 공식 문서나 커뮤니티 포럼을 참조하는 것이 좋습니다.

## 사용자 입력:
> centos에서 group 위치

## ChatGPT 응답:
> CentOS에서 그룹(group) 설정 파일은 {% raw %}`/etc/group`{% endraw %} 파일에 위치합니다. 이 파일은 시스템에 등록된 모든 그룹의 정보를 포함하고 있습니다. 그룹 이름, 그룹 ID(GID), 그룹에 속한 사용자 등의 정보가 기록되어 있습니다.

각 줄은 다음과 같은 형식으로 구성됩니다.

{% raw %}```
<group_name>:<password>:<GID>:<user_list>
```{% endraw %}

여기서 {% raw %}`<group_name>`{% endraw %}은 그룹 이름, {% raw %}`<password>`{% endraw %}는 그룹의 비밀번호(사용하지 않을 경우 비어 있음), {% raw %}`<GID>`{% endraw %}는 그룹 ID, {% raw %}`<user_list>`{% endraw %}는 그룹에 속한 사용자의 목록입니다.

그룹 설정 파일은 일반적으로 시스템 관리자가 직접 편집하여 그룹을 추가하거나 삭제하고, 그룹의 속성을 수정합니다. 사용자를 그룹에 추가하거나 제거하는 등의 작업은 {% raw %}`usermod`{% endraw %} 또는 {% raw %}`groupmod`{% endraw %} 명령을 사용하여 수행할 수 있습니다.

## 사용자 입력:
> lia:x:500:jenkins
jenkins:x:499:lia
무슨 의미

## ChatGPT 응답:
> 이것은 {% raw %}`/etc/passwd`{% endraw %} 파일에서 사용자 정보를 표시하는 행과 {% raw %}`/etc/group`{% endraw %} 파일에서 그룹 정보를 표시하는 행을 나타냅니다. 

첫 번째 행에서 {% raw %}`lia`{% endraw %} 사용자는 {% raw %}`jenkins`{% endraw %} 그룹에 속하며, 두 번째 행에서 {% raw %}`jenkins`{% endraw %} 그룹은 {% raw %}`lia`{% endraw %} 사용자를 포함합니다.

각 행은 콜론({% raw %}`:`{% endraw %})으로 구분된 여러 필드를 포함합니다. 각 필드는 다음과 같은 의미를 가집니다.

- {% raw %}`/etc/passwd`{% endraw %} 파일에서 사용자 정보를 나타내는 행:
  - 첫 번째 필드: 사용자 이름
  - 두 번째 필드: 사용자 비밀번호(보안상의 이유로 실제 비밀번호는 이 파일에 저장되지 않습니다. 대신 비밀번호는 {% raw %}`/etc/shadow`{% endraw %} 파일에 저장됩니다.)
  - 세 번째 필드: 사용자 ID (UID)
  - 네 번째 필드: 기본 그룹 ID (GID)
  - 다섯 번째 필드: 사용자의 전체 이름 또는 기타 정보
  - 여섯 번째 필드: 사용자의 홈 디렉토리 경로
  - 일곱 번째 필드: 사용자의 로그인 셸 경로

- {% raw %}`/etc/group`{% endraw %} 파일에서 그룹 정보를 나타내는 행:
  - 첫 번째 필드: 그룹 이름
  - 두 번째 필드: 그룹 비밀번호(보안상의 이유로 실제 비밀번호는 이 파일에 저장되지 않습니다. 대신 비밀번호는 {% raw %}`/etc/gshadow`{% endraw %} 파일에 저장됩니다.)
  - 세 번째 필드: 그룹 ID (GID)
  - 네 번째 필드: 그룹에 속한 사용자 목록

따라서 위의 예시에서 {% raw %}`lia`{% endraw %} 사용자의 기본 그룹은 {% raw %}`jenkins`{% endraw %} 그룹이며, {% raw %}`jenkins`{% endraw %} 그룹에는 {% raw %}`lia`{% endraw %} 사용자가 포함됩니다. 이것은 사용자와 그룹 간의 관계를 나타내는 것입니다.

## 사용자 입력:
> 그러면, 잘못된건 아니지?

## ChatGPT 응답:
> 일반적으로 사용자는 기본적으로 자신이 속한 기본 그룹에 대한 권한을 가지게 됩니다. 위 예시에서는 {% raw %}`lia`{% endraw %} 사용자가 {% raw %}`jenkins`{% endraw %} 그룹의 멤버이며, {% raw %}`jenkins`{% endraw %} 그룹이 {% raw %}`lia`{% endraw %} 사용자의 기본 그룹으로 설정되어 있으므로 {% raw %}`lia`{% endraw %} 사용자는 {% raw %}`jenkins`{% endraw %} 그룹에 대한 권한을 가지게 됩니다.

따라서 위 설정이 잘못된 것은 아닙니다. 하지만 그룹의 역할과 사용자의 역할이 혼동될 수 있으므로, 시스템을 관리하는 사람들은 이러한 설정을 변경해야 할 수도 있습니다.

