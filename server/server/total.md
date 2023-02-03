---
description: 본 매뉴얼은 가상서버 생성, 접속 및 DNS와 SFTP 사용에 대한 일련의 과정을 설명합니다.
---

# 가상서버 통합 매뉴얼

## 가상서버 생성 방법

### 1. 가상서버 생성하기

<mark style="background-color:blue;">콘솔 > 서버 > 가상서버</mark>

\[가상서버 생성하기] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/19721a16c543b5901674cf57fe87fbc8_1582521749.png" alt=""><figcaption></figcaption></figure>

### 2. OS 이미지 선택하기

OS 이미지는 사용 중에 변경할 수 없으며, OS 이미지 변경을 원할 시에는 가상서버를 삭제하고 다시 만들어야 합니다.

\[새로 설치]를 클릭한 후, 원하는 OS 이미지를 선택합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/15bf717eeb660432396a287f9147e368_1582525122.png" alt=""><figcaption></figcaption></figure>

### 3. 하드웨어 상품 선택하기

제공되는 하드웨어의 상품을 사용 용도에 맞게 선택합니다.

선택된 하드웨어에 따라 시간 별로 청구될 요금이 상이하며, 하드웨어 상품은 가상서버 생성 후에도 변경이 가능합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/7140e12d2557da7e02474d0fa93306d5_1582525242.png" alt=""><figcaption></figcaption></figure>

### 4. 가상서버 이름 설정하기

가상서버 이름은 20자까지 지원이 가능하며, 영어, 숫자 및 특수문자 '-'만 사용 가능합니다.

가상서버 이름은 생성 후에도 변경할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/ee785d7baec674f5aae85dd2deb0a306_1582525303.png" alt=""><figcaption></figcaption></figure>

### 5. SSH 키페어 설정

카페24 클라우드는 shell 접근을 위해서 SSH 키페어를 사용하도록 하고 있습니다.

따로 접근 패스워드를 생성하거나 E-mail 등으로 보내드리지 않으며,

가상서버에 SSH 키페어를 설정하는 방법은 다음과 같습니다.

#### (1) 기존에 등록되어 있는 키페어를 사용

이전에 카페24 클라우드 콘솔에서 생성한 키페어를 적용할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/7c4ba23274531abf51cc31b05242ffd9_1621834120.png" alt=""><figcaption></figcaption></figure>

#### (2) 사용중인 공개키를 등록

사용 중인 키페어의 공개키를 등록하여 기존에 쓰던 개인키 파일로 가상서버에 접속할 수 있습니다.

먼저 \[등록하기] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/02e30fdcdcdb7409c72ffe4b158f4cd9_1621834199.jpg" alt=""><figcaption></figcaption></figure>

<mark style="background-color:blue;">콘솔 > 보안 서비스 > SSH 키페어</mark>

등록할 키페어의 공개키를 복사합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/fc0a122c58ef5c0bb33c8e0427029cf8_1621834588.png" alt=""><figcaption></figcaption></figure>

복사한 내용을 앞서 열어둔 기존 SSH 키페어 등록 창에 붙여 넣기하여 등록합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/05/24/12b8c0d104fd54977cb906d0634a59a2_1621834733.jpg" alt=""><figcaption></figcaption></figure>

#### (3) 새로운 공개키 생성

\[새로만들기] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/fb4c10053a071a85348757dcd8a60f39_1582525539.png" alt=""><figcaption></figcaption></figure>

키페어의 이름을 입력하여 새로 생성합니다.

이름은 20자까지 지원이 가능하며, 영어와 숫자만 지원합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/22053d1bbf2f53f96a104c7a88d44cce_1582527935.png" alt=""><figcaption></figcaption></figure>

개인키가 다운로드 받아지는 것을 확인합니다.

키페어를 사용한 로그인 방법은 [\[SSH 키페어 접속 방법\]](connect/keypair.md)을 참고해 주세요.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/233dd4df8b8227ca0bbbd5425c29ceea_1582527961.png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

SSH 키페어의 개인키는 분실 시 재발급이 불가능합니다.

잃어버리지 않도록 보관에 유의해 주시기 바랍니다.
{% endhint %}

### 6. 공인IP 할당하기

공인 IP 할당은 선택 사항이지만, 외부에서 가상서버로 접속하기 위해서는 필요합니다.

공인 IP는 시간당 3원 과금되며, 계정 당 최대 30개까지 할당됩니다.

공인 IP 할당 방법은 다음과 같습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/11/13/bbaa88e39fd7cad174bddc7d6963a8ea_1605251451.jpg" alt=""><figcaption></figcaption></figure>

#### (1) 기존에 신청되어 있는 공인IP를 할당

