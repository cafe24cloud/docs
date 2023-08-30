---
description: >-
  블록 스토리지를 생성 후 가상서버에 연결하기 위해서는, 웹 콘솔에서 블록 스토리지 연결을 한 뒤, shell에서 직접 mount 해줘야
  합니다.
---

# 블록 스토리지 연결 방법

## 1. 블록 스토리지 생성

<mark style="background-color:blue;">스토리지 > 블록스토리지</mark>

콘솔에서 "블록 스토리지 생성" 하기 버튼을 클릭합니다.&#x20;

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/25/41ac07ee9268cd42ccbf69d32d97f979_1582609005.png" alt=""><figcaption></figcaption></figure>

</div>

####

### (1) 기본정보 입력

* **이름**&#x20;
  * 블록 스토리지 이름을 입력합니다.&#x20;
  * 이름은 20자까지 지원이 가능하며, 영어, 숫자 및 특수문자 '-'만 사용이 가능합니다.
* **설명**
  * 설명을 입력 합니다. (필수사항 X)
* **용량**
  * 블록 스토리지 용량을 입력 합니다.
  * 블록 스토리지 용량은 10GB부터 1000GB까지 설정 가능하며, 10GB 단위로 입력해야 합니다.
  * 블록 스토리지는 생성 후에 확장할 수 있습니다.

기본 정보를 입력한 후, 버튼을 눌러 블록 스토리지를 생성합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/25/9d87136fb5f2b06931a5be93a95a9514_1582609015.png" alt=""><figcaption></figcaption></figure>

</div>







## 2. 블록 스토리지 연결

가상서버에 블록스토리지를 연결합니다.

다음 두 방법 중 한가지를 선택하여 작업합니다.&#x20;

