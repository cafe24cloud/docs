---
description: >-
  SSH 키페어의 개인키를 분실 시 가상서버에 접근할 수 있는 긴급 방법입니다. 패스워드 방식은 보안상 취약하오니 작업완료 후 SSH 키페어를
  사용한 로그인 방식으로 변경하시기 바랍니다.
---

# 분실 시 해결 방법 - Ubuntu

## 1. 싱글 유저 모드 진입

<mark style="background-color:blue;">웹콘솔 > 서버 > 가상서버 > 콘솔접속</mark>

콘솔 우측 상단의 "Send CtrlAltDel" 클릭합니다.

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

부팅 프로세스가 시작하면 5초안에 "Esc 키'"를 눌러 GRUB 부팅 프롬프트를 표시 합니다.

GRUB 부트 프롬프트에 도달하기 위해 시스템을 껐다가 다시 켜야 할 수도 있습니다.

GRUB 부팅 프롬프트가 표시되면 화살표키를 사용하여 "Advanced options for Ubuntu" 선택하여 "엔터" 를 누릅니다.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

"recovery mode"로 표시 되어있는 항목을 선택후 엔터키를 누릅니다.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

우분투 복구 모드에서 키보드의 화살표키를 사용하여 "root Drop to root sheel prompt" 선택후 엔터키를 누릅니다.

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>







## 2. 패스워드 재설정

다음  과정을 순차적으로 실행합니다.  &#x20;

* "**mount -o remount, rw /**" 명령어로 루트 영역파일 시스템을 read, write 권한으로  마운트
* "**passwd ubuntu(계정명)**" 명령어로 ubuntu 계정의 패스워드를 설정
* "**vi /etc/ssh/sshd\_config**" 명령어로 파일을 수정

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

키페어 접속 방식에서 "/etc/ssh/sshd\_config" 파일에서 PasswordAuthentication no 를 yes로파라미터를 업데이트합니다.\
"**reboot now**" 명령어로 서버를 재부팅합니다.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>







## 3. 패스워드접속 확인

계정으로 접속이 잘되는지 확인합니다.

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
