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

요청을 인증하려면 아래와 같이 요청에 ACCESS KEY와 HMAC(해시 기반 메시지 인증 코드)를 포함해야 합니다.

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

```
- 버킷 이름이 고유하고 제약 조건 내에서 사용되지 않는 경우 작업이 성공합니다.
- 동일한 이름의 버킷이 이미 존재하고 사용자가 버킷 소유자인 경우 작업이 성공합니다.
- 버킷 이름이 이미 사용 중인 경우 작업이 실패합니다.
```

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

버킷 개체 목록을 반환합니다.

* Syntax

```shell
GET /{bucket}?max-keys=25 HTTP/1.1
Host: kr.cafe24obs.com

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Parameters

|     Name    |   Type  |            Description           |
| :---------: | :-----: | :------------------------------: |
|   `prefix`  |  String |      지정된 접두사가 포함된 객체만 반환합니다.     |
| `delimiter` |  String | 접두사와 객체 이름의 나머지 부분 사이의 구분 기호입니다. |
|   `marker`  |  String |       반환된 객체 목록의 시작 인덱스입니다.      |
|  `max-keys` | Integer |   반환할 최대 키 수입니다. 기본값은 1000입니다.   |

* HTTP Response

| HTTP Status | Status Code | Description |
| :---------: | :---------: | :---------: |
|    `200`    |      OK     |  버킷을 검색합니다. |

* Bucket Response Entities

GET /{bucket}은 다음 필드가 버킷의 컨테이너를 반환합니다.

|        Name        |    Type   |                    Description                   |
| :----------------: | :-------: | :----------------------------------------------: |
| `ListBucketResult` |   Entity  |                  객체 목록의 컨테이너입니다.                 |
|       `Name`       |   String  |               컨텐츠가 반환되는 버킷의 이름입니다.               |
|      `Prefix`      |   String  |                   객체 키의 접두사입니다.                  |
|      `Marker`      |   String  |               반환된 객체 목록의 시작 인덱스입니다.              |
|      `MaxKeys`     |  Integer  |                  반환되는 최대 키 수입니다.                 |
|     `Delimiter`    |   String  | 설정된 경우 동일한 접두사를 가진 객체가 CommonPrefixes 목록에 나타납니다. |
|    `IsTruncated`   |  Boolean  |          true인 경우 버킷 콘텐츠의 하위 집합만 반환됩니다.          |
|  `CommonPrefixes`  | Container |        여러 개체에 동일한 접두사가 포함된 경우 이 목록에 나타납니다.       |

* Object Response Entities

ListBucketResult에는 객체가 포함되어 있으며, 각 객체는 Contents 컨테이너 내에 있습니다.

|      Name      |   Type  |        Description       |
| :------------: | :-----: | :----------------------: |
|   `Contents`   |  Object |       객체의 컨테이너입니다.       |
|      `Key`     |  String |         객체의 키입니다.        |
| `LastModified` |   Date  |   객체의 마지막 수정 날짜/시간입니다.   |
|     `ETag`     |  String | 객체의 MD-5 해시입니다. (엔티티 태그) |
|     `Size`     | Integer |        객체의 크기입니다.        |
| `StorageClass` |  String |  항상 STANDARD를 반환해야 합니다.  |



#### (5) Get Bucket ACL

버킷의 액세스 제어 목록을 검색합니다.&#x20;

사용자는 버킷 소유자이거나 버킷에 대한 READ\_ACP 권한을 부여받아야 합니다.

* Syntax

```shell
GET /{bucket}?acl HTTP/1.1
Host: kr.cafe24obs.com

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Response Entities

|          Name         |    Type   |                Description               |
| :-------------------: | :-------: | :--------------------------------------: |
| `AccessControlPolicy` | Container |              응답에 대한 컨테이너입니다.             |
|  `AccessControlList`  | Container |             ACL 정보의 컨테이너입니다.             |
|        `Owner`        | Container |   버킷 소유자의 ID 및 DisplayName에 대한 컨테이너입니다.  |
|          `ID`         |   String  |              버킷 소유자의 ID입니다.              |
|     `DisplayName`     |   String  |             버킷 소유자의 표시 이름입니다.            |
|        `Grant`        | Container |     Grantee 및 Permission을 위한 컨테이너입니다.    |
|       `Grantee`       | Container | 권한 있는 사용자의 DisplayName 및 ID에 대한 컨테이너입니다. |
|      `Permission`     |   String  |          Grantee 버킷에 부여된 권한입니다.          |



