---
description: 가상서버 스냅샷을 사용하여 가상서버의 OS 영역을 복구하는 방법은 아래와 같습니다.
---

# 가상서버 스냅샷 사용 방법

## 1. 가상서버의 OS 영역 문제 상황

본 매뉴얼은 다음과 같은 문제 상황에서 가상서버의 OS 영역을 복구하는 방법을 설명합니다.

### (1) 커널 패닉으로 인한 SSH 접근 실패

커널은 Linux OS의 구성 요소로, 명령어를 해석하여 하드웨어를 조작하는 역할을 합니다.

커널 구성 파일이 손상되었거나 올바르게 설치되지 않은 경우, 커널 패닉이 발생합니다.

이 경우 커널이 정상적으로 로드되지 않아 가상서버가 부팅되지 않게 됩니다.

커널 패닉은 가상서버의 콘솔 화면에서 다음과 같이 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/14/57f44550c7d071d6700ef8a86650c539_1618381721.jpg" alt=""><figcaption></figcaption></figure>



### (2) 원격 접속 및 Console 접속 시 키보드 입력 불가

가상서버의 콘솔 화면에서 키보드 입력에 대한 반응이 없는 경우, 가상서버 조작이 불가능합니다.

System Hang이 걸렸다고 판단될 경우, 가상서버의 OS 영역 데이터를 본 매뉴얼을 통해 복구할 수 있습니다.



### (3) 그 외, 서버로의 접근/운영이 불가한 상황

가상서버의 내부적인 문제로 접속할 수 없는 경우, 운영 중인 서비스를 빠르게 정상화하기 위해 본 매뉴얼을 통해 기존 데이터를 복구할 수 있습니다.





## 2. 가상서버의 OS 영역 복구하기

### (1) 신규 가상서버 생성하기

서버 > 가상서버에서 \[새로운 가상서버 생성] 버튼을 클릭하여 복구할 OS 영역의 볼륨을 연결할 새로운 가상서버를 생성합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/14/7154147043d2407feca070ae30f095d0_1618388859.jpg" alt=""><figcaption></figcaption></figure>



### (2) 장애 가상서버의 스냅샷 생성하기

서버 > 가상서버 > 복구하려는 가상서버에서 \[재시작] 버튼을 클릭합니다.

가상서버를 재시작하면 RAM에서 작업하던 데이터를 Disk로 write하게 되어 데이터 유실을 방지할 수 있습니다.

따라서 기존 데이터의 보존을 위해 복구하려는 가상서버를 재시작한 후, 스냅샷을 생성합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/14/05e55e903f2158b98ef6e91e253c4c3d_1618388878.jpg" alt=""><figcaption></figcaption></figure>

서버 > 가상서버 스냅샷 > \[새로운 가상서버 스냅샷 생성] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/12/297a549be379f77b7f1f5198640cefdd_1618208882.jpg" alt=""><figcaption></figcaption></figure>

복구가 필요한 가상서버를 선택하여 스냅샷을 생성합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/12/7792b78b4f80979e0346b27c0ed4f5eb_1618209255.jpg" alt=""><figcaption></figcaption></figure>



### (3) 스냅샷으로 OS 볼륨 생성하기

서버 > 가상서버 스냅샷 > 생성한 스냅샷의 상세정보 탭에서 스냅샷으로 블록 스토리지 생성의 \[생성] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/12/57525a5e98f2b6bb2a6cb9ccd33ae5ec_1618209753.jpg" alt=""><figcaption></figcaption></figure>

생성할 블록 스토리지의 이름을 입력한 후, \[생성] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/12/63ea74923c6c0760a90da2617a3360f3_1618209970.jpg" alt=""><figcaption></figcaption></figure>



### (4) 신규 가상서버에 OS 볼륨 연결하기

스토리지 > 블록 스토리지 > 생성한 블록 스토리지의 \[연결] 버튼을 클릭합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/12/6f17128d01396bf9afd14b91da9ec08a_1618212565.jpg" alt=""><figcaption></figcaption></figure>

블록 스토리지 연결 창에서 신규 가상서버를 선택하여 연결합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/12/8ac638ea60efa192e160d11eeae46666_1618212677.jpg" alt=""><figcaption></figcaption></figure>

신규 가상서버에 연결된 OS 볼륨의 경로를 확인합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/12/1b48ea37c39f20f29e7b733a1f8d4ea2_1618215275.jpg" alt=""><figcaption></figcaption></figure>



### (5) 신규 가상서버에 접속하기

연결한 OS 볼륨을 마운트하여 데이터를 확인하기 위해 신규 가상서버에 접속합니다.

가상서버에 접속하는 방법은 [<mark style="color:blue;">**\[SSH 키페어 접속 방법\]**</mark>](../server/connect/keypair.md)을 참고해 주세요.



### (6) OS 볼륨의 정보 확인하기

다음 명령어로 가상서버에 연결된 OS 볼륨의 정보를 확인합니다.&#x20;

이 때 조회되는 경로는 (4)번에서 확인한 가상서버 연결 경로와 동일합니다.

```shell
$ sudo fdisk -l
```

본 예제에서 복구할 OS 볼륨은 /dev/vdb1으로 연결되어 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/12/3f980474af71a8a7d6e6fa972a80d978_1618214766.jpg" alt=""><figcaption></figcaption></figure>



### (7) OS 볼륨의 UUID 중복 확인하기

#### &#x20;① OS 볼륨의 UUID 확인하기

기본 볼륨과 UUID가 같을 경우, 정상적인 mount 및 부팅이 불가능합니다.

UUID가 동일한 경우 추가된 OS 볼륨의 UUID를 변경하는 작업이 필요하며, 다를 경우 생략 가능합니다.

```shell-session
$ sudo blkid
/dev/vda1: UUID="3ef2b806-efd7-4eef-aaa2-2584909365ff" TYPE="xfs"
/dev/vdb1: UUID="3ef2b806-efd7-4eef-aaa2-2584909365ff" TYPE="xfs"
```

#### ② OS 볼륨의 XFS 파일 시스템 복구하기

다음 명령어를 사용하여 연결한 OS 볼륨을 복구합니다.

이때 대상이 되는 파일시스템은 마운트 되지 않은 상태여야 합니다.

```shell-session
$ sudo xfs_repair -L /dev/vdb1
```

#### ③ OS 볼륨에 새로운 UUID 부여하기

다음 명령어를 사용하여 OS 볼륨에 새로운 UUID를 부여합니다.

```shell-session
$ sudo xfs_admin -U `uuidgen` /dev/vdb1
Clearing log and setting UUID
writing all SBs
new UUID = b0f349ec-d903-4c6d-8cfd-f2a26e3e5e6e
```



### (8) OS 볼륨 마운트하기

본 예제의 /mnt와 같이 mount할 경로를 지정하여 OS 볼륨을 연결합니다.

```shell-session
$ sudo mount /dev/vdb1 /mnt
```



### (9) 데이터 확인하기

mount를 수행한 경로로 이동하여 복구하고자 한 데이터를 확인합니다.&#x20;

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/04/14/3afc0c47894f7aee5edcb312ed6536e6_1618362946.png" alt=""><figcaption></figcaption></figure>
