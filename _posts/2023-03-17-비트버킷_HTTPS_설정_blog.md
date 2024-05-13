---
layout: post
title: "비트버킷 HTTPS 설정"
date: 2023-03-17
tags: [HTTPS, 설정, 버킷, 비트]
categories: blog
---

## 사용자 입력:
> bitbucket에서 저장소를 ssh말고 다른 것도 가능가능하게 만들려면?

## ChatGPT 응답:
> Bitbucket에서 저장소를 SSH를 사용하지 않고 다른 방법으로 설정하려면 HTTPS를 사용할 수 있습니다. 저장소에 액세스하기 위해 HTTPS를 사용하려면 다음 단계를 따라 주세요:

1. Bitbucket에 로그인하십시오.
2. 프로젝트 및 저장소를 선택하십시오.
3. 저장소의 주요 페이지에서 오른쪽 상단에 있는 'Clone' 버튼을 클릭하십시오.
4. 'Clone' 대화 상자에서 'Clone with HTTPS' 옵션을 선택하십시오.
5. HTTPS URL을 복사하십시오. 이 URL은 다음과 같은 형식일 것입니다: `https://<username>@bitbucket.org/<project>/<repository>.git`
6. 로컬 시스템에서 원하는 디렉토리로 이동하십시오.
7. 다음 명령어를 실행하고 복사한 HTTPS URL을 붙여 넣으십시오: `git clone <https_url>`
8. 이제 Git은 Bitbucket 저장소의 복사본을 로컬 디렉토리로 복제합니다.

HTTPS를 사용할 때 Git은 사용자 이름과 암호를 물어볼 수 있습니다. 이를 방지하려면 Git 자격 증명 도우미를 설정하거나 토큰 기반 인증을 사용하여 인증을 자동화할 수 있습니다.

## 사용자 입력:
> submoudle update 할때 계속 ssh때문에 문제가 되는데, ssh말고는 안되나?

## ChatGPT 응답:
> 서브모듈을 사용할 때 SSH를 사용하지 않고 HTTPS를 사용하려면 서브모듈을 HTTPS URL로 설정해야 합니다. 이 작업을 수행하려면 다음 단계를 따라 주세요:

1. `.gitmodules` 파일을 찾아서 텍스트 편집기로 엽니다. 이 파일은 프로젝트의 최상위 폴더에 위치해야 합니다.
2. `[submodule "submodule_name"]` 아래에 있는 `url`을 찾습니다. 이는 현재 SSH URL일 것입니다.
3. SSH URL을 해당 서브모듈의 HTTPS URL로 바꿉니다. 예를 들어, SSH URL이 `git@github.com:user/repository.git`라면, HTTPS URL은 `https://github.com/user/repository.git`와 같습니다.
4. 변경 사항을 저장하고 `.gitmodules` 파일을 닫습니다.
5. 변경 사항을 Git에 추가하고 커밋합니다.

```sh
git add .gitmodules
git commit -m "Update submodule URLs to use HTTPS"
```

6. 이제 서브모듈을 업데이트해야 합니다. 이렇게 하려면 다음 명령어를 실행하십시오:

```sh
git submodule sync
git submodule update --init --recursive
```

이제 서브모듈이 HTTPS를 사용하여 업데이트되고 로컬에 복제됩니다. 이 방법으로 SSH를 사용하지 않고 서브모듈을 사용할 수 있습니다.

## 사용자 입력:
> bitbucket 서버 UI에서 설정할 수는 없나?

## ChatGPT 응답:
> Bitbucket에서 서브모듈 URL을 직접 변경하는 기능은 제공하지 않습니다. 그러나 저장소를 복제하거나 클론할 때 HTTPS 또는 SSH URL을 선택할 수 있습니다. 이렇게 하면 로컬에서 작업하는 동안 선택한 프로토콜을 사용할 수 있습니다.

그러나 서브모듈에 대한 URL은 `.gitmodules` 파일에 저장되므로, 변경하려면 이 파일을 직접 편집해야 합니다. 이렇게 하려면 위에서 언급한 방법대로 하십시오.

