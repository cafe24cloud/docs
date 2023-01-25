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

1. 오브젝트 스토리지에 대한 요청은 인증되거나 인증되지 않을 수 있습니다. 인증되지 않은 요청이 익명 사용자에 의해 전송된다고 가정합니다. 오브젝트 스토리지는 ACL도 지원합니다.

&#x20;

1. **인증**
2.
   1. &#x20;
   2. 요청을 인증하려면 요청이 오브젝스 스토리지 서버로 전송되기 전에 요청에 액세스 키와 HMAC(해시 기반 메시지 인증 코드)를 포함해야 합니다. 카페24 오브젝트 스토리지는 S3 compatible 인증 방식을 사용합니다.



## 2. Access Control List

## 3. Bucket Operation

#### (1) List Buckets

#### (2) PUT Bucket

#### (3) DELETE Bucket

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
