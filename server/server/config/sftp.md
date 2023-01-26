---
description: SSH 키페어를 이용한 SFTP 접속 방법은 다음과 같습니다. 본 메뉴얼에서는 FTP 소프트웨어로 FileZilla를 사용합니다.
---

# SFTP 구성 방법

## 1. SFTP란 무엇인가요?

우선 FTP(File Transfer Protocol)은 인터넷으로 연결된 클라이언트와 서버 사이의 파일 전송을 위한 프로토콜입니다.

하지만 데이터를 평문으로 전송하여 보안이 취약하다는 단점이 있습니다.

FTP 사용을 원하실 경우 [FTP는 어떻게 사용하나요?](ftp.md)를 참고해 주세요.



&#x20;따라서 카페24클라우드는 보안이 강화된 **SFTP(Secure File Transfer Protocol)** 사용을 권장합니다.

SFTP는 SSH방식을 사용하여 서버 간에 암호화된 데이터를 주고 받습니다.

파일을 암호화 하여 전송하기 때문에, 해킹이나 보안을 방지 할 수 있습니다.



FTP Client 소프트웨어를 사용하여 로컬에서 FTP 서버에 접속할 수 있습니다.

본 메뉴얼에서는 FTP Client 프로그램으로 FileZilla를 사용합니다.





## 2. 방화벽 허용

SFTP는 SSH 방식으로 동작하기 때문에 기본적으로 22번 포트를 사용합니다.&#x20;

따라서 다음과 같이 방화벽에서 SSH 22번 허용이 필요합니다.

22번 외 다른 포트로 sftp 접속을 하실 경우 방화벽에서도 해당 포트를 설정해 주세요.



방화벽 설정에 대한 자세한 내용은 [가상서버에 방화벽 연결과 설정은 어떻게 하나요? ](../../../security/security/config.md)을 참고해 주세요.

<figure><img src="../../../.gitbook/assets/image (3) (2).png" alt=""><figcaption></figcaption></figure>



## 3. FileZilla Client를 사용한 ftp 서버 접속

FileZilla Client에 접근할 ftp 호스트를 등록하는 과정은 다음과 같습니다.

#### (1) 사이트 관리자 열기

FileZilla를 실행한 뒤, 왼쪽 상단의 사이트 관리자 아이콘을 클릭해 주세요.

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

#### (2) New Site 추가

사이트 관리자에서 New Site 버튼을 클릭하여 호스트를 추가해 주세요.

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>



#### (3) FTP 서버 정보 입력

FTP 서버 접속을 위한 정보를 입력합니다. 설정이 필요한 정보는 다음과 같습니다.&#x20;

1. 사이트 이름 입력 : 접속하실 호스트의 이름 또는 아이피를 입력해 주세요.
2. 프로토콜 : **SFTP - SSH File Transfer Protocol**을 선택해 주세요.&#x20;
3. 호스트 : 접속하실 서버의 공인 IP를 입력해 주세요.
4. 포트 : SFTP는 기본으로 22번을 사용합니다. (22번 외 다른 포트로 접속도 가능합니다.)
5. 로그온 유형 : 로그온할 유형은 키 파일을 선택해 주세요. 해당 서버에 대한 키페어로 접속하게 됩니다.
6.  사용자 : 로그인할 사용자 계정을 입력합니다. 카페24클라우드 가상서버에서 초기 세팅된 사용자 정보는 다음과 같습니다. &#x20;

    | OS        | 계정     | 접속 방법       |
    | --------- | ------ | ----------- |
    | **Rocky** | rocky  | rocky@공인IP  |
    | Centos    | centos | centos@공인IP |
    | Ubuntu    | ubuntu | ubuntu@공인IP |


7. 키 파일 : **"찾아보기"** 버튼을 클릭하여 해당 서버에 대한 키파일을 등록합니다.

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>



#### (4) FTP 서버 접속 확인

FTP 호스트에 대한 입력을 완료한 후 다음과 같이 접속할 수 있습니다.&#x20;

로컬사이트는 FileZilla Client를 실행시키는 로컬 PC이며, 리모트 사이트는 호스트로 등록한 서버가 됩니다.

<figure><img src="../../../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>



