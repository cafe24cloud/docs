---
description: AWS Python SDK를 사용하여 오브젝트 스토리지를 사용하는 방법은 아래와 같습니다.
---

# AWS Python SDK 사용 방법

## 1. AWS Python S3 SDK 사용하기

AWS Python S3 SDK는 AWS에서 Python 코드를 통해 S3를 이용할 수 있도록 제공하는 도구입니다.&#x20;

카페24 클라우드의 오브젝트 스토리지는 S3 API와 호환이 되므로 해당 SDK 사용이 가능합니다.&#x20;

|                            매뉴얼 테스트 버전                            |                                                                                                                  Python S3 SDK 참고 링크                                                                                                                  |
| :--------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| <p>Boto3 version : boto3 1.6.19</p><p>Python version : 3.8.5</p> | <p>문서 : <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html">Boto3 S3 Docs</a></p><p>예제 : <a href="https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-examples.html">Amazon S3 examples</a></p> |







## 2. AWS Python S3 SDK 설치하기

python 및 pip 패키지 매니저가 설치된 환경에서 boto3를 설치합니다.

```shell-session
$ pip install boto3==1.6.19
```







## 3. 오브젝트 스토리지 API Key 확인하기

[\[오브젝트 스토리지 사용 방법\]](../use.md)을 참고하여 신청한 오브젝트 스토리지의 Access Key와 Secret Key를 확인합니다.

&#x20;





## 4. 코드 예제

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

사용하는 Python 및 SDK 버전에 따라 변경이 필요할 수 있습니다.&#x20;

해당 내용은 AWS Python SDK를 사용하여 카페24 클라우드의 오브젝트 스토리지를 이용하는 예제 코드로, 필요에 따라 응용할 수 있습니다.
{% endhint %}

앞서 확인한 오브젝트 스토리지의 Access Key, Secret Key를 코드에 적용하여 사용합니다.

### (1) 버킷 생성

버킷을 생성합니다.

```shell
import boto3
from botocore.exceptions import ClientError

service_name = 's3'
endpoint_url = 'https://kr.cafe24obs.com'
region_name = 'US'
access_key = '[access_key]'
secret_key = '[secret_key]'

s3_client = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key,
            aws_secret_access_key=secret_key)

def create_bucket(bucket_name):  
    try:
        s3_client.create_bucket(Bucket=bucket_name)
        print("Bucket ["+bucket_name+"] has been created.")
    except ClientError as e:
        logging.error(e)
        return False
    return True

if __name__ == "__main__":
    bucket_name="test-bucket"
    create_bucket(bucket_name)
```

output 예시는 다음과 같습니다.

```shell
Bucket [test-bucket] has been created.
```





### (2) 버킷 삭제&#x20;

오브젝트가 모두 삭제된 빈 버킷에 대해서만 삭제가 가능합니다.

```shell
import logging
import boto3
from botocore.exceptions import ClientError

service_name = 's3'
endpoint_url = 'https://kr.cafe24obs.com'
region_name = 'US'
access_key = '[access_key]'
secret_key = '[secret_key]'

s3_client = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key,
                      aws_secret_access_key=secret_key)

def delete_bucket(bucket_name, region=None):
    try:
        s3_client.delete_bucket(Bucket=bucket_name)
        print("Bucket ["+bucket_name+"] has been deleted.")
    except ClientError as e:
        logging.error(e)
        return False
    return True

if __name__ == "__main__":
    bucket_name="test-bucket"
    delete_bucket(bucket_name)
```

output 예시는 다음과 같습니다.

```shell
Bucket [test-bucket] has been deleted.
```





### (3) 버킷 리스트 조회&#x20;

존재하는 모든 버킷을 조회합니다.&#x20;

```shell
import boto3

service_name = 's3'
endpoint_url = 'https://kr.cafe24obs.com'
region_name = 'US'
access_key = '[access_key]'
secret_key = '[secret_key]'

if __name__ == "__main__":
    s3_client = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key,
                      aws_secret_access_key=secret_key)
 
    response = s3_client.list_buckets()
     
    print('--Existing buckets--')
    if not response['Buckets']:
        print("There's no existing bucket.")
    else:
        for bucket in response['Buckets']:
            print(f'{bucket["Name"]}')
```

output 예시는 다음과 같습니다.

```shell
--Existing buckets--
test-bucket
test-bucket1
test-bucket2
```





### (4) 오브젝트 업로드&#x20;

로컬에 있는 파일을 특정 버킷에 업로드합니다.

