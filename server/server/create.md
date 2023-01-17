# 가상서버 생성 방법

## 1. 콘솔에서 가상서버 생성 버튼을 클릭합니다.

* 서버 > 가상서버 > 가상서버 생성하기

&#x20; <img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/19721a16c543b5901674cf57fe87fbc8_1582521749.png" alt="" data-size="original">



## 2. OS 이미지를 선택합니다.

OS 이미지는 사용 중 변경할 수 없으며, OS 이미지변경을 원할 시에는 가상서버를 삭제하고 다시 만들어야 합니다.

* 새로 설치 > OS 이미지 선택

&#x20; <img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/15bf717eeb660432396a287f9147e368_1582525122.png" alt="" data-size="original">



## 3. 하드웨어 사양을 선택합니다.

제공되는 하드웨어의 사양을 사용 용도에 맞게 선택합니다.

선택된 하드웨어에 따라 시간별로 청구될 요금이 상이하며, 하드웨어 사양은 가상서버 생성 후에도 변경이 가능합니다.

&#x20; <img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/7140e12d2557da7e02474d0fa93306d5_1582525242.png" alt="" data-size="original">



## 4. 가상서버의 이름을 입력합니다.

이름은 20자까지 지원이 가능하며, 영어, 숫자 및 특수문자 '-'만 사용이 가능합니다.

가상서버 이름은 생성 후에도 변경할 수 있습니다.

&#x20; <img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/ee785d7baec674f5aae85dd2deb0a306_1582525303.png" alt="" data-size="original">



## 5. SSH 키페어를 설정합니다.

카페24 클라우드는 Shell의 접근을 위해서 SSH 키페어를 사용하도록 하고 있습니다.

따로 접근 패스워드를 생성하거나 E-mail 등으로 보내드리지 않으며,

가상서버에 SSH 키페어를 설정하는 방법은 다음 세 가지가 있습니다.

#### (1) 기존에 등록되어 있는 키페어를 사용

이전에 카페24 클라우드 콘솔에서 생성한 키페어를 적용할 수 있습니다.

&#x20; <img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/7c4ba23274531abf51cc31b05242ffd9_1621834120.png" alt="" data-size="original">

#### (2) 사용중인 공개키를 등록

사용중인 키페어의 공개키를 등록하여 기존에 쓰던 개인키 파일로 가상서버에 접속할 수 있습니다.

\[등록하기] 버튼을 클릭합니다.

&#x20; <img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/02e30fdcdcdb7409c72ffe4b158f4cd9_1621834199.jpg" alt="" data-size="original">

사용중인 키페어의 공개키를 복사합니다.

* 보안 서비스 > SSH 키페어 > 등록할 키페어의 공개키

&#x20; <img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/fc0a122c58ef5c0bb33c8e0427029cf8_1621834588.png" alt="" data-size="original">

복사한 내용을 앞서 열어둔 기존 SSH 키페어  등록 창에 붙여넣기 합니다.

&#x20; <img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/12b8c0d104fd54977cb906d0634a59a2_1621834733.jpg" alt="" data-size="original">

#### (3)  새로운 공개키 생성

\[새로만들기] 버튼을 클릭합니다.

&#x20; ![](https://filesystem.cafe24.com/hosting/cloud\_service/2020/02/24/fb4c10053a071a85348757dcd8a60f39\_1582525539.png)&#x20;

키페어의 이름을 입력합니다.

이름은 20자까지 지원이 가능하며, 영어와 숫자만 지원합니다.

&#x20; ![](https://filesystem.cafe24.com/hosting/cloud\_service/2020/02/24/22053d1bbf2f53f96a104c7a88d44cce\_1582527935.png)

개인키가 다운로드 받아지는 것을 확인합니다.&#x20;

