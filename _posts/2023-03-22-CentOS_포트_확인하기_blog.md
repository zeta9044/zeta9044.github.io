---
layout: post
title: "CentOS 포트 확인하기"
tags: [CentOS, 포트, 확인]
---

## 사용자 입력:
> centos에서 사용하는 port 알아보기

## ChatGPT 응답:
> CentOS에서 사용 중인 포트는 다음과 같은 방법으로 확인할 수 있습니다.

1. netstat 명령어 사용하기
{% raw %}```
sudo netstat -tulpn
```{% endraw %}
위 명령어를 실행하면 현재 열려있는 모든 포트와 해당 포트를 사용하는 프로세스가 출력됩니다.

2. lsof 명령어 사용하기
{% raw %}```
sudo lsof -i -P -n | grep LISTEN
```{% endraw %}
위 명령어를 실행하면 현재 열려있는 포트와 해당 포트를 사용하는 프로세스가 출력됩니다.

3. firewall-cmd 명령어 사용하기
{% raw %}```
sudo firewall-cmd --list-ports
```{% endraw %}
위 명령어를 실행하면 현재 방화벽에서 허용된 포트 리스트가 출력됩니다.

위 방법들 중에서 하나를 선택하여 해당 CentOS 시스템에서 사용 중인 포트를 확인할 수 있습니다.

## 사용자 입력:
> root이외의 유저가 80포트로 webapp을 실행할려면 어떻게 해야하나?

## ChatGPT 응답:
> 일반적으로 CentOS에서 80 포트와 같은 시스템 포트를 사용하려면 root 권한이 필요합니다. 그러나 다른 사용자가 웹 애플리케이션을 실행하려면 다음과 같은 방법을 사용하여 권한을 부여할 수 있습니다.

1. setcap을 사용하여 실행 권한을 부여하기
{% raw %}```
sudo setcap 'cap_net_bind_service=+ep' /path/to/webapp
```{% endraw %}
위 명령어는 /path/to/webapp 경로에 위치한 웹 애플리케이션에 대해 cap_net_bind_service 능력을 부여하여 다른 사용자가 80 포트와 같은 시스템 포트를 사용할 수 있도록합니다.

2. Port forwarding을 사용하여 권한을 부여하기
root 권한으로 서비스를 실행한 후, 일반 사용자가 접근할 수 있도록 로컬 포트를 원격 포트로 포워딩 할 수 있습니다.

예를 들어, 다음 명령어를 사용하여 로컬 8080 포트를 원격 80 포트로 포워딩 할 수 있습니다.
{% raw %}```
sudo ssh -L 8080:localhost:80 user@server_ip
```{% endraw %}
이제 일반 사용자는 http://localhost:8080을 통해 서버에 연결할 수 있습니다.

위 방법들 중에서 하나를 선택하여 사용자가 80 포트와 같은 시스템 포트를 사용할 수 있도록 권한을 부여할 수 있습니다.

## 사용자 입력:
> 네가 제시한 1번을 좀더 자세히 써줘. webapps 아냐?

## ChatGPT 응답:
> 네, 1번 방법은 setcap을 사용하여 다른 사용자가 root 권한 없이 80 포트를 사용할 수 있도록 허용하는 방법입니다.

아래는 자세한 설명입니다.

1. setcap 명령어 설치
CentOS에서 setcap 명령어를 사용하려면 libcap-devel 패키지를 설치해야합니다. 아래 명령어를 사용하여 설치합니다.

{% raw %}```
sudo yum install libcap-devel
```{% endraw %}

2. setcap을 사용하여 권한 부여
setcap 명령어를 사용하여 /path/to/webapp 경로에 위치한 웹 애플리케이션에 대해 cap_net_bind_service 능력을 부여합니다. 이렇게하면 다른 사용자가 해당 포트를 사용할 수 있도록 권한이 부여됩니다.

예를 들어, /path/to/webapp 경로에 위치한 웹 애플리케이션에 대해 권한을 부여하려면 다음과 같이 명령어를 실행합니다.

{% raw %}```
sudo setcap 'cap_net_bind_service=+ep' /path/to/webapp
```{% endraw %}