```shell
import logging
import boto3
from botocore.exceptions import ClientError
import os

service_name = 's3'
endpoint_url = 'https://kr.cafe24obs.com'
region_name = 'US'
access_key = '[access_key]'
secret_key = '[secret_key]'

s3_client = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key,
                      aws_secret_access_key=secret_key)
    
def upload_file(file_name, bucket, object_name=None):
    # 업로드할 오브젝트 이름이 명시되지 않으면, 기존 파일 이름으로 업로드
    if object_name is None:
        object_name = os.path.basename(file_name)

    try:
        response = s3_client.upload_file(file_name, bucket, object_name)
        print("File ["+file_name+"] is uploaded to bucket ["+bucket_name+"] as object ["+object_name+"]")
    except ClientError as e:
        logging.error(e)
        return False
    return True

if __name__ == "__main__":
    file_name="file/demofile.txt"
    bucket_name="test-bucket"
    object_name = "objectfile.txt"
    upload_file(file_name, bucket_name, object_name)
```

output 예시는 다음과 같습니다.

```shell
File [file/demofile.txt] is uploaded to bucket [test-bucket] as object [objectfile.txt]
```





### (5) 오브젝트 다운로드&#x20;

버킷에 있는 파일을 로컬의 특정 경로로 다운로드합니다.&#x20;

```shell
import logging
import boto3
from botocore.exceptions import ClientError

service_name = 's3'
endpoint_url = 'https://kr.cafe24obs.com'
region_name = 'US'
access_key = '[access_key]'
secret_key = '[secret_key]'

s3_client = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key,
                      aws_secret_access_key=secret_key)
    
def download_object(bucket_name, object_name, local_path):
    try:
        s3_client.download_file(bucket_name, object_name, local_path)
        print("File ["+object_name+"] is downloaded\nfrom bucket ["+bucket_name+"] to local path ["+local_path+"]")
    except ClientError as e:
        logging.error(e)
        return False
    return True

if __name__ == "__main__":

    bucket_name="test-bucket"
    object_name = "objectfile.txt"
    local_path = "file/downloaded_file.txt"
    download_object(bucket_name, object_name, local_path)
```

output 예시는 다음과 같습니다.

```shell
File [objectfile.txt] is downloaded
from bucket [test-bucket] to local path [file/downloaded_file.txt]
```





### (6) 오브젝트 리스트 조회&#x20;

버킷에 있는 모든 파일과 폴더를 조회합니다.&#x20;

**max\_keys** 값은 반환할 파일의 개수를 의미하며, default 값은 1,000입니다.

만약 버킷에 있는 파일 개수가 설정된 MaxKeys 값을 넘기면 IsTruncated 값이 True가 됩니다.&#x20;

다음은 max\_keys를 3으로 설정하여, 버킷에 속한 모든 오브젝트를 3개씩 조회하는 예제입니다.&#x20;

```shell
import boto3

service_name = 's3'
endpoint_url = 'https://kr.cafe24obs.com'
region_name = 'US'
access_key = '[access_key]'
secret_key = '[secret_key]'

s3_client = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key,
                      aws_secret_access_key=secret_key)

def list_objects(bucket_name, max_keys):
    response = s3_client.list_objects(Bucket=bucket_name, MaxKeys=max_keys)
    
    print('List all objects in the bucket')
    print('Object List')
    
    object_num=0
    while True:
        print('- IsTruncated=%r' % response.get('IsTruncated'))
        print('- Marker=%s' % response.get('Marker'))
        print('- NextMarker=%s' % response.get('NextMarker'))
        for content in response.get('Contents'):
            print('\tName : %s\tSize : %d\tOwner : %s' % \
                  (content.get('Key'), content.get('Size'), content.get('Owner').get('ID')))
            object_num+=1

        # IsTruncated 값이 true 일때 다음에 조회할 지점을 알기 위해 현재의 NextMarker 값을 Maker로 지정
        if response.get('IsTruncated'):
            response = s3_client.list_objects(Bucket=bucket_name, MaxKeys=max_keys,
                                       Marker=response.get('NextMarker'))
        else:
            break
            
    print("Total Object Count : %d" % (object_num))

if __name__ == "__main__":
    bucket_name = 'test-bucket'
    max_keys = 3
    response = list_objects(bucket_name, max_keys)
```

output 예시는 다음과 같습니다.

```shell
List all objects in the bucket
Object List
- IsTruncated=True
- Marker=
- NextMarker=test-file-03
    Name : test-file-01    Size : 4096    Owner : clouduser
    Name : test-file-02    Size : 4096    Owner : clouduser
    Name : test-file-03    Size : 4096    Owner : clouduser
- IsTruncated=True
- Marker=test-file-03
- NextMarker=test-file-06
    Name : test-file-04    Size : 4096    Owner : clouduser
    Name : test-file-05    Size : 4096    Owner : clouduser
    Name : test-file-06    Size : 4096    Owner : clouduser

Total Object Count : 6
```





### (7) 오브젝트 삭제&#x20;

특정 버킷에 있는 파일을 삭제합니다.

