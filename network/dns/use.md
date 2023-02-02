---
description: DNS 사용 방법은 아래와 같습니다.
---

# DNS 사용 방법

## 1. DNS란?

DNS(Domain Name System)은 도메인 이름에 대한 IP 주소를 찾는 시스템입니다.

모든 웹 사이트는 각각의 호스트 IP를 가지며, 웹 사이트를 열기 위해서는 이 IP를 알고 주소 창에 입력해야 합니다.

하지만 웹 사이트를 IP로 접근하는 것은 매우 번거롭기 때문에 해당 IP에 매핑된 문자열인 도메인이 등장하게 됩니다.

예를 들어, "https://console.cafe24.com"이라는 URL을 웹 브라우저에 입력하면,

DNS는 "console.cafe24.com"이라는 도메인에 해당하는 IP 주소를 찾아 반환하여

사용자의 PC는 원하는 웹 페이지에 접속하게 됩니다.

DNS 서비스를 이용하여 이러한 작업을 쉽고 빠르게 수행할 수 있습니다.







## 2. 도메인 네임 서버 확인하기

<mark style="background-color:blue;">콘솔 > 네트워킹 > DNS</mark>

도메인 네임 서버는 도메인 이름과 IP의 상호 변환을 가능하게 하는 서버로, 카페24 클라우드에서는 계정 별로 4개의 네임 서버를 제공합니다.

\[네임서버 확인] 버튼을 클릭하여 제공된네임 서버를 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/10/23/6d683164075deec25b842b0a32d45ec4_1603421403.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/10/23/0b5dcdd76c45b1ec0aec8e6bcfe1ee96_1603433282.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
<mark style="color:blue;">**참고사항**</mark>

카페24 클라우드에서 레코드 관리를 하기 위해서는 도메인을 구매한 업체에 4개의 네임 서버를 등록하는 작업이 선행되어야 합니다.
{% endhint %}







## 3. 도메인 등록하기

\[새로운 도메인 등록하기] 버튼을 클릭한 후, 도메인명에 구매한 도메인을 입력합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/10/23/99f7f76308830dedead3bbfb38d8ae34_1603441523.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/10/23/cf314bf03fe94c59ec2aaf1674f23b65_1603441788.jpg" alt=""><figcaption></figcaption></figure>







## 4. 레코드 관리하기

레코드는 도메인에 관한 설정을 하는 데이터입니다.

레코드 관리를 통해 도메인에 대한 레코드를 추가, 수정 및 삭제할 수 있습니다.

레코드의 타입에 상관없이, 호스트명에 **** "**@**"를 입력하면 1차 도메인 자신을 가리키게 됩니다.

### (1) A 레코드

### (2) CNAME 레코드

### (3) MX 레코드

### (4) TXT 레코드
