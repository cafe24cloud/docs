---
description: >-
  SSH 키페어의 개인키를 분실 시 가상서버에 접근할 수 있는 긴급 방법입니다. 패스워든 방식은 보안상 취약하오니 작업완료 후 SSH 키페어를
  사용한 로그인 방식으로 변경하시기 바랍니다.
---

# 분실 시 해결 방법 - CentOS

## 1. 싱글 유저 모드 진입

<mark style="background-color:blue;">웹콘솔 > 서버 > 가상서버 > 콘솔접속</mark>

콘솔 우측 상단의 "Send CtrlAltDel" 클릭합니다.

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

부팅 프로세스가 시작되자마자 "**Esc 키**'"를 눌러 GRUB 부트 프롬프트를 표시 합니다.&#x20;

GRUB 부트 프롬프트에 도달하기 위해 시스템을 껐다가 다시 켜야 할 수도 있습니다.&#x20;



GRUB 부트 프롬프트가 표시되면 첫 번째 부팅 커널을 선택하여 "**E**" 를 누릅니다.&#x20;

(GRUB 프롬프트가 표시되지 않으면 가상서버 부팅 전에 키를 눌러야합니다.)&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/11/6929125a708850b0b4152c3a58606f04_1583916620.png" alt=""><figcaption></figcaption></figure>

&#x20;   &#x20;

아래쪽 화살표(↓)를 눌러서 커널 라인 (**linux16으로 시작**)을 찾아 **ro**를 "**rw init=/sysroot/bin/sh**"로 변경하고 "**console=ttyS0,115200n8**"와 "**console=ttyS0,115200**"값을 삭제합니다.&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/08/11/9e5314349b13951063fd0c7132f8b1fc_1660178417.png" alt=""><figcaption></figcaption></figure>

변경이 완료되면 **CTRL + X** 또는 **F10**을 눌러서 싱글 사용자 모드로 부팅합니다.&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/11/1906fb1604e08c756e8fee1606db4b67_1583915990.png" alt=""><figcaption></figcaption></figure>







## 2. 패스워드 재설정

부팅이 완료 되면 다음 과정을 순차적으로 실행합니다.  &#x20;

* **"chroot /sysroot"** 명령으로 시스템에 엑세스 합니다.
* "**passwd 계정명**" 명령어로 패스워드를 입력합니다. ex) passwd centos
* "**vi /etc/ssh/sshd\_cofig**" 명령어로 파일을 수정합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/11/28c44cc0e92a6c8c9b16e228b3100adc_1583916062.png" alt=""><figcaption></figcaption></figure>

"**/etc/ssh/sshd\_config**" 파일에서 **PasswordAuthentication의 값을 no 에서 yes로** 변경합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/11/33de6f78c81290adf765b5b4c674a853_1583916102.png" alt=""><figcaption></figcaption></figure>

ESC 키를 누른 후, "**wq!**" 로 변경 사항을 저장합니다.&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/11/42e833f3a0da7a9d609600192639f831_1583916126.png" alt=""><figcaption></figcaption></figure>

"**touch /.autorelabel**" 명령어를 실행하여 파일 시스템 레이블을 다시 지정합니다.

"**exit**" 명령어를 실행하여 시스템에서 나옵니다.

"**reboot**" 명령어로 서버를 재시작합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/11/fb9731842367abdf55bfee5bb0d24b12_1583916150.png" alt=""><figcaption></figcaption></figure>







## 3. 패스워드 접속 확인

계정을 이용한 접속이 잘되는지 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/03/11/d486f887a0b5c17db983fa26ab25096a_1583916181.png" alt=""><figcaption></figcaption></figure>









