---
description: 가상서버에 NFS를 구성하는 방법은 아래와 같습니다.
---

# NFS 구성 방법

## 1. NFS란?

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2023/01/04/66fe3949e43858162dd69fc6ba899a9e_1672789103.png" alt=""><figcaption></figcaption></figure>

NFS(Network File System)를 사용하면 원격 호스트에서 네트워크를 통해 파일 시스템을 마운트하고, 로컬로 마운트된 파일 시스템과 상호 작용할 수 있습니다.

따라서 시스템 관리자는 리소스를 네트워크의 중앙 집중식 서버에 통합할 수 있는 장점이 있지만, <mark style="color:red;">네트워크 통신을 하기 때문에 속도 저하의 이슈가 발생할 수 있어 NFS 사용을 권장드리지 않습니다.</mark>







## 2. NFS Server 구성하기

### (1) 패키지 설치하기

먼저 등록된 저장소 내 패키지 정보를 최신으로 업데이트한 후, nfs 패키지 설치를 진행합니다.

{% tabs %}
{% tab title="CentOS / Rocky / Almalinux" %}
```shell-session
$ sudo yum update
$ sudo yum install nfs-utils
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo apt-get update
$ sudo apt-get install nfs-kernel-server
```
{% endtab %}
{% endtabs %}

설치가 완료되었다면 nfs 서비스를 구동한 후, active 상태인지 확인합니다.

{% tabs %}
{% tab title="CentOS / Rocky / Almalinux" %}
```shell-session
$ sudo systemctl start nfs-server
$ sudo systemctl enable nfs-server
$ systemctl status nfs-server
```
{% endtab %}

{% tab title="Ubuntu" %}
<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ sudo systemctl start nfs-kernel-server
</strong>$ sudo systemctl enable nfs-kernel-server
$ systemctl status nfs-kernel-server
</code></pre>
{% endtab %}
{% endtabs %}





### (2) 블록 스토리지 마운트하기

가상서버에 연결된 블록 스토리지를 확인합니다.

블록 스토리지 연결 방법은 [\[블록 스토리지 연결 방법\]](../../../storage/block/connect.md)을 참고해 주세요.

```shell-session
$ sudo fdisk -l
Disk /dev/vda: 30 GiB, 32212254720 bytes, 62914560 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 1EBFE1B6-33F2-4878-A2BB-996868440D06
 
Device       Start      End  Sectors  Size Type
/dev/vda1     2048   206847   204800  100M EFI System
/dev/vda2   206848  2254847  2048000 1000M Linux filesystem
/dev/vda3  2254848  2256895     2048    1M BIOS boot
/dev/vda4  2256896 62914526 60657631 28.9G Linux filesystem
 

Disk /dev/vdb: 10 GiB, 10737418240 bytes, 20971520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

확인한 경로로 파티션을 생성합니다.

<mark style="color:blue;">고객님의 환경에 따라 디스크 경로가 다를 수 있습니다.</mark>

* &#x20; Command (m for help): n   **=> 새로운 파티션 추가**
* &#x20; Select (default p): p   **=> 파티션 타입을 primary로 설정**
* &#x20; Partition number (1-4, default 1): \[enter]   **=> 파티션 번호를 1번(default 값)으로 설정**
* &#x20; First sector (2048-20971519, default 2048): \[enter]   **=> 파티션 시작 영역을 2048(default 값)으로 설정**
* &#x20; Last sector, +sectors or +size{K,M,G,T,P} (2048-20971519, default 20971519): \[enter]   **=> 파티션 마지막 영역을 20971519(default 값)으로 설정**
* &#x20; Command (m for help): p   **=> 파티션 테이블 출력**
* &#x20; Command (m for help): w   **=> 파티션 설정 저장**

```shell-session
$ sudo fdisk /dev/vdb
 
Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
 
Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x42027c24.
 
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-20971519, default 2048):
Last sector, +sectors or +size{K,M,G,T,P} (2048-20971519, default 20971519):
 
Created a new partition 1 of type 'Linux' and of size 10 GiB.
 
Command (m for help): p
Disk /dev/vdb: 10 GiB, 10737418240 bytes, 20971520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x42027c24
 
Device     Boot Start      End  Sectors Size Id Type
/dev/vdb1        2048 20971519 20969472  10G 83 Linux
 
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