#### (6) PUT Bucket ACL

기존 버킷에 대한 액세스 제어 목록을 설정합니다.&#x20;

사용자는 버킷 소유자이거나 버킷에 대한 WRITE\_ACP 권한이 부여되어야 합니다.

* Syntax

```shell
PUT /{bucket}?acl HTTP/1.1
Host: kr.cafe24obs.com

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Request Entities

|          Name         |    Type   |                  Description                 |
| :-------------------: | :-------: | :------------------------------------------: |
| `AccessControlPolicy` | Container |                요청에 대한 컨테이너입니다.               |
|  `AccessControlList`  | Container |               ACL 정보의 컨테이너입니다.               |
|        `Owner`        | Container |     버킷 소유자의 ID 및 DisplayName에 대한 컨테이너입니다.    |
|          `ID`         |   String  |                버킷 소유자의 ID입니다.                |
|     `DisplayName`     |   String  |               버킷 소유자의 표시 이름입니다.              |
|        `Grant`        | Container |       Grantee 및 Permission을 위한 컨테이너입니다.      |
|       `Grantee`       | Container | 권한 부여를 받는 사용자의 DisplayName 및 ID에 대한 컨테이너입니다. |
|      `Permission`     |   String  |            Grantee 버킷에 부여된 권한입니다.            |



#### (7) List Bucket Multipart Uploads

GET /?uploads는 현재 진행 중인 멀티파트 업로드 목록을 반환합니다.

즉, 애플리케이션이 멀티파트 업로드를 시작했지만, 아직 모든 업로드를 완료하지 않았습니다.

* Syntax

```shell
GET /{bucket}?uploads HTTP/1.1
Host: kr.cafe24obs.com

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Parameters

GET /{bucket}?uploads에 대한 매개변수를 지정할 수 있지만 필수는 아닙니다.

|        Name        |   Type  |                            Description                            |
| :----------------: | :-----: | :---------------------------------------------------------------: |
|      `prefix`      |  String |                 키에 지정된 접두사가 포함된 진행 중인 업로드를 반환합니다.                 |
|     `delimiter`    |  String |                  접두사와 개체 이름의 나머지 부분 사이의 구분 기호입니다.                 |
|    `key-marker`    |  String |                         업로드 목록의 시작 마커입니다.                         |
|     `max-keys`     | Integer |                 진행 중인 업로드의 최대 수입니다. 기본값은 1000입니다.                 |
|    `max-uploads`   | Integer |          멀티파트 업로드의 최대 수입니다. 범위는 1-1000입니다. 기본값은 1000입니다.          |
| `upload-id-marker` |  String | 키 마커가 지정되지 않은 경우 무시됩니다. ID 또는 그 뒤에 사전 순으로 나열할 첫 번째 업로드 ID를 지정합니다. |

* Response Entities

