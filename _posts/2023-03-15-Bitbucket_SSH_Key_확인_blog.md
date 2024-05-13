---
layout: post
title: "Bitbucket SSH Key 확인"
date: 2023-03-15
tags: [Bitbucket, Key, 확인, SSH]
categories: blog
---

## 사용자 입력:
> bitbucket의 ssh private key는 어떻게 확인하지?

## ChatGPT 응답:
> 

Bitbucket에서 SSH 프로토콜을 사용하여 리포지토리에 액세스하려면 SSH 키를 설정해야 합니다. SSH 키는 보안을 위해 개인 키와 공개 키로 구성됩니다. 개인 키는 사용자 컴퓨터에 보관되며, 공개 키는 Bitbucket에 등록됩니다.

SSH 프로토콜을 사용하여 Bitbucket에 액세스하려면 다음 단계를 수행하세요.

1. Bitbucket 웹 사이트에서 로그인합니다.
2. 사용자 아이콘을 클릭하고, "Personal Settings"을 선택합니다.
3. 왼쪽 메뉴에서 "SSH Keys"를 선택합니다.
4. "SSH Keys" 페이지에서 등록된 SSH 키 목록이 표시됩니다. "Key" 열에 있는 공개 키를 확인할 수 있습니다.

비밀 키는 Bitbucket에서 직접 볼 수 없습니다. 개인 키는 사용자 컴퓨터에 보관되므로, 로컬 SSH 클라이언트에서 개인 키를 확인할 수 있습니다. 개인 키는 일반적으로 다음 경로에서 찾을 수 있습니다.

- macOS 또는 Linux에서 : ~/.ssh/id_rsa
- Windows에서 : C:\Users\사용자 이름\.ssh\id_rsa

단, 개인 키는 보안 상의 이유로 외부에 노출되어서는 안 되므로, 항상 안전한 장소에 보관해야 합니다.

## 사용자 입력:
> jenkins blue ocean에대해서 설명해줘

## ChatGPT 응답:
> Jenkins Blue Ocean은 Jenkins CI의 UI를 대폭 개선한 새로운 UI입니다. 기존의 Jenkins UI보다 직관적이고, 사용자 친화적으로 개발되었습니다. 이를 통해 Jenkins 사용자들은 더욱 쉽고 빠르게 파이프라인 기반의 CI/CD를 구성할 수 있게 되었습니다.

Jenkins Blue Ocean의 핵심 기능은 다음과 같습니다.

1. 파이프라인 기반의 CI/CD : Jenkins Blue Ocean은 기존의 Jenkins UI에서 불편했던 파이프라인 구성과 관리를 쉽게 할 수 있도록 도와줍니다. 이를 통해 더욱 쉽게 코드를 빌드, 테스트, 배포할 수 있습니다.

2. 대시보드 : Jenkins Blue Ocean은 대시보드를 통해 Jenkins 파이프라인의 실행 상태와 히스토리, 분석 결과 등을 시각적으로 제공합니다. 이를 통해 빠르게 파이프라인 실행 상태를 확인할 수 있습니다.

3. 에디터 : Jenkins Blue Ocean은 파이프라인을 작성하고 관리하는 데 필요한 에디터를 제공합니다. 이를 통해 쉽게 파이프라인을 수정하고 관리할 수 있습니다.

4. 마이크로서비스와의 통합 : Jenkins Blue Ocean은 마이크로서비스 아키텍처와의 통합을 지원합니다. 이를 통해 복잡한 서비스 환경에서도 쉽게 파이프라인을 관리할 수 있습니다.

Jenkins Blue Ocean은 Jenkins에서 파이프라인 기반의 CI/CD를 더욱 쉽게 구성하고 관리할 수 있도록 도와줍니다. 이를 통해 개발자들은 빠르게 릴리스를 수행할 수 있고, 조직은 빠르게 비즈니스 요구사항을 충족시킬 수 있습니다.