3. 웹 애플리케이션 실행
이제 웹 애플리케이션을 실행하면 일반 사용자가 80 포트를 사용할 수 있습니다.

위 방법을 사용하여 setcap을 이용해 권한을 부여하면 다른 사용자가 root 권한 없이 80 포트를 사용할 수 있으므로, 웹 애플리케이션을 실행할 때 일반 사용자의 권한으로 실행하는 것이 좋습니다.

## 사용자 입력:
> 위에 제시한 것처럼 디렉토리에 권한을 주는 건가?

## ChatGPT 응답:
> setcap 명령어를 사용하여 권한을 부여하는 것은, 일반적으로 실행 파일이나 스크립트와 같은 특정한 파일에 대한 권한을 부여하는 것입니다. 디렉토리 자체에 권한을 부여하는 것은 아니며, 디렉토리 내의 파일에 대한 권한을 설정하는 것입니다.

즉, setcap 명령어를 사용하여 cap_net_bind_service 능력을 부여하면, 해당 파일을 실행하는 사용자는 root 권한 없이도 80 포트와 같은 시스템 포트를 사용할 수 있습니다. 따라서, 웹 애플리케이션이 위치한 디렉토리에 대한 권한을 변경하는 것은 아니며, 웹 애플리케이션 파일에 대한 권한을 부여하는 것입니다.

따라서, setcap 명령어를 사용하여 권한을 부여하기 위해서는 권한을 부여할 파일의 경로를 지정해주어야 합니다.

## 사용자 입력:
> 위와 같으면, tomcat 실행 스크립트에다가 주는거 아닌가? startup.sh 와 shutdown.sh.

## ChatGPT 응답:
> 맞습니다. 웹 애플리케이션 서버로 Tomcat을 사용할 경우, Tomcat 실행 스크립트에 권한을 부여하는 것이 일반적입니다. Tomcat 실행 스크립트에는 startup.sh, shutdown.sh, catalina.sh 등이 포함됩니다.

따라서, setcap 명령어를 사용하여 cap_net_bind_service 능력을 부여하기 위해서는 Tomcat 실행 스크립트에 대해 권한을 부여해야합니다.

예를 들어, Tomcat이 설치된 /opt/tomcat 디렉토리에서 Tomcat 실행 스크립트에 대해 권한을 부여하려면 다음과 같이 명령어를 실행합니다.

{% raw %}```
sudo setcap 'cap_net_bind_service=+ep' /opt/tomcat/bin/*.sh
```{% endraw %}

이제, Tomcat 실행 스크립트를 사용하여 Tomcat 서버를 시작하거나 중지할 때, 다른 사용자가 80 포트를 사용할 수 있게 됩니다.

## 사용자 입력:
> 위 명령어를 실행할때 상대경로로 지정할 수 있나?

## ChatGPT 응답:
> 네, setcap 명령어를 실행할 때 상대 경로로 지정할 수 있습니다.

예를 들어, 현재 디렉토리에 위치한 웹 애플리케이션 실행 파일에 대해 권한을 부여하려면 다음과 같이 상대 경로를 사용하여 명령어를 실행할 수 있습니다.

{% raw %}```
sudo setcap 'cap_net_bind_service=+ep' ./webapp
```{% endraw %}

위 명령어에서 {% raw %}`./webapp`{% endraw %}은 현재 디렉토리에서 {% raw %}`webapp`{% endraw %} 파일의 경로를 의미합니다. 즉, 현재 디렉토리에 위치한 {% raw %}`webapp`{% endraw %} 파일에 대해 {% raw %}`cap_net_bind_service`{% endraw %} 능력을 부여합니다.

상대 경로를 사용하여 파일 경로를 지정할 때는, 현재 디렉토리를 기준으로 경로를 지정해야합니다. 따라서, 명령어를 실행하기 전에 웹 애플리케이션 파일이 위치한 디렉토리로 이동한 후 상대 경로를 사용하여 명령어를 실행해야합니다.

