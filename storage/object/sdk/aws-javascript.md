---
description: AWS Javascript SDK를 사용하여 오브젝트 스토리지를 사용하는 방법은 다음과 같습니다.
---

# AWS Javascript SDK 사용 방법

## 1. AWS Javascript v3 S3 SDK 사용하기

AWS Javascript v3 S3 SDK는 AWS에서 Javascript 코드를 통해 S3를 이용할 수 있도록 제공하는 도구입니다.&#x20;

카페24 클라우드의 오브젝트 스토리지는 S3 API와 호환이 되므로 해당 SDK 사용이 가능합니다.&#x20;

|                                                매뉴얼 테스트 버전                                                |                                                                                                                                          Javascript v3 S3 SDK 참고 링크                                                                                                                                         |
| :------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| <p>AWS Javascript SDK version : 3.231.0 </p><p>Node.js version : 14.17.1</p><p>Npm version : 6.14.13</p> | <p>문서 : <a href="https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html">Developer guide - AWS SDK for Javascript 3.x</a></p><p>예제 : <a href="https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/javascript_s3_code_examples.html">Javascript v3 S3 examples</a></p> |





## 2. AWS Javascript v3 S3 SDK 설치하기

Javascript 개발을 위한 실행 환경 Node.js가 설치된 상태에서 진행합니다.&#x20;

npm 프로젝트 디렉터리로 이동한 다음, AWS Javascript v3 SDK를 설치합니다.

```shell-session
$ npm install @aws-sdk/client-s3
```





## 3. 오브젝트 스토리지 API Key 확인하기

****[**\[오브젝트 스토리지 사용 방법\]**](../use.md)을 참고하여 신청한 오브젝트 스토리지의 Access Key와 Secret Key를 확인합니다.

&#x20;



## 4. 자격 증명 프로필 설정하기

인증 파일을 생성하여 Access Key와 Secret Key를 등록합니다.

자세한 정보는 [**AWS SDK for Java v2 - Using Credentials**](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/credentials.html)에서 확인할 수 있습니다.

인증 파일의 기본 경로는 "\~/.aws/credentials"입니다.

```shell-session
$ cat >> ~/.aws/credentials << EOF
[default]
aws_access_key_id = [access_key]
aws_secret_access_key = [secret_key]
EOF
```





## 5. 코드 예제

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

사용하는 Javascript 및 SDK 버전에 따라 변경이 필요할 수 있습니다.&#x20;

해당 내용은 AWS Javascript SDK를 사용하여 카페24 클라우드의 오브젝트 스토리지를 이용하는 예제 코드로, 필요에 따라 응용할 수 있습니다.
{% endhint %}

#### (1) 버킷 생성

버킷을 생성합니다.

```shell
import {CreateBucketCommand, S3Client} from "@aws-sdk/client-s3";

const s3Client = new S3Client({endpoint: "https://kr.cafe24obs.com", forcePathStyle: true, region: "zone-group-cafe24cloud"});

export const bucketParams = {
  Bucket: "test-bucket"
};

export const run = async () => {
  try {
    const data = await s3Client.send(new CreateBucketCommand(bucketParams));
    console.log("Success : Bucket [" + bucketParams.Bucket + "] has been created.\n",)
    console.log("Response :", JSON.stringify(data, null, 4));
  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

output 예시는 다음과 같습니다.

```shell
Success : Bucket [test-bucket] has been created.
Response : {
    "$metadata": {
        "httpStatusCode": 200,
        "requestId": "tx0000084051beac6c5643e-0063b28289-1682e4f-zone-cafe24cloud-prd-obs",
        "attempts": 1,
        "totalRetryDelay": 0
    }
}
```



#### (2) 버킷 삭제&#x20;

오브젝트가 모두 삭제된 빈 버킷에 대해서만 삭제가 가능합니다.

```shell
import { DeleteBucketCommand, S3Client } from "@aws-sdk/client-s3";

const s3Client  = new S3Client({ endpoint: "https://kr.cafe24obs.com", forcePathStyle: true, region: "zone-group-cafe24cloud" });

