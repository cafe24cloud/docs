---
description: S3cmd를 이용하여 카페24 클라우드의 오브젝트 스토리지를 사용하는 방법을 설명합니다.
---

# 오브젝트 스토리지 S3cmd 사용 방법

## 1. S3cmd란?

S3cmd는 CLI(Command-line interface) 환경에서 AWS의 S3를 제어할 수 있는 무료 명령어 도구입니다.&#x20;

카페24클라우드의 오브젝트 스토리지는 AWS S3와 동일한 API를 사용하기 때문에 S3cmd를 사용할 수 있습니다.&#x20;

오브젝트 스토리지 CLI tool로는 S3cmd 외에도 [aws s3](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-commands.html), [s3api](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-apicommands.html)가 존재합니다.



**s3cmd 사용법 공식 문서** : [S3cmd Usage](https://s3tools.org/usage)

**오브젝트 스토리지 사용 방법** : [오브젝트 스토리지는 어떻게 사용하나요?](https://console.cafe24.com/support/faq/view?idx=437)

**오브젝트 스토리지 API 사용 시 용량 정책**

* 베타 서비스 기간 동안 제공하는 오브젝트 스토리지의 최대 저장 공간은 30GB입니다.
* s3cmd, s3 browser 등으로 API를 통해 오브젝트 스토리지를 이용할 경우 1회 업로드 용량에 대한 세부 제한은 없으며, 최대 저장 공간 30GB에 대해서만 제한합니다.





## 2. S3cmd 연동하기

#### (1) s3cmd 설치

오브젝트 스토리지에 접근할 가상서버에 다음과 같이 s3cmd를 설치합니다.

{% tabs %}
{% tab title="CentOS/Rocky" %}
```shell-session
$ sudo yum install epel-release
$ sudo yum install s3cmd
```
{% endtab %}

{% tab title="Ubuntu" %}
```shell-session
$ sudo apt-get update
$ sudo apt-get install s3cmd
```
{% endtab %}
{% endtabs %}



#### (2) S3cmd 환경 설정

카페24클라우드의 오브젝트 스토리지에 접근하기 위해 API key를 이용한 인증 과정을 수행합니다.\
&#x20;Access Key와 Secret Key 조회 방법은 [오브젝트 스토리지는 어떻게 사용하나요?](use.md)을 확인해 주세요

```shell-session
$ s3cmd --configure
Access Key [none]: Access Key 입력
Secret Key [none]: Secret Key 입력
Default Region [none]: [Enter]
S3 Endpoint [none]: kr.cafe24obs.com
DNS-style bucket+hostname:port template for accessing a bucket [none]: kr.cafe24obs.com/%(bucket)/
Encryption password: [Enter]
Path to GPG program [/usr/bin/gpg]: [Enter]
Use HTTPS protocol [Yes]: [Enter]
HTTP Proxy server name: [Enter]
Test access with supplied credentials? [Y/n] [Enter]
Save settings? [y/N] yes
```



#### (3) 연동 확인 (선택)

s3cmd 동작 테스트를 위해 고유한 계정 아이디로 버킷(Bucket)을 생성하고, 버킷 리스트를 조회합니다.&#x20;

다음 동작이 오류 없이 성공하면 오브젝트 스토리지에 정상적으로 연동된 것입니다.

```shell-session
## 버킷 생성
$ s3cmd mb s3://testbucket
Bucket 's3://testbucket/' created

## 버킷 리스트 조회
$ s3cmd ls
2021-10-21 04:51  s3://testbucket
```





## 3.  s3cmd 활용 예시

다음과 같이 오브젝트 스토리지의 버킷에 접근 및 제어할 수 있습니다.

버킷의 특정 폴더에 대해 일괄 적용을 할 경우 -r (recursive) 옵션을 사용합니다.&#x20;

&#x20;

예제 명령어에서 "버킷" 또는 "오브젝트"로 명시된 부분에 실제로 사용할 버킷 이름, 오브젝트 이름을 넣어주세요.&#x20;

&#x20;

#### (1) 버킷 생성

새로운 버킷을 생성합니다.&#x20;

```shell-session
$ s3cmd mb s3://버킷/
```



#### (2) 버킷 리스트 조회

오브젝트 스토리지의 버킷들을 조회합니다.

```shell-session
$ s3cmd ls
```

특정 버킷에 저장된 오브젝트 리스트는 다음과 같이 조회합니다.

```shell-session
$ s3cmd ls s3://버킷
```

&#x20;

#### (3) 버킷 삭제

{% hint style="danger" %}
<mark style="color:red;">**주의 사항**</mark>

삭제한 버킷은 복구할 수 없습니다.&#x20;
{% endhint %}

버킷을 삭제합니다. 기본적으로 비어 있지 않은 버킷은 삭제할 수 없습니다.

```shell-session
$ s3cmd rb s3://버킷
```

오브젝트가 들어 있는 버킷을 삭제할 경우 "-r" 옵션을 사용합니다. 해당 버킷의 모든 오브젝트가 함께 삭제됩니다.

```shell-session
$ s3cmd rb -r s3://버킷
```



#### (4) 버킷, 오브젝트의 용량 확인

버킷의 총용량을 byte 단위로 보여주며, 해당 버킷에 속한 오브젝트의 개수를 조회할 수 있습니다.

버킷 하위의 특정 경로를 지정하여 확인도 가능합니다.

```shell-session
$ s3cmd du s3://버킷/[오브젝트]
```



#### (5) 로컬 디렉터리와 버킷 동기화

로컬 디렉터리와 버킷을 동기화합니다.

이 명령어는 일회성으로 동기화 하므로 명령어 수행 후의 변동 사항이 자동 반영되지는 않습니다.

동일한 이름의 파일이 이미 있으면 덮어씌워지기 때문에 주의가 필요합니다.&#x20;

```shell-session
## 로컬의 내용을 오브젝트 스토리지에 동기화
$ s3cmd sync 로컬경로 s3://버킷
 
## 오브젝트 스토리지의 내용을 로컬에 동기화
$ s3cmd sync s3://버킷 로컬경로
```

&#x20;

#### (6) 버킷, 오브젝트 정보 조회

특정 버킷 및 오브젝트의 상세 정보를 확인할 수 있습니다.

```shell-session
$ s3cmd info s3://버킷/[오브젝트]
```



#### (7) ACL 설정

버킷 및 오브젝트에 대한 접근 제어 정책을 설정합니다.

<주요 옵션>

* \--acl-public : 버킷 및 특정 오브젝트에 모든 사용자가 접근할 수 있도록 설정합니다.
* \--acl-private : 버킷 및 특정 오브젝트 접근 권한을 소유자로 한정합니다.

```shell-session
$ s3cmd setacl s3://버킷/[오브젝트] --acl-public
$ s3cmd setacl s3://버킷/[오브젝트] --acl-private
```



(8) 오브젝트 업로드&#x20;

대상 버킷에 로컬의 파일을 업로드합니다.



$ s3cmd put 파일 s3://버킷

&#x20;

(9) 오브젝트 다운로드&#x20;

오브젝트를 로컬의 지정한 경로에 다운로드합니다.&#x20;

$ s3cmd get s3://버킷/오브젝트 로컬경로

&#x20;

(10) 오브젝트 삭제

{% hint style="danger" %}
<mark style="color:red;">**주의 사항**</mark>

삭제한 오브젝트는 복구할 수 없습니다.
{% endhint %}

```shell-session
$ s3cmd rm s3://버킷/오브젝트
$ s3cmd del s3://버킷/오브젝트
```

&#x20;

#### (11) 오브젝트 복사

버킷1의 오브젝트를 버킷2로 복사합니다.

```shell-session
$ s3cmd cp s3://버킷1/오브젝트 s3://버킷2/[오브젝트]
```



#### (12) 오브젝트 이동

버킷1의 오브젝트를 버킷2로 이동시킵니다.

```shell-session
$ s3cmd mv s3://버킷1/오브젝트 s3://버킷2/[오브젝트]
```

&#x20;

#### (12) 오브젝트 메타데이터 수정

<주요 옵션>

* \--add-header=NAME:VALUE : 메타데이터를 추가합니다.&#x20;
* \--remove-header=NAME : 메타데이터를 제거합니다.&#x20;

```shell-session
$ s3cmd modify s3://버킷/오브젝트 --add-header='Key:Value, Key:Value'
```

예를 들어 Cache-Control 메타데이터를 추가할 경우 다음과 같이 할 수 있습니다.&#x20;

```shell-session
$ s3cmd modify s3://버킷/오브젝트 --add-header='Cache-Control: public, max-age=38000'
```

&#x20;

#### (13) 멀티파트(Multipart) 업로드

S3 api는 기본적으로 멀티파트 기능을 사용하여 대용량 파일을 클라이언트 상에서 여러 파트(part)로 나누어 업로드합니다. &#x20;

S3 브라우저 등에서 예상치 못한 오류로 업로드가 중단될 경우 이미 올라간 멀티파트는 오브젝트 스토리지 상에서 삭제되지 않습니다.

멀티파트 업로드 내역은 S3 브라우저 및 파일 매니저에서 확인 불가능 하기 때문에 S3cmd와 같은 CLI로 확인해야 합니다.

다음과 같이 s3cmd로 중지된 멀티파트를 이어서 업로드 하거나, 멀티파트 업로드 자체를 중단(삭제)할 수 있습니다.&#x20;

사용하지 않는 멀티파트는 불필요한 과금을 방지하기 위해 삭제하는 것을 권장합니다.



**남아있는 멀티파트 업로드 조회**&#x20;

해당 버킷에 남아있는 멀티파트를 조회합니다.

업로드하던 오브젝트의 경로, 멀티파트 업로드ID를 확인할 수 있습니다. &#x20;

```shell-session
$ s3cmd multipart s3://버킷
```



**멀티파트 업로드의 파트 리스트 조회**

멀티파트 업로드ID는 "s3cmd multipart" 명령어로 확인 가능합니다.&#x20;

나눠진 각 파트의 파트넘버, 파일 크기 등을 확인할 수 있습니다.&#x20;

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ s3cmd listmp s3://버킷/오브젝트 멀티파트업로드ID
</strong></code></pre>



**멀티파트 업로드 중지(삭제)**

사용하지 않는 멀티파트 업로드는 다음과 같이 중단합니다. 기존에 업로드된 멀티파트들도 삭제됩니다.&#x20;

```shell-session
$ s3cmd abortmp s3://버킷/오브젝트 멀티파트업로드ID
```



**멀티파트 업로드 재시작**

중지된 멀티파트 업로드를 이어서 업로드 할 경우 사용합니다.

\--upload-id 옵션으로 멀티파트 업로드ID를 지정합니다. &#x20;

이전에 업로드한 멀티파트는 제외하고, 나머지 부분을 이어서 업로드 하게 됩니다.&#x20;

```shell-session
$ s3cmd --upload-id  멀티파트업로드ID put 업로드할파일  s3://버킷/오브젝트
```



**(14) 라이프사이클(Lifecycle)**

오브젝트에 대한 수명주기를 설정하여 동일 주기로 반복적인 작업을 할 수 있습니다.&#x20;

버킷의 모든 오브젝트 혹은 특정 prefix를 가진 오브젝트를 대상으로 지정합니다.

**라이프사이클 정책 조회**

해당 버킷에 설정된 라이프사이클 정책을 확인합니다.&#x20;

```shell-session
$ s3cmd getlifecycle s3://버킷
```



**라이프사이클 정책 설정**

해당 버킷에 라이프사이클 정책을 설정합니다.&#x20;

```shell-session
$ s3cmd setlifecycle 라이프사이클파일 s3://버킷
```

멀티파트를 생성시간 기준으로 하루 후에 삭제하는 라이프사이클 설정 예시는 다음과 같습니다.

<pre class="language-shell-session"><code class="lang-shell-session"><strong>## 라이프사이클 정책파일 생성
</strong><strong>$ sudo cat > lifecycle_abort_multipart &#x3C;&#x3C; EOF
</strong>&#x3C;LifecycleConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  &#x3C;Rule>
    &#x3C;ID>Remove uncompleted uploads&#x3C;/ID>
    &#x3C;Status>Enabled&#x3C;/Status>
    &#x3C;Prefix/>
    &#x3C;AbortIncompleteMultipartUpload>
      &#x3C;DaysAfterInitiation>1&#x3C;/DaysAfterInitiation>
    &#x3C;/AbortIncompleteMultipartUpload>
  &#x3C;/Rule>
&#x3C;/LifecycleConfiguration>
EOF

## 버킷에 정책 적용
$ s3cmd setlifecycle lifecycle_abort_multipart s3://testbucket
</code></pre>



**라이프사이클 정책 삭제**

```shell-session
$ s3cmd dellifecycle s3://버킷
```