디스크를 포맷한 후, 마운트할 디렉터리를 생성합니다.

```shell-session
$ sudo mkfs.xfs /dev/vdb1
$ sudo mkdir /nfs
```

생성한 디렉터리에 디스크를 마운트한 후, 잘 마운트되었는지 확인합니다.

```shell-session
$ sudo mount /dev/vdb1 /nfs
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        366M     0  366M   0% /dev
tmpfs           403M     0  403M   0% /dev/shm
tmpfs           403M  5.6M  397M   2% /run
tmpfs           403M     0  403M   0% /sys/fs/cgroup
/dev/vda4        29G  1.4G   28G   5% /
/dev/vda2       994M  227M  768M  23% /boot
/dev/vda1       100M  5.8M   95M   6% /boot/efi
tmpfs            81M     0   81M   0% /run/user/1000
/dev/vdb1        10G  104M  9.9G   2% /nfs
```





### (3) NFS 설정하기

NFS Server는 /etc/exports 구성 파일을 참조하여 NFS Client에서 파일 시스템에 액세스할 수 있는지 여부를 확인합니다.

따라서 /etc/exports 파일에 "**\[마운트 포인트]    \[NFS Client 사설 IP]\(옵션)**" 형식으로 입력합니다.

```shell-session
$ sudo vi /etc/exports
/nfs    192.168.1.52(rw,no_root_squash,sync)
```

{% hint style="info" %}
<mark style="color:blue;">**참고사항**</mark>

옵션 정보는 다음과 같습니다.

* **ro**: 읽기 요청만 허용&#x20;
* **rw**: 읽기 및 쓰기 요청 허용&#x20;
* **wdelay**: 다른 쓰기 요청이 진행 중인 경우 디스크에 대한 쓰기 요청을 지연&#x20;
* **no\_wdelay wdelay**: 기능을 해제&#x20;
* **root\_squash**: 원격으로 연결된 root 사용자가 root 권한을 갖는 것을 방지&#x20;
* **no\_root\_squash**: root\_squash 기능을 해제&#x20;
* **all\_squash**: root를 포함한 모든 원격 사용자가 root 권한을 갖는 것을 방지&#x20;
* **sync**: 변경 사항이 커밋된 후에만 요청에 응답
{% endhint %}

입력한 설정을 적용시킨 후, 테스트 파일을 생성합니다.

```shell-session
$ sudo exportfs -r
$ sudo touch /nfs/test.txt
```

&#x20;



### (4) NFS 자동 마운트 설정하기

NFS Server 재부팅 시, 마운트가 해제되기 때문에 /etc/rc.local 파일에 마운트 명령어를 추가하여 자동으로 마운트될 수 있도록 합니다.