Bitbucket 서버 UI에서 서브모듈 URL을 변경하려면 기본적으로 저장소를 클론하여 로컬에서 작업한 다음, 변경 사항을 저장소에 다시 푸시해야 합니다. 이렇게 하면 프로젝트의 다른 사용자들도 변경된 서브모듈 URL을 사용할 수 있습니다.

## 사용자 입력:
> http로 바꾸면, 아이디 패스워드를 물어보나?

## ChatGPT 응답:
> HTTP를 사용하여 Git 저장소를 클론하거나 푸시, 패치할 때 기본적으로 사용자 이름과 비밀번호를 물어봅니다. 그러나 아래와 같은 방법을 사용하여 인증 정보를 저장하거나 자동화할 수 있습니다:

1. **Git 자격 증명 저장소(Credential Storage)**: Git에는 자격 증명 저장소라는 기능이 내장되어 있습니다. 이를 사용하면 Git이 저장소에 액세스할 때 사용자 이름과 비밀번호를 기억하도록 설정할 수 있습니다.

```sh
git config --global credential.helper store
```

이 명령을 실행한 후에는 사용자 이름과 비밀번호를 한 번 입력하면, 그 이후에는 Git이 자동으로 사용합니다. 이렇게 하면 로컬 시스템에 자격 증명이 저장되므로 보안에 주의해야 합니다.

2. **개인 액세스 토큰(Personal Access Token, PAT)**: 대부분의 Git 호스팅 서비스는 개인 액세스 토큰을 사용하여 비밀번호 대신 인증할 수 있는 기능을 제공합니다. Bitbucket에서는 "앱 암호(App Password)"라는 이름으로 이 기능을 제공합니다.

Bitbucket에서 앱 암호를 생성하려면 다음 단계를 따라 주세요:

  1. Bitbucket에 로그인합니다.
  2. 프로필 이미지를 클릭한 다음 "비트버킷 설정(Bitbucket settings)"을 선택합니다.
  3. 왼쪽 메뉴에서 "앱 암호(App passwords)"를 클릭합니다.
  4. "새로운 앱 암호 생성(Create app password)"을 클릭하고, 앱 암호의 이름과 사용 권한을 설정합니다.
  5. "생성(Create)"을 클릭하면 앱 암호가 생성되며, 이를 복사하여 안전한 곳에 저장합니다.

이제 Git 저장소에 액세스할 때 사용자 이름과 함께 생성된 앱 암호를 사용하면, 비밀번호를 물어보지 않고 인증할 수 있습니다.

이러한 방법 중 하나를 사용하면 Git 저장소에 액세스할 때 사용자 이름과 비밀번호를 자동으로 제공할 수 있습니다.

## 사용자 입력:
>  ssh://git@192.168.0.119:7999/qtss/jgraphx.git이 서브모듈인데, bamboo에서 source checkout을 하면, 계속 권한문제라고 나온다. bamboo에서 등록한 repository의 공개키가 어디에 등록해야 이런 에러가 나지 않을까?

## ChatGPT 응답:
> Bamboo에서 권한 문제가 발생하는 경우, 저장소에 등록된 공개키가 올바르게 설정되지 않았을 가능성이 있습니다. 공개키를 저장소에 올바르게 추가하려면 다음 단계를 따라 주세요:

1. 먼저, Bamboo에서 사용하는 공개키를 찾습니다. 대부분의 경우, 이 키는 Bamboo 설치 디렉토리에 있는 `~/.ssh/id_rsa.pub` 파일에 있습니다.

2. 공개키 파일을 열고 키를 전체 선택한 다음 복사합니다. 키는 다음과 유사한 형식이어야 합니다: `ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQ...`

3. Bitbucket 서버에 로그인하고 사용자 설정으로 이동합니다. 프로필 이미지를 클릭한 다음 "Bitbucket 설정(Bitbucket settings)"을 선택합니다.

