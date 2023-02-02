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

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/1817a7ac3176d018f050557ed7fdf19d_1583999970.png" alt=""><figcaption></figcaption></figure>

방화벽 생성 창에서 아래의 내용을 입력합니다.

* **이름** : 생성할 방화벽의 이름을 작성합니다.
* **내부 네트워크 허용** : 내부 네트워크에 접근을 허용하고자 한다면 해당 옵션을 사용해 주시기 바랍니다.
* **보안 정책 설정 (INBOUND)**: 인바운드 보안 정책을 추가합니다.
  * **서비스** : SSH, HTTD 등과 같이 필요한 서비스를 선택하면 포트가 자동으로 입력됩니다.
  * **프로토콜** : TCP, UDP 중 사용할 프로토콜을 지정해 주시면 됩니다.
  * **포트범위** : 지원하는 포트 범위는 1-65535입니다.
  * **원격지 IP/CIDR** : 허용할 IP를 입력합니다. 공유기를 사용하시는 경우 [<mark style="color:blue;">\[IP를 확인\]</mark>](http://ifconfig.kr/) 해보시기 바랍니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/12/23201427604e3a110f20a5b34e8d8b07_1584000007.png" alt=""><figcaption></figcaption></figure>





### (2) 방화벽 연결하기

#### a. 방화벽 대시보드에서 연결하기

생성한 방화벽을 선택한 후, 상세 정보에서 가상서버 연결 설정의 \[설정] 버튼을 클릭합니다.











#### b. 가상서버 대시보드에서 연결하기

## 2. 가상서버 접속하기

### (1) Windows 시스템에서 접속하기

### (2) Mac 또는 별도의 SSH 클라이언트 시스템에서 접속하기