export const bucketParams = { Bucket: "test-bucket" };

export const run = async () => {
  try {
    const data = await s3Client.send(new DeleteBucketCommand(bucketParams));
    console.log("Success : Bucket ["+bucketParams.Bucket+"] has been deleted.");
    console.log("Response : ",JSON.stringify(data, null, 4))
    return data; 
  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

output 예시는 다음과 같습니다.

```shell
Success : Bucket [test-bucket] has been deleted.
Response :  {
    "$metadata": {
        "httpStatusCode": 204,
        "requestId": "tx000003362f71a7f0eb52a-0063b28242-168a1f1-zone-cafe24cloud-prd-obs",
        "attempts": 1,
        "totalRetryDelay": 0
    }
}
```



#### (3) 버킷 리스트 조회&#x20;

존재하는 모든 버킷을 조회합니다.&#x20;

```shell
import { ListBucketsCommand, S3Client } from "@aws-sdk/client-s3";

const s3Client  = new S3Client({ endpoint: "https://kr.cafe24obs.com", forcePathStyle: true, region: "zone-group-cafe24cloud" });

export const run = async () => {
  try {
    const data = await s3Client.send(new ListBucketsCommand({}));
    console.log("Success", JSON.stringify(data.Buckets, null, 4));
  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

output 예시는 다음과 같습니다.

```shell
Success [
    {
        "Name": "test-bucket",
        "CreationDate": "2022-12-30T01:44:29.104Z"
    },
    {
        "Name": "test-bucket1",
        "CreationDate": "2022-12-29T06:04:09.457Z"
    },
    {
        "Name": "test-bucket2",
        "CreationDate": "2022-12-29T06:04:16.613Z"
    }
]
```



#### (4) 오브젝트 업로드&#x20;

파일을 특정 버킷에 업로드합니다.

* **새로운 파일 업로드**: 코드상에서 파일명, 파일 내용(Body)을 선언하여 버킷에 업로드하는 방법입니다.

```shell
import {PutObjectCommand, S3Client} from "@aws-sdk/client-s3";

const s3Client = new S3Client({endpoint: "https://kr.cafe24obs.com", forcePathStyle: true, region: "zone-group-cafe24cloud"});

export const bucketParams = {
  Bucket: "test-bucket",
  Key: "test-file.txt",
  Body: "Content of test-file.txt - Hello World!"
};

export const run = async () => {
  try {
    await s3Client.send(new PutObjectCommand(bucketParams));
    console.log("Success : Successfully uploaded newly created file: " + bucketParams.Bucket + "/" + bucketParams.Key);
    return;
  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

```shell
Success : Successfully uploaded newly created file: test-bucket/test-file.txt
```

* **로컬에 있는 파일 업로드**: 기존의 로컬에 있는 파일을 버킷에 업로드하는 방법입니다. &#x20;

```shell
import {PutObjectCommand, S3Client} from "@aws-sdk/client-s3";
import * as path from 'path';
import * as fs from 'fs';

const s3Client = new S3Client({endpoint: "https://kr.cafe24obs.com", forcePathStyle: true, region: "zone-group-cafe24cloud"});
# 로컬에 위치한 업로드할 파일의 경로
const file = "clients\\client-s3\\src\\commands\\cafe24-demo\\files\\test-file-local.txt"
const fileStream = fs.createReadStream(file);

export const uploadParams = {
  Bucket: "test-bucket",
  Key: path.basename(file),
  Body: fileStream
};

export const run = async () => {
  try {
    await s3Client.send(new PutObjectCommand(uploadParams));
    console.log("Success : Successfully uploaded exsisting file " + uploadParams.Bucket + "/" + uploadParams.Key);
    return;
  } catch (err) {
    console.log("Error", err);
  }
};

run()
```

```shell
Success : Successfully uploaded exsisting file test-bucket/test-file-local.pdf
```



#### (5) 오브젝트 다운로드&#x20;

버킷에 있는 파일을 로컬의 특정 경로로 다운로드합니다.&#x20;

```shell
import {GetObjectCommand, S3Client} from "@aws-sdk/client-s3"
import * as fs from 'fs';

const s3Client = new S3Client({endpoint: "https://kr.cafe24obs.com", forcePathStyle: true, region: "zone-group-cafe24cloud"});

export const run = async () => {
  try {
    const bucket_name = "test-bucket";
    const key_name = "test-file.txt";
    const download_bucket_params = {
      Bucket: bucket_name,
      Key: key_name
  };

  # 파일을 다운받을 경로
  const downloadPath = 'clients\\client-s3\\src\\commands\\cafe24-demo\\files\\downloaded-file.txt';
  const data = await s3Client.send(new GetObjectCommand(download_bucket_params));

  console.log("\nDownloading " + key_name + " from " + bucket_name + " ...\n")

  data.Body.pipe(fs.createWriteStream(downloadPath));

  console.log(key_name + " is now downloaded to [" + downloadPath + "]")
  } catch (err) {
    console.log("Error creating and upload object to bucket", err);
    process.exit(1);
  };
};

run();
```

output 예시는 다음과 같습니다.

```shell
Downloading test-file.txt from test-bucket ...
test-file.txt is now downloaded to [clients\client-s3\src\commands\cafe24-demo\files\downloaded-file.txt]
```



#### (6) 오브젝트 리스트 조회&#x20;

버킷에 있는 모든 파일과 폴더를 조회합니다.&#x20;

```shell
import { ListObjectsCommand, S3Client } from "@aws-sdk/client-s3";

const s3Client  = new S3Client({ endpoint: "https://kr.cafe24obs.com", forcePathStyle: true, region: "zone-group-cafe24cloud" });

export const bucketParams = { Bucket: "test-bucket" };

export const run = async () => {
  try {
    const data = await s3Client.send(new ListObjectsCommand(bucketParams));
    console.log("Success : \n", JSON.stringify(data, null, 4));
  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

output 예시는 다음과 같습니다.

```shell
Success : 
 {
    "$metadata": {
        "httpStatusCode": 200,
        "requestId": "tx00000ea81126555bcc2de-0063b27c2f-1682e4f-zone-cafe24cloud-prd-obs",
        "attempts": 1,
        "totalRetryDelay": 0
    },
    "Contents": [
        {
            "Key": "folder/",
            "LastModified": "2023-01-01T14:09:25.283Z",
            "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
            "Size": 0,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "clouduser",
                "ID": "clouduser"
            }
        },
        {
            "Key": "folder/test-file-04",
            "LastModified": "2023-01-01T14:10:04.636Z",
            "ETag": "\"620f0b67a91f7f74151bc5be745b7110\"",
            "Size": 4096,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "clouduser",
                "ID": "clouduser"
            }
        },
        {
            "Key": "test-file-01",
            "LastModified": "2023-01-01T14:08:50.154Z",
            "ETag": "\"620f0b67a91f7f74151bc5be745b7110\"",
            "Size": 4096,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "clouduser",
                "ID": "clouduser"
            }
        }
    ],
    "IsTruncated": false,
    "Marker": "",
    "MaxKeys": 1000,
    "Name": "test-bucket",
    "Prefix": ""
}
```



#### (7) 오브젝트 삭제&#x20;

버킷에서 파일을 삭제합니다.

* **하나의 오브젝트 삭제**: 특정 버킷에 있는 오브젝트를 삭제합니다.&#x20;

```shell
import {DeleteObjectCommand, S3Client} from "@aws-sdk/client-s3";

const s3Client = new S3Client({endpoint: "https://kr.cafe24obs.com", forcePathStyle: true, region: "zone-group-cafe24cloud"});

export const bucketParams = {
  Bucket: "test-bucket",
  Key: "test-file.txt"
};

export const run = async () => {
  try {
    const data = await s3Client.send(new DeleteObjectCommand(bucketParams));
    console.log("Success : Object [" + bucketParams.Key + "] has been deleted.");
    console.log("Response : ", JSON.stringify(data, null, 4))
  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

```shell
Success : Object [test-file.txt] has been deleted.
Response :  {
    "$metadata": {
        "httpStatusCode": 204,
        "requestId": "tx000008ed66bc303224d6c-0063b3e19b-168a1f1-zone-cafe24cloud-prd-obs",
        "attempts": 1,
        "totalRetryDelay": 0
}
```

* **모든 오브젝트 삭제**: 특정 버킷에 있는 모든 오브젝트를 삭제합니다. &#x20;

```shell
import {ListObjectsCommand, DeleteObjectCommand, S3Client} from "@aws-sdk/client-s3";

const s3Client = new S3Client({endpoint: "https://kr.cafe24obs.com", forcePathStyle: true, region: "zone-group-cafe24cloud"});

export const bucketParams = {
  Bucket: "test-bucket"
};

export const run = async () => {
  try {
    console.log("Deleting all objects in the bucket.");
    const data = await s3Client.send(new ListObjectsCommand(bucketParams))
    let noOfObjects = data.Contents;
    for (let i = 0; i < noOfObjects.length; i++) {
      await s3Client.send(new DeleteObjectCommand({Bucket: bucketParams.Bucket, Key: noOfObjects[i].Key}));
  }

  console.log("Success. All objects in bucket [" + bucketParams.Bucket + "] are deleted. : \n", JSON.stringify(data, null, 4));

  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

```shell
Deleting all objects in the bucket.
Success. All objects in bucket [test-bucket] are deleted. : 
 {
    "$metadata": {
        "httpStatusCode": 200,
        "requestId": "tx0000042e054f120512bdb-0063b45ac4-1682e4f-zone-cafe24cloud-prd-obs",
        "attempts": 1,
        "totalRetryDelay": 0
    },
    "Contents": [
        {
            "Key": "folder/",
            "LastModified": "2023-01-03T16:40:55.335Z",
            "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
            "Size": 0,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "clouduser",
                "ID": "clouduser"
            }
        },
        {
            "Key": "folder/test-file-01",
            "LastModified": "2023-01-03T16:41:07.694Z",
            "ETag": "\"620f0b67a91f7f74151bc5be745b7110\"",
            "Size": 4096,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "clouduser",
                "ID": "clouduser"
            }
        },
        {
            "Key": "test-file-02",
            "LastModified": "2023-01-03T16:41:35.762Z",
            "ETag": "\"620f0b67a91f7f74151bc5be745b7110\"",
            "Size": 4096,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "clouduser",
                "ID": "clouduser"
            }
        },
    ],
    "IsTruncated": false,
    "Marker": "",
    "MaxKeys": 1000,
    "Name": "test-bucket",
    "Prefix": ""
}
```



#### (8) 오브젝트 복사&#x20;

파일을 다른 버킷으로 복사합니다.

```shell
import {CopyObjectCommand, S3Client} from "@aws-sdk/client-s3";

const s3Client = new S3Client({endpoint: "https://kr.cafe24obs.com", forcePathStyle: true, region: "zone-group-cafe24cloud"});

# Bucket : 복사한 파일을 붙여넣기 할 버킷
# Key : 붙여넣기 할 파일의 이름
# CopySource : 복사할 파일을 지정. "버킷명/파일경로"
export const bucketParams = {
  Bucket: "test-bucket",
  CopySource: "test-bucket1/bucket-1-file.txt",
  Key: "copied-file.txt"
};

export const run = async () => {
  try {
    const data = await s3Client.send(new CopyObjectCommand(bucketParams));
    console.log("Success : \n", JSON.stringify(data, null, 4));
  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

output 예시는 다음과 같습니다.

```shell
Success : 
 {
    "$metadata": {
        "httpStatusCode": 200,
        "requestId": "tx000007c0798553d2790ae-0063b45773-1682e4f-zone-cafe24cloud-prd-obs",
        "attempts": 1,
        "totalRetryDelay": 0
    },
    "CopyObjectResult": {
        "ETag": "d9ebe4d6aeb33dea41ddb2b57e7b6d80",
        "LastModified": "2023-01-03T16:27:31.755Z"
    }
}
```