기존에 생성한 공인IP가 있는 경우 선택합니다.

#### (2) 새로운 공인IP 신청하고 자동으로 할당

기존에 생성한 공인IP가 없을 경우 선택합니다.

가상서버 생성과 동시에 새로운 공인 IP가 할당됩니다.

#### (3) 가상서버 생성 후 공인IP를 직접 할당

사설망만을 이용해 가상서버를 이용하실 경우 선택합니다.

추후 공인IP 할당이 필요하면 <mark style="background-color:blue;">콘솔 > 네트워킹 > 공인IP</mark>에서 연결할 수 있습니다.

### 7. 방화벽 선택하기

가상서버에 접근을 제어할 수 있는 방화벽을 설정할 수 있습니다.

**가상서버에 방화벽을 설정하지 않으면 모든 접근에 대해 닫혀 있습니다**.

해당 설정 사항은 선택 사항이며, 가상서버 생성 후에 방화벽을 설정하는 방법은 [\[방화벽 설정 방법\]](../../security/security/config.md)을 참고해 주세요.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/cb6ccb8a018c9acc891f0da3d20a48b2_1582528000.png" alt=""><figcaption></figcaption></figure>

### 8. 블록 스토리지 생성하기

블록 스토리지를 생성하여 추가 공간을 확보할 수 있습니다.

해당 설정은 선택 사항이며, 가상서버 생성과 함께 블록 스토리지가 연결됩니다.

블록 스토리지를 사용하기 위해서는 shell에서 연결 작업이 필요하며, [\[블록 스토리지 연결 방법\]](../../storage/block/connect.md)을 참고해 주세요.

새로운 블록 스토리지를 만들기 위해 \[생성하기] 버튼을 누릅니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/9454d1ec912a655c37d7451a169204bf_1582528597.png" alt=""><figcaption></figcaption></figure>

이름과 용량을 입력합니다.

용량은 10GB 이상 1000GB 미만만까지 지원하며, 10GB 단위로 입력합니다.

설정된 용량은 생성 후 확장 기능을 사용하여 용량 확장을 할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/93770250fa2c8ed6933b671bafcae27c_1582528609.png" alt=""><figcaption></figcaption></figure>

### 9. 가상서버 확인하기

설정한 가상서버 및 블록 스토리지의 사양에 따라 시간당 과금액이 결정됩니다.

\[가상서버 생성] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/a5337e19fcf5309c8235ce58c152828b_1582528992.png" alt=""><figcaption></figcaption></figure>

생성할 내역들을 확인한 후 \[확인] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/614e1fe5da56bd04cd9508e50e7be38b_1582529009.png" alt=""><figcaption></figcaption></figure>

가상서버 대시보드에서 가상서버가 생성 완료되었는지 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/24/571f13d1ebbaaf2698680389c3c4e157_1582529445.png" alt=""><figcaption></figcaption></figure>

## 가상서버 접속 방법

가상서버 접속 방법은 다음과 같습니다.

