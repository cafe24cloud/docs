---
description: >-
  가상서버는 기본적으로 모든 접근에 대해서 닫혀 있기 때문에, 방화벽을 설정해야 가상서버로의 접근이 허용됩니다. 가상서버에는 여러 방화벽을
  다중으로 연결할 수 있습니다.
---

# 방화벽 설정 방법

## 1. 방화벽 생성하기

<mark style="background-color:blue;">웹콘솔 > 보안서비스 > 방화벽 > 새로운 방화벽 생성</mark>

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/1817a7ac3176d018f050557ed7fdf19d_1583999970.png" alt=""><figcaption></figcaption></figure>

**"기본정보 입력"**

* **이름** : 방화벽에서 사용할 이름을 작성해줍니다.
* **내부 네트워크 허용** : 내부 네트워크에 접근을 허용하고자 한다면 해당 옵션을 사용해 주시기 바랍니다.
* **보안 정책 설정 ( INBOUND )** &#x20;
  * **서비스** : SSH, HTTD 등과 같이 필요한 서비스를 선택하면 포트가 자동으로 됩니다.
  * **프로토콜** : TCP, UDP 중 사용할 프로토콜을 지정해주시면됩니다.
  * **포트범위** : 지원하는 포트 범위는 1-65535 입니다.
  * **원격지 IP/CIDR** :  허용할 IP 입력합니다. 공유기를 사용하시는경우 ["IP를 확인"](http://ifconfig.kr/) 해보시기 바랍니다.
  * **코멘트** : 각 포워딩 룰에 대한 메모를 남길 수 있습니다.&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/16/fbd4e9be511d0f4747cca7987c8e8592_1647396047.jpg" alt=""><figcaption></figcaption></figure>







## 2. 가상서버에 방화벽 연결하기

웹콘솔 보안서비스에서 방화벽 연결 방법입니다.

<mark style="background-color:blue;">생성한 방화벽 선택 후 > 상세 정보 > 가상서버 연결 설정</mark>

카페24 클라우드의 방화벽 정책으로 서버 간의 연결(사설 통신)은 차단되어 있습니다.

서버 간 내부 연결을 활성화 하기 위해서는 해당 화면에서 "**내부 네트워크 허용**"을 체크합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/09/ff0f0de0ce6b26de352f3e7f430856b0_1617956074.jpg" alt=""><figcaption></figcaption></figure>

연결할 서버를 선택하여 적용 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/cda5e1ad72a8eb1c64d073a9cd70af92_1584000170.png" alt=""><figcaption></figcaption></figure>

상세 정보에서 연결된 가상서버 리스트를 확인할수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/6cc2e864828dcaef02d181b328c578a0_1584000193.png" alt=""><figcaption></figcaption></figure>

가상서버 대시보드에서 방화벽을 연결하는 방법입니다.

방화벽을 연결할 가성서버를 선택한 다음, 기능별 설정 탭에서 "연결" 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/2efc9f9424ce7aaaab9788205b441ac4_1584002222.png" alt=""><figcaption></figcaption></figure>

가상서버에 연결할 방화벽을 선택하여 연결 버튼을 클릭합니다.

하나의 가상서버에 여러 방화벽을 다중 연결할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/06/15/11a93c489612d4cd887d14bd7130d9f4_1655252012.jpg" alt=""><figcaption></figcaption></figure>