## 사용자 입력:
> Jenkins Blue Ocean 플러그인인가?

## ChatGPT 응답:
> 네, 맞습니다. Jenkins Blue Ocean은 Jenkins의 플러그인 중 하나입니다. Jenkins의 기능을 확장하는 방식으로 개발된 플러그인 중 하나이며, 파이프라인 기반의 CI/CD를 보다 쉽게 구성하고 관리할 수 있도록 해줍니다.

Jenkins Blue Ocean 플러그인은 Jenkins의 파이프라인을 시각화하고 대시보드, 에디터, 분석 도구 등을 제공하는 Jenkins의 새로운 UI로서 개발되었습니다. 이를 통해 더욱 직관적이고 사용자 친화적인 환경에서 CI/CD를 구성하고 관리할 수 있습니다.

Jenkins Blue Ocean 플러그인을 설치하면 Jenkins의 기존 UI 대신 새로운 UI가 사용되며, 파이프라인 기반의 CI/CD를 더욱 쉽게 구성할 수 있습니다. Jenkins Blue Ocean 플러그인은 Jenkins의 기능 중 하나로서 매우 유용하며, Jenkins의 기능을 보완하여 CI/CD를 더욱 효율적으로 관리할 수 있게 해줍니다.

## 사용자 입력:
> blue ocean 을 해도 기존 pipeline declarative 는 계속 쓸 수 있나?

## ChatGPT 응답:
> 네, 맞습니다. Blue Ocean을 사용하더라도 기존의 Pipeline Declarative 문법을 계속 사용할 수 있습니다. 

Jenkins는 Pipeline을 위한 Declarative 및 Scripted Syntax 두 가지 문법을 모두 지원합니다. Pipeline Declarative 문법은 YAML을 기반으로 하는 파이프라인 정의 방식으로, 더 간결하고 읽기 쉬운 파이프라인을 작성할 수 있습니다. Scripted Syntax는 Groovy 언어를 기반으로 하는 더욱 유연한 방식의 파이프라인 정의 방법입니다.

Blue Ocean은 Jenkins의 UI를 대폭 개선한 새로운 UI이지만, 내부적으로는 여전히 Jenkins Pipeline을 실행하고 있습니다. 따라서 Blue Ocean을 사용하면서도 기존의 Declarative 문법으로 작성한 파이프라인을 계속 사용할 수 있습니다.

단, Blue Ocean에서는 Declarative 문법을 좀 더 시각적으로 표현하고 편집할 수 있는 에디터를 제공하므로, Declarative 문법을 사용하는 경우에도 Blue Ocean을 사용하면 작성과 관리가 보다 쉬워집니다.

## 사용자 입력:
> hudson.plugins.git.GitException: Command "git submodule update jgraphx" returned status code 1:
stdout: Cloning into 'jgraphx'...

stderr: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
Clone of 'ssh://git@192.168.0.119:7999/qtss/jgraphx.git' into submodule path 'jgraphx' failed

## ChatGPT 응답:
> 이 오류는 Git 서브모듈 업데이트 도중 SSH 인증에 실패하여 발생하는 오류입니다. Git 서브모듈은 별도의 Git 저장소를 참조하는 Git 저장소입니다. 서브모듈을 업데이트하려면 해당 Git 저장소에 대한 권한이 필요합니다.

오류 메시지에 따르면 "Permission denied (publickey)" 오류가 발생하고 있습니다. 이는 SSH 인증 문제를 나타냅니다. Git 서브모듈을 업데이트하려면 해당 저장소에 대한 SSH 개인 키가 필요합니다.

다음과 같은 방법을 시도해 볼 수 있습니다.

