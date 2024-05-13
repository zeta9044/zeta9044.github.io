---
layout: post
title: "Docker 설치 디렉토리 변경."
date: 2023-06-23
tags: [설치, Docker, 디렉토리, 변경]
categories: blog
---

## 사용자 입력:
> ubuntu 22.04.2 LTS에 docker를 설치했는데, 설치 디렉토리를 /mnt/d/docker로 변경하고 싶어. 어떻게 해야돼. 현재, 설치된 container는 없어.

## ChatGPT 응답:
> Docker의 설치 디렉토리를 변경하려면 몇 가지 단계를 따라야 합니다. 아래는 Ubuntu 22.04.2 LTS에서 Docker 설치 디렉토리를 /mnt/d/docker로 변경하는 방법입니다. 이미 설치된 container가 없다고 가정하겠습니다.

1. Docker를 제거합니다.
   ```
   sudo apt remove docker docker-engine docker.io containerd runc
   ```

2. Docker의 설치 디렉토리를 변경합니다.
   - Docker의 기본 설치 디렉토리는 `/var/lib/docker`입니다. 이를 `/mnt/d/docker`로 변경하려면 다음 명령을 실행합니다.
   ```
   sudo systemctl stop docker
   sudo mv /var/lib/docker /mnt/d/docker
   ```

3. Docker의 설정 파일을 수정합니다.
   - Docker의 설정 파일은 `/etc/docker/daemon.json`입니다. 만약 해당 파일이 존재하지 않는다면, 새로 생성할 수 있습니다.
   - 설정 파일을 편집기로 엽니다.
   ```
   sudo nano /etc/docker/daemon.json
   ```

   - 다음과 같이 설정 파일을 편집합니다. `/mnt/d/docker`를 변경하고자 하는 디렉토리 경로로 바꿉니다.
   ```
   {
     "data-root": "/mnt/d/docker"
   }
   ```

4. Docker를 재시작합니다.
   ```
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   ```

이제 Docker의 설치 디렉토리는 /mnt/d/docker로 변경되었습니다. 이후에 새로운 container를 생성하면 해당 디렉토리에 저장될 것입니다.

## 사용자 입력:
> ubuntu 22.04.2 LTS에 docker를 설치했는데, 기존에 설치된 docker를 모두 삭제하고, 설치위치를 /mnt/d/docker로 해서 처음부터 다시 설치하고 싶어.


## ChatGPT 응답:
> Docker를 Ubuntu 22.04.2 LTS에 설치하고, 기존에 설치된 Docker를 제거하고 새로 설치하려면 다음 단계를 따를 수 있습니다:

1. Docker 제거:
   ```shell
   sudo apt remove docker docker-engine docker.io containerd runc
   ```

2. 설치된 Docker 패키지에 대한 모든 구성 파일 및 설정을 제거합니다.
   ```shell
   sudo rm -rf /etc/docker
   sudo rm -rf /var/lib/docker
   ```

   **주의:** 위 명령어는 Docker와 관련된 모든 데이터를 삭제합니다. 이미지, 컨테이너 등 모든 Docker 관련 데이터가 삭제됩니다. 필요한 데이터를 백업해두고 진행하시기 바랍니다.

3. 시스템을 업데이트합니다.
   ```shell
   sudo apt update
   ```

4. Docker 설치를 위해 필요한 패키지를 설치합니다.
   ```shell
   sudo apt install apt-transport-https ca-certificates curl software-properties-common
   ```

5. Docker의 공식 GPG 키를 추가합니다.
   ```shell
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

6. Docker APT 저장소를 추가합니다.
   ```shell
   echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

7. 시스템을 업데이트합니다.
   ```shell
   sudo apt update
   ```

8. Docker를 설치합니다.
   ```shell
   sudo apt install docker-ce docker-ce-cli containerd.io
   ```

9. Docker를 시작하고 부팅 시 자동으로 시작되도록 설정합니다.
   ```shell
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

10. Docker 그룹에 현재 사용자를 추가합니다. 이렇게 하면 Docker를 sudo 없이 실행할 수 있습니다.
   ```shell
   sudo usermod -aG docker $USER
   ```

11. 시스템을 재부팅합니다.
   ```shell
   sudo reboot
   ```

이제 Docker를 /mnt/d/docker에 설치하고 처음부터 사용할 수 있게 되었습니다.

## 사용자 입력:
> 위에서 차례대로 보니까, docker설치를 /mnt/d/docker로 지정하는 부분이 없네.

## ChatGPT 응답:
> 죄송합니다. Docker 설치 시 설치 위치를 지정하는 과정이 빠졌습니다. Docker를 /mnt/d/docker로 설치하기 위해 다음 단계를 따라주십시오:

1. Docker 제거:
   ```shell
   sudo apt remove docker docker-engine docker.io containerd runc
   ```

2. 설치된 Docker 패키지에 대한 모든 구성 파일 및 설정을 제거합니다.
   ```shell
   sudo rm -rf /etc/docker
   sudo rm -rf /var/lib/docker
   ```

   **주의:** 위 명령어는 Docker와 관련된 모든 데이터를 삭제합니다. 이미지, 컨테이너 등 모든 Docker 관련 데이터가 삭제됩니다. 필요한 데이터를 백업해두고 진행하시기 바랍니다.

3. 시스템을 업데이트합니다.
   ```shell
   sudo apt update
   ```

4. Docker 설치를 위해 필요한 패키지를 설치합니다.
   ```shell
   sudo apt install apt-transport-https ca-certificates curl software-properties-common
   ```

5. Docker의 공식 GPG 키를 추가합니다.
   ```shell
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

6. Docker APT 저장소를 추가합니다.
   ```shell
   echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

7. 시스템을 업데이트합니다.
   ```shell
   sudo apt update
   ```

8. Docker를 설치합니다.
   ```shell
   sudo apt install docker-ce docker-ce-cli containerd.io
   ```

9. Docker를 시작하고 부팅 시 자동으로 시작되도록 설정합니다.
   ```shell
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

10. Docker 그룹에 현재 사용자를 추가합니다. 이렇게 하면 Docker를 sudo 없이 실행할 수 있습니다.
   ```shell
   sudo usermod -aG docker $USER
   ```

11. Docker 설치 위치를 변경합니다.
   ```shell
   sudo systemctl stop docker
   sudo mkdir /mnt/d/docker
   sudo rsync -aqxP /var/lib/docker/ /mnt/d/docker
   sudo sed -i 's|/var/lib/docker|/mnt/d/docker|g' /etc/docker/daemon.json
   ```

12. Docker를 시작합니다.
   ```shell
   sudo systemctl start docker
   ```

이제 Docker가 /mnt/d/docker에 설치되었으며, 해당 위치에 모든 데이터가 저장됩니다.

