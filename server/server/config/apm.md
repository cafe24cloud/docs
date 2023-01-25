---
description: 가상서버에 APM를 구성하는 방법은 아래와 같습니다.
---

# APM 구성 방법

## 1. APM이란?

**APM**은 Apache + PHP + MySQL/MariaDB의 조합으로, 이 3가지가 연동되어 운영되도록 만든 환경을 APM이라고 합니다.

APM의 동작 원리는 아래와 같습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/10/04/507a5ed1dc64dcca681a6e10153c113d_1664846004.png" alt=""><figcaption></figcaption></figure>

* 클라이언트가 서버에 웹 페이지의 정보를 요청
* Apache는 해당되는 정보를 주기 위해 PHP에게 스크립트 실행을 요청
* PHP는 MySQL/MariaDB에 쿼리 질의를 요청
* MySQL/MariaDB은 질의에 대한 결과 데이터를 PHP에게 응답
* PHP는 받은 결과 데이터와 스크립트 실행 결과를 HTML 코드로 Apahce에게 응답
* Aapche는 받은 HTML 코드를 클라이언트의 웹 브라우저에 응답

이러한 과정을 거쳐 서버는 클라이언트에 정보를 주게 됩니다.

즉, Apache는 웹 서버, PHP는 해석기(웹 프로그래밍 언어), MySQL/MariaDB은 데이터베이스 서버의 역할을 합니다.

이제 APM을 설치해보도록 하겠습니다.





## 2. Apache 설치하기

#### (1) 패키지 업데이트하기

등록된 저장소 내 패키지 정보를 최신으로 업데이트합니다.

{% tabs %}
{% tab title="CentOS / Rocky" %}

{% endtab %}

{% tab title="Ubuntu" %}

{% endtab %}
{% endtabs %}



#### (2) Apache 설치하기

#### (3) Apache 설치 확인하기

## 3. PHP 설치하기

#### (1) PHP 설치하기

#### (2) PHP 설치 확인하기

#### (3) PHP 모듈 추가하기

## 4. MySQL 설치하기

#### (1) MySQL 설치하기

#### (2) MySQL 설치 확인하기

#### (3) MySQL 기본 보안 설정하기

#### (4) PHP와 연동 확인하기

## 5. MariaDB 설치하기

#### (1) MariaDB 설치하기

#### (2) MariaDB 설치 확인하기

#### (3) MariaDB 기본 보안 설정하기

#### (4) PHP와 연동 확인하기
