---
description: AWS Java SDK를 사용하여 오브젝트 스토리지를 사용하는 방법은 아래와 같습니다.
---

# AWS Java SDK 사용 방법

## 1. AWS Java v2 S3 SDK 사용하기

AWS Java v2 S3 SDK는 AWS에서 Java 코드를 통해 S3를 이용할 수 있도록 제공하는 도구입니다.&#x20;

카페24 클라우드의 오브젝트 스토리지는 S3 API와 호환이 되므로 해당 SDK 사용이 가능합니다.&#x20;

<table><thead><tr><th width="355" align="center">매뉴얼 테스트 버전</th><th align="center">Java v2 S3 SDK 참고 링크</th></tr></thead><tbody><tr><td align="center"><p> AWS Java SDK version : 2.17.230</p><p>Java version : 11.0.11</p></td><td align="center"><p>문서 : <a href="https://docs.aws.amazon.com/ko_kr/sdk-for-java/latest/developer-guide/home.html">Developer guide - AWS SDK for Java 2.x</a></p><p>예제 : <a href="https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/s3/src/main/java/com/example/s3">Java v2 S3 examples</a></p></td></tr></tbody></table>







## 2. AWS Java v2 S3 SDK 설치하기

Maven 프로젝트의 pom.xml에 dependency를 추가합니다.

```shell
<dependency>
    <groupId>software.amazon.awssdk</groupId>
    <artifactId>bom</artifactId>
    <version>2.17.230</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```







## 3. 오브젝트 스토리지 API Key 확인하기

[\[오브젝트 스토리지 사용 방법\]](../use.md)을 참고하여 신청한 오브젝트 스토리지의 Access Key와 Secret Key를 확인합니다.

&#x20;





## 4. 자격 증명 프로필 설정하기

인증 파일을 생성하여 Access Key와 Secret Key를 등록합니다.

자세한 정보는 [<mark style="color:blue;">AWS SDK for Java v2 - Using Credentials</mark>](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/credentials.html)에서 확인할 수 있습니다.

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

사용하는 Java 및 SDK 버전에 따라 변경이 필요할 수 있습니다.&#x20;

해당 내용은 AWS Java SDK를 사용하여 카페24 클라우드의 오브젝트 스토리지를 이용하는 예제 코드로, 필요에 따라 응용할 수 있습니다.
{% endhint %}

### (1) 버킷 생성

버킷을 생성합니다.

```shell
package cafe24.demo;

import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.core.waiters.WaiterResponse;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.CreateBucketRequest;
import software.amazon.awssdk.services.s3.model.HeadBucketRequest;
import software.amazon.awssdk.services.s3.model.HeadBucketResponse;
import software.amazon.awssdk.services.s3.model.S3Exception;
import software.amazon.awssdk.services.s3.waiters.S3Waiter;
import java.net.URI;

public class bucket_create {
    public static void main(String[] args) {
        final String endpoint_url = "https://kr.cafe24obs.com";
        final String region_name = "kr1";
        String bucketName = "test-bucket";
        Region newRegion = Region.of(region_name);
        ProfileCredentialsProvider credentialsProvider = ProfileCredentialsProvider.create();

        S3Client s3Client = S3Client.builder()
            .region(newRegion)
            .endpointOverride(URI.create(endpoint_url))
            .credentialsProvider(credentialsProvider)
            .build();

        System.out.format("Creating a bucket named %s\n", bucketName);
            try {
                S3Waiter s3Waiter = s3Client.waiter();
                CreateBucketRequest bucketRequest = CreateBucketRequest.builder()
                    .bucket(bucketName)
                    .build();
    
                s3Client.createBucket(bucketRequest);
                HeadBucketRequest bucketRequestWait = HeadBucketRequest.builder()
                    .bucket(bucketName)
                    .build();
                
                // 버킷이 생성되기까지 대기
                WaiterResponse<HeadBucketResponse> waiterResponse = s3Waiter.waitUntilBucketExists(bucketRequestWait);
                waiterResponse.matched().response().ifPresent(System.out::println);
                System.out.println(bucketName +" has been created.");
            } catch (S3Exception e) {
                System.err.println(e.awsErrorDetails().errorMessage());
                System.exit(1);
            }
            s3Client.close();
    }
}
```

output 예시는 다음과 같습니다.

```shell
Bucket [test-bucket] has been created.
```





### (2) 버킷 삭제&#x20;

오브젝트가 모두 삭제된 빈 버킷에 대해서만 삭제가 가능합니다.

