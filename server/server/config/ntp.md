---
description: >-
  가상서버 시간 동기화를 하는 방법은 아래와 같습니다. 카페24클라우드의 가상서버는 기본적으로 UTC 시간대로 설정되어있습니다. 변경이 필요한
  경우 본 매뉴얼을 참고해 주세요.
---

# NTP 구성 방법

## 1. 가상서버 timezone 변경

가상서버의 local time은 다음과 같이 변경할 수 있습니다.

#### (1) 타임존 검색하기

다음 명령어로 timezone에 대한 리스트를 확인합니다.

```shell-session
$ timedatectl list-timezones
```



#### (2) timezone 변경

동기화가 필요한 타임존으로 변경합니다. 다음 명령어로 한국 표준시로 설정합니다.

```
$ sudo timedatectl set-timezone Asia/Seoul
```



#### (3) 시간 동기화 확인

다음 명령어로 local time이 정상적으로 바뀌었는지 확인합니다.

```
$ date
Thu Feb 18 13:21:22 KST 2021
```



#### (4) 원격지 NTP 서버와의 동기화

해당 작업은 영구적으로 적용되지 않습니다.

&#x20;시간 동기화를 영구적으로 지속하기 위해서는 NTP 서버와 동기화시키는 작업이 필요합니다.

이를 위해 chrony 또는 ntpd를 이용할 수 있습니다. 다음 중 원하는 방법을 선택하여 수행해 주세요.

단, CentOS8부터는 ntp를 지원하지 않으니 chrony 사용을 권장합니다.

<mark style="color:red;">가상서버에서는 Hardware clock에 직접적으로 접근하지 않아 hwclock 등의 명령어로 시간 설정을 할 수 없습니다.</mark>

