---
description: 로드밸런서 생성 방법은 다음과 같습니다. 본 매뉴얼은 가상서버에 Apache HTTP 웹서버를 설치하여 진행하였습니다.
---

# 로드밸런서 사용 방법

## 1. 로드밸런서란?

로드밸런서는 여러 대의 가상서버로 트래픽을 효율적으로 분산시키는 장치입니다.

이를 위해 원하는 프로토콜과 알고리즘을 선택하여 가상서버로 트래픽을 포워딩 할 규칙을 추가 할 수 있습니다.

로드밸런서의 포트로 들어간 트래픽을 가상서버 포트로 포트포워딩 하여 서버에 전달하게 됩니다.

<figure><img src="../../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>







## 2. 로드밸런서 생성 및 설정

### (1) 로드밸런서 생성

<mark style="background-color:blue;">웹콘솔>네트워킹>로드밸런서>새로운 로드밸런서 생성</mark>

**"새로운 로드밸런서 생성"** 버튼을 클릭합니다.

<figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

### **- 지원 프로토콜**

TCP와 HTTPS 프로토콜의 포트의 경우 사용자 지정이 가능합니다.

**a. TCP protocol**

데이터 전송 없이 tcp Handshake로 연결 하는 프로토콜입니다. TCP의 모니터링 대상 경로는 없습니다.



**b. HTTP protocol**

URL Path 디폴트로 "/"를 사용하며 url\_path 를 검색하는데 사용하는 http Method 방식은 "get" 방식 입니다.

정상을 의미하는 http 상태 코드 는 "200,201,202" 입니다.



**c. HTTPS protocol**

HTTP와 동일하게 동작하지만 연결된 가상서버의 SSL 인증서에 대한 유효성 검사를 수행합니다.

SSL 인증서가 없는 경우 에러가 발생하므로 대용으로 TLS-HELLO 방식의 프로토콜을 사용할 수 있습니다.

HTTPS의 모니터링 대상 경로는 없습니다.



**d. TERMINATED\_HTTPS protocol**

해당 프로토콜을 사용하면 인증서를 로드밸런서에 등록하여 암호화의 종단점을 로드밸런서로 지정하게 됩니다.&#x20;

XFF(X-Forwarded-For) header를 사용하여 HTTPS 요청을 한 실제 클라이언트 IP를 웹서버에 로그로 남길 때 유용합니다.&#x20;

TERMINATED\_HTTPS를 사용하면 아래 예시와 같이 통신하게 됩니다.

<figure><img src="../../.gitbook/assets/image (8) (4).png" alt=""><figcaption></figcaption></figure>



### **- 지원 알고리즘**

#### a. Round Robin

연결된 가상서버들에 순차적으로 요청을 전달하는 방식으로, 보편적으로 사용됩니다.

만약 로드밸런서에 연결된 서버 A, B, C가 있다면 요청을 A -> B -> C -> A 순으로 분산합니다.

모든 서버에 동일한 비율로 부하가 주어지기 때문에 비슷한 스펙의 서버들로 이뤄진 환경에서 주로 사용합니다.

&#x20;

#### b. Least Connection

활성화된 커넥션을 실시간으로 감지하여 부하가 가장 적은 서버에게 요청을 전달합니다.

&#x20;

**c. Source IP Hash**

{% hint style="danger" %}
<mark style="color:red;">**참고 사항**</mark>

Source IP Hash 알고리즘은 모든 서버에 같은 비율로 부하분산 하지 않으므로 특정 서버에 접속이 몰릴 위험성이 있습니다.
{% endhint %}

Source IP 주소를 바탕으로 Hash 한 결과를 이용하여 요청을 분산하는 방법입니다.

동일한 출발지 IP에서 오는 요청은 지속적으로 하나의 하위 서버에 할당 됩니다.

&#x20;

&#x20;

### **- 모니터링 대상 경로**

선택하는 프로토콜에 따라 모니터링 대상 경로는 다음과 같습니다.

#### &#x20;a. TCP

데이터 전송 없이 TCP handshake로 통신합니다. 따라서 모니터링 대상 경로는 "/"를 탐색하며 따로 지정할 수 없습니다.

&#x20;

#### b. HTTP

디폴트로 "/" 경로를 모니터링 합니다. 하지만 "/"로 설정할 경우 서비스 접속이 원활하지 않을 수 있습니다.