```shell
package cafe24.demo;

import java.net.URI;
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.DeleteBucketRequest;
import software.amazon.awssdk.services.s3.model.DeleteObjectRequest;
import software.amazon.awssdk.services.s3.model.ListObjectsV2Request;
import software.amazon.awssdk.services.s3.model.ListObjectsV2Response;
import software.amazon.awssdk.services.s3.model.S3Exception;
import software.amazon.awssdk.services.s3.model.S3Object;

public class bucket_delete {
    public static void main(String[] args) {
        final String endpoint_url = "https://kr.cafe24obs.com";
        final String region_name = "kr1";
        Region newRegion = Region.of(region_name);
        ProfileCredentialsProvider credentialsProvider = ProfileCredentialsProvider.create();
        String bucketName = "test-bucket";

        S3Client s3Client = S3Client.builder()
                .region(newRegion)
                .endpointOverride(URI.create(endpoint_url))
                .credentialsProvider(credentialsProvider)
                .build();
       
        try {
            // 버킷 삭제를 위해 버킷에 있는 모든 오브젝트 삭제
            ListObjectsV2Request listObjectsV2Request = ListObjectsV2Request.builder()
                    .bucket(bucketName)
                    .build();
            ListObjectsV2Response listObjectsV2Response;

            do {
                listObjectsV2Response = s3Client.listObjectsV2(listObjectsV2Request);
                for (S3Object s3Object : listObjectsV2Response.contents()) {
                    DeleteObjectRequest request = DeleteObjectRequest.builder()
                            .bucket(bucketName)
                            .key(s3Object.key())
                            .build();
                    s3Client.deleteObject(request);
                }
            } while (listObjectsV2Response.isTruncated());
            System.out.println("All objects in the bucket are deleted.");

            // 비어 있는 버킷 삭제
            DeleteBucketRequest deleteBucketRequest = DeleteBucketRequest.builder().bucket(bucketName).build();
            s3Client.deleteBucket(deleteBucketRequest);
            System.out.println("Bucket ["+bucketName+"] has been deleted.");
            
        } catch (S3Exception e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
}
```

output 예시는 다음과 같습니다.

```shell
All objects in the bucket are deleted.
Bucket [test-bucket] has been deleted.
```





### (3) 버킷 리스트 조회&#x20;

존재하는 모든 버킷을 조회합니다.&#x20;

```shell
package cafe24.demo;

import java.net.URI;
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.ListBucketsRequest;
import software.amazon.awssdk.services.s3.model.ListBucketsResponse;

public class bucket_list {
    public static void main(String[] args) {
        final String endpoint_url = "https://kr.cafe24obs.com";
        final String region_name = "kr1";
        Region newRegion = Region.of(region_name);
        ProfileCredentialsProvider credentialsProvider = ProfileCredentialsProvider.create();

        S3Client s3Client = S3Client.builder()
            .region(newRegion)
            .endpointOverride(URI.create(endpoint_url))
            .credentialsProvider(credentialsProvider)
            .build();
            
        System.out.println("--Existing buckets--");
        ListBucketsRequest listBucketsRequest = ListBucketsRequest.builder().build();
        ListBucketsResponse listBucketsResponse = s3Client.listBuckets(listBucketsRequest);
        listBucketsResponse.buckets().stream().forEach(x -> System.out.println("Name : " +x.name()+ " CreationDate : " + x.creationDate()));
        s3Client.close();
    }
}
```

output 예시는 다음과 같습니다.

```shell
--Existing buckets--
Name : test-bucket CreationDate : 2022-12-30T01:44:29.104Z
Name : test-bucket1 CreationDate : 2022-12-29T06:04:09.457Z
Name : test-bucket2 CreationDate : 2022-12-29T06:04:16.613Z
```





### (4) 오브젝트 업로드&#x20;

로컬에 있는 파일을 특정 버킷에 업로드합니다.

파일 업로드 시에 파일의 contentType은 기본으로 binary/octet-stream으로 지정됩니다.&#x20;

특정 타입으로 업로드 하기 위해서는 PutObjectRequest 객체에 contentType 정보를 담아야 합니다.

