---
description: 가상서버를 생성하는 방법은 아래와 같습니다.
---

# 가상서버 생성 방법

{% embed url="https://www.youtube.com/watch?v=83QPdG1behI" %}



## 1. 가상서버 생성하기

<mark style="background-color:blue;">콘솔 > 서버 > 가상서버</mark>

\[가상서버 생성하기] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/19721a16c543b5901674cf57fe87fbc8_1582521749.png" alt=""><figcaption></figcaption></figure>







## 2. OS 이미지 선택하기

OS 이미지는 사용 중에 변경할 수 없으며, OS 이미지 변경을 원할 시에는 가상서버를 삭제하고 다시 만들어야 합니다.

\[새로 설치]를 클릭한 후, 원하는 OS 이미지를 선택합니다.

<figure><img src="../../.gitbook/assets/2023-07-19 09 35 12.png" alt=""><figcaption></figcaption></figure>







## 3. 하드웨어 상품 선택하기

제공되는 하드웨어의 상품을 사용 용도에 맞게 선택합니다.

선택된 하드웨어에 따라 시간 별로 청구될 요금이 상이하며, 하드웨어 상품은 가상서버 생성 후에도 변경이 가능합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/7140e12d2557da7e02474d0fa93306d5_1582525242.png" alt=""><figcaption></figcaption></figure>

</div>







## 4. 가상서버 이름 설정하기

가상서버 이름은 20자까지 지원이 가능하며, 영어, 숫자 및 특수문자 '-'만 사용 가능합니다.

가상서버 이름은 생성 후에도 변경할 수 있습니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/ee785d7baec674f5aae85dd2deb0a306_1582525303.png" alt=""><figcaption></figcaption></figure>

</div>







## 5. SSH 키페어 설정

카페24 클라우드는 shell 접근을 위해서 SSH 키페어를 사용하도록 하고 있습니다.

따로 접근 패스워드를 생성하거나 E-mail 등으로 보내드리지 않으며,

가상서버에 SSH 키페어를 설정하는 방법은 다음과 같습니다.

### (1) 기존에 등록되어 있는 키페어를 사용

이전에 카페24 클라우드 콘솔에서 생성한 키페어를 적용할 수 있습니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/7c4ba23274531abf51cc31b05242ffd9_1621834120.png" alt=""><figcaption></figcaption></figure>

</div>





### (2) 사용중인 공개키를 등록

사용 중인 키페어의 공개키를 등록하여 기존에 쓰던 개인키 파일로 가상서버에 접속할 수 있습니다.

먼저 \[등록하기] 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/02e30fdcdcdb7409c72ffe4b158f4cd9_1621834199.jpg" alt=""><figcaption></figcaption></figure>

</div>

<mark style="background-color:blue;">콘솔 > 보안 서비스 > SSH 키페어</mark>

등록할 키페어의 공개키를 복사합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/fc0a122c58ef5c0bb33c8e0427029cf8_1621834588.png" alt=""><figcaption></figcaption></figure>

</div>

복사한 내용을 앞서 열어둔 기존 SSH 키페어 등록 창에 붙여 넣기하여 등록합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/12b8c0d104fd54977cb906d0634a59a2_1621834733.jpg" alt=""><figcaption></figcaption></figure>

</div>





### (3) 새로운 공개키 생성

\[새로만들기] 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/fb4c10053a071a85348757dcd8a60f39_1582525539.png" alt=""><figcaption></figcaption></figure>

</div>

키페어의 이름을 입력하여 새로 생성합니다.

이름은 20자까지 지원이 가능하며, 영어와 숫자만 지원합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/22053d1bbf2f53f96a104c7a88d44cce_1582527935.png" alt=""><figcaption></figcaption></figure>

</div>

개인키가 다운로드 받아지는 것을 확인합니다.

