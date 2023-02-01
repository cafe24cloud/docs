---
description: 블록 스토리지 확장을 위해서는 웹 콘솔의 확장 기능과, shell을 이용한 실제 용량 확장을 진행히야 합니다.
---

# 블록 스토리지 확장 방법

## 1. 블록 스토리지 마운트 해제 및 패키지 설치

가상서버에서 블록 스토리지를 해제 후 확장작업시 필요한 linux 패키지를 설치합니다.

{% hint style="danger" %}
<mark style="color:red;">**주의 사항**</mark>

용량 확장시 데이타가 삭제 되거나 유실 될수 있습니다. 작업전 백업을 꼭 하시고 작업 하시기 바랍니다.(기본)
{% endhint %}

CentOS/Rocky 사용시에, shell 에서 확장된 블록스토리지를 반영하기 위해서는 "**cloud-utils-growpart**" 패키지가 필요 합니다.



**"df -h" 명령어와 "mount | grep /dev/vdb1"** 명령어를 이용하여 확장할 블록 스토리지를 확인 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/03/f8708843c54ecef516b4877440c669ff_1583192744.png" alt=""><figcaption></figcaption></figure>

"**umount /data**" 명령어로 블록 스토리지를 해제 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/03/08023e60afda47886f4adb981c76f99a_1583192773.png" alt=""><figcaption></figcaption></figure>

CentOS, Rocky 이미지를 사용할 시에 **cloud-utils-growpart** 패키지를 설치 합니다.

해당 패키지는 확장된 블록 스토리지 용량을 반영하기 위해서 필요합니다.&#x20;

명령어 : "**yum install cloud-utils-growpart**"

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/03/c54d49369a0e1ea567a7099ea90f9ef1_1583192697.png" alt=""><figcaption></figcaption></figure>







## 2. 블록 스토리지 확장하기

{% hint style="info" %}
<mark style="color:blue;">**참고 사항**</mark>

블록 스토리지의 용량을 확장하기 위해서는 가상서버 연결을 먼저 해제해야 합니다.
{% endhint %}

<mark style="background-color:blue;">웹콘솔 → 스토리지 → 블록스토리지 용량 확장 →  확장</mark>&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/03/20fd81770f4c2625c26a2cdd21c0d90b_1583192591.png" alt=""><figcaption></figcaption></figure>

블록 스토리지 확장 대상에 용량을 입력 후 금액을 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/03/731fdcbae469964ac35024da447820f7_1583192609.png" alt=""><figcaption></figcaption></figure>

확장된 블록 스토리지를 가상서버에 다시 연결합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/03/b74fd85894142daf7bd098a4309bdc9a_1583192618.png" alt=""><figcaption></figcaption></figure>



## 3.  확장된 블록 스토리지 적용하기

가상서버에 로그인하여 shell에서 확장된 블록 스토리지를 확인 후 "growpart" 명령어로 확장된 용량을 적용 합니다.

① "**growpart"** 명령어를 사용합니다.

② "**lsblk"** 명령어로 확장할 블록스토리지를 확인합니다.

③ "**growpart /dev/vdb 1**" 명령어로 블록스토리지를 확장합니다.

④ **"lsblk"** 명령어로 확장된 블록스토리지를 확인합니다.

⑤ "**mount /dev/vdb1 /data**" 명령어로 블록스토리지를 마운트 합니다.

⑥ "**xfs\_growfs /dev/vdb1**" 명령어로 섹터 변경.

⑦ "**df -h**" 명령어로 확장된 용량을 확인합니다.  &#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/07/31/5855ffed45a4efc2c4719237cd39e39a_1596179044.png" alt=""><figcaption></figcaption></figure>
