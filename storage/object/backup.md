---
description: >-
  S3cmd를 이용하여 카페24 클라우드 오브젝트 스토리지로 데이터(db 포함) 백업을 설정합니다. 해당 스크립트는
  OS(Centos/ubuntu/Rocky/Almalinux) 구분없이 적용 가능합니다.
---

# 오브젝트 스토리지를 이용한 데이터 백업 방법

{% hint style="danger" %}
본 매뉴얼에 안내되는 백업 설정은 서비스 구성 환경에 따라 정상적으로 동작하지 않을 수 있습니다. 이에 따른 백업 설정 수정은 사용자가 진행해 주셔야 합니다.\
백업 프로세스에서 발생하는 문제와 데이터 유실에 대한 책임은 사용자에게 있습니다. 또한, 사용자는 반드시 중요한 데이터를 별도로 보호하고, 백업 완료 후에는 데이터가 올바르게 복원되었는지 확인해야 합니다.
{% endhint %}

## 1. 사전 준비

### (1) 오브젝트 스토리지 신청 및 S3cmd 설치

아래 매뉴얼을 참고하여 오브젝트 스토리지 신청 및 S3cmd 설치가 선행되어야 합니다.

[\[오브젝트 스토리지 신청\]](https://docs.cafe24cloud.com/home/storage/object/use#2.)\
[\[S3cmd 설치 및 연동\]](https://docs.cafe24cloud.com/home/storage/object/s3cmd)





### (2) 시간 동기화

백업 스크립트를 cron 서비스에 등록하여 일정 주기로 실행할 수 있습니다.\
때문에 서버 OS내 시간과 실제 시간간 동기화가 이루어져야 설정한 시간에 백업이 진행됩니다.\
시간 동기화는 아래 매뉴얼을 참고하시기 바랍니다.

[\[OS 시간 동기화\]](https://docs.cafe24cloud.com/home/server/server/config/ntp)





### (3) 디스크 여유 공간 체크

백업이 진행되는 동안 OS내 생성된 데이터를 오브젝트 스토리지로 업로드후 삭제됩니다.

원활한 백업 및 서비스 운영을 위해 1일치 백업 데이터의 여유 공간을 확보하시기 바랍니다.

아래 명령어(df -Th)를 통해 디스크 여유 공간 확인이 가능합니다.

```shell-session
#### 매뉴얼에선 /dev/vda1 파티션의 /home/backup 디렉토리로 백업을 진행.
#### /dev/vda1 파티션이 마운트된 / 디렉토리 사용량이 12%(약3.4G)로 여유 공간 확인
$ sudo df -Th
Filesystem     Type      Size  Used Avail Use% Mounted on
udev           devtmpfs  2.0G     0  2.0G   0% /dev
tmpfs          tmpfs     394M  1.1M  393M   1% /run
/dev/vda1      ext4       29G  3.4G   26G  12% /
tmpfs          tmpfs     2.0G     0  2.0G   0% /dev/shm
tmpfs          tmpfs     5.0M     0  5.0M   0% /run/lock
tmpfs          tmpfs     2.0G     0  2.0G   0% /sys/fs/cgroup
/dev/loop0     squashfs   92M   92M     0 100% /snap/lxd/24061
/dev/vda15     vfat      105M  6.1M   99M   6% /boot/efi
/dev/loop3     squashfs   50M   50M     0 100% /snap/snapd/18357
/dev/loop4     squashfs   64M   64M     0 100% /snap/core20/1828
/dev/loop5     squashfs   50M   50M     0 100% /snap/snapd/18596
/dev/loop2     squashfs   64M   64M     0 100% /snap/core20/1852
tmpfs          tmpfs     394M     0  394M   0% /run/user/1000l
```







## 2. 백업 스크립트 동작 방식

백업 스크립트의 동작 방식은 rsync 및 mysqldump 명령어를 통해 데이터 및 DB를 로컬 호스트에 백업후\
백업된 데이터를 s3cmd 명령어를 통해 오브젝트 스토리지로 데이터 업로드됩니다.

DB 백업의 경우 database 단위 및 table 단위로 각각 백업 진행됩니다.

<figure><img src="../../.gitbook/assets/cloud_backup_flow.JPG" alt=""><figcaption><p>백업 스크립트 동작 순서</p></figcaption></figure>

data 백업만 원하실 경우, 스크립트내 "\_MysqlDump" 를 주석 처리\
DB 백업만 원하실 경우, 스크립트내 "\_Data\_Backup"를 주석 처리 하시기 바랍니다.

```sh
function _Main() {

   mkdir -p ${BACKUP_DIR}
   mkdir -p ${LOG_DIR}

   BACKUP_S_TIME=$(date)
   echo ${BACKUP_S_TIME} >> ${START_LOG}

   _Data_Backup           #### DB 백업만 원할 경우 해당 라인 주석

   _MySQL_Dump            #### data 백업만 원할 경우 해당 라인 주석

   _Upload_OBS  

   rm -rf ${BACKUP_DIR}/${BACKUP_D_TIME}_${HOST}

   s3cmd rm ${OBS_BUCKET}/${KEEP_DATE}_${HOST}/ --recursive

   BACKUP_E_TIME=$(date)
   echo ${BACKUP_E_TIME} >> ${END_LOG}

}
```







## 3. 백업 스크립트 다운로드

{% hint style="info" %}
2 종류의 백업 스크립트를 제공하며 각 스크립트는 고객님 서비스 환경에 맞게 커스텀하여 이용해 주시기 바랍니다.
{% endhint %}

### (1) 백업 스크립트 다운로드

```shell-session
#### 원하시는 두 스크립트중 선택하여 아래 명령어를 통해 다운로드 받습니다.
#### 스크립트 소유자를 root 계정으로 다운로드합니다. 

#### mysql root 패스워드가 스크립트에 포함된 백업 스크립트
$ curl -s https://kr.cafe24obs.com/cafe24-cloud-backup/include_mysql_pw_backup.sh |sudo tee /home/include_mysql_pw_backup.sh > /dev/null

#### mysql root 패스워드가 스크립트에 포함되지 않는 백업 스크립트
$ curl -s https://kr.cafe24obs.com/cafe24-cloud-backup/exclude_mysql_pw_backup.sh |sudo tee /home/exclude_mysql_pw_backup.sh > /dev/null
```





### (2) mysql root 패스워드가 포함된 백업 스크립트

스크립트내 mysql root 패스워드를 지정해야 합니다. 보안상 권장되지 않는 방법이며 스크립트를 실행하면 아래와 같은 warning 메세지가 출력됩니다. 보안상 권장되지 않는 방법이나 서비스 환경으로 인해 "mysql\_config\_editor" 유틸리티 설정이 불가할 경우 이용 가능합니다.\
(<mark style="color:red;">**mysql 5.6.6 미만, mariadb 10.2.4 미만 버전에선 mysql\_config\_editor 유틸리티 제공 안함**</mark>)

```shell-session
mysqldump: [Warning] Using a password on the command line interface can be insecure.
```





### (3) mysql root 패스워드가 포함되지 않는 백업 스크립트

스크립트에 mysqldump 명령어 실행시 -p(-- password) 옵션을 사용하여 비밀번호를 직접 입력하여\
DB 데이터를 추출하는 방식은 보안상 취약하여 권장하는 방식이 아닙니다. (스크립트가 실행되면서 다른 사용자가 명령어 히스토리나 프로세스 목록을 모니터링하고 있을 경우 패스워드가 노출될 우려가 있습니다.)

하여 "mysql\_config\_editor" 유틸리티를 이용하여 난독화된 인증 자격 증명을 저장하고 "-p" 옵션이 아닌\
"--login-path=root" 옵션으로 변경하여 접속하는 방식을 권장합니다.

아래 명령어는 "mysql\_config\_editor" 유틸리티를 이용한 mysql 접속 방법입니다.

```shell-session
$ sudo mysql_config_editor set --login-path=root --host=localhost --user=root --password --port=3306
Enter password: [root 패스워드 입력]
 
$ sudo ls -alt /root/.mylogin.cnf          ####해당 파일이 생성되었는지 확인 
-rw------- 1 root root 156 Mar 22 10:38 /root/.mylogin.cnf
 
$ sudo mysql --login-path=root             ####"--login-path=root" 옵션으로 패스워드 없이 mysql 접근 가능한지 확인
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3958
Server version: 8.0.32-0ubuntu0.20.04.2 (Ubuntu)
 
Copyright (c) 2000, 2023, Oracle and/or its affiliates.
 
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
 
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
 
mysql>
```

위와 같이 "mysql\_config\_editor" 유틸리티 설정이 완료하셨을 경우, "--login-path=root" 옵션을 이용하여 mysql 접속이 가능하며 "**mysql root 패스워드가 포함되지 않는 백업 스크립트**" 사용이 가능합니다.







## 4. 백업 스크립트 설정

다운로드한 백업 스크립트를 파일 편집기(vi 또는 nano)를 이용해 변수 설정 및 로그 경로 확인(수정)이 가능합니다.

```shell-session
#### 백업 스크립트 다운로드한 경로에서 vi 명령어를 실행하는게 아닐 경우 절대 경로로 입력 필요 
$ sudo vi include_mysql_pw_backup.sh
$ sudo vi exclude_mysql_pw_backup.sh
```

### (1) 변수 설정

위와 같이 파일 편집기로 스크립트를 확인해 보면 아래와 같이 변수를 설정할 수 있습니다.

```bash
##### Custom Variable Zone ###################################################################################
BACKUP_D_TIME=$(date -I)
HOST=$(hostname)
MYSQL=$(which mysql)
MYSQLDUMP=$(which mysqldump)
MYSQL_PW="####"
DUMP_OPTION="--max-allowed-packet=1024M --add-drop-table --add-drop-database --single-transaction"

SRC_DIR="/home"
BACKUP_DIR="/home/backup"

RSYNC=$(which rsync)
S3CMD=$(which s3cmd)
OBS_BUCKET="###"
KEEP_DATE=$(date +%Y-%m-%d --date '7 days ago')
```

| variable          | Description                                                                                                                           |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| MYSQL\_PW="####"  | <p>mysql root 패스워드가 포함된 스크립트를 사용할때 설정<br>####안에 mysql root 패스워드 입력</p>                                                                |
| DUMP\_OPTION=""   | <p>mysqldump시 적용되는 옵션으로 기본값은 아래와 같음<br>--max-allowed-packet=1024M --add-drop-table --add-drop-database --single-transaction</p>       |
| SRC\_DIR=""       | data backup 진행시 백업되는 경로. 기본값 : /home                                                                                                  |
| BACKUP\_DIR=""    | <p>백업 데이터가 저장되는 localhost 디렉토리 경로<br>설정한 디렉토리에 "[백업날짜]_[hostname]" 형식으로 디렉토리 생성<br>해당 디렉토리를 오브젝트 스토리지에 업로드<br>기본값 : /home/backup</p>  |
| OBS\_BUCKET="###" | <p>localhost에 백업된 데이터를 업로드할 오브젝트 스토리지 버킷명<br>###안에 버킷명 입력 (eg. s3://backup)</p>                                                       |
| KEEP\_DATE=       | <p>오브젝트 스토리지에 백업된 데이터 보관 주기 설정<br>기본값 : 7일 (date +%Y-%m-%d --date '<mark style="color:blue;"><strong>7</strong></mark> days ago')</p> |





### (2) 로그

위와 같이 파일 편집기로 스크립트를 확인해 보면 백업시 생성되는 로그 확인이 가능합니다.

```bash
##### Basic Constant Zone ####################################################################################
LOG_DIR="${BACKUP_DIR}/log"
LOG_FILE="${LOG_DIR}/rsync_backup.log"
OBS_LOG_FILE="${LOG_DIR}/obs_upload.log"
START_LOG="${LOG_DIR}/backup_start.log"
END_LOG="${LOG_DIR}/backup_end.log"
```

| variable       | Description                                                                             |
| -------------- | --------------------------------------------------------------------------------------- |
| LOG\_DIR       | <p>로그 파일이 저장되는 디렉토리<br>경로 : <strong>${BACKUP_DIR}/log</strong></p>                      |
| LOG\_FILE      | <p>data 백업(rsync)시 발생되는 로그<br>경로 : <strong>${LOG_DIR}/rsync_backup.log</strong></p>     |
| OBS\_LOG\_FILE | <p>오브젝트 스토리지에 데이터 업로드할 때 발생되는 로그<br>경로 : <strong>${LOG_DIR}/obs_upload.log</strong></p> |
| START\_LOG     | <p>백업 시작 시간 로그<br>경로 : <strong>${LOG_DIR}/backup_start.log</strong></p>                 |
| END\_LOG       | <p>백업 완료 시간 로그<br>경로 : <strong>${LOG_DIR}/backup_end.log</strong></p>                   |







## 5. cron 스케줄 등록

cron 서비스에 스케줄 등록 방법은 아래와 같습니다.

```shell-session
#### 아래 명령어를 통해 crontab 파일을 엽니다.
$ sudo crontab -e 

#### 스케줄 등록 방법 
*     *     *     *     *  command to be executed
-     -     -     -     -
|     |     |     |     |
|     |     |     |     +----- day of the week (0 - 6) (Sunday = 0)
|     |     |     +------- month (1 - 12)
|     |     +--------- day of the month (1 - 31)
|     +----------- hour (0 - 23)
+------------- minute (0 - 59)

#### 백업 스크립트 등록 예시
#### 매일 08시 45분에 /bin/bash /home/include_mysql_pw_backup.sh 스크립트를 실행함
45 8 * * * /bin/bash /home/include_mysql_pw_backup.sh
```

스케줄 등록후 cron 서비스 재시작하여 적용해 줍니다.

{% tabs %}
{% tab title="CentOS / Rocky / Almalinux" %}
```
$ sudo systemctl reload crond
```
{% endtab %}

{% tab title="Ubuntu" %}
```
$ sudo systemctl restart cron
```
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
주의 사항

cron 서비스는 백그라운드로 실행되며 스크립트 파일에 실행 권한이 없을 경우, cron 서비스에 스케줄을 등록하여도 스크립트가 실행되지 않습니다. 백업 스크립트 파일에 실행 권한이 있는지 확인하시기 바랍니다.
{% endhint %}

스크립트 소유자를 root 계정으로 다운로드하였기 때문에 root 계정에 대하여 실행 권한을 부여합니다.

```shell-session
#### 아래와 같이 실행 권한이 없음
$ sudo ls -alt exclude_mysql_pw_backup.sh
-rw-r--r--. 1 root root 2536 Apr  6 09:41 exclude_mysql_pw_backup.sh

#### user에 대하여 실행(x) 권한 부여
$ sudo chmod u+x exclude_mysql_pw_backup.sh

#### root 계정에 대하여 실행 권한 부여됨
$ sudo ls -alt exclude_mysql_pw_backup.sh
-rwxr--r--. 1 root root 2536 Apr  6 09:41 exclude_mysql_pw_backup.sh
```
