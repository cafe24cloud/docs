---
description: 인증서를 사용 방법은 아래와 같습니다.
---

# 인증서 사용 방법

## 1. 인증서 관리 서비스란?

카페24 클라우드의 인증서 관리 서비스는 인증서를 콘솔에서 편리하게 관리할 수 있도록 지원합니다.&#x20;

인증서 관리 서비스를 통해 공인 인증서를 등록하여 클라우드 콘솔 내 다른 서비스와 연계할 수 있습니다.&#x20;

또한, 관리자는 SMS 및 Email로 인증서의 만료 예정일, 폐기 여부 등을 알람 받을 수 있으며,&#x20;

인증서의 발급 기관, 만료일 등의 상세 정보를 확인할 수 있습니다.

{% hint style="info" %}
카페24 클라우드의 인증서 관리 서비스는 콘솔 내 **로드밸런서** 서비스에만 적용 가능합니다.
{% endhint %}







## 2. 인증서 생성하기

### (1) 인증서 등록하기

<mark style="background-color:blue;">콘솔 > 보안 서비스 > 인증서 관리</mark>

\[새로운 인증서 생성] 버튼을 클릭합니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/14/1d6ac987dc590c6d7485dce940276986_1647184249.png" alt=""><figcaption></figcaption></figure>

</div>

인증서 등록 창에서 아래 항목을 확인하여 인증서를 등록합니다.

{% hint style="info" %}
<mark style="color:blue;">**참고사항**</mark>

인증서 관리 서비스는 **공인 인증서**만 등록 가능하며, **x.509 encoded**된 **.pem 형식**의 인증서를 지원합니다.

