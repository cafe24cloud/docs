---
description: 본 매뉴얼은 대규모 Apache 웹서버 동시 접속 유입에 대응 하기 위해 성능을 최적화하는 방법을 소개합니다.
---

# Apache 웹서버 성능 튜닝 방법

## 1. 웹서버 성능 튜닝이 필요한 이유

Apache, Nginx 등의 웹서버를 기본 설정으로 서비스하면 대량의 동시 접속을 처리하지 못해 서비스가 불안정할 수 있습니다.

가령 웹서버를 로드밸런서에 연결하여 사용하는 경우에 웹서버가 요청을 처리하지 못하면 장애가 발생합니다.

이를 방지하기 위해 사전에 웹서버의 성능을 튜닝하여 안정적인 서비스를 할 수 있습니다.

또한, 웹 애플리케이션이 필요로 하는 만큼의 자원을 사용할 수 있도록 리눅스 시스템의 커널 파라미터를 확인하는 작업도 필요합니다.



**매뉴얼 테스트 환경**

* OS : CentOS 8.4.2105
* Kernel : 4.18.0-305.3.1.el8.x86\_64
* Apache HTTP Server : 2.4.37

{% hint style="danger" %}
<mark style="color:red;">**주의 사항**</mark>&#x20;

서비스별로 환경 및 구성 방식이 다르므로 본 매뉴얼에서 제공하는 설정값은 절대적인 것이 아닙니다.

튜닝에 앞서 CPU, RAM과 같은 하드웨어 자원이 충분한지 고려되어야 합니다.

웹 서버를 운영하며 축적한 지표(ex. 최대 동시접속 수, 메모리 사용량)에 따라 최적화된 값을 설정하여야 합니다.
{% endhint %}





## 2. Linux 커널 튜닝

리눅스는 범용적으로 사용하기 위한 기본 설정이 되어 있습니다.

하지만 트래픽을 특수하게 많이 받는 웹 서버 등을 구동할 때는 커널 튜닝을 고려할 필요가 있습니다.



(1) Maximum file count 수정하기

리눅스 전체 시스템에서 활용할 수 있는 최대 파일 개수를 확인합니다.

충분히 큰 값으로 설정되어 있어서 일반적으로는 수정하지 않아도 되지만 필요에 따라 값을 높일 수 있습니다.

```shell-session
$ sudo sysctl -a | grep file-max
fs.file-max = 3257003
```

file-max 값을 400000으로 설정할 경우 다음과 같이 적용합니다.

```shell-session
$ echo "fs.file-max=4000000" | sudo tee -a /etc/sysctl.conf
```



#### (2) Open file Limit 수정하기&#x20;

리눅스는 프로세스별로 시스템의 자원을 얼마나 사용하게 할 것인지를 제한합니다.

접속 요청량에 비해 웹서버가 사용할 수 있는 자원(파일, 프로세스 수 등)이 적으면 서비스가 정상적으로 동작하지 않습니다.

이 경우 사전에 제한 값을 높이는 작업이 필요합니다. 기존 open files limit과 max user processes를 확인합니다.&#x20;

```shell-session
$ ulimit -a | grep open
open files (-n) 1024

$ ulimit -a | grep processes
max user processes (-u) 30798
```

다음과 같이 Hard limit과 Soft Limit을 동일하게 수정할 수 있습니다.

이때 설정하는 값은 앞서 확인한 fs.file-max의 값보다 작아야 합니다.

```shell-session
$ sudo su -

# ulimit -SHn 65536
# ulimit -SHu 65536

# exit
```

설정값을 가상서버 **재부팅 후에도 유지**하기 위해서는 /etc/security/limits.conf 파일을 수정합니다.

```shell-session
$ sudo tee -a /etc/security/limits.conf << EOF
* soft nofile 65536
* hard nofile 65536
* soft nproc 65536
* hard nproc 65536
EOF
```





## 3. Apache 웹서버 튜닝