|                 Name                |    Type   |                              Description                             |
| :---------------------------------: | :-------: | :------------------------------------------------------------------: |
|     `ListMultipartUploadsResult`    | Container |                             결과 컨테이너입니다.                              |
| `ListMultipartUploadsResult.Prefix` |   String  |                       접두사 요청 매개변수로 지정된 접두사입니다.                       |
|               `Bucket`              |   String  |                          버킷 콘텐츠를 수신하는 버킷입니다.                         |
|             `KeyMarker`             |   String  |                   key-marker 요청 매개변수로 지정된 키 마커입니다.                   |
|           `UploadIdMarker`          |   String  |                 upload-id-marker 요청 매개변수로 지정된 마커입니다.                 |
|           `NextKeyMarker`           |   String  |              IsTruncated가 true인 경우 후속 요청에서 사용할 키 마커입니다.              |
|         `NextUploadIdMarker`        |   String  |            IsTruncated가 true인 경우 후속 요청에서 사용할 업로드 ID 마커입니다.           |
|             `MaxUploads`            |  Integer  |                 max-uploads 요청 매개변수로 지정된 최대 업로드 수입니다.                |
|             `Delimiter`             |   String  |           설정된 경우 동일한 접두사를 가진 개체가 CommonPrefixes 목록에 나타납니다.           |
|            `IsTruncated`            |  Boolean  |                  true인 경우 버킷 업로드 콘텐츠의 하위 집합만 반환됩니다.                  |
|               `Upload`              | Container | Key, UploadId, InitiatorOwner, StorageClass 및 Initiated 요소의 컨테이너입니다. |
|                `Key`                |   String  |                      멀티파트 업로드가 완료되면 객체의 키가 됩니다.                      |
|              `UploadId`             |   String  |                         멀티파트 업로드를 식별하는 ID입니다.                        |
|             `Initiator`             | Container |                 업로드를 시작한 사용자의 ID와 DisplayName을 포함합니다.                |
|            `DisplayName`            |   String  |                         개시자의 Display name입니다.                        |
|                 `ID`                |   String  |                              개시자의 ID입니다.                             |
|               `Owner`               | Container |            업로드된 객체를 소유한 사용자의 ID 및 DisplayName에 대한 컨테이너입니다.           |
|            `StorageClass`           |   String  |         결과 객체를 저장하는 데 사용되는 메서드입니다. 표준 또는 REDUCED\_REDUNDANCY         |
|             `Initiated`             |    Date   |                       사용자가 업로드를 시작한 날짜와 시간입니다.                       |
|           `CommonPrefixes`          | Container |                  여러 객체에 동일한 접두사가 포함된 경우 이 목록에 나타납니다.                 |
|       `CommonPrefixes.Prefix`       |   String  |                 접두사 요청 매개변수로 정의된 접두사 뒤의 키 하위 문자열입니다.                 |





## 4. Object Operation

#### (1) Put Object

버킷에 객체를 추가합니다.

이 작업을 수행하려면 버킷에 대한 쓰기 권한이 있어야 합니다.

* Syntax

```shell
PUT /{bucket}/{object} HTTP/1.1
Host: kr.cafe24obs.com 

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Request Headers

|        Name      |          Description          |                             Valid Values                            | Required |
| :--------------: | :---------------------------: | :-----------------------------------------------------------------: | :------: |
|    content-md5   | 메시지의 base64로 인코딩된 MD-5 해시입니다. |                      문자열 (기본값이 아님. 제약 조건이 없음.)                      |    No    |
|   content-type   |         표준 MIME 유형입니다.        |                모든 MIME 유형 (기본값: binary/octet-stream)                |    No    |
| x-amz-meta-<...> |   사용자 메타데이터로, 개체와 함께 저장됩니다.   |                        최대 8KB의 문자열 (기본값이 아님.)                       |    No    |
|     x-amz-acl    |  이미 사용할 수 있도록 만들어져 있는 ACL입니다. | `private`, `public-read`, `public-read-write`, `authenticated-read` |    No    |



#### (2) Copy Object

객체를 복사하기 위해 PUT을 사용하고, 대상 버킷과 객체 이름을 지정합니다.

* Syntax

```shell
PUT /{dest-bucket}/{dest-object} HTTP/1.1
x-amz-copy-source: {source-bucket}/{source-object}
Host: kr.cafe24obs.com
    
Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Request Headers

