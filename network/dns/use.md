---
description: DNS 사용 방법은 아래와 같습니다.
---

# DNS 사용 방법

## 1. DNS란?

DNS(Domain Name System)은 도메인 이름에 대한 IP 주소를 찾는 시스템입니다.

모든 웹 사이트는 각각의 호스트 IP를 가지며, 웹 사이트를 열기 위해서는 이 IP를 알고 주소 창에 입력해야 합니다.

하지만 웹 사이트를 IP로 접근하는 것은 매우 번거롭기 때문에 해당 IP에 매핑된 문자열인 도메인이 등장하게 됩니다.

예를 들어, "https://console.cafe24.com"이라는 URL을 웹 브라우저에 입력하면,

DNS는 "console.cafe24.com"이라는 도메인에 해당하는 IP 주소를 찾아 반환하여 사용자의 PC는 원하는 웹 페이지에 접속하게 됩니다.

DNS 서비스를 이용하여 이러한 작업을 쉽고 빠르게 수행할 수 있습니다.







## 2. 도메인 네임 서버 확인하기

<mark style="background-color:blue;">콘솔 > 네트워킹 > DNS</mark>

도메인 네임 서버는 도메인 이름과 IP의 상호 변환을 가능하게 하는 서버로, 카페24 클라우드에서는 계정 별로 4개의 네임 서버를 제공합니다.

\[네임서버 확인] 버튼을 클릭하여 제공된 네임 서버를 확인할 수 있습니다.

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

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/10/23/47ca031f9555adc34dfcb4936473d0b9_1603442523.jpg" alt=""><figcaption></figcaption></figure>

레코드의 타입에 상관없이, 호스트명에 **** "**@**"를 입력하면 1차 도메인 자신을 가리키게 됩니다.

카페24 클라우드에서 제공하는 레코드는 다음과 같습니다.

### (1) A 레코드

A 레코드는 호스트명과 가상서버의 IP 주소를 매핑하기 위해 주로 사용합니다.

#### a. **1차 도메인 등록하기**

호스트명에 "**@**"를 입력하여 1차 도메인을 등록합니다.

다음과 같이 입력할 경우, "cafe24clouds.com"에 대해 IP "183.222.255.133"이 등록됩니다.&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/11/12/ec41f1c2ca4f4f21e93c6a8050d1aec0_1605157683.jpg" alt=""><figcaption></figcaption></figure>



#### b. 2차 도메인 등록하기

호스트명에 등록할 2차 도메인을 입력하고, 해당 도메인으로 접속할 가상서버의 IP를 입력합니다.

다음과 같이 입력할 경우, "demo-A.cafe24clouds.com"에 대해 IP "183.222.255.133"이 등록됩니다.&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/10/26/856560ea2644182ef6c4c928206dc6d5_1603647538.jpg" alt=""><figcaption></figcaption></figure>





### (2) CNAME 레코드

CNAME 레코드는 기존의 도메인에 대한 별칭을 지정하는 레코드입니다.&#x20;

호스트명에는 별칭으로 새롭게 지정할 2차 도메인명을 입력하고, 설정값에는 기존의 도메인을 입력합니다.

CNAME 레코드는 IP 주소가 자주 바뀌는 환경에서 레코드 수정 횟수를 줄일 수 있어 편리합니다.

하지만 실제 IP 주소를 얻을 때까지 DNS 정보를 여러 번 요청해야 하므로 성능 저하가 있을 수 있다는 점을 유의해야 합니다.

다음과 같이 입력할 경우, "demo-cname.cafe24clouds.com"이 "demo-A.cafe24clouds.com"의 별칭 도메인이 됩니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/10/26/1f15373697f598c801957707f03820ec_1603646921.jpg" alt=""><figcaption></figcaption></figure>





### (3) MX 레코드

MX(Mail Exchanger Record) 레코드는 사용자의 도메인에 대한 메일 서버를 지정하기 위해 사용합니다.

본 매뉴얼에서는 Google Workspace Email에 대한 MX 레코드를 등록하였습니다.

예를 들어 도메인이 "cafe24clouds.com"이고 Google 계정명이 "demo24"인 경우,

"**demo24@cafe24clouds.com**"**을 메일 주소**로 이용할 수 있게 됩니다.

Google Workspace 콘솔에서 제공하는 MX 레코드를 우선순위 오름차순으로 등록합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/11/06/f912c8adb946d36824753a47e62d49a9_1604650333.jpg" alt=""><figcaption></figcaption></figure>

최종 입력 화면은 다음과 같습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/11/05/3defd927ac135901920eb00827952fa4_1604536117.jpg" alt=""><figcaption></figcaption></figure>

MX 레코드를 등록한 도메인으로 메일 수신/발신이 가능한 것을 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/11/06/48b69ec94c8cf4891cf21b4039edf27b_1604649367.jpg" alt=""><figcaption></figcaption></figure>





### (4) TXT 레코드

TXT 레코드는 도메인에 대한 텍스트 정보를 설정하는 것으로, 주로 SPF 레코드와 같은 용도로 사용됩니다.

본 매뉴얼에서는 TXT 레코드를 SPF 기능으로 사용하는 방법에 대해 설명합니다.

TXT 레코드에 SPF 텍스트를 등록합니다.

예를 들어 **** "**v=spf1 include:\_spf.google.com \~all**"는 Google 메일 서버를 인증한다는 의미입니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/11/06/6c55a6789855ec5e7496c36848a84206_1604650564.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
<mark style="color:blue;">**참고사항**</mark>

**SPF(Sender Policy Framework)란?**

SPF는 해당 메일이 승인된 메일 서버에서 정상적으로 발신된 것인지 확인하는 기능을 합니다.

SPF를 사용하면 메일 스푸핑을 방지할 수 있다는 장점이 있습니다.

메일 스푸핑은 발신지를 위조하여 승인된 조직에서 보내는 것처럼 보이게 하는 것으로, 악성 소프트웨어 배포 등의 악의적인 목적으로 쓰입니다.

이에 SPF를 등록하여 수신 서버에서 이메일이 승인된 메일 서버에서 보낸 것인지 인증할 수 있도록 합니다.
{% endhint %}