{% tabs %}
{% tab title="CentOS / Rocky / Almalinux" %}
```shell-session
$ sudo umount /dev/vdb1
$ sudo vi /etc/rc.local
mount /dev/vdb1 /nfs
$ sudo chmod u+x /etc/rc.local
$ sudo systemctl start rc-local
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo umount /dev/vdb1
$ sudo vi /etc/rc.local
#!/bin/bash
mount /dev/vdb1 /nfs
$ sudo chmod u+x /etc/rc.local
$ sudo systemctl start rc-local
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
<mark style="color:blue;">**참고사항**</mark>

/etc/fstab 파일에 자동 마운트 설정을 할 수도 있지만, 마운트가 되지 않을 경우에 싱글 모드로 진입할 수 있기 때문에 /etc/rc.local 파일에 설정하는 것을 권장드립니다.
{% endhint %}

설정을 한 후, 재부팅을 해보면 마운트가 잘 되어 있는 것을 확인할 수 있습니다.

```shell-session
$ sudo reboot
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        366M     0  366M   0% /dev
tmpfs           403M     0  403M   0% /dev/shm
tmpfs           403M  5.6M  397M   2% /run
tmpfs           403M     0  403M   0% /sys/fs/cgroup
/dev/vda4        29G  1.5G   28G   5% /
/dev/vda2       994M  227M  768M  23% /boot
/dev/vda1       100M  5.8M   95M   6% /boot/efi
/dev/vdb1        10G  104M  9.9G   2% /nfs
tmpfs            81M     0   81M   0% /run/user/1000
```

{% hint style="info" %}
<mark style="color:blue;">**참고사항**</mark>

자동 마운트 설정을 하였음에도 불구하고, 마운트가 되지 않을 경우에는 수동으로 마운트를 진행해주셔야 합니다.
{% endhint %}

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

NFS Server 경우, 마운트를 해제하지 않고 재부팅하면 장애가 발생할 수 있으므로 재부팅 전 마운트 해제를 권장드립니다.
{% endhint %}

&#x20;

&#x20;



## 3. NFS Client 구성하기

### (1) 패키지 설치하기

마찬가지로 등록된 저장소 내 패키지 정보를 최신으로 업데이트한 후, nfs 패키지 설치를 진행합니다.

{% tabs %}
{% tab title="CentOS / Rocky / Almalinux" %}
```shell-session
$ sudo yum update
$ sudo yum install nfs-utils
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo apt-get update
$ sudo apt-get install nfs-common
```
{% endtab %}
{% endtabs %}

&#x20;



### (2) NFS 마운트하기

마운트할 디렉터리를 생성한 후, NFS Server에 마운트합니다.

"**mount -t nfs \[NFS Server 사설 IP]:\[NFS Server 마운트 포인트] \[NFS Client 마운트 포인트]**" 형식으로 명령어를 실행합니다.

```shell-session
$ sudo mkdir /data
$ sudo mount -t nfs 192.168.1.17:/nfs /data
```

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

가상서버에 연결된 방화벽의 "내부 네트워크 허용"이 체크되어 있어야 합니다.
{% endhint %}

마운트가 되었으며, 마운트한 디렉터리를 확인해 보면 NFS Server에서 생성한 테스트 파일을 볼 수 있습니다.

```shell-session
$ df -h
Filesystem         Size  Used Avail Use% Mounted on
devtmpfs           366M     0  366M   0% /dev
tmpfs              403M     0  403M   0% /dev/shm
tmpfs              403M  5.5M  397M   2% /run
tmpfs              403M     0  403M   0% /sys/fs/cgroup
/dev/vda4           29G  1.4G   28G   5% /
/dev/vda2          994M  227M  768M  23% /boot
/dev/vda1          100M  5.8M   95M   6% /boot/efi
tmpfs               81M     0   81M   0% /run/user/1000
192.168.1.17:/nfs   10G  104M  9.9G   2% /data
[user@nfs-client ~]$ ls /data
test.txt
```

&#x20;



### (3) NFS 자동 마운트 설정하기

NFS Client 재부팅 시, 마운트가 해제되기 때문에 마찬가지로 /etc/rc.local 파일에 마운트 명령어를 추가하여 자동으로 마운트될 수 있도록 합니다.

{% tabs %}
{% tab title="CentOS / Rocky / Almalinux" %}
```shell-session
$ sudo vi /etc/rc.local
mount -t nfs 192.168.1.17:/nfs /data
$ sudo chmod u+x /etc/rc.local
$ sudo systemctl start rc-local
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo vi /etc/rc.local
#!/bin/bash
mount -t nfs 192.168.1.17:/nfs /data
$ sudo chmod u+x /etc/rc.local
$ sudo systemctl start rc-local
```
{% endtab %}
{% endtabs %}

설정을 한 후, 재부팅을 해보면 마운트가 잘 되어 있는 것을 확인할 수 있습니다.

```shell-session
$ sudo reboot
$ df -h
Filesystem         Size  Used Avail Use% Mounted on
devtmpfs           366M     0  366M   0% /dev
tmpfs              403M     0  403M   0% /dev/shm
tmpfs              403M  5.5M  397M   2% /run
tmpfs              403M     0  403M   0% /sys/fs/cgroup
/dev/vda4           29G  1.5G   28G   5% /
/dev/vda2          994M  227M  768M  23% /boot
/dev/vda1          100M  5.8M   95M   6% /boot/efi
192.168.1.17:/nfs   10G  104M  9.9G   2% /data
tmpfs               81M     0   81M   0% /run/user/1000
```

{% hint style="info" %}
<mark style="color:blue;">**참고사항**</mark>

자동 마운트 설정을 하였음에도 불구하고, 마운트가 되지 않을 경우에는 수동으로 마운트를 진행해주셔야 합니다.
{% endhint %}
