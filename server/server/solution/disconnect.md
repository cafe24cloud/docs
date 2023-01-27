---
description: 본 매뉴얼은 가상서버 SSH 키페어 접속 시, 발생 가능한 문제들에 대한 해결책을 제시합니다.
---

# 접속 오류 해결 방법

## 1. 일반적인 원인

가상서버 SSH 키페어 접속 오류의 빈번한 원인은 다음과 같습니다.

접속 문제가 있으실 경우, 아래의 사항들이 지켜졌는지 확인해 주세요.

### (1) 가상서버 OS별 계정 정보

카페24 클라우드에서 제공하는 OS별 가상서버 초기 계정은 다음과 같습니다.

|   OS   |   계정   |    접속 방법    |
| :----: | :----: | :---------: |
|  Rocky |  rocky |  rocky@공인IP |
| CentOS | centos | centos@공인IP |
| Ubuntu | ubuntu | ubuntu@공인IP |



### (2) 방화벽 설정

가상서버 SSH 키페어 접속을 하기 위해서는 가상서버에 아래와 같이 22번 INBOUND 보안 정책이 추가되어 있어야 합니다.

자세한 방법은 [<mark style="color:blue;">**\[방화벽 설정 방법\]**</mark>](../../../security/security/config.md)을 참고해 주세요.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/09/902590a1c1bbd8987f32004c56f8dfef_1607499585.jpg" alt=""><figcaption></figcaption></figure>



### (3) 가상서버 상태 확인

가상서버가 "운영중"인지 확인합니다. "중지" 혹은 "재부팅" 중인 서버에는 접속이 불가능합니다.





## 2. 에러별 해결 방안

가상서버 SSH 키페어 접속 시, 에러별 해결 방안은 다음과 같습니다.

### (1) Server refused our key, Permission denied

* **Putty 접속 시 에러 메시지** : Server refused our key, No supported authentication methods available

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/28/eb155c7cb47be9757e301071d6581fa0_1609145677.jpg" alt=""><figcaption></figcaption></figure>

* **리눅스 터미널 접속 시 에러 메시지** : Permission denied

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/12/28/0a0c40306aa00d3673915d244c78b7ea_1609145339.jpg" alt=""><figcaption></figcaption></figure>

해당 오류가 발생하는 이유는 아래와 같습니다.

#### ① 가상서버 접 시 잘못된 계정명을 사용할 경우

해당 내용에 대해서는 [**\[**<mark style="color:blue;">**가상서버 OS별 계정 정보\]**</mark>](disconnect.md#1-os)를 참고해 주세요.

#### ② 사용자 계정의 .ssh 디렉터리가 누락되었거나 권한 문제가 있는 경우

가상서버에 접속하여 SSH 키페어 접속에 필요한 디렉터리 및 파일의 권한을 확인합니다.

본 매뉴얼에서는 centos 계정을 예로 들어 설명합니다. "centos"에 해당하는 부분은 접속하고자 하는 계정명으로 바꿔주세요.

SSH 키페어 접속이 불가능할 경우, \[SSH 키페어 분실 시 해결 방법]을 참고하여 패스워드 설정 후, 콘솔 접속을 해주세요.









#### ③ 키페어 접속을 위한 공개키 정보가 손상된 경우

#### ④ 가상서버에 접속하려는 사용자가 추가 혹은 삭제된 경우

#### ⑤ 개인(.pem)파일의 .ppk 파일 변환 과정에서 오류가 있는 경우





### (2) Unprotected private key file

### (3) Remote host identification has changed

### (4) Network error: Connection timed out
