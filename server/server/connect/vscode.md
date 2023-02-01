---
description: 본 매뉴얼은 VSCode에서 카페24클라우드의 가상서버에 SSH 키페어 및 패스워드 방식으로 접속하는 방법을 안내합니다.
---

# VSCode 접속 방법

## 1. 사전 준비 사항

### (1) Visual Studio Code 설치

PC의 OS에 맞는 Visual Studio Code(VSCode)를 설치합니다.

설치 링크 : [Visual Studio Code](https://code.visualstudio.com/)

&#x20;



### (2) 가상서버 생성

VSCode를 이용해서 접속할 가상서버를 생성합니다.

가상서버 생성 방법은 &#x20;

[\[가상서버 생성 방법](../create.md)을 참고해 주세요.

{% hint style="danger" %}
<mark style="color:red;">**주의 사항**</mark>

VSCode에서 SSH 접속을 하기 위해서는 해당 가상서버에서 Inbound 22번 포트가 허용되어야 합니다.  [\[방화벽 설정 방법\]](../../../security/security/config.md)&#x20;
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>







## 2. 원격 접속 확장 프로그램 설치

### (1) VSCode 확장 프로그램 설치

VSCode를 연 다음, 다음 과정을 통해 가상서버 SSH 접속에 필요한 확장 프로그램을 설치합니다.

① **Ctrl + Shift + x** 혹은 좌측에 있는 Extension 아이콘을 클릭합니다.

② 검색창에 **Remote Development**를 검색합니다.

③ Install 클릭하여 확장 프로그램을 설치합니다.

<figure><img src="../../../.gitbook/assets/image (3) (4).png" alt=""><figcaption></figcaption></figure>



### (2) Remote-SSH 설정 파일 열기

F1 키를 눌러 명령어 입력창을 연 다음, **Remote-SSH: Open SSH Configuration File을** 입력합니다.

<figure><img src="../../../.gitbook/assets/image (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

업데이트할 SSH 설정 파일로 User 디렉터리 하위의 config 파일을 선택합니다.

<figure><img src="../../../.gitbook/assets/image (13) (2).png" alt=""><figcaption></figcaption></figure>







## 3. 가상머신 접속 정보 입력

다음 두 가지 방법 중 하나를 택해 가상서버에 접속할 수 있습니다.

<mark style="color:red;">카페24클라우드는 보안상의 이유로 키페어 접속을 권장하며, 비밀번호 노출시 정보 유출 위험이 있어 패스워드 설정을 권장하지 않습니다.</mark>

* [SSH Keypair 방식으로 접속](vscode.md#1-ssh-keypair)
* [Password 방식으로 접속](vscode.md#2-password)&#x20;

### (1) SSH Keypair 방식으로 접속

가상서버를 생성할 때 다운로드된 .pem 확장자의 개인키 파일을 사용해 접속합니다.

① 접속할 가상머신의 정보를 입력하고, 파일을 저장합니다.

Host : 생성한 SSH 커넥션의 이름

* HostName : 가상머신의 공인 IP
* User : 초기 사용자 계정 혹은 별도로 생성한 계정  [\[ 초기 사용자 계정 정보 \]](vscode.md#undefined)
* IdentityFile : 가상서버의 .pem 파일의 절대 경로

② 좌측 메뉴 하단에 생성된 원격 SSH 접속 메뉴를 클릭합니다.

③ 상단의 드롭다운을 클릭하여 Remote Explorer를 **SSH Targets**로 선택합니다.

④ 생성된 SSH Target 의 원격 접속 버튼을 클릭합니다.&#x20;

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>





### (2) Password 방식으로 접속

<mark style="color:red;">카페24클라우드는 보안상의 이유로 키페어 접속을 권장하며, 비밀번호 노출시 정보 유출 위험이 있어 패스워드 설정을 권장하지 않습니다.</mark>

패스워드를 이용해 접속하고자 하는 경우 가상머신에서 이를 허용하는 설정이 필요합니다.

#### a.  가상서버에 SSH 접속하기

[\[SSH 키페어 접속 방법\]](keypair.md)을 참고하여 가상서버에 접속합니다.



#### b. SSH 설정 파일 수정하기

패스워드 로그인을 허용하도록 SSH 설정 파일을 수정합니다.

```shell-session
$ sudo sed -i $'s/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
```



#### c. 패스워드 설정하기

접속할 계정의 패스워드를 설정합니다.

```shell-session
$ sudo passwd 계정명
```



#### d. SSH 데몬 재시작

SSH 데몬을 재시작 합니다.

```shell-session
$ sudo systemctl restart sshd
```



#### e. VSCode의 ssh config 파일 설정

VSCode에서 열어둔 SSH config 파일에 접속 정보를 입력합니다.

① 접속할 가상머신의 정보를 입력하고, 파일을 저장합니다.

* Host : 생성한 SSH 커넥션의 이름
* HostName : 가상머신의 공인 IP
* User : 초기 사용자 계정 혹은 별도로 생성한 계정  [\[ 초기 사용자 계정 정보 \]](vscode.md#undefined)

② 좌측 메뉴 하단에 생성된 원격 SSH 접속 메뉴를 클릭합니다.

③ 상단의 드롭다운을 클릭하여 Remote Explorer를 **SSH Targets**로 선택합니다.

④ 생성된 SSH Target 의 원격 접속 버튼을 클릭합니다.&#x20;

<figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>







## 3. 가상서버 접속

### (1) 접속할 OS 선택

새로운 VSCode window가 열리면 접속할 가상서버의 OS로 **Linux**를 선택합니다.

<figure><img src="../../../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

처음 접속하는 경우 fingerprint를 등록합니다.

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>





### (2) 가상서버 접속 확인

**Ctrl + \`(backquote)**으로 터미널을 열어 가상서버의 프롬포트가 뜨는 것을 확인합니다.

패스워드 방식으로 접속한 경우, 해당 계정에 설정한 패스워드를 입력해야 합니다.

<figure><img src="../../../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>





### (3) 가상서버 폴더 열기

다음 방법으로 가상서버 내의 디렉터리를 VSCode에서 폴더로 열 수 있습니다.&#x20;

① 좌측의 Explorer에서 Open Folder 버튼을 클릭합니다.

② 명령창이 열리면 조회할 경로를 입력합니다.

<figure><img src="../../../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>







## 4. 개발 환경에 필요한 확장 프로그램 설치

### (1) 확장 프로그램 설치

개발 환경에 맞는 확장 프로그램을 설치합니다.

① **Ctrl + Shift + x** 혹은 좌측에 있는 Extension 아이콘을 클릭합니다.

② 필요한 확장 프로그램을 검색합니다.

③ **Install In SSH:HOST** 문구를 확인한 다음, 설치를 진행합니다.

<figure><img src="../../../.gitbook/assets/image (8) (2).png" alt=""><figcaption></figcaption></figure>





### (2) 테스트 코드 구동

테스트 파일을 생성한 뒤, 실행하면 터미널을 통해 실행 결과를 확인할 수 있습니다.

<figure><img src="../../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>



### **※ 초기 사용자 계정 정보**

카페24 클라우드에서 제공되는 OS들의 일반계정 정보는 다음과 같습니다.

| **OS**     | **계정** | **접속 방법**   |
| ---------- | ------ | ----------- |
| **Rocky**  | rocky  | rocky@공인IP  |
| **CentOS** | centos | centos@공인IP |
| **Ubuntu** | ubuntu | ubuntu@공인IP |

