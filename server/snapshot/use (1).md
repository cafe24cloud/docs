---
description: 가상서버 및 블록 스토리지 스냅샷을 생성 및 삭제하는 방법은 다음과 같습니다.
---

# 가상서버 스냅샷 사용 방법

## 목차

#### [1. 스냅샷에 대한 이해](<use (1).md#1.-1>)  &#x20;

[(1) 스냅샷 이란?   \
](<use (1).md#1>)[(2) 스냅샷의 작동 방식](<use (1).md#2>)

#### [2. 가상서버 및 블록 스토리지 스냅샷 생성](<use (1).md#2.-1>)  &#x20;

[(1) 가상서버 준비   \
](<use (1).md#1-1>)[(2) 전체(가상서버 + 블록 스토리지) 스냅샷 생성   \
](<use (1).md#2-+>)[(3) 블록 스토리지 스냅샷 생성](<use (1).md#3>)

#### <mark style="color:blue;">3</mark>[<mark style="color:blue;">.</mark> ](broken-reference)<mark style="color:blue;"></mark>[<mark style="color:blue;">스냅샷을 이용한 오토스케일 구성</mark>](<use (1).md#3.-1>)    <mark style="color:blue;"></mark>   &#x20;

<mark style="color:blue;"></mark>[<mark style="color:blue;">(1) 오토스케일 구성</mark>](<use (1).md#1-2>)<mark style="color:blue;"></mark>

#### [4. 가상서버 및 블록 스토리지 스냅샷 삭제](<use (1).md#4.-1>)  &#x20;

[(1) 가상서버 스냅샷과 연결된 블록 스토리지 스냅샷 모두 삭제   \
](<use (1).md#1-3>)[(2) 단독 블록 스토리지 스냅샷 삭제](<use (1).md#2-1>)

#### [5. 리소스 삭제 불가 케이스별 해결 방안](<use (1).md#5.-1>)  &#x20;

[(1) 블록 스토리지를 삭제하기 위해서는 먼저 연결된 블록 스토리지 스냅샷을 삭제해야 합니다.   \
](<use (1).md#1-.>)[(2) 블록 스토리지 스냅샷이 가상서버 스냅샷과 연결되어 있습니다.   \
](<use (1).md#2-.>)[(3) 현재 가상서버 스냅샷이 오토스케일에서 사용되고 있습니다.   \
](<use (1).md#3-.>)[(4) 블록 스토리지 삭제 시 오토스케일에 구성되어 있어 삭제하지 못하는 경우](<use (1).md#4>)







## 1. 스냅샷에 대한 이해

### (1) 스냅샷 이란?

* 특정 시간에 데이터 저장 장치의 상태를 별도의 파일이나 이미지로 저장하는 기술
* 장애시 스냅샷을 생성한 일정 시점까지의 데이터를 복원할 수 있습니다.
* 또한 스냅샷을 이용하여 인스턴스를 증설하거나 오토스케일 사용 시 스냅샷 이미지를 등록할 수 있습니다.





### (2) 스냅샷에 대한 이미지 설명

&#x20; **스냅샷이 특정 시점의 볼륨 상태를 캡처하는 방법을 보여줍니다.**

* cafe24 가상서버의 현재 시점을 기준으로 스냅샷을 생성합니다.
* 가상서버와 연결 된 블록 스토리지와 함께 스냅샷을 생성 시 해당 블록 스토리지 스냅샷은 가상서버 스냅샷에 종속됩니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/06/c66995f788b265c99a6ca4293f085e92_1675665281.jpg" alt=""><figcaption></figcaption></figure>

**동일한 서버의 증설이 필요할 시 해당 스냅샷을 이용하여 손쉽게 확장할 수 있습니다.**

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/06/116114d89eb4bc24a87255605bfdc680_1675663590.jpg" alt=""><figcaption></figcaption></figure>







## 2. 가상서버 및 블록 스토리지 스냅샷 생성

### (1) 가상서버 준비

스냅샷을 생성할 가상서버를 준비합니다.\
본 예제에서는 추가 블록 스토리지를 연결 해 놓은 상태입니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/01/31/7d707bbd3fb2fd725e692e562a99be0d_1675148962.jpg" alt=""><figcaption></figcaption></figure>





### (2) 가상서버 + 블록 스토리지 스냅샷 생성

<mark style="background-color:blue;">**콘솔  > 서버  > 가상서버 스냅샷  > + 스냅샷 생성하기**</mark>\
기존에 가상서버와 연결된 블록 스토리지와 함께 전체 스냅샷을 생성합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/01/31/a8ca208330f67dde60d517403a68c2b0_1675149152.jpg" alt=""><figcaption></figcaption></figure>

상단에 스냅샷을 생성할 대상의 가상서버를 선택 후 스냅샷 명을 작성합니다.\
기존 가상서버에 블록 스토리지가 연결되어 있을 시 해당 블록 스토리지도 스냅샷이 생성됩니다.\
설명 부분은 선택사항 입니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/01/31/f565a40620473e76a3985c97fbda8548_1675149399.jpg" alt=""><figcaption></figcaption></figure>

생성 버튼 클릭 시 아래의 화면과 같이 가상서버 스냅샷이 생성된 것을 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/01/31/15da948d4ea261e526af64cf7f26f2aa_1675149950.jpg" alt=""><figcaption></figcaption></figure>

<mark style="background-color:blue;">**콘솔  > 스토리지  > 블록 스토리지 스냅샷**</mark> \
아래의 화면과 같이가상서버 스냅샷과 함께 블록 스토리지 스냅샷이 같이 생성된 것을 확인할 수 있습니다.\
전체 스냅샷(가상서버, 블록 스토리지)으로 생성한 블록 스토리지의 기본 스냅샷 명은 <mark style="color:blue;">**snapshot for \[가상서버 snapshot명]**</mark>** ** 입니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/01/31/a8a13911a38a0ef0cb1208a4ac0aae19_1675150508.jpg" alt=""><figcaption></figcaption></figure>





### (3) 가상서버 없이 개별 블록 스토리지 스냅샷 생성

<mark style="background-color:blue;">**콘솔  > 스토리지  > 블록 스토리지 스냅샷  > + 새로운 블록 스토리지 생성**</mark>\
블록 스토리지 스냅샷을 생성합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/01/e3c37a9b418424bd7503f0ffa6f6b483_1675212329.jpg" alt=""><figcaption></figcaption></figure>

상단에 스냅샷을 생성할 대상의 블록 스토리지를 선택 후 스냅샷 명을 작성합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/01/53f294ac506ed164669eeb36289a3248_1675213071.jpg" alt=""><figcaption></figcaption></figure>

아래의 화면과 같이 블록 스토리지 스냅샷이 생성된 것을 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/01/1b8f8558b2029b46424f81c8d3786726_1675213314.jpg" alt=""><figcaption></figcaption></figure>







## 3. 스냅샷을 이용한 오토스케일 구성

### (1) 오토스케일 구성

&#x20; 스냅샷을 이용한 오토스케일 구성 방법은 아래의 매뉴얼을 참조하시기 바랍니다.

* **오토스케일 매뉴얼** : <mark style="color:blue;">\[</mark>[<mark style="color:blue;">오토스케일 사용 방법</mark>](../autoscale/use.md)<mark style="color:blue;">]</mark>







## 4. 가상서버 및 블록 스토리지 스냅샷 삭제

### (1) 가상서버 스냅샷과 연결된 블록 스토리지 스냅샷 모두 삭제

<mark style="background-color:blue;">**콘솔  > 서버  > 가상서버 스냅샷**</mark>\
가상서버 스냅샷 삭제 시 해당 스냅샷과 연결된 블록 스토리지 스냅샷이 함께 삭제됩니다.\
단, 가상서버 스냅샷과 연결된 블록 스토리지 스냅샷을 단독으로 삭제는 불가능합니다.\
또한, 가상서버 스냅샷 삭제할 시 연결된 블록 스토리지 스냅샷도 함께 삭제되어 복구가 불가능합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/01/25e644e543ed97e813302130594258d3_1675217114.jpg" alt=""><figcaption></figcaption></figure>

**상기 내용에 동의합니다** 왼쪽 체크박스 선택 후 웹 콘솔 암호를 입력하여 삭제를 진행합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/01/faa64be634f60ad95df4d3ea9abb8b29_1675217273.jpg" alt=""><figcaption></figcaption></figure>





### (2) 개별 블록 스토리지 스냅샷 삭제

<mark style="background-color:blue;">**콘솔  > 스토리지  > 블록 스토리지 스냅샷**</mark>\
연결된 가상서버 스냅샷이 없는 블록 스토리지 스냅샷은 아래와 같이 삭제할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/01/9802dfcc0ad8b3a159cf1ae40355d9ca_1675218459.jpg" alt=""><figcaption></figcaption></figure>

**상기 내용에 동의합니다** 체크박스 선택 후 웹 콘솔 암호를 입력하여 삭제를 진행합니다.\
블록 스토리지 스냅샷을 삭제할 시 복구가 불가능 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/01/d4b9096114d82c75a63ec60ac1795b12_1675218479.jpg" alt=""><figcaption></figcaption></figure>







## 5. 리소스 삭제 불가 케이스별 해결 방안

### (1) 블록 스토리지를 삭제하기 위해서는 먼저 연결된 블록 스토리지 스냅샷을 삭제해야 합니다.

<mark style="background-color:blue;">**콘솔  > 블록 스토리지  > 삭제 할 블록 스토리지 선택 후 하단 메뉴의 삭제  > 가상서버 연결 해제  > 블록 스토리지 삭제**</mark>\
삭제하려는 블록 스토리지의 스냅샷이 존재할 경우 기존 블록 스토리지는 삭제되지 않습니다.\
연결된 블록 스토리지 스냅샷을 먼저 삭제하신 후 블록 스토리지를 삭제 바랍니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/01/5cd5eaf6ff7089726876bc345bd0219f_1675219430.jpg" alt=""><figcaption></figcaption></figure>

블록 스토리지 화면에서 해당 블록 스토리지의 생성된 스냅샷을 확인 및 삭제가 가능합니다.\
삭제를 진행하기 위해 해당 블록 스토리지 선택 후 하단 메뉴의 생성된 스냅샷에서 삭제를 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/08/feb5d84798155eaba6f013b7c82fcd62_1675843196.jpg" alt=""><figcaption></figcaption></figure>

삭제할 스냅샷 선택 및 **상기 내용에 동의합니다** 체크 및 콘솔 비밀번호 입력 후 삭제를 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/08/6c88d3102f1c0ff624fb013b7b9c15fd_1675843330.jpg" alt=""><figcaption></figcaption></figure>

삭제할 블록 스토리지를 선택한 후 선택 삭제를 클릭하여 삭제를 진행합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/06/28a0dfc48a707428ee7abb4ab688a74b_1675673445.jpg" alt=""><figcaption></figcaption></figure>

<mark style="color:blue;">**아래의 이미지는 각 Volume 을 삭제하기 위한 과정을 나타낸 예시 입니다.**</mark>

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/21/ac9fe1d0a365d36a68a3d128cb75e4df_1676970569.jpg" alt=""><figcaption></figcaption></figure>

해당 볼륨으로 생성한 스냅샷이나 스냅샷을 생성한 블록 스토리지 정보는 아래와 같이 두 가지 방법으로 확인할 수 있습니다.\
\
**블록 스토리지에서 해당 스냅샷 확인**

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/21/c49d4e10b98cf00a522ab030f0917cbb_1676941793.jpg" alt=""><figcaption></figcaption></figure>

**블록 스토리지 스냅샷에서 해당 스냅샷을 생성한 블록 스토리지 정보 확인**

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/21/b5177b69918a93867dc4405e12ba0245_1676941828.jpg" alt=""><figcaption></figcaption></figure>





### (2) 블록 스토리지 스냅샷이 가상서버 스냅샷과 연결되어 있습니다.

<mark style="background-color:blue;">**콘솔  > 서버  > 가상서버 스냅샷  > 해당 가상서버 스냅샷 삭제**</mark>** ** \
삭제하려는 블록 스토리지의 스냅샷이 가상서버 스냅샷과 연결되어 있는경우 블록 스토리지만 단독으로 삭제가 불가능합니다.\
<mark style="color:red;">가상서버 스냅샷 삭제 시 연결되어 있는 블록 스토리지 스냅샷도 같이 삭제 됩니다.</mark>

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/01/329209b14818af85ba615bf9857b1bf8_1675226041.jpg" alt=""><figcaption></figcaption></figure>

가상서버와 마운트 된 블록 스토리지와 함께 생성된 스냅샷에서 해당 블록스토리지 스냅샷은 가상서버 스냅샷에 종속되어 단독으로 삭제가 불가합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/21/1038e23ed6d03e192592775d57cc7a91_1676944327.jpg" alt=""><figcaption></figcaption></figure>

블록 스토리지 스냅샷 화면에서 삭제하려는 블록 스토리지 스냅샷과 연결 된 가상서버 스냅샷 이름과 ID를 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/07/4411a3fb1cb4b240ad6071b2477e201b_1675732965.jpg" alt=""><figcaption></figcaption></figure>

가상서버 스냅샷 화면에서 기존에 확인한 스냅샷 이름과 ID를 비교한 후 동일할 시 아래와 같이 삭제를 진행 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/07/6c01c6d425fdf193ccf2db361fb74a33_1675732976.jpg" alt=""><figcaption></figcaption></figure>

가상서버 스냅샷 삭제 시 연결된 블록 스토리지 스냅샷도 같이 삭제되는 것을 확인하실 수 있습니다.\
삭제 시 저장된 데이터는 복구가 불가능합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/07/d2b368b3c27b7ed9e0400dae76f97580_1675733018.jpg" alt=""><figcaption></figcaption></figure>





### (3) 현재 가상서버 스냅샷이 오토스케일에서 사용되고 있습니다.

<mark style="background-color:blue;">**콘솔  > 서버  > 오토스케일  > 해당 오토스케일 삭제 후 가상서버 스냅샷 삭제**</mark>\
해당 가상서버 스냅샷이 등록된 오토스케일을 삭제하셔야 가상서버 스냅샷 삭제가 가능합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/01/f1fb6d552953da6650273efbef9c1fff_1675227127.jpg" alt=""><figcaption></figcaption></figure>

**오토스케일 기본 구조 및 스냅샷 삭제 순서**

오토스케일이 해당 오토스케일 그룹으로 구성된 인스턴스를 모니터링 하며 리소스 임계치에 따라 해당 인스턴스 스냅샷을 이용하여 서버를 Scale Out 하거나 Scale In 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/07/bcad69fe11da75298add01cbc8eeb53b_1675746246.jpg" alt=""><figcaption></figcaption></figure>

\
오토스케일 생성에 사용된 가상서버 스냅샷 및 블록 스토리지 스냅샷은 단독으로 삭제할 수 없습니다.\
오토스케일 생성에 사용된 가상서버 스냅샷 및 블록 스토리지 스냅샷을 삭제하기 위해서는 오토스케일을 먼저 삭제해야 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/16/048707de1232c7bd50b2f7c1722174f3_1676523511.jpg" alt=""><figcaption></figcaption></figure>

해당 오토스케일 삭제를 진행합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/06/554595951701c0f6e243631b9634e85d_1675671070.jpg" alt=""><figcaption></figcaption></figure>

**상기 내용에 동의합니다.** 체크박스를 선택한 후 콘솔 비밀번호를 입력 후에 삭제를 진행합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/06/8011ca791a3182d3a4576bbc82729aea_1675671085.jpg" alt=""><figcaption></figcaption></figure>

가상서버 스냅샷 화면에서 삭제할 가상서버 스냅샷을 선택한 후 삭제를 진행합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/06/c1f6f4702bd77d8c0985304f855bdffc_1675671625.jpg" alt=""><figcaption></figcaption></figure>





### (4) 블록 스토리지로 생성한 스냅샷이 오토스케일이 구성되어 있어 원본 블록 스토리지 삭제가 불가능한 경우

<mark style="background-color:blue;">**콘솔  > 서버  > 오토스케일  > 해당 오토스케일 삭제  > 가상서버 스냅샷 삭제  > 블록 스토리지 삭제**</mark>\
오토스케일 생성에 사용된 가상서버 스냅샷 및 블록 스토리지 스냅샷을 삭제하기 위해서는 먼저 오토스케일을 삭제해야 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/07/3a5156eb91ce1fa7cac957d1e4c98449_1675735678.jpg" alt=""><figcaption></figcaption></figure>

<mark style="color:blue;">**오토스케일 부터 원본 블록 스토리지 삭제까지의 과정은 아래의 이미지를 통해 확인하실 수 있습니다.**</mark>

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/16/1f71e9e867dda896abc57aabedffbecf_1676524923.jpg" alt=""><figcaption></figcaption></figure>

구성되어 있는 오토스케일을 삭제합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/07/186096574ee9a364c26435366e03fc46_1675735690.jpg" alt=""><figcaption></figcaption></figure>

해당 블록 스토리지 스냅샷과 연결 된 가상서버 스냅샷을 삭제합니다.\
가상서버 스냅샷을 삭제 시 연결되어 있는 블록 스토리지 스냅샷도 같이 삭제됩니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/07/dfdb4ac462b5c228c4a428c60d734296_1675735697.jpg" alt=""><figcaption></figcaption></figure>

블록 스토리지에 연결된 가상서버와 연결을 해제 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/16/58d49c048f2a98c4012ed52e17ff7c57_1676525536.jpg" alt=""><figcaption></figcaption></figure>

블록 스토리지 삭제를 진행합니다.&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/07/c7077c89ef91108dbd97328bb0600754_1675735707.jpg" alt=""><figcaption></figcaption></figure>