* [가상서버에 접속할 수 있는 ID/PW가 없는데 어떻게 접속하나요?](total.md#1-id-pw)
*



### (1) 가상서버에 접속할 수 있는 ID/PW가 없는데 어떻게 접속하나요?

카페24 클라우드는 고객의 데이터 보호와 보안 강화를 위해 ID/PW를 직접 고객님께 제공하지 않고, SSH 키페어를 사용한 접속 방식을 사용합니다.

우선,  SSH 키페어 접속하기 위한 방화벽 설정을 합니다.&#x20;

{% hint style="danger" %}
<mark style="color:red;">**참고 사항**</mark>

카페24 클라우드는 보안상 모든 포트가 막혀있는 상태로 가상서버가 생성됩니다. 방화벽 연결을 하지 않으면 가상서버에 접속할 수 없습니다.&#x20;
{% endhint %}

이를 방지하기 위해 다음 방화벽 설정을 이용하여 22번 포트를 열어 주시기 바랍니다. [\[방화벽 설정 방법\]](../../security/security/config.md)&#x20;

&#x20;

가상서버의 OS에 따라 다음 방법으로 ssh 접속을 할 수 있습니다.&#x20;

* #### 윈도우 시스템에서의 가상서버 접근
* mac



#### a.  윈도우 시스템에서의 가상서버 접근

가상서버 생성 시 만든 cafe24 키페어 사용을 위해서 **puttygen** 프로그램을 이용해서 키 값을 변환합니다.

키페어 변경 후, **putty** 프로그램을 이용해서 가상서버에 접속합니다.

① [<mark style="color:blue;">Download PuTTY</mark>](https://www.chiark.greenend.org.uk/\~sgtatham/putty/latest.html)에서 puttygen.exe, putty.exe 파일을 다운로드 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/06/17/45c66d948d8dddbae87be60be6120f07_1655455874.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/06/17/96afd66b852df89687e680ad034e410a_1655455886.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

정상적인 동작을 위해서 최신 버전을 사용하는 것을 권장합니다.

낮은 버전 사용 시, 특정 OS와 OpenSSH 버전 문제로 원격 접속이 불가할 수 있습니다.
{% endhint %}

② puttygen.exe를 실행하여 \[Load] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/04/a236b19d56cac8dc13233c7cac79fc3f_1607068573.png" alt=""><figcaption></figcaption></figure>

③ 발급 받은 cafe24 키페어의 개인키 파일인 cafe24.pem을 선택합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/04/3d78217fccfd38b632f55a17ada81cb9_1607068629.png" alt=""><figcaption></figcaption></figure>

④ \[Save private key] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/04/0ba531f4b03f3aac470780f640fb1e37_1607068666.png" alt=""><figcaption></figcaption></figure>

⑤ 발급 받은 키페어 이름과 동일한 이름으로 저장해 주세요. 생성 후 해당 파일 이름이 가상서버를 생성하며, 사용한 키페어 이름과 일치하는지 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/03/29/cd22ed2035c0f97b411572d4a69b9338_1616977384.png" alt=""><figcaption></figcaption></figure>

⑥ putty.exe를 실행하여 <mark style="background-color:blue;">Category > Connection > SSH > Auth</mark>에서 \[Browse]를 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/04/a59a46807d59670bb2269d4314261084_1607068767.png" alt=""><figcaption></figcaption></figure>

⑦  ⑤번에서 저장한 cafe24.ppk를 선택합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/04/022ec530dd5c22339e2001a68f962868_1607068800.png" alt=""><figcaption></figcaption></figure>

⑧ <mark style="background-color:blue;">Category > Session</mark>에서 Host Name (or IP address)에 접속하려는 가상서버의 공인IP를 입력한 후, \[Open] 버튼을 클릭합니다.

가상서버의 22번 포트로 접속하도록 하며, **이때 해당 가상서버의 22번 포트로 방화벽이 열려 있지 않다면 접속이 불가합니다.** 접속이 안될 경우, 방화벽 설정을 확인해 주시기 바랍니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/07/30/26974cad5c741f06794cd0e4113ba377_1627625855.png" alt=""><figcaption></figcaption></figure>

⑨ 카페24 클라우드에서 제공하는 OS는 보안상 일반 계정으로만 접속이 가능합니다. 아래의 계정 정보를 참고하여 접속한 후, root 권한으로 변경합니다.

|   OS   |   계정   |      접속 방법      |
| :----: | :----: | :-------------: |
|  Rocky |  rocky |  rocky@가상서버공인IP |
| CentOS | centos | centos@가상서버공인IP |
| Ubuntu | ubuntu | ubuntu@가상서버공인IP |

```shell-session
$ sudo -i
```

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/21/c1430ed520c82eb27629c225aea9174c_1582268758.png" alt=""><figcaption></figcaption></figure>

#### (2) Mac 또는 SSH 클라이언트 시스템에서 접속하기

cafe24 키페어의 개인키 권한을 600으로 변경한 후, 접속합니다.

```shell-session
$ chmod 600 cafe24.pem
$ ssh -i cafe24.pem [일반계정]@[가상서버공인IP]
```

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/07/30/4f0bbaf45d5dae00d470e2181c73a241_1627625875.png" alt=""><figcaption></figcaption></figure>



#### B-2. SSH 키페어 분실 시 대처 방법

{% hint style="danger" %}
<mark style="color:red;">**주의 사항**</mark>

카페24 클라우드는 기본으로 키페어 접속 방법을 권장하며, 가상서버에 대한 패스워드는 제공하지 않습니다.&#x20;

하지만 불가피하게 패스워드를 통한 콘솔 접속을 원하실 경우 아래의 매뉴얼로 패스워드 생성이 가능합니다.



<mark style="color:red;">**비밀번호 노출로 인한 정보 유출 등의 보안상 이유로 패스워드 설정을 권장하지 않습니다.**</mark>
{% endhint %}

&#x20;****&#x20;

사용하시는 가상서버의 OS에 해당하는 매뉴얼을 참고하여 주세요.

* [**SSH키페어를 분실했습니다. 어떻게 해야 하나요 ? - Centos**](https://console.cafe24.com/support/faq/view?idx=92)
* [**SSH키페어를 분실했습니다. 어떻게 해야 하나요 ? - ubuntu**](https://console.cafe24.com/support/faq/view?idx=107)















