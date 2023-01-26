---
description: 가상서버의 vsftp Passive mode 설정 방법은 다음과 같습니다.
---

# FTP 구성 방법

### 1. FTP란 무엇인가요?

FTP(File Transfer Protocol)는 인터넷으로 연결된 클라이언트와 서버 사이의 파일 전송을 위한 프로토콜입니다.

하지만 데이터를 평문으로 전송하여 보안이 취약하다는 단점이 있습니다.

따라서 카페24클라우드는 보안이 강화된 <mark style="color:blue;">**SFTP(Secure File Transfer Protocol)**</mark>의 사용을 권장합니다.

SFTP 사용 방법은 [SFTP 접속은 어떻게 할 수 있나요?](sftp.md) 를 참고해 주세요.

본 매뉴얼에서는 FTP Passive mode 설정하는 방법을 설명합니다.

&#x20;

FTP에는 FTP 서버와 FTP 클라이언트가 통신하는 방법에 따라 Passive 모드와 Active 모드가 있습니다.

FTP 서버와 클라이언트는 각각 두 개의 포트를 사용하여 통신 합니다.

1. Command port: 두 서버 간의 연결을 제어
2. Data port : 두 서버 간의 데이터 전송

&#x20;

(1) Passive mode

데이터 전송 요청을 클라이언트가 서버에게 하는 방식입니다.

클라이언트 방화벽의 영향을 받는 Active mode의 단점을 보완합니다.

Passive mode의 통신 방법은 다음과 같습니다.



1. 클라이언트에서 서버의 21번 포트로 접속
2. 서버에서 클라이언트의 요청에 응답(ack) 하며 클라이언트가 접속할 자신의 Data port 전달
3. 클라이언트에서 서버의 Data port로 접속 시도
4. 서버가 클라이언트의 요청에 응답(ack)

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>





