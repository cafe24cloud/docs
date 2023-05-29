---
description: 가상서버 모니터링 알람 설정 및 조회하는 방법은 다음과 같습니다.
---

# 사용량 알림 설정 방법

## 1. 알람 수신자 설정

### (1) 관리자 추가

&#x20;<mark style="background-color:blue;">콘솔 > 오른쪽 상단의 드롭다운 \[알림 관리]</mark>

가상서버 모니터링 알람을 수신할 관리자를 추가합니다.&#x20;

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/01/06/769842fcb80584ca8b3bb21cc329dadd_1609912852.jpg" alt=""><figcaption></figcaption></figure>

</div>

해당 계정 소유자 외의 관리자를 추가할 수 있습니다.

관리자 추가 버튼을 클릭합니다.&#x20;

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/01/06/6c5d8e8242dd8b46a05c4aaaa6ddc9ee_1609910278.jpg" alt=""><figcaption></figcaption></figure>

</div>

&#x20;



### (2) 관리자 정보 입력

추가할 관리자에 대한 정보를 입력합니다.

관리자 알림 항목을 통해 알람 발송 범위를 설정할 수 있습니다.

#### **a. 서비스 관련 알림**

* E-mail : 클라우드 서비스의 생성/삭제 등 각종 클라우드 서비스 관련 알림, 서비스 차단/해지 관련 알림

&#x20;

**b. 결제 관련 알림**

* E-mail : 클라우드 요금 결제/미납 등의 결제 관련 알림 제공
* SMS : 클라우드 요금 결제/미납 등의 결제 관련 알림 제공

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/01/06/0af52e92e9e88ab4767c6f89c7dfd2b6_1609910544.jpg" alt=""><figcaption></figcaption></figure>

</div>





&#x20;

## 2. 가상서버 모니터링 알람 설정

### (1) 알람 설정

<mark style="background-color:blue;">콘솔 > 가상서버</mark>

모니터링 알람을 설정할 가상서버를 선택한 뒤 알람 설정 버튼을 클릭합니다. &#x20;

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/01/06/118a99fd28c71f6742faddaa5de8d2d4_1609910189.jpg" alt=""><figcaption></figcaption></figure>

</div>

&#x20;

### (2) 알람 조건 선택

모니터링 알람 설정 팝업에서 알람 조건을 선택 및 추가합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/01/06/2c3c9801e394c4d6731800b101fa1e03_1609895669.jpg" alt=""><figcaption></figcaption></figure>

</div>

&#x20;



### (3) 알람 조건 생성

가상서버의 리소스 별로 알람 규칙을 설정 할 수 있습니다.&#x20;

선택한 항목에 대한 조건이 충족될 경우 수신 대상자에게 이메일, SMS 알람이 가게 됩니다.

수신 대상 리스트에서 수신자를 선택할 수 있으며, 추가가 필요한 경우 하단의 '관리자 추가로 이동'을 클릭합니다.

&#x20;

제공되는 가상서버 알람 항목은 다음과 같습니다.&#x20;

* **CPU** : CPU 사용률을 이상 / 이하 조건으로 설정할 수 있습니다.&#x20;
* **Memory** : Memory 사용률을 이상 / 이하 조건으로 설정할 수 있습니다.&#x20;
* **Traffic** : Traffic(Outbound) 사용률을 이상 조건으로 설정합니다.
  * 지정한 지속시간 내에 발생한 트래픽의 누적치가 임계값에 도달하면 알람을 발송합니다.
  * 다만 알람은 5분 단위로 임계치 도달 여부를 확인하여 발송 됩니다.
  * 실질적인 트래픽 발생 시간과 알람 수신 시간은 최대 5분의 차이가 있을 수 있습니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/04/06/e53d71c8f0674f728b5071584029a48b_1649204136.jpg" alt=""><figcaption></figcaption></figure>

</div>

&#x20;





## 3. 알람 수신 및 내역 확인

### (1) 모니터링 알람 Email 수신

설정한 알람 기준에 도달하면 해당 규칙에 등록한 관리자의 Email 및 SMS로 모니터링 알람이 발송됩니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/01/06/29b4162a062a0c2b13fe5d81eb5279bc_1609910909.jpg" alt=""><figcaption></figcaption></figure>

</div>





### (2) 알람 내역 확인

<mark style="background-color:blue;">콘솔 > 가상서버 > 알람 내역</mark>

설정한 조건에 따라 발생한 알람은 "알람 내역" 메뉴에서 확인 할 수 있습니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/01/06/e636404c06edce0c69f44302e4e1b106_1609911860.jpg" alt=""><figcaption></figcaption></figure>

</div>

모니터링 알람 내역에서는 선택한 기간에 발생한 알람의 내역을 확인할 수 있습니다.

대상은 알람이 발생한 가상서버를 의미하며, 수신자는 알람을 수신한 관리자의 이름입니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/01/06/5a92e4bf78775c75ea24973fb0ae43e5_1609900521.jpg" alt=""><figcaption></figcaption></figure>

</div>

&#x20;