|              Name              |           Description           |                             Valid Values                            | Required |
| :----------------------------: | :-----------------------------: | :-----------------------------------------------------------------: | :------: |
|        x-amz-copy-source       |       소스 버킷 이름과 객체 이름입니다.       |                            {bucket}/{obj}                           |    Yes   |
|            x-amz-acl           |   이미 사용할 수 있도록 만들어져 있는 ACL입니다.  | `private`, `public-read`, `public-read-write`, `authenticated-read` |    No    |
|  x-amz-copy-if-modified-since  |     타임스탬프 이후 수정된 경우에만 복사합니다.    |                              Timestamp                              |    No    |
| x-amz-copy-if-unmodified-since |   타임스탬프 이후 수정되지 않은 경우에만 복사합니다.  |                              Timestamp                              |    No    |
|       x-amz-copy-if-match      | 객체 ETag가 ETag와 일치하는 경우에만 복사합니다. |                              Entity Tag                             |    No    |
|    x-amz-copy-if-none-match    |   객체 ETag가 일치하지 않는 경우에만 복사합니다.  |                              Entity Tag                             |    No    |

* Response Entities

|       Name       |    Type   |      Description     |
| :--------------: | :-------: | :------------------: |
| CopyObjectResult | Container |    응답 요소의 컨테이너입니다.   |
|   LastModified   |    Date   | 원본 객체의 마지막 수정 날짜입니다. |
|       Etag       |   String  |    새 객체의 ETag입니다.    |



#### (3) Remove Object

개체를 제거합니다.

포함하는 버킷에 설정된 WRITE 권한이 필요합니다.

* Syntax

```shell
DELETE /{bucket}/{object} HTTP/1.1
Host: kr.cafe24obs.com 

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```



#### (4) Get Object

버킷에서 객체를 검색합니다.

* Syntax

```shell
GET /{bucket}/{object} HTTP/1.1
Host: kr.cafe24obs.com     

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Request Headers

|         Name        |           Description           |          Valid Values          | Required |
| :-----------------: | :-----------------------------: | :----------------------------: | :------: |
|        range        |          검색할 개체의 범위입니다.         | Range: bytes=beginbyte-endbyte |    No    |
|  if-modified-since  |     타임스탬프 이후 수정된 경우에만 가져옵니다.    |            Timestamp           |    No    |
| if-unmodified-since |   타임스탬프 이후 수정되지 않은 경우에만 가져옵니다.  |            Timestamp           |    No    |
|       if-match      | 객체 ETag가 ETag와 일치하는 경우에만 가져옵니다. |           Entity Tag           |    No    |
|    if-none-match    | 객체 ETag가 ETag와 일치하는 경우에만 가져옵니다. |           Entity Tag           |    No    |

* Response Headers

|      Name     |               Description              |
| :-----------: | :------------------------------------: |
| Content-Range | 데이터 범위로, 범위 헤더 필드가 요청에 지정된 경우에만 반환됩니다. |



#### (5) Get Object Info

객체에 대한 정보를 반환합니다.&#x20;

이 요청은 Get Object 요청과 동일한 헤더 정보를 반환하지만, 객체 데이터 페이로드가 아닌 메타데이터만 포함합니다.

* Syntax

```shell
HEAD /{bucket}/{object} HTTP/1.1
Host: kr.cafe24obs.com     

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Request Headers

|         Name        |           Description           |          Valid Values          | Required |
| :-----------------: | :-----------------------------: | :----------------------------: | :------: |
|        range        |          검색할 개체의 범위입니다.         | Range: bytes=beginbyte-endbyte |    No    |
|  if-modified-since  |     타임스탬프 이후 수정된 경우에만 가져옵니다.    |            Timestamp           |    No    |
| if-unmodified-since |   타임스탬프 이후 수정되지 않은 경우에만 가져옵니다.  |            Timestamp           |    No    |
|       if-match      | 객체 ETag가 ETag와 일치하는 경우에만 가져옵니다. |           Entity Tag           |    No    |
|    if-none-match    | 객체 ETag가 ETag와 일치하는 경우에만 가져옵니다. |           Entity Tag           |    No    |



#### (6) Get Object ACL

객체에 대한 ACL 검색합니다.

* Syntax