* [블록 스토리지 메뉴에서 연결하기](connect.md#1-1)
* [가상서버 메뉴에서 연결하기](connect.md#2)



### (1) 블록 스토리지 메뉴에서 연결하기

<mark style="background-color:blue;">스토리지 > 블록 스토리지</mark>

사용할 블록 스토리지를 선택합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/25/e89113233aecba29ee5436576aaca613_1582610550.png" alt=""><figcaption></figcaption></figure>

</div>

연결할 가상서버를 선택 후 연결 버튼을 클릭합니다.   &#x20;

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/25/6ef175bc2dae6690243e3aae3f77a20f_1582610559.png" alt=""><figcaption></figcaption></figure>

</div>

&#x20;



### (2) 가상서버 메뉴에서 연결하기

<mark style="background-color:blue;">서버 > 가상서버</mark>

가상서버에 연결할 블록 스토리지를 다중 선택할 수 있습니다.

가상서버를 선택 한 후, 기능별 설정에서 "블록 스토리지 연결"을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/04/06/b5cc91ff5efcb6d572b567d0be1899f3_1649204634.png" alt=""><figcaption></figcaption></figure>

해당 가상서버에 연결할 블록 스토리지를 선택합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/04/06/e94d60aa010026ee32c5540027005f72_1649204643.png" alt=""><figcaption></figcaption></figure>

&#x20;



## 3. 연결된 블록 스토리지 정보 확인

<mark style="background-color:blue;">서버 > 가상서버</mark>&#x20;

가상서버를 선택한 후, 기능별 설정에서 가상서버에 연결된 블록 스토리지 정보를 확인합니다.&#x20;

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/04/06/d16209ff28135764c9efc82bd24da73e_1649204654.png" alt=""><figcaption></figcaption></figure>

</div>

#### &#x20;





## 4. 블록 스토리지 파티셔닝

가상서버에서 블록 스토리지를 파티셔닝 합니다.

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

새 블록 스토리지가 아닌 기존의 데이터가 있는 블록 스토리지의 경우,

해당 작업 없이 바로 **6. 디스크 마운트하기** 과정을 진행하시면 됩니다.
{% endhint %}

### (1) 디스크 정보 확인

"**fdisk -l**" 명령어로 디스크 정보를 확인합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/25/e4b0c787449220fe43719c686ac97bf4_1582616476.png" alt=""><figcaption></figcaption></figure>

</div>

&#x20;



### (2) 파티션 구성 하기

a. **fdisk /dev/vdc** 명령어로 파티션 작업을 진행 합니다.

b. Command (m for help): **n** 새로운 파티션을 생성합니다. [※ 참고사항](connect.md#undefined-2)

c. Select (default p): **p**

&#x20;   "p" 입력후 나오는 값들에 **default** 입력하시거나 엔터를 쳐서 결과값을 넘어갑니다.

d. Command (m for help): **w** 저장합니다

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/25/2cbb06a4c1911c3f05b1be4d6f61d940_1582615835.png" alt=""><figcaption></figcaption></figure>

</div>







## 5. 파일시스템 생성 하기

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

새 블록 스토리지가 아닌 기존의 데이터가 있는 블록 스토리지의 경우,

해당 작업 없이 바로 **6. 디스크 마운트하기** 과정을 진행하시면 됩니다.
{% endhint %}

### (1) 파일 시스템 생성하기

&#x20;**mkfs.xfs /dev/vdc1** 으로 파일시스템을 생성합니다.

ext4, ext3, ext2 등 다양한 [파일시스템](https://ko.wikipedia.org/wiki/%ED%8C%8C%EC%9D%BC\_%EC%8B%9C%EC%8A%A4%ED%85%9C)이 지원됩니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/25/a10ca426909aa13566046d2abdd062cf_1582615876.png" alt=""><figcaption></figcaption></figure>

</div>

&#x20;



### (2) 파일 시스템 설정 확인

**fdisk -l** 명령어로 파일시스템 설정을 확인 합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/25/6cd40129232bf892f13a2a29272e7b79_1582615890.png" alt=""><figcaption></figcaption></figure>

</div>







## 6. 디스크 마운트하기

### (1) 디스크 마운트

마운트할 디렉토리를 생성 후 디스크 연결작업 진행 합니다.&#x20;

a. **mkdir /data** 로 디렉토리를 만듭니다.

b. **mount /dev/vdc1 /data/** 마운트 합니다.&#x20;

c. **df -h** 명령어로 /data 폴더와의 연결을 확인합니다.&#x20;

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/26/16b8627f2dbcae14a160d1a5098de915_1582705375.png" alt=""><figcaption></figcaption></figure>

</div>





### (2) fstab 등록

가상서버 재부팅 이후에도 지속적으로 마운트 진행 하려면 아래와같은 방법을 적용합니다.

**blkid** 명령어로 디스크에 UUID 값을 확인합니다.

블록 스토리지 연결/해제 시 디바이스명이 바뀔 수 있기 때문에 자동 마운트 시에는 UUID를 입력합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/25/2dde6068fc54381d4df02102a825a52c_1582616280.png" alt=""><figcaption></figcaption></figure>

</div>

"**vi /etc/fstab**" 명령어로 /etc/fstab 파일을 열어 수정합니다.&#x20;

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/25/ff319ddb1635041b836ad6e424dfc464_1582616012.png" alt=""><figcaption></figcaption></figure>

</div>



#### **※ 참고사항**

**fdisk 명령어 옵션 주요 정보입니다.**

<table><thead><tr><th width="143">명령어</th><th>내용</th></tr></thead><tbody><tr><td>d</td><td>파티션을 삭제 합니다. </td></tr><tr><td>l</td><td>파티션 타입 종료를 조회합니다.</td></tr><tr><td>n</td><td>새로운 파티션을 추가합니다.</td></tr><tr><td>p</td><td>현재 디스크 정보를 보여줍니다.</td></tr><tr><td>v</td><td>파티션 테이블을 확인합니다.</td></tr><tr><td>i</td><td>파티션 정보를 보여줍니다. </td></tr></tbody></table>
