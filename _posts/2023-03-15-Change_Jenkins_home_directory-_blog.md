---
layout: post
title: "Change Jenkins home directory."
date: 2023-03-15
tags: [home, Jenkins, Change, directory]
categories: blog
---

## 사용자 입력:
> jenkins 2.3 home 디렉토리 바꾸는 법

## ChatGPT 응답:
> Jenkins 2.3에서 Home 디렉토리를 변경하는 방법은 다음과 같습니다:

1. Jenkins 서버를 중지합니다.

2. 이전 Home 디렉토리를 백업합니다. (선택 사항)

3. 새 Home 디렉토리를 생성합니다.

4. Jenkins 서버의 환경 변수 중 JENKINS_HOME을 새 Home 디렉토리 경로로 변경합니다.

- Windows에서는 제어판 > 시스템 > 고급 시스템 설정 > 환경 변수에서 변경할 수 있습니다.
- Linux에서는 셸에서 "export JENKINS_HOME=새 디렉토리 경로" 명령을 실행합니다.

5. Jenkins 서버를 시작합니다.

6. Jenkins 관리 페이지로 이동하여 "Configure System"을 선택합니다.

7. "Home directory" 항목에서 새 Home 디렉토리 경로를 입력합니다.

8. 변경 사항을 저장하고 Jenkins 서버를 다시 시작합니다.

새로운 Home 디렉토리에는 Jenkins 서버의 모든 데이터 및 설정이 저장됩니다. 이전 Home 디렉토리를 백업하여 문제가 발생할 경우 복원할 수 있도록 해야 합니다.

## 사용자 입력:
> centos에서 jenkins 2.3 home 디렉토리 바꾸는 법

## ChatGPT 응답:
> CentOS에서 Jenkins 2.3의 Home 디렉토리를 변경하는 방법은 다음과 같습니다:

1. Jenkins 서버를 중지합니다.

2. 새로운 Home 디렉토리를 생성합니다.

```
sudo mkdir /new/jenkins/home/directory
```

3. 기존 Home 디렉토리를 백업합니다. (선택 사항)

```
sudo mv /var/lib/jenkins /var/lib/jenkins_backup
```

4. Jenkins의 환경 변수 중 JENKINS_HOME을 새 Home 디렉토리 경로로 변경합니다.

```
sudo vi /etc/sysconfig/jenkins
```

5. 아래와 같이 변경합니다.

```
JENKINS_HOME=/new/jenkins/home/directory
```

6. 변경 사항을 저장하고 나갑니다.

7. Jenkins 서버를 시작합니다.

```
sudo systemctl start jenkins
```

8. Jenkins 관리 페이지로 이동하여 "Configure System"을 선택합니다.

9. "Home directory" 항목에서 새 Home 디렉토리 경로를 입력합니다.

10. 변경 사항을 저장하고 Jenkins 서버를 다시 시작합니다.

새로운 Home 디렉토리에는 Jenkins 서버의 모든 데이터 및 설정이 저장됩니다. 이전 Home 디렉토리를 백업하여 문제가 발생할 경우 복원할 수 있도록 해야 합니다.

## 사용자 입력:
> 위와 같이 했는데 변경안되는데

## ChatGPT 응답:
> 만약 CentOS에서 위의 과정을 따라 JENKINS_HOME 환경 변수를 변경하였음에도 변경사항이 적용되지 않는다면, 다음과 같은 단계를 수행하여 문제를 해결할 수 있습니다.

1. Jenkins 서버를 중지합니다.

```
sudo systemctl stop jenkins
```

2. 새로운 Home 디렉토리로 복사합니다.

```
sudo cp -a /var/lib/jenkins /new/jenkins/home/directory
```

3. 기존 Home 디렉토리를 백업합니다. (선택 사항)

```
sudo mv /var/lib/jenkins /var/lib/jenkins_backup
```

4. Jenkins의 환경 변수 중 JENKINS_HOME을 새 Home 디렉토리 경로로 변경합니다.

```
sudo vi /etc/sysconfig/jenkins
```

5. 아래와 같이 변경합니다.

```
JENKINS_HOME=/new/jenkins/home/directory
```

6. 변경 사항을 저장하고 나갑니다.

7. SELinux 정책을 변경합니다.

```
sudo chcon -R -t jenkins_home_t /new/jenkins/home/directory
```

8. Jenkins 서버를 시작합니다.

```
sudo systemctl start jenkins
```

9. Jenkins 관리 페이지로 이동하여 "Configure System"을 선택합니다.

10. "Home directory" 항목에서 새 Home 디렉토리 경로를 입력합니다.

