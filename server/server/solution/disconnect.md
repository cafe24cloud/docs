---
description: 본 매뉴얼은 가상서버 접속 시, 발생 가능한 문제들에 대한 해결책을 제시합니다.
---

# 접속 오류 해결 방법

## 1. 일반적인 원인

가상서버 접속 오류의 빈번한 원인은 다음과 같습니다.

접속 문제가 있으실 경우, 아래의 사항들이 지켜졌는지 확인해 주세요.

### (1) 가상서버 OS별 계정 정보

카페24 클라우드에서 제공하는 OS별 가상서버 초기 계정은 다음과 같습니다.

|     OS    |     계정    |        접속 방법       |
| :-------: | :-------: | :----------------: |
|   Rocky   |   rocky   |   rocky@가상서버공인IP   |
|   CentOS  |   centos  |   centos@가상서버공인IP  |
|   Ubuntu  |   ubuntu  |   ubuntu@가상서버공인IP  |
| Almalinux | almalinux | almalinux@가상서버공인IP |





### (2) 방화벽 설정

가상서버 SSH 키페어 접속을 하기 위해서는 가상서버에 아래와 같이 22번(SSH) INBOUND 보안 정책이 추가되어 있어야 합니다.

자세한 방법은 [<mark style="color:blue;">\[방화벽 설정 방법\]</mark>](../../../security/security/config.md)을 참고해 주세요.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/09/902590a1c1bbd8987f32004c56f8dfef_1607499585.jpg" alt=""><figcaption></figcaption></figure>





### (3) 가상서버 상태

가상서버가 "운영중"인지 확인합니다. "중지" 혹은 "재부팅" 중인 서버에는 접속이 불가능합니다.







## 2. 에러별 해결 방안

가상서버 접속 시, 에러별 해결 방안은 다음과 같습니다.

### (1) Server refused our key, Permission denied

* **Putty 접속 시 에러 메시지** : Server refused our key, No supported authentication methods available

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/28/eb155c7cb47be9757e301071d6581fa0_1609145677.jpg" alt=""><figcaption></figcaption></figure>

* **리눅스 터미널 접속 시 에러 메시지** : Permission denied

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/28/0a0c40306aa00d3673915d244c78b7ea_1609145339.jpg" alt=""><figcaption></figcaption></figure>

해당 에러가 발생하는 이유는 아래와 같습니다.

#### a. 가상서버 접속 시 잘못된 계정명을 사용할 경우