키페어를 사용한 로그인 방법은 다음 링크를 참조해 주시기 바랍니다. [\[SSH 키페어 사용법\]](https://console.cafe24.com/support/faq/view?idx=71)

{% hint style="danger" %}
SSH 키페어의 개인키는 분실 시 재발급이 불가능합니다. 잃어버리지 않도록 보관에 유의해 주시기 바랍니다.
{% endhint %}

&#x20; ![](https://filesystem.cafe24.com/hosting/cloud\_service/2020/02/24/233dd4df8b8227ca0bbbd5425c29ceea\_1582527961.png)



## 6. 공인IP를 할당합니다.

공인 IP 할당은 선택 사항이지만, 외부에서 가상서버로 접속하기 위해서는 필요합니다.

공인 IP는 시간당 3원 과금되며, 계정 당 최대 30개까지 할당됩니다.

공인 IP 할당 방법은 다음과 같습니다.

#### (1) 기존에 신청되어 있는 공인IP를 할당

기존에 생성한 공인IP가 있는 경우 선택합니다.

#### (2) 새로운 공인IP 신청하고 자동으로 할당

기존에 생성한 공인IP가 없을 경우 선택합니다.

가상서버 생성과 동시에 새로운 공인 IP가 할당됩니다.

#### (3) 가상서버 생성 후 공인IP를 직접 할당

사설망만을 이용해 가상서버를 이용하실 경우 선택합니다.

추후 공인IP 연동이 필요하면 네트워킹 > 공인IP 메뉴에서 연결 할 수 있습니다.

&#x20; ![](https://filesystem.cafe24.com/hosting/cloud\_service/2020/11/13/bbaa88e39fd7cad174bddc7d6963a8ea\_1605251451.jpg)



## 7. 방화벽을 설정합니다.

가상서버에 접근을 제어할 수 있는 방화벽을 설정할 수 있습니다.

**가상서버에 방화벽을 설정하지 않으면 모든 접근에 대해 닫혀 있습니다**.

해당 설정 사항은 선택 사항이며, 가상서버 생성 후에 방화벽을 설정할 수 있습니다. [\[방화벽 설정방법\]](https://console.cafe24.com/support/faq/view?idx=95)

&#x20; ![](https://filesystem.cafe24.com/hosting/cloud\_service/2020/02/24/cb6ccb8a018c9acc891f0da3d20a48b2\_1582528000.png)



## 8. 블록 스토리지를 생성합니다.

블록 스토리지를 생성하여 추가 공간을 확보할 수 있습니다.&#x20;

해당 설정은 선택 사항이며, 가상서버 생성과 함께 블록 스토리지가 연결됩니다.

블록 스토리지를 사용하기 위해서는 shell에서 연결 작업이 필요합니다. 다음 링크를 참조해 주세요. [\[블록 스토리지 연결법\]](https://console.cafe24.com/support/faq/view?idx=47)

다음 절차에 따라서 새로운 블록 스토리지를 만듭니다.

생성하기 버튼을 누릅니다.

&#x20; ![](https://filesystem.cafe24.com/hosting/cloud\_service/2020/02/24/9454d1ec912a655c37d7451a169204bf\_1582528597.png)

용량을 설정합니다.

용량은 10GB 이상 1000GB까지 지원하며, 10GB 단위로 입력합니다.

설정된 용량은 생성 후 확장 기능을 사용하여 용량 확장을 할 수 있습니다.

&#x20; ![](https://filesystem.cafe24.com/hosting/cloud\_service/2020/02/24/93770250fa2c8ed6933b671bafcae27c\_1582528609.png)



## 9. 가상서버를 생성합니다.

선택한 하드웨어 사양과 블록 스토리지의 용량에 따라 시간당 과금액이 결정됩니다. \[가상서버 생성] 버튼을 눌러 주세요.

&#x20; ![](https://filesystem.cafe24.com/hosting/cloud\_service/2020/02/24/a5337e19fcf5309c8235ce58c152828b\_1582528992.png)&#x20;

설정한 사항들을 확인 후 \[확인] 버튼을 눌러 생성을 완료합니다.

&#x20; ![](https://filesystem.cafe24.com/hosting/cloud\_service/2020/02/24/614e1fe5da56bd04cd9508e50e7be38b\_1582529009.png)



## 10. 생성된 가상서버 정보를 확인합니다.

가상서버 대시보드에서 가상서버가 생성 완료되었는지 확인합니다.

&#x20; ![](https://filesystem.cafe24.com/hosting/cloud\_service/2020/02/24/571f13d1ebbaaf2698680389c3c4e157\_1582529445.png)
