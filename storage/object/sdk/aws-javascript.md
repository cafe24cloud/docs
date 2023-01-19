---
description: '''AWS Javascript SDK를 사용하여 오브젝트 스토리지를 사용하는 방법은 다음과 같습니다.'
---

# AWS Javascript SDK 사용 방법

## 1. AWS Javascript v3 S3 SDK 사용하기

AWS Javascript v3 S3 SDK는 AWS에서 Javascript 코드를 통해 S3를 이용할 수 있도록 제공하는 도구입니다.&#x20;

카페24 클라우드의 오브젝트 스토리지는 S3 API와 호환이 되므로 해당 SDK 사용이 가능합니다.&#x20;

|                                                매뉴얼 테스트 버전                                                |                                                                                                                                          Javascript v3 S3 SDK 참고 링크                                                                                                                                         |
| :------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| <p>AWS Javascript SDK version : 3.231.0 </p><p>Node.js version : 14.17.1</p><p>Npm version : 6.14.13</p> | <p>문서 : <a href="https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html">Developer guide - AWS SDK for Javascript 3.x</a></p><p>예제 : <a href="https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/javascript_s3_code_examples.html">Javascript v3 S3 examples</a></p> |





## 2. Javascript v3 S3 SDK 설치

Javascript 개발을 위한 실행 환경 Node.js가 설치된 상태에서 진행합니다.&#x20;

npm 프로젝트 디렉터리로 이동한 다음, AWS Javascript v3 SDK를 설치합니다.

```shell-session
$ npm install @aws-sdk/client-s3
```





## 3. 카페24 클라우드 오브젝트 스토리지의 API Key 확인하기

****[**\[오브젝트 스토리지 사용 방법\]**](../use.md)을 참고하여 신청한 오브젝트 스토리지의 Access Key와 Secret Key를 확인합니다.

&#x20;



## 4. 자격 증명 프로필 설정

인증 파일을 생성하여 Access Key와 Secret Key를 등록합니다.

자세한 정보는 [AWS SDK for Java v2 - Using Credentials](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/credentials.html)에서 확인할 수 있습니다.

인증 파일의 기본 경로는 "\~/.aws/credentials"입니다.

```shell-session
$ cat >> ~/.aws/credentials << EOF
[default]
aws_access_key_id = [access_key]
aws_secret_access_key = [secret_key]
EOF
```





## 5. 코드 예제