{% hint style="info" %}
<mark style="color:blue;">**참고 사항**</mark>

가상서버에 index.html과 같은 파일을 생성하여 "/index.html"과 같은 static한 경로로 설정하는 것을 권장합니다.
{% endhint %}

&#x20;

#### c. HTTPS

HTTPS 프로토콜의 경우 모니터링 대상 경로는 디폴트로 "/"를 탐색하며 따로 지정할 수 없습니다.

&#x20;

#### c. TERMINATED\_HTTPS

디폴트로 "/" 경로를 모니터링 합니다. 하지만 "/"로 설정할 경우 서비스 접속이 원활하지 않을 수 있습니다.

{% hint style="info" %}
<mark style="color:blue;">**참고 사항**</mark>

가상서버에 index.html과 같은 파일을 생성하여 "/index.html"과 같은 static한 경로로 설정하는 것을 권장합니다.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (10) (2).png" alt=""><figcaption></figcaption></figure>

Terminated\_https 프로토콜을 선택한 경우, 적용할 인증서를 선택합니다.

인증서를 등록하는 방법은  [\[인증서 사용 방법\]](../../security/certificate/use.md)를 참고해 주세요.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>





### (2) 가상서버 연결

<mark style="background-color:blue;">웹콘솔>네트워킹>로드밸런서>상세정보>가상서버 연결</mark>

로드밸런서 메뉴에서 해당 로드밸런서를 선택 한 후, 상세정보 탭에서 가상서버 연결 메뉴를 클릭합니다.

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

로드밸런서에 연결할 가상서버를 선택해 주세요.

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

이때, 연결하는 가상서버에 등록된 방화벽에는 "**내부 네트워크 접속 허용**"이 되어 있어야 합니다.\


<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>





### (3) IP 제한 설정

<mark style="background-color:blue;">웹콘솔>네트워킹>로드밸런서>상세정보>IP 제한 설정</mark>

해당 포트포워딩 룰에 대한 IP 제한 설정을 할 수 있습니다.

따로 설정하지 않으면 모든 IP의 접근을 허용하므로 필요에 따라 지정해 주세요.

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>





### (4) 포워딩 룰 설정

<mark style="background-color:blue;">웹콘솔>네트워킹>로드밸런서>상세정보>포워딩 룰 설정</mark>

생성된 로드밸런서에 대한 포워딩 룰을 추가 및 삭제할 수 있습니다.&#x20;

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Terminated\_https 프로토콜을 선택한 경우, 로드밸런서에 적용된 인증서를 변경할 수 있습니다.

리스트에는 인증서 상태가 '**정상**', '**확인 불가**'인 것만 노출됩니다.

인증서를 등록하는 방법은 [\[인증서 사용 방법\]](../../security/certificate/use.md)를 참고해 주세요.

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>





### (5) 소속 가상서버 연결 상태 확인

<mark style="background-color:blue;">웹콘솔>네트워킹>로드밸런서>상세정보>소속 가상서버 연결 상태</mark>

해당 로드밸런서에 연결된 가상서버들의 상태를 확인 할 수 있습니다.

각각의 포워딩 룰에 따른 가상서버의 연결 상태 모니터링이 가능합니다.

연결된 가상서버에서 포워딩 룰에 대한 포트가 열려 있지 않은 경우 통신이 불가능 합니다.

연결 상태가 '부분 이상', '오류' 인 경우 가상서버에서 해당 포트를 listening 상태로 만들어 주세요.

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>







## 3. 포워딩 프로토콜 별 사용 예제

아래와 같은 웹서버 설정에 앞서 로드밸런서에 적절한 포워딩 룰이 설정되어 있어야 합니다.

### (1) HTTP 프로토콜

#### a. 웹서버 패키지 설치

```shell-session
[centos@demotest2 ~]$ sudo yum install httpd
```

&#x20;

#### b. Apache 웹서버의 Document Root에 index.html 생성

a. **"cd /var/www/html/"** 명령어로 이동합니다.

b. **"touch index.html"** 명령어로 파일을 생성합니다

이때의 Docoument Root는 httpd.conf 파일의 Document Root와 일치하도록 합니다.

여기서 index.html은 로드밸런서에서 80번 포트로 접속할 첫 번째 페이지입니다.

파일의 이름과 확장자가 httpd.conf 파일의 DirectoryIndex와 같도록 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/23/c49b7b439bbd09bfa881a094a1c3a006_1600834187.jpg" alt=""><figcaption></figcaption></figure>