1. Jenkins 서버에서 Git 서브모듈에 대한 SSH 키를 설정합니다. 이를 위해 Jenkins 서버에서 다음 명령어를 실행하여 SSH 키를 생성합니다.
   
   ```
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

   이후에 생성된 SSH 개인 키를 Jenkins 서버의 `~/.ssh/` 디렉토리에 복사합니다. 

2. Jenkins에서 해당 Git 서브모듈을 사용하는 작업을 수행하는 사용자에 대한 SSH 키를 등록합니다. 이를 위해 다음과 같이 수행합니다.

   - Jenkins 대시보드에서 해당 작업을 실행하는 빌드 에이전트를 선택하고, 해당 빌드 에이전트의 구성 페이지로 이동합니다.
   - "Credentials" 탭에서 해당 사용자에 대한 SSH 키를 등록합니다. 이 때, 개인 키를 "Private Key" 필드에 입력하고, 공개 키를 Git 서브모듈이 저장된 Git 서버의 설정에 추가합니다.

위 방법을 시도해보고도 문제가 해결되지 않으면, 해당 Git 서브모듈에 대한 권한이 올바르게 구성되어 있는지 다시 한번 확인해 보시기 바랍니다.

## 사용자 입력:
> jenkins에서 nodejs 14.x.x 설정하는 법 알려줘

## ChatGPT 응답:
> Jenkins에서 Node.js 14.x.x를 설정하려면 다음 단계를 따르면 됩니다.

1. Node.js 14.x.x를 설치합니다. 예를 들어, Ubuntu에서는 다음 명령어를 사용하여 Node.js 14.x.x를 설치할 수 있습니다.

   ```
   curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

2. Jenkins에서 "Global Tool Configuration"을 엽니다.

3. "NodeJS" 항목을 찾고, "Add NodeJS" 버튼을 클릭합니다.

4. "Name" 필드에 원하는 이름을 입력합니다. 예를 들어, "Node.js 14.x.x"로 입력할 수 있습니다.

5. "Install automatically" 옵션을 선택합니다.

6. "Install from nodejs.org" 옵션을 선택합니다.

7. "Version" 필드에서 "14.x"를 선택합니다.

8. "Save" 버튼을 클릭하여 변경 사항을 저장합니다.

이제 Jenkins에서 Node.js 14.x.x를 사용할 수 있습니다. Jenkinsfile에서 다음과 같은 코드를 사용하여 Node.js 14.x.x를 사용할 수 있습니다.

```
pipeline {
    agent {
        node {
            label 'my-node-label'
            // Node.js 14.x.x 설정
            tool 'Node.js 14.x.x'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'node --version'
            }
        }
    }
}
```

위 코드에서 `tool 'Node.js 14.x.x'` 부분이 Node.js 14.x.x를 설정하는 부분입니다. 이를 통해 Jenkins에서 Node.js 14.x.x를 사용하여 빌드 및 테스트 등을 수행할 수 있습니다.

## 사용자 입력:
> pipeline declarative 로 알려줘

## ChatGPT 응답:
> Jenkins의 파이프라인을 Declarative Syntax로 작성하려면 다음과 같이 작성할 수 있습니다.

```
pipeline {
    agent any
    tools {
        nodejs "Node.js 14.x.x"
    }
    stages {
        stage('Build') {
            steps {
                sh 'node --version'
            }
        }
    }
}
```

위 코드에서 `tools` 섹션은 `Node.js 14.x.x`를 사용하도록 구성하는 부분입니다. `sh 'node --version'`은 해당 노드 버전이 정상적으로 설정되었는지 확인하는 코드입니다. 

위 코드에서 사용된 `pipeline` 블록은 파이프라인의 시작점을 정의하는 데 사용됩니다. `agent` 블록은 빌드를 수행하는 노드를 선택하는 데 사용됩니다. `tools` 블록은 빌드에 필요한 도구를 선택하는 데 사용됩니다. `stages` 블록은 파이프라인을 구성하는 단계를 정의하는 데 사용됩니다.

위 코드에서는 `Build`라는 하나의 단계만 정의하고 있지만, 필요한 만큼 더 많은 단계를 추가할 수 있습니다. 파이프라인은 단계 단위로 실행되며, 각 단계에서는 여러 개의 스텝으로 나눌 수 있습니다.

