---
description: 블록 스토리지 해제를 위해서는 shell에서 블록 스토리지 mount를 해제한 뒤, 웹 콘솔에서 블록 스토리지 연결 해제를 하여야 합니다.
---

# 블록 스토리지 해제 방법

## 1. 블록 스토리지 마운트 해제

Shell에서 블록 스토리지의 mount를 해제 합니다.

{% hint style="info" %}
<mark style="color:blue;">**참고 사항**</mark>

대시보드에서 "블록 스토리지" 해제를 먼저 진행할 경우 서버에서 i/o 에러가 발생할 수 있습니다.&#x20;

데이터 유실 방지를 위해 shell에서 먼저 마운트를 해제해 주시기 바랍니다.
{% endhint %}



### (1) 연결 해제 할 블록 스토리지 확인

명령어 : **fdisk -l** 나 **df -h**&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/26/caa413608bdd52167f0ad45d1a734191_1582695776.png" alt=""><figcaption></figcaption></figure>

블록 스토리지를 마운트 해제 합니다.

명령어 : **umount**

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/26/b4f64f4ef88dff428f0d2f84cb5ea1e8_1582704933.png" alt=""><figcaption></figcaption></figure>

부팅 시 mount 설정이 되어 있다면 "**/etc/fstab**" 에 등록된 마운트 포인트를 삭제해야 합니다.

{% hint style="info" %}
<mark style="color:blue;">**참고 사항**</mark>

**/etc/fstab** 내용을 삭제해야 향후 서버 재부팅이나 디스크 마운트 시 문제가 발생하지 않습니다.
{% endhint %}

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/26/9098086149d67a1eff25bc51f6a99cdc_1582696026.png" alt=""><figcaption></figcaption></figure>



## 2. 웹 콘솔에서 블록 스토리지 해제

&#x20;웹 콘솔을 이용하여 다음 두 가지 방법으로 블록 스토리지를 해제 할 수 있습니다.

* [가상서버 대시보드에서 블록 스토리지 해제하기](disconnect.md#1-1)
* [블록 스토리지 대시보드에서 블록 스토리지 해제하기](disconnect.md#2)



### (1) 가상서버 대시보드에서 해제하기

{% hint style="info" %}
<mark style="color:blue;">**참고 사항**</mark>

가상서버의 상태가 운영중일 때 블록 스토리지 해제가 가능합니다. 가상서버의 상태를 운영 중으로 변경하여 주세요.
{% endhint %}

가상서버 선택 후 기능별 설정에서 블록 스토리지 항목에서 해제 버튼을 누릅니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/26/72f62cdf1047ce89e16668e2351ad5ec_1582696083.png" alt=""><figcaption></figcaption></figure>

&#x20;해제할 블록스토리지를 확인하고 **해제하기** 버튼을 클릭 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/26/843f94ef9c890fd57557b5a9f2cdf277_1582696112.png" alt=""><figcaption></figcaption></figure>





### (2) 블록 스토리지 대시보드에서 해제하기

<mark style="color:blue;">연결되어 있는 가상서버의 상태가 운영중일 때 블록 스토리지 해제가 가능합니다. 가상서버의 상태를 운영 중으로 변경하여 주세요.</mark>&#x20;

해제할 블록스토리지를 선택 후 상세정보의 **블록 스토리지 기능 > 해제** 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/26/67f5ba86b1239dee1cf73f8d8279b9f2_1582696202.png" alt=""><figcaption></figcaption></figure>

&#x20;해제할 블록스토리지를 확인하고 **해제하기** 버튼을 클릭 합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/02/26/a0fcaccd69e73aa9d6f16c18b985a687_1582696213.png" alt=""><figcaption></figcaption></figure>