## 사용자 입력:
>  sudo setcap 'cap_net_bind_service=+ep' ./*.sh
fatal error: Invalid argument
usage: setcap [-q] [-v] (-r|-|<caps>) <filename> [ ... (-r|-|<capsN>) <filenameN> ]

 Note <filename> must be a regular (non-symlink) file.


## ChatGPT 응답:
> 해당 오류는 현재 디렉토리에 위치한 모든 {% raw %}`.sh`{% endraw %} 파일에 대해 권한을 부여하기 위해, {% raw %}`sudo setcap 'cap_net_bind_service=+ep' ./*.sh`{% endraw %} 명령어를 실행했을 때 발생하는 문제입니다.

setcap 명령어는 기본적으로 인자로 전달된 파일이 정규 파일(non-symlink file)인 경우에만 동작합니다. 따라서, 해당 명령어를 사용하여 권한을 부여하기 전에, 인자로 전달된 파일이 정규 파일인지 확인해야 합니다.

따라서, 정규 파일이 아닌 다른 파일(심볼릭 링크 등)이 존재할 경우 위와 같은 오류가 발생할 수 있습니다. 이 경우, 명령어를 실행하기 전에 정규 파일인지 확인하고, 정규 파일만을 대상으로 권한을 부여해야 합니다.

예를 들어, {% raw %}`/opt/tomcat/bin`{% endraw %} 디렉토리에 위치한 모든 {% raw %}`.sh`{% endraw %} 파일에 대해 권한을 부여하려면, 다음과 같이 {% raw %}`find`{% endraw %} 명령어와 함께 사용할 수 있습니다.

{% raw %}```
sudo find /opt/tomcat/bin -type f -name '*.sh' -exec setcap 'cap_net_bind_service=+ep' {} +
```{% endraw %}

위 명령어에서 {% raw %}`-type f`{% endraw %} 옵션은 정규 파일만을 대상으로 검색하도록 지정하고, {% raw %}`-name '*.sh'`{% endraw %} 옵션은 {% raw %}`.sh`{% endraw %} 확장자를 가진 파일만을 대상으로 검색하도록 지정합니다. 그리고 {% raw %}`-exec`{% endraw %} 옵션을 사용하여 검색된 파일에 대해 {% raw %}`setcap`{% endraw %} 명령어를 실행합니다.

이와 같은 방법을 사용하여 권한을 부여하면, 해당 디렉토리에 존재하는 정규 파일에 대해서만 권한을 부여할 수 있습니다.

## 사용자 입력:
> 여전히 안되는데...

## ChatGPT 응답:
> 안타깝게도, 제가 제공한 방법으로도 문제가 해결되지 않는다면, 더 많은 정보를 제공해주셔야 합니다. 

어떤 문제가 발생하고 있는지, 어떤 오류 메시지가 표시되는지, 그리고 명령어를 실행한 시스템이 어떤 환경인지에 대한 정보를 알려주시면 더욱 정확한 답변을 제공할 수 있습니다.

그리고, 앞서 설명한 대로 setcap 명령어를 사용하여 권한을 부여하는 것은 특정 파일에 대해서만 권한을 부여하는 것이므로, 권한을 부여할 파일이 올바른 경로에 위치하고, 해당 파일이 정규 파일인지 확인해야합니다.

만약, 여전히 문제가 발생한다면 다른 방법을 고려해보는 것이 좋습니다. 예를 들어, 일반 사용자가 80 포트를 사용할 수 있도록 Tomcat을 일반 사용자의 계정으로 실행하거나, Tomcat을 80 포트가 아닌 다른 포트(예: 8080)로 실행하고, 일반 사용자가 해당 포트를 사용할 수 있도록 방화벽 등을 설정하는 등의 방법이 있을 수 있습니다.

## 사용자 입력:
> sudo ssh -L 80:localhost:80 user@server_ip 여기서 , user@server_ip는 개별적으로 다 지정하는 거야?


## ChatGPT 응답:
> 네, {% raw %}`sudo ssh -L 80:localhost:80 user@server_ip`{% endraw %} 명령어에서 {% raw %}`user@server_ip`{% endraw %} 부분은 개별적으로 지정해주어야 합니다. 

여기서 {% raw %}`user`{% endraw %}는 원격 서버에 로그인하기 위한 사용자 이름이며, {% raw %}`server_ip`{% endraw %}는 원격 서버의 IP 주소입니다. 

즉, 해당 명령어를 실행하기 전에, 원격 서버에 로그인하기 위한 사용자 계정이 있어야하며, 해당 사용자 계정의 비밀번호를 입력해야합니다. 또한, 원격 서버의 IP 주소도 필요합니다. 

따라서, {% raw %}`user@server_ip`{% endraw %} 부분을 개별적으로 지정하여 해당 명령어를 실행해야합니다.

## 사용자 입력:
> 위에 네가 말한 대로 setcap을 이용해서 tomcat을 실행했는데도, port 80이 아래와 같은 상태로 없어. Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:1521            0.0.0.0:*               LISTEN      3353/docker-proxy
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      18471/sshd
tcp        0      0 0.0.0.0:5432            0.0.0.0:*               LISTEN      30317/docker-proxy
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1741/master
tcp6       0      0 :::50832                :::*                    LISTEN      6500/java
tcp6       0      0 :::1521                 :::*                    LISTEN      3361/docker-proxy
tcp6       0      0 :::22                   :::*                    LISTEN      18471/sshd
tcp6       0      0 :::5432                 :::*                    LISTEN      30324/docker-proxy
tcp6       0      0 ::1:25                  :::*                    LISTEN      1741/master
tcp6       0      0 127.0.0.1:8006          :::*                    LISTEN      32271/java
tcp6       0      0 :::9000                 :::*                    LISTEN      10775/java
udp        0      0 127.0.0.1:323           0.0.0.0:*                           1182/chronyd
udp6       0      0 ::1:323                 :::*                                1182/chronyd


## ChatGPT 응답:
> 현재 목록에 표시된 결과에서는, 80 포트를 사용하는 프로세스가 없는 것으로 보입니다. Tomcat 서버가 80 포트를 사용하도록 설정하였지만, 여전히 80 포트를 사용하는 프로세스가 없는 경우에는 Tomcat 서버가 정상적으로 실행되지 않을 수 있습니다.

확인해야 할 사항들은 다음과 같습니다.

1. Tomcat 서버의 포트 설정
Tomcat 서버의 server.xml 파일에서 포트 설정을 확인하십시오. 보통 Tomcat에서는 HTTP 포트를 8080, HTTPS 포트를 8443으로 기본 설정하고 있습니다. 따라서, Tomcat 서버의 설정 파일에서 HTTP 포트를 80으로 변경한 경우에는, 해당 포트로 접근하기 위해서는 브라우저 주소창에 {% raw %}`http://localhost`{% endraw %}와 같이 포트 번호를 지정해주어야 합니다.

2. Tomcat 서버 실행 확인
Tomcat 서버를 실행하는 과정에서 오류가 발생하지 않았는지 확인하십시오. Tomcat 서버가 올바르게 실행되지 않았다면, 해당 포트가 사용되지 않을 수 있습니다. 이 경우에는 Tomcat 서버를 정상적으로 실행하기 위해 오류를 해결해야합니다.

3. 권한 설정 확인
setcap 명령어를 사용하여 권한을 부여했는지 확인하십시오. 권한이 부여되지 않았다면, 해당 포트를 사용할 수 없을 수 있습니다.

위 세 가지 사항을 확인해보고, 문제를 해결해보시기 바랍니다.

## 사용자 입력:
> tomcat을 80포트로 실행하려면 어떻게 해야 하나?

## ChatGPT 응답:
> Tomcat을 80 포트로 실행하기 위해서는 다음과 같은 작업이 필요합니다.

1. Tomcat 서버의 server.xml 파일을 열어서, Connector 태그에서 port 속성을 80으로 수정합니다.
{% raw %}```
<Connector port="80" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```{% endraw %}
2. Tomcat을 실행하기 위해 일반 사용자 계정으로 로그인합니다.
3. setcap 명령어를 사용하여 해당 사용자에게 {% raw %}`cap_net_bind_service`{% endraw %} 능력을 부여합니다.
{% raw %}```
sudo setcap 'cap_net_bind_service=+ep' /path/to/tomcat/bin/*.sh
```{% endraw %}
4. Tomcat 서버를 실행합니다.
{% raw %}```
sh /path/to/tomcat/bin/startup.sh
```{% endraw %}

위와 같이 설정하고 실행하면, 일반 사용자도 80 포트를 사용하여 Tomcat 서버에 접근할 수 있습니다.

하지만, 80 포트는 시스템 포트 중에서 가장 많이 사용되는 포트 중 하나이므로, 해당 포트를 사용할 때는 보안 이슈가 발생할 수 있습니다. 따라서, 보안 측면을 고려하여 80 포트 대신 다른 포트(예: 8080)를 사용하는 것을 권장합니다.

## 사용자 입력:
> setcap을 하면 log도 root권한으로 생기나?

## ChatGPT 응답:
> setcap 명령어로 설정한 권한은 네트워크 소켓 생성과 관련된 권한을 설정하기 때문에, 파일 쓰기 등의 파일 시스템 권한과는 관련이 없습니다. 따라서, Tomcat이 로그 파일을 생성할 때에는 여전히 Tomcat 프로세스가 실행되는 사용자 계정의 권한으로 로그 파일이 생성됩니다.

즉, setcap 명령어로 권한을 설정하더라도, Tomcat이 로그 파일을 쓰기 위해 생성하는 파일은 여전히 Tomcat이 실행되는 사용자 계정의 권한으로 생성됩니다. 따라서, Tomcat이 로그 파일을 생성하는 디렉토리에 대해 사용자 계정이 쓰기 권한을 가지고 있어야 합니다.

## 사용자 입력:
> sudo startup.sh로 하면, 80포트 실행할 수 있나?

## ChatGPT 응답:
> {% raw %}`sudo startup.sh`{% endraw %} 명령어를 실행하면, 현재 사용자가 아닌 root 권한으로 Tomcat을 실행합니다. 따라서, 해당 명령어를 실행하면 일반 사용자도 80 포트를 사용하여 Tomcat 서버에 접근할 수 있게 됩니다.

하지만, 보안 측면에서 권장되지 않는 방법입니다. root 권한으로 실행하는 것은 보안 취약점으로 이어질 수 있으며, 불필요한 권한 부여를 유발할 수 있습니다. 따라서, 가능하다면 root 권한으로 Tomcat을 실행하는 대신, 일반 사용자 계정으로 Tomcat을 실행하고 권한을 설정하는 것이 좋습니다.

따라서, 일반 사용자 계정으로 Tomcat을 실행하고, setcap 명령어를 사용하여 해당 사용자에게 {% raw %}`cap_net_bind_service`{% endraw %} 능력을 부여하면, 일반 사용자도 80 포트를 사용하여 Tomcat 서버에 접근할 수 있습니다.

## 사용자 입력:
> 해당 사용자에게 cap_net_bind_service 능력을 부여하는 방법 알려줘

## ChatGPT 응답:
> cap_net_bind_service 능력은 root 권한이 아닌 사용자도 특정 포트를 바인딩할 수 있도록 해주는 기능입니다. 따라서, 해당 사용자에게 cap_net_bind_service 능력을 부여하려면 다음과 같은 명령어를 실행하십시오.

{% raw %}```
sudo setcap 'cap_net_bind_service=+ep' /path/to/tomcat/bin/*.sh
```{% endraw %}

위 명령어에서 {% raw %}`/path/to/tomcat/bin/*.sh`{% endraw %} 부분은 Tomcat 서버의 실행 스크립트가 있는 경로를 지정합니다. 보통은 {% raw %}`$CATALINA_HOME/bin/*.sh`{% endraw %}와 같은 경로에 위치하므로, 이를 기준으로 명령어를 실행하면 됩니다.

명령어를 실행하면, 해당 사용자에게 cap_net_bind_service 능력이 부여됩니다. 이제 해당 사용자는 특정 포트를 사용하여 서비스를 실행할 수 있게 됩니다.