4. 왼쪽 메뉴에서 "SSH 키(SSH keys)"를 클릭합니다.

5. "키 추가(Add key)" 버튼을 클릭합니다.

6. "키 레이블(Key label)" 입력란에 키의 이름을 입력하고, "키(Key)" 입력란에 복사한 공개키를 붙여 넣습니다.

7. "키 추가(Add key)"를 클릭하여 키를 저장합니다.

이제 Bamboo에서 사용하는 공개키가 Bitbucket 계정에 추가되었습니다. 이로 인해 Bamboo에서 저장소에 액세스할 때 권한 문제가 해결되어야 합니다.

만약 이 방법으로도 문제가 해결되지 않는다면, Bitbucket 서버의 프로젝트 또는 저장소 설정에서 Bamboo에서 사용하는 사용자 계정이 올바른 권한을 갖도록 확인해야 합니다. 저장소의 "Access Management" 또는 "User and group access" 설정에서 Bamboo 사용자 계정이 적절한 권한(예: 읽기 및 쓰기)을 갖도록 설정해야 합니다.

## 사용자 입력:
> 위에 처리한대로 했는데도 권한 오류가 나면?

## ChatGPT 응답:
> 위에서 언급한 단계를 모두 완료했음에도 권한 오류가 발생한다면, 추가로 확인해야 할 몇 가지 사항이 있습니다.

1. **Bamboo 사용자 권한 확인**: Bitbucket 서버의 프로젝트 또는 저장소 설정에서 Bamboo에서 사용하는 사용자 계정이 올바른 권한을 갖도록 확인해야 합니다. 저장소의 "Access Management" 또는 "User and group access" 설정에서 Bamboo 사용자 계정이 적절한 권한(예: 읽기 및 쓰기)을 갖도록 설정해야 합니다.

2. **SSH 알려진 호스트 파일 확인**: Bamboo에서 사용하는 SSH 클라이언트가 Bitbucket 서버의 호스트 키를 올바르게 인식하고 있는지 확인합니다. 이를 위해 Bamboo 서버의 `~/.ssh/known_hosts` 파일에 Bitbucket 서버의 호스트 키가 있는지 확인해야 합니다. 이 파일에 올바른 호스트 키가 없으면, 다음 명령을 실행하여 호스트 키를 추가할 수 있습니다:

   ```sh
   ssh-keyscan 192.168.0.119 >> ~/.ssh/known_hosts
   ```

3. **SSH 구성 확인**: Bamboo 서버의 SSH 구성 파일(`~/.ssh/config`)에 올바른 설정이 있는지 확인합니다. 예를 들어, `IdentityFile` 설정이 올바른 비공개 키를 가리키고 있는지 확인해야 합니다. 이 파일에 다음과 같은 내용이 포함되어야 합니다:

   ```
   Host 192.168.0.119
       Port 7999
       User git
       IdentityFile ~/.ssh/id_rsa
   ```

   이 설정이 없거나 올바르지 않은 경우, 이 파일을 수정하고 변경 사항을 저장한 다음 Bamboo에서 작업을 다시 시도하십시오.

4. **Bamboo 로그 확인**: Bamboo 서버의 로그 파일을 확인하여 문제가 발생한 이유에 대한 추가 정보를 찾을 수 있습니다. 로그 파일은 일반적으로 Bamboo 설치 디렉토리의 `logs` 폴더에 위치하며, 오류 및 경고 메시지를 검토하여 문제를 해결하는 데 도움이 될 수 있는 정보를 찾을 수 있습니다.

5. **방화벽 및 네트워크 문제 확인**: Bitbucket 서버와 Bamboo 서버 간의 연결 문제가 있는지 확인합니다. 방화벽 또는 보안 그룹 설정이 두 서버 사이의 통신을 차단하지 않도록 해야 합니다.

위의 추가 확인 사항을 검토한 후에도 문제가 해결되지 않는 경우, Bamboo 및 Bitbucket 서버에 대한 도움말 또는 지원 문서를 참조하거나 해당 제품의 지원 팀에 문의하여