httpd.conf 파일에서 위의 설정을 확인합니다. httpd.conf 파일의 경로는 /etc/httpd/conf/httpd.conf 입니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/23/6eb5e385a5d2a8923d96e1926100744e_1600834598.jpg" alt=""><figcaption></figcaption></figure>

#### c. Apache 데몬 실행

데몬을 실행하여 active 상태인 것을 확인합니다.

enable은 서버를 재부팅 할 경우 해당 서비스를 자동으로 활성화 시키는 명령어입니다.

```shell-session
[centos@demoserver1 html]$ sudo systemctl enable httpd
[centos@demoserver1 html]$ sudo systemctl restart httpd
[centos@demoserver1 html]$ sudo systemctl status httpd
```

#### d. 브라우저 확인

브라우저에 **"http://로드밸런서 공인ip"** 또는 **"로드밸런서 ip에 등록한 도메인"**을 입력해 주세요.

연결된 가상 서버의 Default root에 위치한 index.html 파일로 이동합니다.

브라우저를 새로고침 할 때마다 지정한 알고리즘에 따라 트래픽이 분산 되는 것을 확인 할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (21) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>





### (2) HTTPS 프로토콜

[HTTP 프로토콜 사용 예제](use.md#1-http)에 대한 내용이 선행되었음을 가정합니다.

#### a. 구매한 인증서 설정

```shell-session
[centos@demoserver1 ~]$ sudo cp server.key /etc/httpd/conf/
[centos@demoserver1 ~]$ sudo cp server.crt /etc/httpd/conf/
```

개인키 파일(.key)와 인증서 파일(.crt)를  /etc/httpd/conf/ 디렉토리로 복사합니다.



#### b. httpd.conf 수정

**Port 추가**

웹 서버의 설정 파일 (매뉴얼의 경우는 /etc/httpd/conf/httpd.conf)에 들어가 웹서버에 포트를 추가해 줍니다.

42번 라인으로 간 다음 HTTPS 통신을 위한 443번 포트를 추가해 주세요.

이때 포트는 443 뿐만 아니라 사용자 지정 가능합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/23/16896ed485d12de550e3f8ec8e083a0d_1600821827.jpg" alt=""><figcaption></figcaption></figure>

**httpd.conf 수정**

httpd.conf 파일의 맨 아래쪽에 다음과 같은 내용을 추가합니다.

본 설정으로 SSL 인증서 파일과 개인키 파일의 위치를 지정합니다.

DocumentRoot의 경우 고객님의 index 파일이 있는 경로를 지정해 주세요.

```xml
NameVirtualHost *:443
<VirtualHost *:443>
 SSLEngine on
 SSLCertificateFile /etc/httpd/conf/server.crt
 SSLCertificateKeyFile /etc/httpd/conf/server.key
 SetEnvIf User-Agent ".*MSIE.*" nokeepalive ssl-unclean-shutdown
 CustomLog logs/ssl_request_log "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x "%r" %b"
 DocumentRoot /var/www/html
</VirtualHost>
```

&#x20;

#### **c. Apache 재시작**

**"sudo systemctl  restart httpd"**와 **"sudo systemctl status httpd"** 명령어로 Apache 데몬을 재시작 하고, 상태를 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/23/fe8453c11dfc1fa295d0836ae9c55f27_1600822358.jpg" alt=""><figcaption></figcaption></figure>



#### d. 브라우저 확인

브라우저에 "https://로드밸런서 공인ip" 또는 로드밸런서 ip에 등록한 도메인을 입력해 주세요.

연결된 가상 서버의 Default root에 위치한 index.html 파일로 이동합니다.

새로고침을 하면 로드밸런서에 연결된 웹 서버에 지정한 알고리즘으로 트래픽이 분산되는 것을 확인 할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/16/fa71f3110aad03ecc3d10a996bd34cf3_1647392223.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/16/09d445bd36b8645a715174dbb8e9168d_1647392273.jpg" alt=""><figcaption></figcaption></figure>

&#x20;



### (3) TCP 프로토콜

[HTTP 프로토콜 사용 예제](use.md#1-http)에 대한 내용이 선행되었음을 가정합니다.

#### a. httpd.conf 파일 설정

httpd.conf 파일에서 tcp로 허용할 포트를 입력합니다.

본 예제에서는 8080으로 지정했습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/23/907a8f7c6c4339dba92a3ca0a9f8d0d4_1600836342.jpg" alt=""><figcaption></figcaption></figure>

httpd.conf 파일의 맨 아래쪽에 위의 설정을 입력합니다.

포트와 DocumentRoot는 고객님의 경우에 맞게 지정해 주세요.

```xml
NameVirtualHost *:8080
<VirtualHost *:8080>
 DocumentRoot /var/www/html
</VirtualHost>
```



#### b. Apache 데몬 실행

a. **"sudo systemctl restart httpd"** 명령어로 httpd를 재시작합니다.

b. **"sudo systemctl status httpd**" 명령어로 데몬의 상태를 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/23/b23d0b164c9c8296b254e42e41e6e76f_1600834704.jpg" alt=""><figcaption></figcaption></figure>

&#x20;

#### c. 브라우저 확인

브라우저에 "http://로드밸런서 공인ip:TCP포트" 또는 "로드밸런서 ip에 등록한 도메인:TCP포트"를 입력해 주세요.

연결된 가상 서버의 Default root에 위치한 index.html 파일로 이동합니다.

브라우저를 새로고침 할 때마다 지정한 알고리즘에 따라 트래픽이 분산 되는 것을 확인 할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/10/05/13da31fefac64e4fff2e4c3987f0a677_1601882111.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/10/05/aab5540a235f95360e116ff0a36011c1_1601882126.jpg" alt=""><figcaption></figcaption></figure>

&#x20;

&#x20;



### (4) TERMINATED\_HTTPS 프로토콜

[HTTP 프로토콜 사용 예제](use.md#1-http)에 대한 내용이 선행되었음을 가정합니다.

또한 포워딩 룰 설정 시 정상적인 인증서가 등록되어 있어야 합니다.&#x20;

&#x20;

#### a. 인증서 도메인에 로드밸런서의 공인 IP 등록

로드밸런서에 등록한 인증서 도메인에 공인 IP를 레코드로 등록합니다.&#x20;

예를 들어 도메인이 "\*.cafe24clouds.com"라면 다음과 같이 설정할 수 있습니다.&#x20;

도메인 등록 방법에 대해서는 [\[DNS 사용 방법\]](../dns/use.md)을 참고해 주세요.&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/16/d5ff60021d2785e1248ba650b173fe83_1647359809.jpg" alt=""><figcaption></figcaption></figure>

#### b. httpd.conf 파일 설정

httpd.conf 파일의 LogFormat에 XFF header를 추가하여 웹서버 access log에서 실제 클라이언트의 IP를 확인할 수 있습니다.

"log\_config\_module" 모듈에서 CustomLog로 지정하여 사용중인 LogFormat에 **%{X-Forwarded-For}i** 를 추가합니다.

```xml
<IfModule log_config_module>
    LogFormat "%h [ %{X-Forwarded-For}i ] %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    LogFormat "%h %l %u %t \"%r\" %>s %b" common
    <IfModule logio_module>
      LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
    </IfModule>
    CustomLog "logs/access_log" combined
</IfModule>
```

&#x20;

#### c. Apache 재시작

**"sudo systemctl  restart httpd"**와 **"sudo systemctl status httpd"** 명령어로 Apache 데몬을 재시작하고, 상태를 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/23/fe8453c11dfc1fa295d0836ae9c55f27_1600822358.jpg" alt=""><figcaption></figcaption></figure>

&#x20;

#### d. 브라우저 확인

브라우저에 "https://도메인"을 입력해 주세요.

연결된 가상 서버의 Default root에 위치한 index.html 파일로 이동합니다.

새로고침을 하면 로드밸런서에 연결된 웹 서버에 지정한 알고리즘으로 트래픽이 분산되는 것을 확인 할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/16/87c10691d709d3afd6d265f9609a8485_1647360689.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/16/b496a22bd9dcea2e1e0cd0567b31a39b_1647360701.jpg" alt=""><figcaption></figcaption></figure>

&#x20;

#### e. 웹서버에서 클라이언트 IP 확인하기

로드밸런서에 연결한 하위 웹서버의 access log에 실제 클라이언트의 IP가 수집되는 것을 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/16/d5efc85cdbef1a0928f9ef85dd1aabab_1647361421.jpg" alt=""><figcaption></figcaption></figure>