Declarative Syntax는 Scripted Syntax와 달리 좀 더 간결하고 직관적이며, 파이프라인을 더욱 쉽게 이해하고 유지보수할 수 있게 합니다.

## 사용자 입력:
> pipeline scripted 로 알려줘

## ChatGPT 응답:
> Jenkins의 파이프라인을 Scripted Syntax로 작성하려면 다음과 같이 작성할 수 있습니다.

```
node {
    def nodejsHome = tool 'Node.js 14.x.x'
    sh "${nodejsHome}/bin/node --version"
}
```

위 코드에서 `node` 블록은 빌드를 수행하는 노드를 선택하는 데 사용됩니다. `tool` 함수는 `Node.js 14.x.x`를 사용하도록 구성합니다. `sh` 함수는 해당 노드 버전이 정상적으로 설정되었는지 확인하는 코드입니다.

위 코드에서는 하나의 단계만 정의하고 있지만, 필요한 만큼 더 많은 단계를 추가할 수 있습니다. Scripted Syntax는 Declarative Syntax와 달리 자유로운 문법을 사용할 수 있으며, 파이프라인을 더욱 유연하게 작성할 수 있습니다.

다음은 Scripted Syntax로 작성된 간단한 파이프라인 예시입니다.

```
node {
    stage('Build') {
        def mvnHome = tool 'Maven 3.8.4'
        sh "${mvnHome}/bin/mvn clean package"
    }
    stage('Test') {
        def mvnHome = tool 'Maven 3.8.4'
        sh "${mvnHome}/bin/mvn test"
    }
}
```

위 코드에서는 `node` 블록을 사용하여 빌드를 수행할 노드를 선택하고 있습니다. `stage` 함수는 파이프라인을 구성하는 단계를 정의하는 데 사용됩니다. 각 단계에서는 `def`로 시작하는 변수를 사용하여 필요한 도구를 설정하고, `sh` 함수를 사용하여 쉘 명령어를 실행할 수 있습니다. 이렇게 작성된 Scripted Syntax는 복잡한 파이프라인을 작성하는 데 유용합니다.

## 사용자 입력:
> def buildForUi() {
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
}  이 코드는 yarn을 실행하는데 이 때, node 14.x.x로 실행해야 하면 어떻게 코딩해야하니?

## ChatGPT 응답:
> 해당 파이프라인 코드에서 `node` 명령어를 직접 사용하고 있지 않고, `yarn` 명령어만 사용하고 있습니다. 하지만 `yarn` 명령어를 실행하면 내부적으로 Node.js를 사용하므로, Node.js 14.x.x를 사용하려면 다음과 같이 수정할 수 있습니다.

1. Node.js 14.x.x를 Jenkins에서 설정합니다. 이전에 설명한 방법과 같이 "Global Tool Configuration"에서 "Add NodeJS" 버튼을 클릭하여 Node.js 14.x.x를 추가합니다.

2. 파이프라인 코드에서 `buildForUi` 함수를 사용하여 UI 빌드를 수행하는 블록을 정의하고 있습니다. `dir` 함수를 사용하여 디렉토리를 이동한 후, `sh` 함수를 사용하여 `yarn` 명령어를 실행하고 있습니다. 이 부분을 다음과 같이 수정합니다.

   ```
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
   ```

   위 코드에서 `withEnv(['PATH+NODEJS=/usr/local/bin'])` 함수를 사용하여 `PATH` 환경 변수에 `/usr/local/bin`을 추가합니다. 이렇게 하면 Node.js 14.x.x가 설치된 디렉토리가 PATH 환경 변수에 추가되어, `yarn` 명령어가 Node.js 14.x.x를 사용하게 됩니다.

위와 같이 수정하면 `yarn` 명령어가 Node.js 14.x.x를 사용하여 실행됩니다.