인증서를 등록이 실패할 경우 [자주 발생하는 오류](use.md#2)를 참고해 주세요.
{% endhint %}

① **인증서 이름**

등록할 SSL 인증서의 이름을 지정합니다.

인증서의 이름은 기존에 등록된 인증서와 중복될 수 없습니다.

② **개인 키 입력**

인증서의 복호화된 개인 키를 붙여 넣습니다.

RSA 개인 키의 예시는 다음과 같습니다.

```shell
-----BEGIN RSA PRIVATE KEY-----
 ... Base64–encoded private key ...
-----END RSA PRIVATE KEY-----
```

③ **인증서 본문**

인증서의 본문을 붙여 넣습니다.

인증서 본문의 예시는 다음과 같습니다.

```shell
-----BEGIN CERTIFICATE-----
 ... Base64–encoded certificate ...
-----END CERTIFICATE-----
```

④ **인증서 체인(선택)**

인증서 체인은 선택 사항으로, 보유한 인증서 체인이 있는 경우 붙여 넣습니다.&#x20;

발급된 인증서 체인이 있으나 등록하지 않으면 인증서 검증 시 오류가 발생합니다.

인증서 체인에는 Root CA의 PEM 파일이 포함되어 있어야 합니다.

인증서 체인의 예시는 다음과 같습니다.

```shell
-----BEGIN CERTIFICATE-----
 ... Base64–encoded certificate ...
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
 ... Base64–encoded certificate ...
-----END CERTIFICATE-----
```



<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/14/4775fddd82d4f714ae2665fb19dcaea9_1647186667.png" alt=""><figcaption></figcaption></figure>

</div>





### (2) 자주 발생하는 오류

인증서 등록이 실패하는 경우는 다음과 같습니다.

#### a. **개인 키와 인증서 본문이 일치하지 않습니다.**&#x20;

입력한 개인 키와 인증서 본문이 달라 인증서 검증이 불가한 상태입니다.&#x20;

동일 인증서의 파일이 맞는지 확인이 필요합니다.



#### b. **개인 키/인증서 본문/CA 인증서 본문 형식이 올바르지 않습니다.**

각 요소의 파일 형식에 오류가 있으면 발생합니다.&#x20;

x.509 encoded된 .pem 형식의 인증서가 잘린 부분 없이 복사/붙여 넣기 되었는지 확인합니다.



#### c. **폐기/만료된 인증서는 등록이 불가합니다.**&#x20;

폐기되거나 만료된 상태의 인증서는 등록할 수 없습니다.



#### d. **사설 인증서는 지원하지 않습니다.**&#x20;

카페24 클라우드의 인증서 관리 서비스는 정식 CA의 승인을 받은 인증서만 지원합니다.&#x20;

따라서 openssl 등으로 CA 없이 자체 발행한 인증서는 등록이 안 될 수 있습니다.



#### e. **개인 키가 암호화되어 있습니다. 복호화 과정을 거친 후 입력하여 주시기 바랍니다.**

개인 키는 복호화된 상태에서 입력해야 합니다.&#x20;

암호화된 개인 키는 다음과 같이 복호화 할 수 있습니다.

<pre class="language-shell-session"><code class="lang-shell-session"><strong>## openssl rsa –in [암호화된 개인키] -out [복호화하여 저장할 키파일 이름]
</strong><strong>$ openssl rsa -in encrypted.pem -out decrypted.pem
</strong></code></pre>







## 3. 인증서 확인하기

### (1) 인증서 정보 확인하기

등록한 인증서에 대한 다음과 같은 정보를 확인할 수 있습니다.

① **이름** : 인증서를 등록하면서 지정한 이름입니다.&#x20;

② **대표 도메인** : 인증서에 대표로 설정된 도메인입니다.&#x20;

③ **유효기간 시작일 및 종료일** : 인증서의 발급일과 만료일입니다.

④ **등록일시** : 카페24 클라우드에 해당 인증서를 등록한 일자입니다.

그 외에도 상세정보 탭에서 해당 인증서의 핑거 프린트, 일련번호, 발급기관, 서명 알고리즘을 확인할 수 있습니다.&#x20;

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/15/e35be2939fa46ed9976ac579d064dea3_1647328127.jpg" alt=""><figcaption></figcaption></figure>

</div>





### (2) 인증서 상태 확인하기

인증서를 등록하면 상태를 자동으로 체크하여 상태 값을 제공하며, 인증서의 상태 값은 다음과 같습니다.&#x20;

<table><thead><tr><th width="178" align="center">상태 값</th><th align="center">설명</th></tr></thead><tbody><tr><td align="center">정상</td><td align="center">인증 만료일이 도래하지 않은 정상적인 상태의 인증서</td></tr><tr><td align="center">만료</td><td align="center">인증 만료일 이후의 인증서 (expired)</td></tr><tr><td align="center">폐기</td><td align="center">폐기된 인증서 (revoked)</td></tr><tr><td align="center">등록실패</td><td align="center">정상적으로 등록되지 않은 인증서 (인증서의 정보가 부정확하여 발생)</td></tr><tr><td align="center">확인불가</td><td align="center">공급사로부터 인증서 상태를 확인할 수 없는 인증서</td></tr></tbody></table>

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/14/9e0adc7ea835f6f5a04940f23804ede3_1647190432.jpg" alt=""><figcaption></figcaption></figure>

</div>

{% hint style="info" %}
<mark style="color:blue;">**참고사항**</mark>

인증서 관리에 등록된 인증서에 대하여 상태를 주기적으로 체크하여 알람 서비스를 제공합니다.

<mark style="background-color:blue;">콘솔 오른쪽 상단의 계정명 > 알람관리</mark>

관리자는 \[관리자 추가] 버튼을 클릭한 후, 관리자 정보를 입력합니다.

그리고 **서비스 관련 알림**에 체크를 하면, 인증서에 대한 알림을 받을 수 있습니다.

* 만료 예정 인증서 : 만료 30일 전부터 5일 단위로 만료 알림 발송
* 폐기 인증서 : 폐기된 인증서를 매일 체크하여 알람 발송
{% endhint %}





### (3) 인증서 사용 여부 확인하기

인증서의 사용 여부와 인증서가 사용된 서비스를 확인할 수 있습니다.

① **인증서 사용 여부**

인증서의 사용 여부 값은 다음과 같습니다.

<table><thead><tr><th width="178" align="center">사용 여부 값</th><th align="center">설명</th></tr></thead><tbody><tr><td align="center">사용중</td><td align="center">인증서가 카페24 클라우드의 다른 서비스와 연계되어 사용 중인 경우</td></tr><tr><td align="center">미사용</td><td align="center">인증서가 카페24 클라우드의 다른 서비스와 함께 사용되지 않는 경우</td></tr></tbody></table>

② **사용중인 서비스**&#x20;

\[보기] 버튼을 클릭하여 카페24 클라우드에서 해당 인증서와 연계 되어 사용 중인 서비스를 확인할 수 있습니다.

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/14/e95102bfc5ccd75850cbbaaf17b581a5_1647188832.jpg" alt=""><figcaption></figcaption></figure>

</div>

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/14/663c550a23feddbe9c81388eb617fda0_1647189254.jpg" alt=""><figcaption></figcaption></figure>

</div>







## 4. 인증서 연결하기

카페24 클라우드에서 제공하는 로드밸런서의 TERMINATED\_HTTPS  프로토콜을 사용하면 웹 서버에서 클라이언트 IP를 수집할 수 있습니다.&#x20;

이를 위해서는 로드밸런서에 정상적으로 동작하는 인증서를 등록해야 하며, 자세한 방법은 [<mark style="color:blue;">\[로드밸런서 사용 방법\]</mark>](../../network/loadbalancer/create.md)을 참고해 주세요.







## 5. 인증서 삭제하기

삭제할 인증서의 상세정보 탭에서 \[삭제] 버튼을 클릭합니다.

삭제한 인증서는 복구가 불가능하며, 다른 서비스에서 사용 중인 인증서는 해제 전에 삭제할 수 없습니다.&#x20;

<div align="left">

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/03/16/ba1c86c4257bec4a58e76112d0847db4_1647367441.jpg" alt=""><figcaption></figcaption></figure>

</div>
