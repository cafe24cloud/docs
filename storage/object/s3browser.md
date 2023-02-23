---
description: S3Browser 오브젝트 스토리지를 사용하는 방법을 안내합니다.
---

# 오브젝트 스토리지S3Browser 사용 방법

## 1. S3Browser?

S3Browser는 카페24 클라우드의 오브젝트 스토리지와 연동하여 사용할 수 있는 Windows 클라이언트 도구입니다.\
오브젝트 스토리지의 API키를 S3Browser에 등록하여 PC와 오브젝트 스토리지 간에 파일을 업로드, 다운로드 할 수 있습니다.\
&#x20;\
• **S3Browser 다운로드** : [S3Browser](https://s3browser.com/)\
• **S3Browser 공식 사용 가이드** : [S3Browser 공식 문서](https://s3browser.com/help.aspx)\
&#x20;

&#x20;

## 2. S3Browser 설치하기

### (1) 설치 파일 다운로드

[S3Browser](https://s3browser.com/)에서 "Download S3 Browser Freeware" 버튼을 클릭하여 설치 파일을 다운받습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/f558d6ef6cb0397f8fff6821a6c4fbfd_1677043818.png" alt=""><figcaption></figcaption></figure>

#### &#x20;&#x20;

### (2) S3 Browser 설치하기

라이선스 정책에 동의한 후, Next를 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/fb10c78b5cf23084294a8afc545e603b_1677043831.jpg" alt=""><figcaption></figcaption></figure>

설치할 경로를 지정하고 Next를 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/d64cdbbe9b7996eed860865ed4660d21_1677043859.jpg" alt=""><figcaption></figcaption></figure>

아이콘 생성 여부를 체크하고 Next를 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/81dd922e4f95a99e2b93fc5cf3b1bf13_1677043871.jpg" alt=""><figcaption></figcaption></figure>

설치 완료됨을 확인하고 S3Browser를 엽니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/ee1090a76fac02b312f41da17d92a0b1_1677043883.jpg" alt=""><figcaption></figcaption></figure>

\
&#x20;\
&#x20;

## 3. S3Browser 계정 연동

Accounts > Add new account\
S3 Browser에서 오브젝트 스토리지에 접근하기 위해 계정을 연동합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/20/26de1dc27eabbc89b57ff0d7294ffe0b_1676903006.png" alt=""><figcaption></figcaption></figure>

카페24 오브젝트 스토리지의 **API 키**를 확인하는 방법은 [\[오브젝트 스토리지는 어떻게 사용하나요?\]](use.md) 를 참고해 주세요.\
① Account Name : 등록하는 계정의 이름을 지정합니다.\
② Account Type : "S3 Compatible Storage" 를 선택합니다.\
③ REST Endpoint : "kr.cafe24obs.com" 을 입력합니다.\
④ Access Key ID : 오브젝트 스토리지의 Access Key를 입력합니다.\
⑤ Secret Key : 오브젝트 스토리지의 Secret Key를 입력합니다.\
⑥ Use secure transfer : 오브젝트 업로드, 다운로드 시에 SSL/TLS 방식을 사용하는 것으로, 활성화합니다.\
⑦ Add new account를 클릭하여 설정을 저장합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/5a61e5bee985ea64b557382302c86fa8_1677043898.jpg" alt=""><figcaption></figcaption></figure>

\
&#x20;\
&#x20;&#x20;

## 3. 버킷 생성하기

"New Bucket" 버튼을 클릭하여 새로운 버킷을 생성합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/f699be2bfc88214a5e83e7cc4d9e05ae_1677043943.jpg" alt=""><figcaption></figcaption></figure>

① 버킷의 이름을 지정합니다.\
② "Create new bucket"을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/ea7c729248c8825e8dbbe139eef95288_1677043971.jpg" alt=""><figcaption></figcaption></figure>

\
&#x20;\
&#x20;

## 4. 파일 업로드

① 파일을 업로드할 버킷을 클릭 합니다.\
② Upload를 클릭합니다.\
③ 파일 또는 폴더를 업로드 할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/5f0b8039b1591feae9e457f627605dba_1677048171.jpg" alt=""><figcaption></figcaption></figure>

&#x20;S3Browser에서 마우스 우측 클릭으로 파일을 업로드 할 수도 있습니다.&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/8f47ad7bc5ab1e09b769e980d0c2fd57_1677048155.jpg" alt=""><figcaption></figcaption></figure>

해당 버킷에 파일 또는 폴더가 업로드된 것을 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/595d05ed628048784141f0e2dea25ee1_1677048143.jpg" alt=""><figcaption></figcaption></figure>



\
&#x20;&#x20;

## 5. 파일 다운로드

① 다운로드 할 파일이 있는 버킷을 선택합니다.\
② 다운로드 할 파일을 선택합니다.\
③ 다운로드 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/60d391c27751019839caf873664e954a_1677048091.jpg" alt=""><figcaption></figcaption></figure>

다운받고자 하는 파일 위에서 마우스 우측 클릭을 하여 파일을 다운로드 할 수도 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/95ef95272fe9367d14a2166ddf58f906_1677049260.jpg" alt=""><figcaption></figcaption></figure>

파일을 다운로드 받을 위치를 지정합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/2ccb62613be2f098fde8b5e1c4176385_1677048120.jpg" alt=""><figcaption></figcaption></figure>

PC의 해당 위치에 파일이 다운로드 된 것을 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/1383843d6b677bda36c29780d4b6fe81_1677048132.jpg" alt=""><figcaption></figcaption></figure>

\


&#x20;&#x20;

## 6. 버킷의 공개 여부 설정하기

버킷의 공개 여부로 모든 사용자에 대한 버킷 정보 조회 권한을 설정할 수 있습니다. 기본 설정은 비공개입니다.\
&#x20;

### (1) 버킷 공개 설정

① 공개 설정을 할 버킷을 선택합니다.\
② "Make public" 버튼을 클릭합니다.

③ Permissions 탭에서 모든 사용자에 대한 Read 권한이 부여된 것을 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/38255df4d1bc4850d430ba35fa951b16_1677055565.jpg" alt=""><figcaption></figcaption></figure>

버킷의 권한이 공개일 경우 버킷에 속한 파일 정보 및 계정 정보가 url을 통해 노출됩니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/607b516c359dbfac69fb829e534d320c_1677055550.jpg" alt=""><figcaption></figcaption></figure>

\
&#x20;

### (2) 버킷 비공개 설정

① 비공개 설정을 할 버킷을 선택합니다.\
② "Make private" 버튼을 클릭합니다.

③ Permissions 탭에서 모든 사용자에 대한 Read 권한이 제거된 것을 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/130fc7a3c2bebd4ff510bf4bd313d634_1677055639.jpg" alt=""><figcaption></figcaption></figure>

버킷의 권한이 비공개인 경우 버킷의 정보가 url을 통해 노출되지 않습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/906c66baf6be834d4f59994fd792b464_1677055654.jpg" alt=""><figcaption></figcaption></figure>

&#x20;



&#x20;

## 7. 파일의 공개 여부 설정하기

파일의 공개 여부로 모든 사용자에 대한 파일의 데이터 조회 권한을 설정할 수 있습니다.  기본 설정은 비공개입니다.\
&#x20;

### (1) 파일 공개 설정

① 공개 설정을 할 파일을 선택합니다.\
② "Make public" 버튼을 클릭합니다.\
③ Permissions 탭에서 모든 사용자에 대한 Read 권한이 부여된 것을 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/41acbb841c831abcb4ff791e497fa754_1677055949.jpg" alt=""><figcaption></figcaption></figure>

파일의 권한이 공개일 경우 url을 통해 파일의 데이터를 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/8492acefe368714a834966dc119dc5cb_1677055976.jpg" alt=""><figcaption></figcaption></figure>

&#x20;



### (2) 파일 비공개 설정

① 비공개 설정을 할 파일을 선택합니다.\
② "Make private" 버튼을 클릭합니다.\
③ Permissions 탭에서 모든 사용자에 대한 Read 권한이 제거된 것을 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/7fa2a56929e0d784385214abe28ee487_1677055997.jpg" alt=""><figcaption></figcaption></figure>

파일의 권한이 비공개인 경우 파일의 데이터가 url을 통해 노출되지 않습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/02/22/b3ca429ccb90cf53aa80449b1dcf7093_1677056018.jpg" alt=""><figcaption></figcaption></figure>

\
&#x20;
