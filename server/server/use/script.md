---
description: 가상서버 및 오토스케일 생성 시에 자동 스크립트를 적용하는 방법은 아래와 같습니다.
---

# 자동 스크립트 적용 방법

## 1. 자동 스크립트란?

가상서버를 생성할 때 자동 스크립트 기능을 이용하여 일반적인 구성 작업을 자동으로 수행할 수 있습니다.

자동 스크립트는 가상서버가 최초 부팅될 때 실행되는 스크립트이며, 이후 가상서버를 정지/재시작하는 경우는 실행되지 않습니다.

다음 두 가지 방법으로 자동 스크립트를 작성할 수 있습니다.

#### **a. cloud-init**

cloud-init은 가상서버의 초기 설정을 자동화하는 도구입니다.&#x20;

가상서버 생성 후, 부팅 시 cloud-init 서비스가 스크립트에서 지시된 내용을 수행합니다.&#x20;

스크립트의 시작 부분에 **#cloud-config** 구문을 명시하고, yaml 타입으로 작성합니다.

[Cloud-init 공식 문서](https://cloudinit.readthedocs.io/en/latest/index.html)에서 자세한 정보를 확인할 수 있습니다.

&#x20;

#### **b. shell script**

shell script는 shell(운영체제의 명령 해석기)에서 처리하는 스크립트 형식입니다.

스크립트의 시작 부분에 **#!/bin/bash** 구문을 명시합니다.

&#x20;

&#x20;



## 2. 자동 스크립트 적용 시 주의점

{% hint style="danger" %}
<mark style="color:red;">**주의 사항**</mark>

<mark style="color:red;">자동 스크립트가 올바르게 작성되지 않은 경우, 가상서버가 정상 부팅 및 동작하지 않을 수 있습니다.</mark>

<mark style="color:red;">카페24 클라우드는 보안 강화의 목적으로 키페어 접속 방식을 추천하며, 초기 패스워드 설정을 권장하지 않습니다.</mark> &#x20;

불가피하게 패스워드 설정을 할 경우, 유추하기 어려운 강력한 패스워드를 설정해야 합니다. 특히 root는 시스템의 관리자 계정이므로 보안에 각별한 주의가 필요합니다. &#x20;
{% endhint %}

{% hint style="info" %}
<mark style="color:blue;">**참고 사항**</mark>

&#x20;**\[보안상 강력한 패스워드]**&#x20;

* 알파벳 대문자, 소문자, 특수 문자, 숫자 중 두 종류 이상의 문자 구성과 8자 이상의 길이로 구성된 문자열



**\[보안상 취약하여 설정하면 안 되는 패스워드]**

* 특정 패턴을 갖는 패스워드 : aaabb, 123123, q1w2e3,1qa2ws, p@ssword 등
* 개인정보가 포함된 패스워드: 가족 이름, 생일, 주소 등을 포함하는 문자열
* 이용자의 ID가 포함된 패스워드 : 계정명이 centos인 경우 비밀번호를 cent123으로 설정
* 그 외 모든 유추 가능한 패스워드 : 널리 알려진 단어 및 표현
{% endhint %}

* 자동 스크립트를 실행할 때 외부 환경의 접근이 필요할 경우, 해당 가상서버에 적절한 방화벽 설정이 되어 있어야 합니다.
* 자동 스크립트를 적용한 스냅샷의 이미지로 새로운 가상서버를 생성할 경우, 자동 스크립트는 동일하게 동작합니다. 하지만 때에 따라 예외적인 상황도 존재하므로, 스냅샷으로 가상서버를 생성할 때 이전과 동일한 스크립트를 적용하는 것을 권장합니다.
* 입력된 스크립트는 root 사용자 권한으로 실행되며, 파일 또한 root 사용자 소유권으로 생성됩니다.
* 자동 스크립트에 지정된 명령은 대화형으로 실행할 수 없습니다. 따라서 패키지를 설치할 때 -y 옵션을 사용하는 등 스크립트가 중단되지 않도록 해야 합니다.
* 자동 스크립트를 이용해 패키지 설치를 할 때, 때에 따라 시간이 다소 소요될 수 있습니다.
* 로그 설정으로 진행 상황을 확인할 수 있습니다.







## 3. 자동 스크립트 적용 방법

자동 스크립트는 가상서버와 오토스케일 생성 단계에서 적용할 수 있습니다.

* **가상서버 매뉴얼** : [\[가상서버 생성 방법\]](../create.md)
* **오토스케일 매뉴얼** : [\[오토스케일 사용 방법\]](../../autoscale/use.md)

가상서버 및 오토스케일을 생성하는 팝업에서 다음 부분에 스크립트를 작성합니다.

<div align="left">

<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

</div>







## 4.  cloud-init 예시

### (1) 초기 패스워드 설정

**a. 일반 계정의 패스워드 설정**

가상서버의 일반 계정(centos, ubuntu 등)의 패스워드를 다음 스크립트로 초기화할 수 있습니다.

ssh\_pwauth를 True로 입력하여 패스워드 방식으로 SSH 접속을 하도록 설정합니다.

password 필드에 원하는 패스워드를 기재합니다.

```yaml
#cloud-config
password: [비밀번호]
chpasswd: { expire: False }
ssh_pwauth: True
```



#### b. 특정 계정의 패스워드 설정

다음과 같이 특정 계정에 대한 패스워드를 설정할 수 있습니다.

password 필드에는 원하는 패스워드를 기재합니다.

```yaml
#cloud-config
ssh_pwauth: True
chpasswd:
    list: |
        [계정명]:[비밀번호]
    expire: False
```





### (2) 패키지 업데이트/업그레이드

다음 스크립트로 표준 cloud 배포판 이미지의 패키지 및 레퍼지토리를 업데이트/업그레이드할 수 있습니다.

```yaml
#cloud-config
package_update: true
package_upgrade: true
repo-upgrade: true
repo-upgrade: all
```



&#x20;

### (3) 스크립트 로깅 설정

**output** 속성을 사용하여 스크립트의 실행 로그를 지정한 파일에 저장할 수 있습니다.

cloud-init 패키지에서 기본으로 사용하는 /var/log/cloud-init.log 와 겹치지 않는 경로를 사용해야 합니다.

```yaml
#cloud-config
package_update: true
package_upgrade: true
repo-upgrade: true
repo-upgrade: all
output: {all: '| tee -a /var/log/cloud-init-output.log'} 
```

가상서버에 SSH 접속한 뒤 cloud-init 스크립트의 진행 상태를 다음 명령어로 실시간 확인할 수 있습니다.

cloud-init 스크립트에서 지정한 로그 파일의 마지막 10줄을 출력합니다.

```shell-session
$ tail -f /var/log/cloud-init-output.log
```



&#x20;&#x20;

### (4) selinux 비활성화

**runcmd** 모듈을 사용해서 최초 부팅된 가상서버의 OS에서 명령어를 실행할 수 있습니다.

다음 두 가지 방법으로 작성 가능합니다.

```yaml
#cloud-config
runcmd:
  # 방법1) 배열 형식의 입력 ','로 구분하여 작성.
  - [ ls, -al, /root ]
  # 방법2) 실제 OS에서의 명령어 입력 방식과 동일하게 작성.
  - ls -al /root 
```

이를 활용하여 redhat 계열 OS 이미지에서 기본 동작 하는 selinux 설정을 disable 할 수 있습니다.&#x20;

설정 파일을 수정한 후 서버를 재부팅 하도록 설정합니다.

```yaml
#cloud-config
runcmd:
  - sed -i "s/SELINUX=enforcing/SELINUX=disabled/" /etc/sysconfig/selinux
  - sed -i "s/SELINUX=enforcing/SELINUX=disabled/" /etc/selinux/config
  - reboot
```



&#x20;

### (5) text 형식의 파일 생성

**write\_files**을 사용하여 가상서버에 파일을 자동 생성할 수 있습니다.

path에 대한 값으로 파일을 생성할 경로를 지정합니다.

contents의 값으로 해당 파일에 저장할 내용을 입력합니다.

이때 파이프(|)를 사용하면 해당 value 값의 줄 바꿈을 인식하여 파일에 저장합니다.

```yaml
#cloud-config
write_files:
  - path: 파일 경로
    content: |
        Hello World!
        Hello Cloud-init! 
```

이를 응용하여 nginx 설치에 필요한 레퍼지토리 설정 파일을 생성하고 nginx를 설치할 수 있습니다.

아래 예시는 CentOS 7 기준으로, 다른 OS의 경우 [nginx 공식문서](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/)를 참고하여 작성해야 합니다.

```yaml
#cloud-config
write_files:
  - path: /etc/yum.repos.d/nginx.repo
    content: |
        [nginx]
        name=nginx repo
        baseurl=http://nginx.org/packages/centos/7/$basearch/
        gpgcheck=0
        enabled=1
packages:
  - nginx
output: {all: '| tee -a /var/log/cloud-init-output.log'}
```





### (6) 애플리케이션 패키지 설치

cloud-init으로 가상서버에 패키지를 자동 설치할 수 있습니다.

패키지에 따라 설치 시간이 소요되는 경우가 있어 가상서버 접속 후 바로 확인이 불가능 할 수 있습니다.

이 경우 **output**으로 스크립트에 대한 로그를 남겨 진행 상황을 확인합니다.

<mark style="color:red;">가상서버의 OS 이미지에서 지원하는 패키지를 설치하여야 합니다.</mark>

#### a. 패키지 설치

**packages**를 사용해 패키지를 자동 설치합니다.

예시와 같이 패키지명 또는 패키지의 특정 버전을 명시할 수 있습니다.

```yaml
#cloud-config
packages:
  - 패키지명
  - [패키지명, 버전]
```

&#x20;

#### b. APM 설치

다음과 같이 APM(Apache+PHP+MariaDB)패키지를 한 번에 설치할 수 있습니다.

CentOS의 경우 httpd, Ubuntu의 경우 apache2 패키지를 설치합니다.

필요에 따라 **runcmd**를 사용하여 데몬 enable 및 start를 하고, **output**으로 cloud-init 스크립트에 대한 로그 파일을 남길 수 있습니다.

```yaml
#cloud-config
packages:
  - httpd
  - php
  - mariadb-server
runcmd:
  - systemctl enable httpd
  - systemctl enable mariadb
  - systemctl start mariadb
  - systemctl start httpd
output: {all: '| tee -a /var/log/cloud-init-output.log'}
```

&#x20;



### (7) timezone 변경

다음과 같이 부팅 단계에서 가상서버의 타임존을 KST로 변경할 수 있습니다.

카페24 클라우드의 가상서버는 기본적으로 UTC 시간대로 생성됩니다.

```yaml
#cloud-config
timezone: Asia/Seoul
```



&#x20;

### (8) 응용 예시

[4. cloud-init 스크립트 예시](script.md#4.-cloud-init)에서 알아본 모듈들을 혼합하여 다음과 같이 사용할 수 있습니다.

```yaml
#cloud-config
package_update: true
package_upgrade: true
repo-upgrade: true
repo-upgrade: all
write_files:
  - path: /etc/yum.repos.d/nginx.repo
    content: |
        [nginx]
        name=nginx repo
        baseurl=http://nginx.org/packages/centos/7/$basearch/
        gpgcheck=0
        enabled=1
packages:
  - httpd
  - php
  - mariadb-server
ssh_pwauth: True
chpasswd:
    list: |
        centos:cafe@$P@sswd
    expire: False
runcmd:
  - systemctl enable httpd
  - systemctl enable mariadb
  - systemctl start mariadb
  - systemctl start httpd
  - sed -i "s/SELINUX=enforcing/SELINUX=disabled/" /etc/sysconfig/selinux
  - sed -i "s/SELINUX=enforcing/SELINUX=disabled/" /etc/selinux/config
  - reboot
timezone: Asia/Seoul
output: {all: '| tee -a /var/log/cloud-init-output.log'}
```

&#x20;





## 5. shell script 예시

본 예시는 CentOS 기준으로 작성되었으며, OS 별로 적절한 명령어를 작성해야 합니다.&#x20;

### (1) 초기 패스워드 설정

가상서버의 일반 계정(centos, ubuntu 등)의 패스워드를 다음 스크립트로 초기화 할 수 있습니다.

```shell
#!/bin/bash
sed -i $'s/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
systemctl restart sshd
echo 'centos:[비밀번호]' | sudo chpasswd
```

&#x20;



### (2) 애플리케이션 패키지 설치

shell에서 동작하는 명령어와 동일하게 구성하여 스크립트화 할 수 있습니다.

패키지 설치의 경우 -y 옵션을 필수적으로 설정하여 스크립트가 중지 없이 동작하도록 해야 합니다.

```shell
#!/bin/bash
yum install -y httpd php mariadb-server
```



&#x20;

### (3) timezone 변경

다음과 같이 부팅 단계에서 가상서버의 타임존을 KST로 변경할 수 있습니다.

카페24 클라우드의 가상서버는 기본적으로 UTC 시간대로 생성됩니다.

```shell
#!/bin/bash
timedatectl set-timezone Asia/Seoul
```



&#x20;

### (4) 응용 예시

[5. Shell 스크립트 예시](script.md#5.-shell)에서 알아본 예제들을 혼합하여 다음과 같이 사용할 수 있습니다.

```shell
#!/bin/bash
## 패스워드 접속 설정
sed -i $'s/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
systemctl restart sshd
echo 'centos:cafe@$P@sswd' | sudo chpasswd
## 타임존 설정
timedatectl set-timezone Asia/Seoul
## 패키지 업데이트
yum update
## APM 설치
yum install -y httpd php mariadb-server
## 데몬 enable 및 start
systemctl enable httpd
systemctl enable mariadb
systemctl start httpd
systemctl start mariadb
```