```shell
GET /{bucket}/{object}?acl HTTP/1.1
Host: kr.cafe24obs.com     

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Response Entities

|          Name         |    Type   |                Description               |
| :-------------------: | :-------: | :--------------------------------------: |
| `AccessControlPolicy` | Container |                응답 컨테이너입니다.               |
|  `AccessControlList`  | Container |             ACL 정보의 컨테이너입니다.             |
|        `Owner`        | Container |   객체 소유자의 ID 및 DisplayName에 대한 컨테이너입니다.  |
|          `ID`         |   String  |              객체 소유자의 ID입니다.              |
|     `DisplayName`     |   String  |             객체 소유자의 표시 이름입니다.            |
|        `Grant`        | Container |     Grantee 및 Permission을 위한 컨테이너입니다.    |
|       `Grantee`       | Container | 권한 있는 사용자의 DisplayName 및 ID에 대한 컨테이너입니다. |
|      `Permission`     |   String  |          Grantee 객체에 부여된 권한입니다.          |



#### (7) Set Object ACL

기존 객체에 대한 ACL을 설정합니다.

* Syntax

```shell
PUT /{bucket}/{object}?acl
Host: kr.cafe24obs.com     

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Response Entities

|          Name         |    Type   |                Description               |
| :-------------------: | :-------: | :--------------------------------------: |
| `AccessControlPolicy` | Container |                응답 컨테이너입니다.               |
|  `AccessControlList`  | Container |             ACL 정보의 컨테이너입니다.             |
|        `Owner`        | Container |   객체 소유자의 ID 및 DisplayName에 대한 컨테이너입니다.  |
|          `ID`         |   String  |              객체 소유자의 ID입니다.              |
|     `DisplayName`     |   String  |             객체 소유자의 표시 이름입니다.            |
|        `Grant`        | Container |     Grantee 및 Permission을 위한 컨테이너입니다.    |
|       `Grantee`       | Container | 권한 있는 사용자의 DisplayName 및 ID에 대한 컨테이너입니다. |
|      `Permission`     |   String  |          Grantee 객체에 부여된 권한입니다.          |



#### (8) Initiate Multi-part Upload

멀티파트 업로드 프로세스를 시작합니다.

* Syntax

```shell
POST /{bucket}/{object}?uploads
Host: kr.cafe24obs.com     

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Request Headers

|       Name       |          Description          |                             Valid Values                            | Required |
| :--------------: | :---------------------------: | :-----------------------------------------------------------------: | :------: |
|    content-md5   | 메시지의 base64로 인코딩된 MD-5 해시입니다. |                      문자열 (기본값이 아님. 제약 조건이 없음.)                      |    No    |
|   content-type   |         표준 MIME 유형입니다.        |                모든 MIME 유형 (기본값: binary/octet-stream)                |    No    |
| x-amz-meta-<...> |   사용자 메타데이터로, 개체와 함께 저장됩니다.   |                        최대 8KB의 문자열 (기본값이 아님.)                       |    No    |
|     x-amz-acl    |  이미 사용할 수 있도록 만들어져 있는 ACL입니다. | `private`, `public-read`, `public-read-write`, `authenticated-read` |    No    |

* Response Entities

|                Name               |    Type   |                  Description                  |
| :-------------------------------: | :-------: | :-------------------------------------------: |
| `InitiatedMultipartUploadsResult` | Container |                  결과 컨테이너입니다.                  |
|              `Bucket`             |   String  |              객체 콘텐츠를 수신하는 버킷입니다.              |
|               `Key`               |   String  |             키 요청 매개변수에서 지정한 키입니다.             |
|             `UploadId`            |   String  | 멀티파트 업로드를 식별하는 upload-id 요청 매개 변수로 지정된 ID입니다. |



#### (9) Multipart Upload Part

멀티파트 업로드를 사용하여 객체를 업로드합니다.

* Syntax

```shell
PUT /{bucket}/{object}?partNumber=&uploadId= HTTP/1.1
Host: kr.cafe24obs.com     
                                                    
Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* HTTP Response

| HTTP Status |  Status Code |               Description              |
| :---------: | :----------: | :------------------------------------: |
|     404     | NoSuchUpload | 지정된 업로드 ID가 이 객체에서 시작된 업로드와 일치하지 않습니다. |



#### (10) List Multipart Upload Parts

특정 멀티파트 업로드를 위해 업로드된 파트를 나열합니다.

