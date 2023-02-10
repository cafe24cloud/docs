---
description: 오브젝트 스토리지에 CORS 설정을 하는 방법은 아래와 같습니다.
---

# 오브젝트 스토리지 CORS 설정 방법

## 1. CORS란?

CORS(Cross-Origin Resource Sharing)는 한 도메인에 로드된 클라이언트 웹 응용 프로그램이 다른 도메인의 리소스와 상호 작용하는 방법을 정의합니다.

CORS 기능을 사용하면, 웹 브라우저가 외부 웹 사이트나 서비스의 요청을 수행할 수 있는 도메인을 설정할 수 있도록 합니다.

<figure><img src="../../.gitbook/assets/cors.png" alt=""><figcaption></figcaption></figure>

예를 들어, 브라우저 스크립트가 다른 도메인의 리소스에 대해 GET 요청을 할 경우 다음과 같이 동작합니다.

① GET 요청을 보내기 전에 cross-origin 도메인의 리소스에대한 OPTIONS 액세스 요청을 사전에 전송합니다.

② cross-origin 도메인은 요청한 리소스에 대해 origin 도메인이 수행 가능한 HTTP 요청 유형을 회신합니다.

③ 실제로 GET 요청을 전송합니다.

④ 승인된 도메인에서 전송되었기 때문에 요청한 리소스를 반환하여 응답합니다.







## 2. S3cmd 연동하기







## 3. CORS 환경 구성하기

## 4. CORS 설정하기