Apache http 웹서버는 MPM(Multi-Processing Modules, 다중 처리 모듈)을 사용하여 클라이언트로부터 받은 요청을 처리합니다.

Apache 웹서버는 버전 2.4 기준으로 Prefork, Worker, Event 방식 3가지를 제공합니다.

&#x20;

#### (1) MPM 개요

MPM 설정을 활용하면 웹 애플리케이션의 성격에 따라 클라이언트로부터 받은 요청을 어떤 방식으로 처리할 것인지를 결정할 수 있습니다.

MPM에 대한 더 자세한 정보는 [Apache HTTP Server 공식 페이지](https://httpd.apache.org/docs/2.4/mpm.html)에서 확인 할 수 있습니다.

* **prefork**
  * 미리 여러 개의 프로세스를 생성하여 하나의 프로세스가 하나의 요청을 처리하는 방식 (non-thread)
  * 프로세스 간 메모리를 공유하지 않아 안정적
  * 오래된 모듈과 호환이 필요한 서비스에 적합
  * 메모리 사용량이 상대적으로 높음
* **worker**
  * 하나의 프로세스에 연결된 여러 스레드에서 요청을 각각 처리하는 방식 (멀티 스레드)(multi-thread)
  * 스레드들이 메모리를 공유하기 때문에 메모리 사용량이 prefork 방식에 비해 적음
  * 동시접속자가 많은 서비스에 유용
* event
  * Apache 2.4 버전부터 제공됨
  * worker 방식을 기반으로 동작
  * 요청을 분산하는 스레드를 따로 두어 처리 지연을 최소화하는 구조
  * 속도가 가장 빠르며, 동시접속자가 많은 서비스에 유용



#### (2) Apache MPM 설정 확인하기

현재 Apache 웹서버에서 MPM이 어떤 방식으로 동작하는지 확인합니다.

다음과 같이 "Server MPM" 항목으로 확인할 수 있습니다.

가상서버의 운영체제 및 Apache의 버전에 따라 기본 동작 방식이 다르게 설정됩니다.

```shell-session
$ httpd -V | grep MPM
Server MPM: event
```



#### (3) MPM 모듈 변경하기

앞서 확인한 MPM 동작 방식의 변경을 원할 경우 다음을 수행합니다.

00-mpm.conf 파일을 열어 사용하고자 하는 모듈의 주석을 해제하고 기존의 모듈은 주석처리 합니다.

```shell-session
$ sudo vi /etc/httpd/conf.modules.d/00-mpm.conf
```

예를 들어 기존의 event 방법을 prefork 으로 변경하려면 mpm\_prefork\_module을 사용하도록 주석 해제하고 mpm\_event\_module을 주석 처리해야 합니다.

```shell-session
LoadModule mpm_prefork_module modules/mod_mpm_prefork.so
#LoadModule mpm_worker_module modules/mod_mpm_worker.so
#LoadModule mpm_event_module modules/mod_mpm_event.so
```



#### (4) 튜닝 값 추가하기

#### **prefork 방식**

/etc/httpd/conf.modules.d/00-mpm.conf 파일에 다음 설정값을 추가합니다. 수치는 서비스 특성에 맞게 조절하여 주세요.

```xml
<Ifmodule mpm_prefork_module>
  ServerLimit            1024
  StartServers               5
  MinSpareServers          5
  MaxSpareServers        10
  MaxRequestWorkers  1024
  MaxConnectionsPerChild     0
</IfModule>
```

prefork 튜닝 옵션에 대한 상세 설명은 다음과 같습니다.

prefork 방식의 경우 동시 접속을 처리하는 MaxRequestWorkers의 기본값이 256으로 낮기 때문에 적당한 튜닝이 필요합니다.

튜닝 값은 가상서버의 메모리 사용량을 고려하여 산정해야 합니다. 사용 중인 메모리를 고려하여 확인하는 방법은 [가상서버 RAM 사용량에 따른 튜닝값 확인하기](tuning.md#ram)를 참고해 주세요.

| **옵션**                                                                                   | **설명**                                                                    | **기본값** | <p><strong>RAM</strong></p><p><strong>4GB</strong></p> | <p><strong>RAM</strong></p><p><strong>32GB</strong></p> |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------- | ------------------------------------------------------ | ------------------------------------------------------- |
| **ServerLimit**                                                                          | 프로세스 수의 최대값으로,MaxRequestWorkers보다 커야한다.                                   | 256     | 300                                                    | 3050                                                    |
| **StartServers**                                                                         | 웹 서버 가동 시 처음에 실행시킬 프로세스의 수                                                | 5       | 5                                                      | 5                                                       |
| **MinSpareServers**                                                                      | 유지해야 할 여분의 프로세스 수의 최소값                                                    | 5       | 5                                                      | 5                                                       |
| **MaxSpareServers**                                                                      | 여분의 프로세스 수의 최대값                                                           | 10      | 10                                                     | 10                                                      |
| <p><strong>MaxRequestWorkers</strong><br><strong>(MaxClients)</strong></p>               | 클라이언트가 동시에 접속할 수 있는 최대값                                                   | 256     | 300                                                    | 3050                                                    |
| <p><strong>MaxConnectionsPerChild</strong><br><strong>(MaxRequestsPerChild)</strong></p> | <p>자식 프로세스가 종료 되기 전까지 처리할 수 있는 요청의 개수.<br>값이 0이면 프로세스가 종료되지 않음을 의미한다.</p> | 2000    | 2000                                                   | 2000                                                    |



#### **worker 방식**

/etc/httpd/conf.modules.d/00-mpm.conf 파일에 다음 설정값을 추가합니다. 수치는 서비스 특성에 맞게 조절하여 주세요.

```xml
<Ifmodule mpm_worker_module>
  ServerLimit               20
  StartServers               3
  MinSpareThreads        75
  MaxSpareThreads      250
  MaxRequestWorkers   500
  ThreadsPerChild         25
</IfModule>
```

worker 튜닝 옵션에 대한 상세 설명은 다음과 같습니다.

튜닝 값은 가상서버의 메모리 사용량을 고려하여 산정해야 합니다. 사용 중인 메모리를 고려하여 확인하는 방법은 [가상서버 RAM 사용량에 따른 튜닝값 확인하기](tuning.md#ram)를 참고해 주세요.

| **옵션**                                                                     | **설명**                                                                              | **기본값** | <p><strong>RAM</strong></p><p><strong>4GB</strong></p> | <p><strong>RAM</strong></p><p><strong>32GB</strong></p> |
| -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ------- | ------------------------------------------------------ | ------------------------------------------------------- |
| **ServerLimit**                                                            | 프로세스 수의 최대값으로, ServerLimit은 MaxRequestWorkers를 ThreadsPerChild으로 나눈 값보다 크거나 같아야 한다. | 16      | 13                                                     | 123                                                     |
| **StartServers**                                                           | 웹 서버 가동 시 처음에 실행시킬 프로세스의 수                                                          | 3       | 3                                                      | 3                                                       |
| **MinSpareThreads**                                                        | 유지해야 할 여분의 스레드 수의 최소값                                                               | 75      | 75                                                     | 75                                                      |
| **MaxSpareThreads**                                                        | 여분의 프로세스 수의 최대값                                                                     | 250     | 250                                                    | 250                                                     |
| <p><strong>MaxRequestWorkers</strong><br><strong>(MaxClients)</strong></p> | 생성되는 스레드의 최대치로, 클라이언트가 동시에 접속할 수 있는 최대값                                             | 400     | 309                                                    | 3057                                                    |
| **ThreadsPerChild**                                                        | 하나의 프로세스가 처리할 수 있는 최대 스레드의 수                                                        | 25      | 25                                                     | 25                                                      |



#### **event 방식**

/etc/httpd/conf.modules.d/00-mpm.conf 파일에 다음 설정값을 추가합니다. 수치는 서비스 특성에 맞게 조절하여 주세요.

```xml
<Ifmodule mpm_event_module>
  ServerLimit               20
  StartServers               3
  MinSpareThreads        75
  MaxSpareThreads      250
  MaxRequestWorkers   500
  ThreadsPerChild         25
</IfModule> 
```

worker 튜닝 옵션에 대한 상세 설명은 다음과 같습니다.

튜닝 값은 가상서버의 메모리 사용량을 고려하여 산정해야 합니다. 사용 중인 메모리를 고려하여 확인하는 방법은 [가상서버 RAM 사용량에 따른 튜닝값 확인하기](tuning.md#ram)를 참고해 주세요.



#### **가상서버 RAM 사용량에 따른 튜닝 값 확인하기**

웹서버의 높은 성능을 위해서는 RAM 자원을 잘 관리하여 swapping을 방지해야 합니다.&#x20;

메모리 swap이 발생하면 웹페이지의 지연이 발생하며 원활한 서비스가 불가능할 수 있습니다.

이를 방지하기 위해 각 httpd 프로세스(스레드)가 사용 가능한 메모리를 고려하여 웹서버를 튜닝하여야 합니다.&#x20;

다음 명령어로 현재 가상서버에서 사용 중인 RAM 용량과 권장 튜닝 값을 확인할 수 있습니다. &#x20;

{% hint style="danger" %}
<mark style="color:red;">**주의 사항**</mark>

**아래와 같이 제공되는 권장값은 절대적인 수치가 아니며, 웹서버의 정상적인 동작을 보장하지 않습니다.** 웹서버 구성 환경 및 기타 특이 사항을 고려하여 테스트를 통해 적정 값을 설정하여야 합니다. &#x20;
{% endhint %}

해당 명령어는 OS별 웹서비스의 기본 서비스명으로 웹서버의 동작 여부를 검사합니다. ( CentOS, Rocky : "httpd", Ubuntu : "apache2")

따라서 서비스명이 기본 이름과 다를 경우 결과 확인이 불가능합니다.&#x20;

또한, RPM 설치되어 systemd로 관리되는 웹서비스에 대해서만 지원합니다.

스크립트 동작을 위해서는 smem 패키지가 가상서버에 설치되어 있어야 합니다.&#x20;

```shell-session
$ curl -sLf https://cloud-tech.cafe24.com/mpm | bash - 
```

1. 웹서버가 가상서버에서 동작 중인지를 나타냅니다. 웹서버가 Active 상태가 아니면 권장 튜닝 값을 확인할 수 없습니다.&#x20;
2. 웹서버에 설정된 MPM method를 나타냅니다.&#x20;
3. 가상서버의 RAM 사용률을 나타냅니다.&#x20;
   1. total RAM : 가상서버에 할당된 총 메모리
   2. used RAM : 사용된 메모리
   3. RAM used by httpd (total) : 전체 httpd 서비스가 실제로 점유하고 있는 메모리의 양
   4. RAM used by single httpd proc (avg) : 하나의 httpd 프로세스가 사용하는 메모리의 평균값
   5. RAM used by other processes (excepting httpd) : httpd 프로세스를 제외한 프로세스의 메모리 사용량
4. 가상서버의 메모리 사용량을 고려한 MPM 튜닝 값
   1. <mark style="color:red;">**해당 값은 절대적인 값이 아니며, 충분한 테스트를 통해 적용되어야 합니다.**</mark>&#x20;

<figure><img src="../../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>



#### (5) Syntax 체크 및 적용

Apache 웹서버 환경 파일의 구문이 맞는지 확인하기 위해 검사를 진행합니다.

**"Syntax OK"**가 뜨면 설정에 이상이 없음을 의미합니다.

```shell-session
$ httpd -t
Syntax OK
```

Apache HTTP 웹서버를 재시작하여 설정을 적용합니다.

```shell-session
$ sudo systemctl restart httpd
```

앞서 MPM 방식을 변경한 경우, 원하는 모듈로 동작 중인지 확인합니다.

```shell-session
$ httpd -V | grep MPM
Server MPM: worker
```



