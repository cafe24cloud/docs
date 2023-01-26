---
description: S3cmd를 이용하여 카페24 클라우드의 오브젝트 스토리지를 사용하는 방법을 설명합니다.
---

# 오브젝트 스토리지 S3cmd 사용 방법

### 1. S3cmd란?

1. S3cmd는 CLI(Command-line interface) 환경에서 AWS의 S3를 제어할 수 있는 무료 명령어 도구입니다.&#x20;
2. 카페24클라우드의 오브젝트 스토리지는 AWS S3와 동일한 API를 사용하기 때문에 S3cmd를 사용할 수 있습니다.&#x20;
3. 오브젝트 스토리지 CLI tool로는 S3cmd 외에도 [aws s3](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-commands.html), [s3api](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-apicommands.html)가 존재합니다.
   1. &#x20;
4. **- s3cmd 사용법 공식 문서** : [S3cmd Usage](https://s3tools.org/usage)
5. &#x20;
6. **- 오브젝트 스토리지 사용 방법** : [오브젝트 스토리지는 어떻게 사용하나요?](https://console.cafe24.com/support/faq/view?idx=437)
7. &#x20;
8. **- 오브젝트 스토리지 API 사용 시 용량 정책** :
   1. &#x20;1\. 베타 서비스 기간 동안 제공하는 오브젝트 스토리지의 최대 저장 공간은 30GB입니다.
   2. &#x20;2\. s3cmd, s3 browser 등으로 API를 통해 오브젝트 스토리지를 이용할 경우 1회 업로드 용량에 대한 세부 제한은 없으며,
   3. &#x20;  최대 저장 공간 30GB에 대해서만 제한합니다.
   4. &#x20;

* &#x20;
* &#x20;

### 2. S3cmd 연동하기

1. #### (1) s3cmd 설치
   1. 오브젝트 스토리지에 접근할 가상서버에 다음과 같이 s3cmd를 설치합니다.