```shell
package cafe24.demo;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.net.URI;
import java.nio.file.Paths;
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.awscore.exception.AwsServiceException;
import software.amazon.awssdk.core.exception.SdkClientException;
import software.amazon.awssdk.core.sync.RequestBody;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.PutObjectRequest;
import software.amazon.awssdk.services.s3.model.PutObjectResponse;
import software.amazon.awssdk.services.s3.model.S3Exception;

public class object_upload {
    public static void main(String[] args) throws S3Exception, AwsServiceException, SdkClientException, IOException {
        final String endpoint_url = "https://kr.cafe24obs.com";
        final String region_name = "kr1";
        Region newRegion = Region.of(region_name);
        ProfileCredentialsProvider credentialsProvider = ProfileCredentialsProvider.create();
        String bucketName = "test-bucket";

        //업로드할 파일의 로컬 경로
        String filePath = "demo\\src\\main\\java\\cafe24\\demo\\files\\test-file.txt";
        String keyName = Paths.get(filePath).getFileName().toString();

        //업로드할 파일의 contentType 지정
        String contentType = "text/plain";
  
        S3Client s3Client = S3Client.builder()
                .region(newRegion)
                .endpointOverride(URI.create(endpoint_url))
                .credentialsProvider(credentialsProvider)
                .build();

        PutObjectRequest objectRequest = PutObjectRequest.builder()
                .bucket(bucketName)
                .contentType(contentType)
                .key(keyName)
                .build();

        PutObjectResponse response = s3Client.putObject(objectRequest, RequestBody.fromBytes(getObjectFile(filePath)));
        System.out.println(response);
        System.out.println("Object ["+keyName+"] has been uploaded to the bucket ["+bucketName+"].");
    }
    private static byte[] getObjectFile(String filePath) {
        FileInputStream fileInputStream = null;
        byte[] bytesArray = null;

        try {
            File file = new File(filePath);
            bytesArray = new byte[(int) file.length()];
            fileInputStream = new FileInputStream(file);
            fileInputStream.read(bytesArray);

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fileInputStream != null) {
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return bytesArray;
    }
}
```

output 예시는 다음과 같습니다.

```shell
PutObjectResponse(ETag="056f5ca5dbd6a49717737a6e4ede4f78")
Object [test-file.txt] has been uploaded to the bucket [test-bucket].
```





### (5) 오브젝트 다운로드&#x20;

버킷에 있는 파일을 로컬의 특정 경로로 다운로드합니다.&#x20;

다운받으려는 경로에 동일한 이름의 파일이 있으면 다운로드 실패합니다.

```shell
package cafe24.demo;

import java.io.IOException;
import java.net.URI;
import java.nio.file.Path;
import java.nio.file.Paths;
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.awscore.exception.AwsServiceException;
import software.amazon.awssdk.core.exception.SdkClientException;
import software.amazon.awssdk.core.sync.ResponseTransformer;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.GetObjectRequest;
import software.amazon.awssdk.services.s3.model.S3Exception;

public class object_download {
    public static void main(String[] args) throws S3Exception, AwsServiceException, SdkClientException, IOException {
        final String endpoint_url = "https://kr.cafe24obs.com";
        final String region_name = "kr1";
        Region newRegion = Region.of(region_name);
        ProfileCredentialsProvider credentialsProvider = ProfileCredentialsProvider.create();
        String bucketName = "test-bucket";
        String keyName = "test-file.txt";

        //오브젝트를 다운로드할 경로
        Path destination =  Paths.get("demo\\src\\main\\java\\cafe24\\demo\\files\\downloaded-file.txt");

        S3Client s3Client = S3Client.builder()
                .region(newRegion)
                .endpointOverride(URI.create(endpoint_url))
                .credentialsProvider(credentialsProvider)
                .build();

        GetObjectRequest getObjectRequest = GetObjectRequest.builder()
                .bucket(bucketName)
                .key(keyName)
                .build();

        s3Client.getObject(getObjectRequest, ResponseTransformer.toFile(destination));
        System.out.println("Object has been downloaded to local. \n Downloaded Path : ["+destination+"]");
    }
}
```

output 예시는 다음과 같습니다.

```shell
Object has been downloaded to local. 
 Downloaded Path : [demo\src\main\java\cafe24\demo\files\downloaded-file.txt]
```





### (6) 오브젝트 리스트 조회&#x20;

버킷에 있는 모든 파일과 폴더를 조회합니다.&#x20;

