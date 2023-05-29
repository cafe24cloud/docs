---
description: SSH 키페어를 사용하여 가상서버에 접속하는 방법은 아래와 같습니다.
---

# SSH 키페어 접속 방법

## 1. 방화벽 설정하기

<mark style="color:red;">카페24 클라우드는 보안상 모든 포트가 막혀있는 상태로 가상서버가 생성됩니다.</mark>

<mark style="color:red;">따라서 방화벽 연결을 하지 않으면 가상서버에 접속할 수 없습니다.</mark>&#x20;

이를 방지하기 위해 다음과 같이 22번 포트를 열어 주시기 바랍니다.

### (1) 방화벽 생성하기

<mark style="background-color:blue;">콘솔 > 보안 서비스 > 방화벽</mark>

\[새로운 방화벽] 생성 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/1817a7ac3176d018f050557ed7fdf19d_1583999970.png" alt=""><figcaption></figcaption></figure>

</div>

방화벽 생성 창에서 아래의 내용을 입력합니다.

* **이름** : 생성할 방화벽의 이름을 작성합니다.
* **내부 네트워크 허용** : 내부 네트워크에 접근을 허용하고자 한다면 해당 옵션을 사용해 주시기 바랍니다.
* **보안 정책 설정 (INBOUND)**: 인바운드 보안 정책을 추가합니다.
  * **서비스** : SSH, HTTD 등과 같이 필요한 서비스를 선택하면 포트가 자동으로 입력됩니다.
  * **프로토콜** : TCP, UDP 중 사용할 프로토콜을 지정해 주시면 됩니다.
  * **포트범위** : 지원하는 포트 범위는 1-65535입니다.
  * **원격지 IP/CIDR** : 허용할 IP를 입력합니다. 공유기를 사용하시는 경우 [<mark style="color:blue;">IP를 확인</mark>](http://ifconfig.kr/)해보시기 바랍니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/23201427604e3a110f20a5b34e8d8b07_1584000007.png" alt=""><figcaption></figcaption></figure>

</div>





### (2) 방화벽 연결하기

#### a. 방화벽 대시보드에서 연결하기

생성한 방화벽을 선택한 후, 상세 정보에서 가상서버 연결 설정의 \[설정] 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/09/ff0f0de0ce6b26de352f3e7f430856b0_1617956074.jpg" alt=""><figcaption></figcaption></figure>

</div>

{% hint style="info" %}
<mark style="color:blue;">**참고사항**</mark>

카페24 클라우드의 방화벽 정책으로 서버 간의 연결(사설 통신)은 차단되어 있습니다.

서버 간의 연결을 활성화하기 위해서는 해당 화면에서 **내부 네트워크 허용**을 체크합니다.
{% endhint %}

가상서버 연결 설정 창에서 연결할 가상서버를 선택한 후, \[적용] 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/cda5e1ad72a8eb1c64d073a9cd70af92_1584000170.png" alt=""><figcaption></figcaption></figure>

</div>

상세 정보에서 연결된 가상서버 리스트를 확인할 수 있습니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/6cc2e864828dcaef02d181b328c578a0_1584000193.png" alt=""><figcaption></figcaption></figure>

</div>



#### b. 가상서버 대시보드에서 연결하기

<mark style="background-color:blue;">콘솔 > 서버 > 가상서버</mark>

방화벽을 연결할 가상서버를 선택한 후, 기능별 설정에서 방화벽의 \[연결] 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/2efc9f9424ce7aaaab9788205b441ac4_1584002222.png" alt=""><figcaption></figcaption></figure>

</div>

방화벽 연결 창에서 가상서버에 연결할 방화벽을 선택한 후, \[연결] 버튼을 클릭합니다.

하나의 가상서버에 여러 방화벽을 다중 연결할 수 있습니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/06/15/debb70798d1bf047d0df9ecfcf0d70ee_1655252381.jpg" alt=""><figcaption></figcaption></figure>

</div>







## 2. 가상서버 접속하기

<mark style="color:red;">카페24 클라우드는 고객의 데이터 보호와 보안 강화를 위해 ID/PW를 직접 고객님께 제공하지 않고,</mark>

<mark style="color:red;">다음과 같은 방법으로 SSH 키페어를 사용한 접속 방식을 사용합니다.</mark>

### (1) Windows 시스템에서 접속하기

가상서버 생성 시 만든 cafe24 키페어 사용을 위해서 **puttygen** 프로그램을 이용해서 키 값을 변환합니다.&#x20;

키페어 변경 후, **putty** 프로그램을 이용해서 가상서버에 접속합니다.&#x20;

① [<mark style="color:blue;">Download PuTTY</mark>](https://www.chiark.greenend.org.uk/\~sgtatham/putty/latest.html)에서 puttygen.exe, putty.exe 파일을 다운로드 합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/06/17/45c66d948d8dddbae87be60be6120f07_1655455874.jpg" alt=""><figcaption></figcaption></figure>

</div>

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/06/17/96afd66b852df89687e680ad034e410a_1655455886.jpg" alt=""><figcaption></figcaption></figure>

</div>

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

정상적인 동작을 위해서 최신 버전을 사용하는 것을 권장합니다.

낮은 버전 사용 시, 특정 OS와 OpenSSH 버전 문제로 원격 접속이 불가할 수 있습니다.
{% endhint %}

② puttygen.exe를 실행하여 \[Load] 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/04/a236b19d56cac8dc13233c7cac79fc3f_1607068573.png" alt=""><figcaption></figcaption></figure>

</div>

③ 발급 받은 cafe24 키페어의 개인키 파일인 cafe24.pem을 선택합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/04/3d78217fccfd38b632f55a17ada81cb9_1607068629.png" alt=""><figcaption></figcaption></figure>

</div>

④ \[Save private key] 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/04/0ba531f4b03f3aac470780f640fb1e37_1607068666.png" alt=""><figcaption></figcaption></figure>

</div>

⑤ 발급 받은 키페어 이름과 동일한 이름으로 저장해 주세요. 생성 후 해당 파일 이름이 가상서버를 생성하며, 사용한 키페어 이름과 일치하는지 확인합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/03/29/cd22ed2035c0f97b411572d4a69b9338_1616977384.png" alt=""><figcaption></figcaption></figure>

</div>

⑥ putty.exe를 실행하여 <mark style="background-color:blue;">Category > Connection > SSH > Auth</mark>에서 \[Browse]를 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/04/a59a46807d59670bb2269d4314261084_1607068767.png" alt=""><figcaption></figcaption></figure>

</div>

⑦ ⑤번에서 저장한 cafe24.ppk를 선택합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/04/022ec530dd5c22339e2001a68f962868_1607068800.png" alt=""><figcaption></figcaption></figure>

</div>

⑧ <mark style="background-color:blue;">Category > Session</mark>에서 Host Name (or IP address)에 접속하려는 가상서버의 공인IP를 입력한 후, \[Open] 버튼을 클릭합니다.

가상서버의 22번 포트로 접속하도록 하며, **이때 해당 가상서버의 22번 포트로 방화벽이 열려 있지 않다면 접속이 불가합니다.** 접속이 안될 경우, 방화벽 설정을 확인해 주시기 바랍니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/07/30/26974cad5c741f06794cd0e4113ba377_1627625855.png" alt=""><figcaption></figcaption></figure>

</div>

⑨ 카페24 클라우드에서 제공하는 OS는 보안상 일반 계정으로만 접속이 가능합니다. 아래의 계정 정보를 참고하여 접속한 후, root 권한으로 변경합니다.

|     OS    |     계정    |        접속 방법       |
| :-------: | :-------: | :----------------: |
|   Rocky   |   rocky   |   rocky@가상서버공인IP   |
|   CentOS  |   centos  |   centos@가상서버공인IP  |
|   Ubuntu  |   ubuntu  |   ubuntu@가상서버공인IP  |
| Almalinux | almalinux | almalinux@가상서버공인IP |

```shell-session
$ sudo -i
```

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/21/c1430ed520c82eb27629c225aea9174c_1582268758.png" alt=""><figcaption></figcaption></figure>

</div>





### (2) Mac 또는 SSH 클라이언트 시스템에서 접속하기

cafe24 키페어의 개인키 권한을 600으로 변경한 후, 접속합니다.

```shell-session
$ chmod 600 cafe24.pem
$ ssh -i cafe24.pem [일반계정]@[가상서버공인IP]
```

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/07/30/4f0bbaf45d5dae00d470e2181c73a241_1627625875.png" alt=""><figcaption></figcaption></figure>

</div>