11. 변경 사항을 저장하고 Jenkins 서버를 다시 시작합니다.

이제 변경된 Home 디렉토리가 정상적으로 적용되어 Jenkins가 실행됩니다.

## 사용자 입력:
> jenkins service 설정은 어디에 있나?

## ChatGPT 응답:
> Jenkins 서비스 설정은 운영 체제에 따라 다를 수 있습니다. 다음은 일반적인 경우의 Jenkins 서비스 설정 파일 경로입니다.

- CentOS, Red Hat, Amazon Linux: `/etc/sysconfig/jenkins`
- Ubuntu, Debian: `/etc/default/jenkins`
- macOS: `/Library/LaunchDaemons/org.jenkins-ci.plist`

위 경로에서 Jenkins 서비스 설정 파일을 찾을 수 있습니다. 설정 파일을 열어 내용을 확인하거나 수정할 수 있습니다. 서비스 설정 파일을 수정하려면 관리자 권한으로 텍스트 편집기를 실행하거나 명령줄에서 sudo를 사용하여 편집기를 실행해야 할 수 있습니다. 변경된 설정이 적용되기 위해서는 Jenkins 서비스를 다시 시작해야 합니다.

## 사용자 입력:
> system.d쪽을 고치려해

## ChatGPT 응답:
> systemd를 사용하여 Jenkins 서비스를 관리하는 경우, 서비스 설정 파일은 다음 경로에 있습니다.

```
/etc/systemd/system/jenkins.service
```

이 파일을 열어서 내용을 확인하거나 수정할 수 있습니다. 파일의 내용은 다음과 같이 보일 것입니다.