키페어를 사용한 로그인 방법은 [\[SSH 키페어 접속 방법\]](connect/keypair.md)을 참고해 주세요.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/233dd4df8b8227ca0bbbd5425c29ceea_1582527961.png" alt=""><figcaption></figcaption></figure>

</div>

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

SSH 키페어의 개인키는 분실 시 재발급이 불가능합니다.&#x20;

잃어버리지 않도록 보관에 유의해 주시기 바랍니다.
{% endhint %}







## 6. 공인IP 할당하기

공인 IP 할당은 선택 사항이지만, 외부에서 가상서버로 접속하기 위해서는 필요합니다.

공인 IP는 시간당 3원 과금되며, 계정 당 최대 30개까지 할당됩니다.

공인 IP 할당 방법은 다음과 같습니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/11/13/bbaa88e39fd7cad174bddc7d6963a8ea_1605251451.jpg" alt=""><figcaption></figcaption></figure>

</div>

### (1) 기존에 신청되어 있는 공인IP를 할당

기존에 생성한 공인IP가 있는 경우 선택합니다.





### (2) 새로운 공인IP 신청하고 자동으로 할당

기존에 생성한 공인IP가 없을 경우 선택합니다.

가상서버 생성과 동시에 새로운 공인 IP가 할당됩니다.





### (3) 가상서버 생성 후 공인IP를 직접 할당

사설망만을 이용해 가상서버를 이용하실 경우 선택합니다.

추후 공인IP 할당이 필요하면 <mark style="background-color:blue;">콘솔 > 네트워킹 > 공인IP</mark>에서 연결할 수 있습니다.







## 7. 방화벽 선택하기

가상서버에 접근을 제어할 수 있는 방화벽을 설정할 수 있습니다.

**가상서버에 방화벽을 설정하지 않으면 모든 접근에 대해 닫혀 있습니다**.

해당 설정 사항은 선택 사항이며, 가상서버 생성 후에 방화벽을 설정하는 방법은 [\[방화벽 설정 방법\]](../../security/security/config.md)을  참고해 주세요.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/cb6ccb8a018c9acc891f0da3d20a48b2_1582528000.png" alt=""><figcaption></figcaption></figure>

</div>







## 8. 블록 스토리지 생성하기

블록 스토리지를 생성하여 추가 공간을 확보할 수 있습니다.

해당 설정은 선택 사항이며, 가상서버 생성과 함께 블록 스토리지가 연결됩니다.

블록 스토리지를 사용하기 위해서는 shell에서 연결 작업이 필요하며, [\[블록 스토리지 연결 방법\]](../../storage/block/connect.md)을 참고해 주세요.

새로운 블록 스토리지를 만들기 위해 \[생성하기] 버튼을 누릅니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/9454d1ec912a655c37d7451a169204bf_1582528597.png" alt=""><figcaption></figcaption></figure>

</div>

이름과 용량을 입력합니다.

용량은 10GB 이상 1000GB 미만만까지 지원하며, 10GB 단위로 입력합니다.

설정된 용량은 생성 후 확장 기능을 사용하여 용량 확장을 할 수 있습니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/93770250fa2c8ed6933b671bafcae27c_1582528609.png" alt=""><figcaption></figcaption></figure>

</div>







## 9. 가상서버 확인하기

설정한 가상서버 및 블록 스토리지의 사양에 따라 시간당 과금액이 결정됩니다.

\[가상서버 생성] 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/a5337e19fcf5309c8235ce58c152828b_1582528992.png" alt=""><figcaption></figcaption></figure>

</div>

생성할 내역들을 확인한 후 \[확인] 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/614e1fe5da56bd04cd9508e50e7be38b_1582529009.png" alt=""><figcaption></figcaption></figure>

</div>

가상서버 대시보드에서 가상서버가 생성 완료되었는지 확인합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/571f13d1ebbaaf2698680389c3c4e157_1582529445.png" alt=""><figcaption></figcaption></figure>

</div>