* [**chrony로 설정하기**](ntp.md#2-1.-chrony)
* [**ntp로 설정하기**](https://console.cafe24.com/support/faq/view?idx=191#B-2)

##

## 2. 원격지 ntp 서버와 시간 동기화

### 2-1. chrony로 설정하기

#### (1) ntpd 중지

ntp가 동작 중인 경우 이를 중지시킵니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo systemctl stop ntpd
$ sudo systemctl disable ntpd
```
{% endtab %}

{% tab title="Ubuntu" %}
<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ sudo systemctl stop ntp
</strong>$ sudo systemctl disable ntp
</code></pre>
{% endtab %}
{% endtabs %}



#### (2) chrony 패키지 설치

&#x20;chrony를 이용하여 가상서버의 시간을 NTP 서버와 동기화 할 수 있습니다.

OS별 설치 방법은 다음과 같습니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo yum install chrony
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo apt install chrony
```
{% endtab %}
{% endtabs %}



#### (3) chrony 데몬 동작 시키기&#x20;

chronyd 데몬을 동작 시킵니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo systemctl start chronyd
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo systemctl start chrony
```
{% endtab %}
{% endtabs %}

서버 재 부팅 시에도 chronyd 데몬이 자동 시작되도록 설정합니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo systemctl enable chronyd
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo systemctl enable chronyd
```
{% endtab %}
{% endtabs %}



#### (4) chrony.conf 파일 수정 (생략 가능)

chrony.conf 파일을 vi로 열어봅니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo vi /etc/chrony.conf
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo vi /etc/chrony/chrony.conf
```
{% endtab %}
{% endtabs %}



원하는 ntp 서버를 등록합니다.

ntp package 설치와 동시에 기본 설정된 pool을 사용해도 기능상 문제없으나, 지리적으로 가까운 ntp pool을 등록하기 위해 다음을 수행합니다.

* NTP pool 확인&#x20;
  * [**NTP Pool Project**](https://www.ntppool.org/ko/)에서 확인 가능합니다. (한국 시각에 맞는 NTP pool은 [**kr.pool.ntp.org**](https://www.ntppool.org/zone/kr)에서 확인 할 수 있습니다.)
* iburst 옵션
  * 해당 ntp 서버와 동기화하는 시간을 최소화합니다.
* 다수의 서버를 등록
  * &#x20;0, 1, 2, 3의 숫자를 붙여 나열합니다.
  * 그러면 등록한 서버들에 랜덤으로 접속하여 시간 정보를 가져오게 됩니다.

```shell
$ sudo vi /etc/chrony.conf

# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
server 0.kr.pool.ntp.org iburst
server 1.asia.pool.ntp.org iburst
server 2.asia.pool.ntp.org iburst
```



서비스를 재시작하여 변경 사항을 적용합니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo systemctl restart chronyd
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo systemctl restart chrony
```
{% endtab %}
{% endtabs %}

chrony는 서버 시간에 의존성을 갖는 어플리케이션의 장애를 최소화하기 위해 시간 동기화를 서서히 수행합니다.

하지만 필요에 따라 다음 명령어를 사용하여 즉시 동기화를 수행할 수 있습니다.

로컬의 시간이 급격하게 변할 경우 기존 어플리케이션이 영향받을 수 있으니 유의해 주세요.

```shell-session
$ sudo chronyc -a makestep
  200 OK
```



## 5. 동기화 확인

#### (1) chronyc sources

다음 명령어로 chrony.conf의 내용이 잘 반영되고 있는지 확인합니다.

```shell-session
$ chronyc sources
```

상태 지표는 다음과 같습니다.

MS 항목 : 가상머신과 해당 원격지 NTP 서버의 결합 상태를 보여줍니다.

* \* : 정상적으로 동기화 중
* \+ : 필요할 때 동기화 가능한 상태인 secondary 서버
* \- : 가상서버와 결합하지 않은 서버
* ? : 연결이 끊어졌거나 패킷 통신을 실패
* x : 해당 서버의 시간이 다른 NTP 서버들과 일치하지 않는 경우
* \~ : 시간 변동성이 큰 소스



출력 결과 예시는 다음과 같습니다. 이처럼 \* 표시를 띄는 서버 정보가 있으면 동기화된 것입니다.

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>



#### (2) chronyc tracking

다음 명령어로 로컬 서버와 원격지 타임 서버 간 오차에 대한 통계를 확인할 수 있습니다.

```shell-session
$ chronyc -a tracking
```



상태 지표는 다음과 같습니다.

* Reference ID : 동기화 중인 원격지 서버의 IP 혹은 도메인. 값이 127.127.1.1인 경우 원격 서버와 연결되지 않고 로컬 모드로 동작 중임을 의미.&#x20;
* Stratum : 로컬 서버가 동기화된 타임 서버로부터 몇 hop에 걸쳐 있는지를 의미.
* Ref time : 가장 마지막에 동기화가 실시된 UTC 시간
* System time : 원격 서버와의 시간이 틀어졌을 경우 chronyd는 어플리케이션의 장애를 최소화하기 위해 시스템 시간을 천천히 조정하여 동기화를 수행한다.\
  &#x20;     해당 옵션은 이 작업으로 인한 원격지 시간과 시스템 시간의 차이를 의미.
* Last offset : 마지막 업데이트에서 측정된 오프셋
* RMS offset : 장시간 측정된 오프셋의 평균값
* Frequency : chronyd가 시간을 동기화 하지 않을 경우 시스템 시간이 틀어질 확률
* Residual freq : 동기화 중인 타임 서버의 frequency와 로컬 서버의 frequency의 오차
* Skew : frequency에 대한 예상 오차 범위
* Root delay : 동기화되는 타임 서버에 대한 네트워크 경로 지연 수치&#x20;
* Root dispersion : 로컬 서버와 원격지 타임 서버 사이에서 발생한 최대 시간 차이
* Update interval : 가장 최근에 수행된 두 동기화 사이의 시간차
* Leap status : Normal일 경우 정상 동기화된 것이며, 동기화 실패한 경우 Not synchronized 표시

출력 결과 예시는 다음과 같습니다.&#x20;

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>



### 6. 서버 시간 확인하기

서버의 시간이 잘 변경되었는지 확인합니다.

```shell-session
$ date
Mon Feb 22 10:47:43 KST 2021
```



### 2-2. ntpd로 설정하기

#### (1) chronyd 중지

chronyd가 동작 중인 경우 이를 중지 시킵니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo systemctl stop chronyd
$ sudo systemctl disable chronyd
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo systemctl stop chrony
$ sudo systemctl disable chrony
```
{% endtab %}
{% endtabs %}



#### (2) NTP 패키지 설치

ntp를 이용하여 가상서버의 시간을 NTP 서버와 동기화 할 수 있습니다.

OS별 설치 방법은 다음과 같습니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo yum install ntp
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo apt install ntp
```
{% endtab %}
{% endtabs %}



#### 3. ntpd 데몬 동작 시키기

ntpd 데몬을 동작 시킵니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo systemctl start ntpd
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo systemctl start ntp
```
{% endtab %}
{% endtabs %}

서버 재 부팅 시에도 ntpd 데몬이 자동 시작되도록 설정합니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo systemctl enable ntpd
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo systemctl start ntp
```
{% endtab %}
{% endtabs %}



#### (4) ntp.conf 수정 (생략 가능)

vi로 ntp.conf 파일을 열어봅니다.

```shell-session
$ sudo vi /etc/ntp.conf
```



원하는 ntp 서버를 등록합니다.

ntp package 설치와 동시에 기본 설정된 pool을 사용해도 기능상 문제없으나, 지리적으로 가까운 NTP pool을 등록하기 위해 다음을 수행합니다.

* ntp 서버 확인&#x20;
  * [**NTP Pool Project**](https://www.ntppool.org/ko/)에서 확인 가능합니다.
  * 한국 시각에 맞는 NTP pool은 [**kr.pool.ntp.org**](https://www.ntppool.org/zone/kr)에서 확인 할 수 있습니다.
* iburst 옵션
  * 해당 ntp 서버와 동기화되는 시간을 최소화합니다.
* 다수의 서버를 등록
  * 0, 1, 2, 3의 숫자를 붙여 나열합니다.
  * 등록한 서버들에 무작위로 접속하여 시간 정보를 가져오게 됩니다.

```shell
$ sudo vi /etc/ntp.conf
# Use public servers from the pool.ntp.org project.

# Please consider joining the pool (http://www.pool.ntp.org/join.html).

server 0.kr.pool.ntp.org      iburst

server 1.asia.pool.ntp.org   iburst

server 2.asia.pool.ntp.org   iburst
```



서비스를 재시작합니다.&#x20;

{% tabs %}
{% tab title="CentOS / Rocky" %}
```shell-session
$ sudo systemctl restart ntpd
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo systemctl restart ntp
```
{% endtab %}
{% endtabs %}



#### (5) 동기화 확인

다음 명령어로 서버 동기화 상태를 확인 할 수 있습니다.

```shell-session
$ ntpq -p
```

상태 지표는 다음과 같습니다.

* remote 항목 : 가상서버가 동기화 중인 원격지 NTP 서버로, 결합 상태는 좌측의 기호로 확인할 수 있습니다.
* \* : 정상적으로 동기화 중
* \+ : 필요할 때 동기화 가능한 상태인 secondary 서버
* \- : 가상서버와 결합하지 않은 서버 &#x20;

따라서 다음 결과와 같이 \* 표시를 띄는 서버 정보가 있으면 동기화된 것입니다.

결과 예시는 다음과 같습니다.

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

#### (6) 서버 시간 확인하기

서버의 시간이 잘 변경되었는지 확인합니다.

```shell-session
$ date
Mon Feb 22 10:47:43 KST 2021
```

\
