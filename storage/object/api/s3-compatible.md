---
description: S3 Compatible API를 사용하여 오브젝트 스토리지를 사용하는 방법은 다음과 같습니다.
---

# S3 Compatible API 사용 방법

## 1. S3 Compatible API 사용하기

#### (1) 지원되는 기능

카페24 클라우드에서 제공하는 오브젝트 스토리지는 [<mark style="color:blue;">Amazon S3 API</mark>](https://docs.aws.amazon.com/ko\_kr/AmazonS3/latest/API/Welcome.html)의 기본 데이터 액세스 모델과 호환되는 RESTful API를 지원합니다.

따라서 지원되는 Amazon S3 기능은 다음과 같습니다.

|          Feature          |   Status  |  Description |
| :-----------------------: | :-------: | :----------: |
|        List Buckets       | Supported |              |
|       Delete Bucket       | Supported |              |
|       Create Bucket       | Supported |              |
|      Bucket Lifecycle     | Supported |              |
|     Bucket Replication    |  Partial  | Zone 간에만 허용됨 |
| Policy (Buckets, Objects) | Supported |              |
|       Bucket Website      | Supported |              |
|   Bucket ACLs (Get, Put)  | Supported |              |
|      Bucket Location      | Supported |              |
|    Bucket Notification    | Supported |              |
|   Bucket Object Versions  | Supported |              |
|   Get Bucket Info (HEAD)  | Supported |              |
|   Bucket Request Payment  | Supported |              |
|         Put Object        | Supported |              |
|       Delete Object       | Supported |              |
|         Get Object        | Supported |              |
|   Object ACLs (Get, Put)  | Supported |              |
|   Get Object Info (HEAD)  | Supported |              |
|        POST Object        | Supported |              |
|        Copy Object        | Supported |              |
|     Multipart Uploads     | Supported |              |
|       Object Tagging      | Supported |              |
|       Bucket Tagging      | Supported |              |
|       Storage Class       | Supported |              |



#### (2) 지원되지 않는 헤더

지원되지 않는 헤더 필드는 다음과 같습니다.

|         Name        |   Type   |
| :-----------------: | :------: |
|        Server       | Response |
| x-amz-delete-marker | Response |
|      x-amz-id-2     | Response |
|   x-amz-version-id  | Response |



#### (3) 접근 방법

버킷에 접근하는 방법은 두 가지가 있습니다.

* URI에서 버킷을 최상위 디렉터리로 식별

```shell
GET /bucketname HTTP/1.1 
Host: kr.cafe24obs.com
```

* 가상 버킷 호스트 이름을 통해 버킷을 식별

```shell
 GET / HTTP/1.1
 Host: bucketname.kr.cafe24obs.com 
```

{% hint style="info" %}
<mark style="color:blue;">**참고사항**</mark>

가상 버킷 호스트 이름을 통해 버킷을 식별하는 경우, wild card DNS가 필요합니다.&#x20;
{% endhint %}



#### (4) Common Request Headers

|  Request Header  |     Description     |
| :--------------: | :-----------------: |
| `CONTENT_LENGTH` |    요청 본문의 길이입니다.    |
|      `DATE`      | 요청 시간 및 날짜(UTC)입니다. |
|      `HOST`      |    호스트 서버의 이름입니다.   |
|  `AUTHORIZATION` |      인증 토큰입니다.      |



#### (5) Common Response Status

| HTTP Status |          Response Code          |
| :---------: | :-----------------------------: |
|    `100`    |             Continue            |
|    `200`    |             Success             |
|    `201`    |             Created             |
|    `202`    |             Accepted            |
|    `204`    |            NoContent            |
|    `206`    |         Partial content         |
|    `304`    |           NotModified           |
|    `400`    |         InvalidArgument         |
|    `400`    |          InvalidDigest          |
|    `400`    |            BadDigest            |
|    `400`    |        InvalidBucketName        |
|    `400`    |        InvalidObjectName        |
|    `400`    | UnresolvableGrantByEmailAddress |
|    `400`    |           InvalidPart           |
|    `400`    |         InvalidPartOrder        |
|    `400`    |          RequestTimeout         |
|    `400`    |          EntityTooLarge         |
|    `403`    |           AccessDenied          |
|    `403`    |          UserSuspended          |
|    `403`    |       RequestTimeTooSkewed      |
|    `404`    |            NoSuchKey            |
|    `404`    |           NoSuchBucket          |
|    `404`    |           NoSuchUpload          |
|    `405`    |         MethodNotAllowed        |
|    `408`    |          RequestTimeout         |
|    `409`    |       BucketAlreadyExists       |
|    `409`    |          BucketNotEmpty         |
|    `411`    |       MissingContentLength      |
|    `412`    |        PreconditionFailed       |
|    `416`    |           InvalidRange          |
|    `422`    |       UnprocessableEntity       |
|    `500`    |          InternalError          |



#### (6) 인증 방법

오브젝트 스토리지에 대한 요청은 인증되거나 인증되지 않을 수 있으며, 인증되지 않은 요청은 익명의 사용자에 의해 전송된다고 가정합니다.&#x20;

요청을 인증하려면 요청에 ACCESS KEY와 HMAC(해시 기반 메시지 인증 코드)를 포함해야 합니다.

```shell
HTTP/1.1
PUT /buckets/bucket/object.mpeg
Host: kr.cafe24obs.com
Date: Tue, 6 Dec 2022 15:39:01 +0000
Content-Encoding: mpeg
Content-Length: 9999999

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

**HMAC을 생성하는 방법**은 다음과 같습니다.

* 헤더 문자열의 값을 가져옵니다.
  * 요청 헤더 문자열을 표준 형식으로 정규화합니다.
  * SHA-1 해싱 알고리즘을 사용하여 HMAC를 생성합니다. 자세한 내용은 RFC 2104 및 HMAC를 참조하십시오.
  * HMAC 결과를 base-64로 인코딩합니다.
* 헤더를 canonical form으로 정규화합니다.
  * x-amz-로 시작하는 모든 필드를 가져옵니다.
  * 필드가 모두 소문자인지 확인합니다.&#x20;
  * 필드를 사전 순으로 정렬합니다.
  * 동일한 필드 이름의 여러 인스턴스를 단일 필드로 결합하고 필드 값을 쉼표로 구분합니다.
  * 필드 값의 공백과 줄 바꿈을 단일 공백으로 바꿉니다.
  * 콜론 전후의 공백을 제거합니다.
  * 각 필드 뒤에 새 줄을 추가합니다.
  * 필드를 헤더에 다시 병합합니다.
* base-64로 인코딩된 HMAC 문자열로 바꿉니다.





## 2. Access Control List

오브젝트 스토리지는 ACL(Access Control List) 기능을 지원합니다.&#x20;

**ACL**은 사용자가 버킷 또는 객체에서 수행할 수 있는 작업을 지정하는 액세스 권한 부여 목록입니다.

각 권한은 아래와 같이 버킷에 적용될 때와 객체에 적용될 때 다른 의미를 갖습니다.

|   Permission   |          Bucket          |         Object        |
| :------------: | :----------------------: | :-------------------: |
|     `READ`     |    버킷의 객체를 나열할 수 있습니다    |     객체를 읽을 수 있습니다.    |
|     `WRITE`    |  버킷의 객체를 쓰거나 삭제할 수 있습니다. |          N/A          |
|   `READ_ACP`   |    버킷 ACL을 읽을 수 있습니다.    |   객체 ACL을 읽을 수 있습니다.  |
|   `WRITE_ACP`  |    버킷 ACL을 작성할 수 있습니다.   |   객체 ACL에 쓸 수 있습니다.   |
| `FULL_CONTROL` | 버킷의 객체에 대한 전체 권한을 갖습니다.  | 객체 ACL을 읽거나 쓸 수 있습니다. |





## 3. Bucket Operation

#### (1) List Buckets

생성한 버킷 목록을 반환합니다.

인증된 사용자가 생성한 버킷만 반환하므로 익명 요청을 할 수 없습니다.

* Syntax

```shell
GET / HTTP/1.1
Host: kr.cafe24obs.com

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Response Entities

|           Name           |    Type   |              Description              |
| :----------------------: | :-------: | :-----------------------------------: |
|         `Buckets`        | Container |            버킷 목록의 컨테이너입니다.            |
|         `Bucket`         | Container |            버킷 정보의 컨테이너입니다.            |
|          `Name`          |   String  |               버킷 이름입니다.               |
|      `CreationDate`      |    Date   |           버킷이 생성된 UTC 시간입니다.          |
| `ListAllMyBucketsResult` | Container |              결과 컨테이너입니다.              |
|          `Owner`         | Container | 버킷 소유자의 ID 및 DisplayName에 대한 컨테이너입니다. |
|           `ID`           |   String  |             버킷 소유자의 ID입니다.            |
|       `DisplayName`      |   String  |           버킷 소유자의 표시 이름입니다.           |



#### (2) PUT Bucket

새 버킷을 생성합니다.

버킷을 생성하려면 요청을 인증하기 위한 액세스 키가 있어야 하므로, 익명 사용자로는 버킷을 생성할 수 없습니다.

* Constraints

```
일반적으로 버킷 이름은 도메인 이름 제약 조건을 따라야 합니다.
- 버킷 이름은 고유해야 합니다.
- 버킷 이름은 소문자로 시작하고 끝나야 합니다.
- 버킷 이름에는 대시(-)가 포함될 수 있습니다.
```

* Syntax

```shell
PUT /{bucket} HTTP/1.1
Host: kr.cafe24obs.com
x-amz-acl: public-read-write

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Parameters

|     Name    |  Description |                             Valid Values                            | Required |
| :---------: | :----------: | :-----------------------------------------------------------------: | :------: |
| `x-amz-acl` | Canned ACLs. | `private`, `public-read`, `public-read-write`, `authenticated-read` |    No    |

* HTTP Response

<pre><code>- 버킷 이름이 고유하고 제약 조건 내에서 사용되지 않는 경우 작업이 성공합니다.
- 동일한 이름의 버킷이 이미 존재하고 사용자가 버킷 소유자인 경우 작업이 성공합니다.
<strong>- 버킷 이름이 이미 사용 중인 경우 작업이 실패합니다.
</strong></code></pre>

| HTTP Status |     Status Code     |        Description        |
| :---------: | :-----------------: | :-----------------------: |
|    `409`    | BucketAlreadyExists | 버킷이 이미 다른 사용자의 소유권에 있습니다. |



#### (3) DELETE Bucket

버킷을 삭제합니다.

성공적인 버킷 제거 후, 버킷 이름을 재사용할 수 있습니다.

* Syntax

```shell
DELETE /{bucket} HTTP/1.1
Host: kr.cafe24obs.com

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* HTTP Response

| HTTP Status | Status Code | Description |
| :---------: | :---------: | :---------: |
|    `204`    |  No Content |  버킷을 삭제합니다. |



#### (4) GET Bucket

#### (5) Get Bucket ACL

#### (6) PUT Bucket ACL

#### (7) List Bucket Multipart Uploads

## 4. Object Operation

#### (1) Put Object

#### (2) Copy Object

#### (3) Remove Object

#### (4) Get Object

#### (5) Get Object Info

#### (6) Get Object ACL

#### (7) Set Object ACL

#### (8) Initiate Multi-part Upload

#### (9) Multipart Upload Part

#### (10) List Multipart Upload Parts

#### (11) Complete Multipart Upload

#### (12) Abort Multipart Upload