```
[Unit]
Description=Jenkins Automation Server
After=network.target

[Service]
User=jenkins
ExecStart=/usr/bin/java -jar /usr/lib/jenkins/jenkins.war
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

위 예제에서는 Jenkins를 jenkins 사용자로 실행하며, 실행 파일은 `/usr/lib/jenkins/jenkins.war` 입니다. 이 파일을 수정하여 Jenkins 서비스를 설정할 수 있습니다. 변경된 설정이 적용되기 위해서는 systemd 데몬을 다시 로드해야 하므로, 다음과 같은 명령어를 사용합니다.

```
sudo systemctl daemon-reload
```

그리고 Jenkins 서비스를 다시 시작합니다.

```
sudo systemctl restart jenkins
```

이제 systemd를 사용하여 Jenkins 서비스를 변경하였습니다.

## 사용자 입력:
> -- Unit jenkins.service has begun starting up.
 3월 15 18:50:31 liabuild systemd[1]: jenkins.service: main process exited, code=exited, status=1/FAILURE
 3월 15 18:50:32 liabuild jenkins[10506]: jenkins: /home/lia/.jenkins is not a directory
 3월 15 18:50:31 liabuild systemd[1]: Failed to start Jenkins Continuous Integration Server.
-- Subject: Unit jenkins.service has failed
위에서 /home/lia/.jenkins는 분명히 directory인데, 아니라고 나오는데 왜 그럴까?

## ChatGPT 응답:
> Jenkins 서비스가 시작할 때 `/home/lia/.jenkins` 디렉토리가 없기 때문에 서비스가 실패한 것으로 보입니다. 

일반적으로 Jenkins는 서비스 시작시 `$JENKINS_HOME` 환경 변수를 확인하고 해당 디렉토리를 사용하게 됩니다. `$JENKINS_HOME` 환경 변수가 설정되어 있지 않거나 해당 디렉토리가 존재하지 않으면 Jenkins는 기본 디렉토리를 사용하게 됩니다. 

Jenkins 서비스를 시작하기 전에 다음을 확인해보세요.

1. `$JENKINS_HOME` 환경 변수가 정확히 설정되어 있는지 확인하세요. `$JENKINS_HOME` 환경 변수는 Jenkins 서비스 설정 파일에 설정되어 있어야 합니다. 

2. `$JENKINS_HOME` 디렉토리가 존재하는지 확인하세요. 만약 디렉토리가 존재하지 않는다면, Jenkins 서비스가 시작하기 전에 디렉토리를 생성해야 합니다. 

다음과 같이 `$JENKINS_HOME` 디렉토리를 생성하고, 해당 디렉토리에 적절한 권한을 부여하면 Jenkins 서비스가 시작될 것입니다.

```
sudo mkdir /home/lia/.jenkins
sudo chown -R jenkins:jenkins /home/lia/.jenkins
```

위 예제에서는 `/home/lia/.jenkins` 디렉토리를 생성하고, 해당 디렉토리에 jenkins 사용자의 권한을 부여하였습니다. 이제 Jenkins 서비스를 다시 시작하여 정상적으로 실행되는지 확인해보세요.

## 사용자 입력:
> 
[Unit]
Description=Jenkins Continuous Integration Server
Requires=network.target
After=network.target

[Service]
Type=notify
NotifyAccess=main
ExecStart=/usr/bin/jenkins
Restart=on-failure
SuccessExitStatus=143

# Configures the time to wait for start-up. If Jenkins does not signal start-up
# completion within the configured time, the service will be considered failed
# and will be shut down again. Takes a unit-less value in seconds, or a time span
# value such as "5min 20s". Pass "infinity" to disable the timeout logic.
#TimeoutStartSec=90

# Unix account that runs the Jenkins daemon
# Be careful when you change this, as you need to update the permissions of
# $JENKINS_HOME, $JENKINS_LOG, and (if you have already run Jenkins)
# $JENKINS_WEBROOT.
User=jenkins
Group=jenkins

# Directory where Jenkins stores its configuration and workspaces
Environment="JENKINS_HOME=/home/lia/.jenkins"
#Environment="JENKINS_HOME=/var/lib/jenkins"
WorkingDirectory=/home/lia/.jenkins
#WorkingDirectory=/var/lib/jenkins


## ChatGPT 응답:
> 위의 서비스 설정 파일에서는 `/home/lia/.jenkins` 디렉토리를 `$JENKINS_HOME` 환경 변수로 사용하도록 설정되어 있습니다. 

그러나 Jenkins 서비스가 시작될 때, 해당 디렉토리에 대한 권한 문제로 인해 서비스가 실패한 것으로 보입니다. 

다음과 같이 `$JENKINS_HOME` 디렉토리를 생성하고, 해당 디렉토리에 적절한 권한을 부여하면 Jenkins 서비스가 시작될 것입니다.

```
sudo mkdir /home/lia/.jenkins
sudo chown -R jenkins:jenkins /home/lia/.jenkins
```

위 예제에서는 `/home/lia/.jenkins` 디렉토리를 생성하고, 해당 디렉토리에 jenkins 사용자의 권한을 부여하였습니다. 이제 Jenkins 서비스를 다시 시작하여 정상적으로 실행되는지 확인해보세요.

## 사용자 입력:
> 위에 jenkins가 아니라 .jenkins라서 문제인가?

## ChatGPT 응답:
> 예, 맞습니다. Jenkins 서비스에서 `$JENKINS_HOME` 환경 변수를 `/home/lia/.jenkins`로 설정하였는데, 이 경로가 잘못되어 있습니다. 

Jenkins 서비스의 `$JENKINS_HOME` 환경 변수는 Jenkins가 설치된 디렉토리가 아니라, Jenkins 서버가 사용하는 데이터와 설정이 저장되는 디렉토리를 지정해야 합니다.

일반적으로 Jenkins는 `/var/lib/jenkins` 디렉토리를 `$JENKINS_HOME` 디렉토리로 사용합니다. 따라서 `$JENKINS_HOME`을 `/var/lib/jenkins`로 수정하여 Jenkins 서비스 설정 파일을 다시 작성해보세요. 

수정된 Jenkins 서비스 설정 파일은 다음과 같이 보일 것입니다.

```
[Unit]
Description=Jenkins Continuous Integration Server
Requires=network.target
After=network.target

[Service]
Type=notify
NotifyAccess=main
ExecStart=/usr/bin/jenkins
Restart=on-failure
SuccessExitStatus=143

# Configures the time to wait for start-up. If Jenkins does not signal start-up
# completion within the configured time, the service will be considered failed
# and will be shut down again. Takes a unit-less value in seconds, or a time span
# value such as "5min 20s". Pass "infinity" to disable the timeout logic.
#TimeoutStartSec=90

# Unix account that runs the Jenkins daemon
# Be careful when you change this, as you need to update the permissions of
# $JENKINS_HOME, $JENKINS_LOG, and (if you have already run Jenkins)
# $JENKINS_WEBROOT.
User=jenkins
Group=jenkins

# Directory where Jenkins stores its configuration and workspaces
#Environment="JENKINS_HOME=/home/lia/.jenkins"
Environment="JENKINS_HOME=/var/lib/jenkins"
#WorkingDirectory=/home/lia/.jenkins
WorkingDirectory=/var/lib/jenkins
```

위와 같이 `$JENKINS_HOME` 환경 변수를 `/var/lib/jenkins`로 수정한 후에, Jenkins 서비스를 다시 시작하여 정상적으로 실행되는지 확인해보세요.