* Syntax

```shell
GET /{bucket}/{object}?uploadId=123 HTTP/1.1
Host: kr.cafe24obs.com     

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Response Entities

|                Name               |    Type   |                            Description                           |
| :-------------------------------: | :-------: | :--------------------------------------------------------------: |
| `InitiatedMultipartUploadsResult` | Container |                            결과 컨테이너입니다.                           |
|              `Bucket`             |   String  |                        객체 컨텐츠를 수신하는 버킷입니다.                       |
|               `Key`               |   String  |                       키 요청 매개변수에서 지정한 키입니다.                      |
|             `UploadId`            |   String  |           멀티파트 업로드를 식별하는 upload-id 요청 매개 변수로 지정된 ID입니다.          |
|            `Initiator`            | Container |               업로드를 시작한 사용자의 ID와 DisplayName을 포함합니다.              |
|                `ID`               |   String  |                            게시자의 ID입니다.                           |
|           `DisplayName`           |   String  |                          게시자의 표시 이름입니다.                          |
|              `Owner`              | Container |          업로드된 개체를 소유한 사용자의 ID 및 DisplayName에 대한 컨테이너입니다.         |
|           `StorageClass`          |   String  |   결과 객체를 저장하는 데 사용되는 메서로,`STANDARD` 또는 REDUCED\_REDUNDANCY 입니다.  |
|         `PartNumberMarker`        |   String  |      IsTruncated가 true인 경우 후속 요청에서 사용할 파트 마커입니다. 목록에 선행합니다.      |
|       `NextPartNumberMarker`      |   String  |     IsTruncated가 true인 경우 후속 요청에서 사용할 다음 파트 마커입니다. 목록의 끝입니다.     |
|             `MaxParts`            |  Integer  |           max-parts 요청 매개변수에 지정된 대로 응답에 허용되는 최대 파트입니다.           |
|           `IsTruncated`           |  Boolean  |                true인 경우 객체 업로드 콘텐츠의 하위 집합만 반환됩니다.                |
|               `Part`              | Container | Key, Part, InitiatorOwner, StorageClass 및 Initiated 요소의 컨테이너입니다. |
|            `PartNumber`           |  Integer  |                           파트의 식별 번호입니다.                          |
|               `ETag`              |   String  |                          파트의 엔터티 태그입니다.                          |
|               `Size`              |  Integer  |                          업로드된 파트의 크기입니다.                         |



#### (11) Complete Multipart Upload

업로드된 부분을 조합하고, 새 개체를 생성하여 멀티파트 업로드를 완료합니다.

* Syntax

```shell
POST /{bucket}/{object}?uploadId= HTTP/1.1
Host: kr.cafe24obs.com     
                                                    
Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

* Request Entities

|            Name           |    Type   |           Description          | Required |
| :-----------------------: | :-------: | :----------------------------: | :------: |
| `CompleteMultipartUpload` | Container |    하나 이상의 파트로 구성된 컨테이너입니다다.    |    Yes   |
|           `Part`          | Container | PartNumber 및 ETag에 대한 컨테이너입니다. |    Yes   |
|        `PartNumber`       |  Integer  |           파트의 식별자입니다.          |    Yes   |
|           `ETag`          |   String  |         파트의 엔터티 태그입니다.         |    Yes   |

* Response Entities

|              Name             |    Type   |      Description      |
| :---------------------------: | :-------: | :-------------------: |
| CompleteMultipartUploadResult | Container |      응답 컨테이너입니다.      |
|            Location           |    URI    | 새 객체의 리소스 식별자(경로)입니다. |
|             Bucket            |   String  |  새 객체가 포함된 버킷의 이름입니다. |
|              Key              |   String  |       객체의 키입니다.       |
|              ETag             |   String  |    새 객체의 엔터티 태그입니다.   |



#### (12) Abort Multipart Upload

멀티파트 업로드를 중단합니다.

* Syntax

```shell
DELETE /{bucket}/{object}?uploadId= HTTP/1.1
Host: kr.cafe24obs.com     

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```