```shell
import logging
import boto3
from botocore.exceptions import ClientError

service_name = 's3'
endpoint_url = 'https://kr.cafe24obs.com'
region_name = 'US'
access_key = '[access_key]'
secret_key = '[secret_key]'

s3_client = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key,
                      aws_secret_access_key=secret_key)
    
def delete_file(bucket_name, object_name):
    try:
        response = s3_client.delete_object(Bucket=bucket_name, Key=object_name)
        print("Deleted a file ["+object_name+"] from bucket ["+bucket_name+"]")
    except ClientError as e:
        logging.error(e)
        return False
    return True

if __name__ == "__main__":
    bucket_name="test-bucket"
    object_name = "demofile.txt"
    delete_file(bucket_name, object_name) 
```

output 예시는 다음과 같습니다.

```shell
Deleted a file [demofile.txt] from bucket [test-bucket]
```





### (8) 버킷 정책 등록

버킷에 정책을 등록합니다.&#x20;

다음 예제를 통해 버킷에 hotlinking 방지 정책을 적용할 수 있습니다.&#x20;

**hotlinking**은 자신의 소유가 아닌 사진, 음원 등을 관리자의 허락 없이 이미지 링크를 이용해 무단 도용하는 것을 의미합니다.&#x20;

오브젝트의 링크를 호출할 때마다 트래픽이 발생하므로, hotlinking 방지 정책을 통해 이를 예방할 수 있습니다.&#x20;

```shell
import json
import logging
import boto3
from botocore.exceptions import ClientError
import os

service_name = 's3'
endpoint_url = 'https://kr.cafe24obs.com'
region_name = 'US'
access_key = '[access_key]'
secret_key = '[secret_key]'

s3_client = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key,
                         aws_secret_access_key=secret_key)

def set_bucket_policy(bucket_name, bucket_policy):
    try:
        response = s3_client.put_bucket_policy(
            Bucket=bucket_name, Policy=bucket_policy)
        print("Bucket policy is set")
    except ClientError as e:
        logging.error(e)
        return False
    return True

if __name__ == "__main__":
    bucket_name = "test-bucket"

    # 오브젝트 링크 사용을 허용할 웹페이지 지정
    webpage_url = "https://my-webpage.com/"
    
    bucket_policy = {
        "Version":  "2008-10-17",
        "Id":  "preventHotLinking",
        "Statement":  [
            {
                "Sid":  "1",
                "Effect":  "Allow",
                "Principal":  {
                    "AWS":  "*"
                },
                "Action":  "s3:GetObject",
                "Resource": f"arn:aws:s3:::{bucket_name}/*",
                "Condition":  {
                    "StringLike":  {
                        "aws:Referer":  [
                            f"{webpage_url}*"
                        ]
                    }
                }
            }
        ]
    }

    # 버킷 정책을 JSON dictionary 형식에서 String으로 변환한 후 적용 
    bucket_policy = json.dumps(bucket_policy)

    set_bucket_policy(bucket_name, bucket_policy)
```

output 예시는 다음과 같습니다.

```shell
Bucket policy is set
```





### (9) 버킷 정책 리스트 조회

버킷에 등록된 정책을 조회합니다.

```shell
import logging
import boto3
from botocore.exceptions import ClientError

service_name = 's3'
endpoint_url = 'https://kr.cafe24obs.com'
region_name = 'US'
access_key = '[access_key]'
secret_key = '[secret_key]'

s3_client = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key,
                      aws_secret_access_key=secret_key)
    
def retrive_bucket_policy(bucket_name):
    try:
        response = s3_client.get_bucket_policy(Bucket=bucket_name)
        print("Retrive bucket policy of bucket ["+bucket_name+"]")
        print(response['Policy'])
    except ClientError as e:
        logging.error(e)
        return False
    return True

if __name__ == "__main__":
    bucket_name="test-bucket"
    retrive_bucket_policy(bucket_name)
```

output 예시는 다음과 같습니다.

```shell
Retrive bucket policy of bucket [test-bucket]
{"Version": "2008-10-17", "Id": "preventHotLinking", "Statement": [{"Sid": "1", "Effect": "Allow", "Principal": {"AWS": "*"}, "Action": "s3:GetObject", "Resource": "arn:aws:s3:::test-bucket/*", "Condition": {"StringLike": {"aws:Referer": ["https://my-webpage.com/*"]}}}]}
```





### (10) 버킷 정책 삭제

버킷에 등록된 정책을 삭제합니다.

```shell
import boto3

service_name = 's3'
endpoint_url = 'https://kr.cafe24obs.com'
region_name = 'US'
access_key = '[access_key]'
secret_key = '[secret_key]'

s3_client = boto3.client(service_name, endpoint_url=endpoint_url, aws_access_key_id=access_key,
                         aws_secret_access_key=secret_key)

if __name__ == "__main__":
    bucket_name = "test-bucket"
    s3_client.delete_bucket_policy(Bucket=bucket_name)
    print("Bucket policy is deleted from bucket ["+bucket_name+"]")
```

output 예시는 다음과 같습니다.

```shell
Bucket policy is deleted from bucket [test-bucket]
```