```shell
package cafe24.demo;

import java.net.URI;
import java.util.List;
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.ListObjectsRequest;
import software.amazon.awssdk.services.s3.model.ListObjectsResponse;
import software.amazon.awssdk.services.s3.model.S3Exception;
import software.amazon.awssdk.services.s3.model.S3Object;

public class object_get_list {
    public static void main(String[] args) {
        final String endpoint_url = "https://kr.cafe24obs.com";
        final String region_name = "kr1";
        String bucketName = "test-bucket";
        Region newRegion = Region.of(region_name);
        ProfileCredentialsProvider credentialsProvider = ProfileCredentialsProvider.create();

        S3Client s3Client = S3Client.builder()
                .region(newRegion)
                .endpointOverride(URI.create(endpoint_url))
                .credentialsProvider(credentialsProvider)
                .build();
                
        System.out.println("Get list of all objects in the bucket.");
        
        try {
            ListObjectsRequest listObjects = ListObjectsRequest
                    .builder()
                    .bucket(bucketName)
                    .build();

            ListObjectsResponse res = s3Client.listObjects(listObjects);
            List<S3Object> objects = res.contents();
            
            if (objects == null || objects.isEmpty()) {
                System.out.println("The bucket is empty.");
                return;
            }
            
            for (S3Object value : objects) {
                System.out.print("\n Name : " + value.key() + "\tSize : " + calKb(value.size()) + "KBs\tOwner ID : "
                        + value.owner().id());
            }
        } catch (S3Exception e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        s3Client.close();
    }
    // bytes를 kbs로 변환
    private static long calKb(Long val) {
        return val / 1024;
    }
}
```

output 예시는 다음과 같습니다.

```shell
Get list of all objects in the bucket.
 Name : folder/    Size : 0KBs    Owner ID : clouduser
 Name : folder/test-file-04    Size : 4KBs    Owner ID : clouduser
 Name : folder/test-file-05    Size : 4KBs    Owner ID : clouduser
 Name : folder/test-file-06    Size : 4KBs    Owner ID : clouduser
 Name : test-file-01    Size : 4KBs    Owner ID : clouduser
 Name : test-file-02    Size : 4KBs    Owner ID : clouduser
 Name : test-file-03    Size : 4KBs    Owner ID : clouduser
```





### (7) 오브젝트 삭제&#x20;

특정 버킷에 있는 파일을 삭제합니다.

```shell
package cafe24.demo;

import java.net.URI;
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.DeleteObjectRequest;

public class object_delete {
    public static void main(String[] args) {
        final String endpoint_url = "https://kr.cafe24obs.com";
        final String region_name = "kr1";
        Region newRegion = Region.of(region_name);
        ProfileCredentialsProvider credentialsProvider = ProfileCredentialsProvider.create();
        String bucketName = "test-bucket";
        String fileName = "test-file.txt";
        
        S3Client s3Client = S3Client.builder()
                .region(newRegion)
                .endpointOverride(URI.create(endpoint_url))
                .credentialsProvider(credentialsProvider)
                .build();
                
        DeleteObjectRequest deleteObjectRequest = DeleteObjectRequest.builder()
                .bucket(bucketName)
                .key(fileName)
                .build();

        s3Client.deleteObject(deleteObjectRequest);
        System.out.println("Object ["+fileName+"] has been deleted from the bucket.");
    }
}
```

output 예시는 다음과 같습니다.

```shell
Object [test-file.txt] has been deleted from the bucket.
```





### (8) 오브젝트 복사&#x20;

파일을 다른 버킷으로 복사합니다.

```shell
package cafe24.demo;

import java.net.URI;
import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.CopyObjectRequest;
import software.amazon.awssdk.services.s3.model.CopyObjectResponse;
import software.amazon.awssdk.services.s3.model.S3Exception;

public class object_copy {
    public static void main(String[] args) {
        final String endpoint_url = "https://kr.cafe24obs.com";
        final String region_name = "kr1";
        Region newRegion = Region.of(region_name);
        ProfileCredentialsProvider credentialsProvider = ProfileCredentialsProvider.create();

        // 복사하려는 오브젝트의 이름
        String objectKey = "bucket1-file.txt";
        // 복사하려는 오브젝트가 위치한 버킷
        String fromBucket = "test-bucket1";
        // 붙여넣기 할 버킷
        String toBucket = "test-bucket";

        S3Client s3Client = S3Client.builder()
        .region(newRegion)
        .endpointOverride(URI.create(endpoint_url))
        .credentialsProvider(credentialsProvider)
        .build();
        
        CopyObjectRequest copyReq = CopyObjectRequest.builder()
            .sourceKey(objectKey)
            .sourceBucket(fromBucket) 
            .destinationBucket(toBucket)
            .destinationKey(objectKey)
            .build();

        try {
            CopyObjectResponse copyRes = s3Client.copyObject(copyReq);
            System.out.println("Copying object ["+objectKey+"] from ["+fromBucket+"] to ["+toBucket+"] has been completed");
            System.out.println(copyRes.copyObjectResult().toString());        
        } catch (S3Exception e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
}
```

output 예시는 다음과 같습니다.

```shell
Copying object [bucket1-file.txt] from [test-bucket1] to [test-bucket] has been completed
CopyObjectResult(ETag=d9ebe4d6aeb33dea41ddb2b57e7b6d80, LastModified=2023-01-03T18:05:31.688Z)
```