해당 내용에 대해서는 [<mark style="color:blue;">\[가상서버 OS별 계정 정보\]</mark>](disconnect.md#1-os)를 참고해 주세요.



#### b. 사용자 계정의 .ssh 디렉터리가 누락되었거나 권한 문제가 있는 경우

가상서버에 접속하여 SSH 키페어 접속에 필요한 디렉터리 및 파일의 권한을 확인합니다.

본 매뉴얼에서는 centos 계정을 예로 들어 설명합니다. "centos"에 해당하는 부분은 접속하고자 하는 계정명으로 바꿔주세요.

SSH 키페어 접속이 불가능할 경우, 다음 매뉴얼을 참고하여 패스워드를 설정하여 콘솔 접속을 해주세요.

* &#x20;[<mark style="color:blue;">\[SSH 키페어 분실 시 해결 방법 - CentOS\]</mark>](../../../security/keypair/lost.md)
* &#x20;[<mark style="color:blue;">\[SSH 키페어 분실 시 해결 방법 - Ubuntu\]</mark>](../../../security/keypair/ubuntu.md)

① **리눅스 홈 디렉터리**의 권한이 **"drwx r-x r-x**"인지 확인합니다. 권한이 일치하지 않을 경우, 다음 명령어로 권한을 변경합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/17/8e91dbb8b3bff0a8a32c591d552511e1_1608174134.jpg" alt=""><figcaption></figcaption></figure>

```shell-session
$ sudo chmod 755 /home
```

② **사용자 홈 디렉터리**의 권한이 **"drwx --- ---**"인지 확인합니다. 권한이 일치하지 않을 경우, 다음 명령어로 권한을 변경합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/17/526a2a4f055c33a386a31be7b7c6b6a8_1608174122.jpg" alt=""><figcaption></figcaption></figure>

```shell-session
$ sudo chmod 700 /home/centos/
```

③ **.ssh 디렉터리**의 권한이 **"drwx --- ---**"인지 확인합니다. 권한이 일치하지 않을 경우, 다음 명령어로 권한을 변경합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/17/8ecd42ceea1938437eaaef81319f75b1_1608173800.jpg" alt=""><figcaption></figcaption></figure>

```shell-session
$ sudo chmod 700 /home/centos/.ssh/
```

④ **authorized\_keys 파일**의 권한이 "**-rw- --- ---**"인지 확인합니다. 권한이 일치하지 않을 경우, 다음 명령어로 권한을 변경합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/04/19/ef6b27bc8461eaf06e9ffc66a62ba393_1650326872.jpg" alt=""><figcaption></figcaption></figure>

```shell-session
$ sudo chmod 600 /home/centos/.ssh/authorized_keys
```



#### c. 키페어 접속을 위한 공개키 정보가 손상된 경우

authorized\_keys 파일이 정상적으로 설정되어 있는지 확인합니다.

```shell-session
$ vi /home/centos/.ssh/authorized_keys
```

authorized\_keys 파일의 내용에 해당 가상서버의 공개키가 등록되어 있는지 확인합니다.&#x20;

없을 경우, 가상서버에 연결된 키페어의 공개키를 복사하여 authorized\_keys 파일에 붙여 넣기 합니다.

가상서버의 공개키는 <mark style="background-color:blue;">콘솔 > 보안 서비스 > SSH 키페어</mark>에서 확인 가능합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/17/8ceb8497d6d5576005c57fa91c87dc54_1608179629.jpg" alt=""><figcaption></figcaption></figure>



#### d. 가상서버에 접속하려는 사용자가 추가 혹은 삭제된 경우

접속하려는 계정 정보가 가상서버에 없는 경우 에러가 발생할 수 있습니다.

계정을 추가하기 위해서는 [<mark style="color:blue;">\[사용자 계정 추가 방법\]</mark>](../../../security/keypair/useradd.md)을 참고해 주세요.



#### e. 개인키의 파일 변환 과정에서 오류가 있는 경우

개인키 파일(.pem)이 알맞은 방법으로 .ppk 파일로 변환되었는지 확인합니다.

해당 과정은 [<mark style="color:blue;">\[SSH 키페어 접속 방법\]</mark>](../connect/keypair.md)을 참고해 주세요.





### (2) Unprotected private key file

개인키 파일의 권한이 너무 열려있어 안전하지 않을 때 발생하는 오류입니다.

보안을 위해 개인키 파일에는 다른 사용자의 읽기, 쓰기 작업을 제한해야 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/17/bbcb2aee3d097d4b2c21668e7c569eb4_1608181998.jpg" alt=""><figcaption></figcaption></figure>

해당 개인키 파일의 권한을 다음과 같이 변경해 주세요.

```shell-session
$ chmod 600 cafe24.pem
```





### (3) Remote host identification has changed

해당 IP로 접속한 적이 있는 가상서버와 RSA 공개키를 교환한 상태에서, 같은 IP에 대한 가상서버가 변경되었을 때 발생하는 에러입니다.

하나의 공인 IP를 여러 가상서버에 번갈아 연결하여 접속 시도할 때 발생합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/17/2e4324cf531521144fe657eab49b341b_1608183765.jpg" alt=""><figcaption></figcaption></figure>

에러 문구에서 **/home/user\_name/.ssh/known\_hosts** 부분을 복사하여 파일을 연 다음, 가상서버의 공인 IP를 검색하고, 해당 문자열을 삭제합니다.

그리고 파일을 저장한 후, 다시 접속을 시도합니다.

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ vi /home/centos/.ssh/known_hosts
</strong></code></pre>





### (4) Network error: Connection timed out

* **Putty 접속 시 에러 메시지** : Network error: Connection timed out

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/28/23d3920fe57610a7d40ad5e1a1ee6136_1609145638.jpg" alt=""><figcaption></figcaption></figure>

* **리눅스 터미널 접속 시 에러 메시지** : port 22: Connection timed out

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/28/d1fcb9a0c61c0e99d542d46a4504f7aa_1609145783.jpg" alt=""><figcaption></figcaption></figure>

위의 에러는 해당 가상서버에 연결된 방화벽에 22번(SSH) INBOUND 보안 정책이 추가되어 있지 않아서 발생한 에러입니다.

따라서 [<mark style="color:blue;">\[방화벽 설정 방법\]</mark>](../../../security/security/config.md)을 참고하여 방화벽 설정을 해주세요.
