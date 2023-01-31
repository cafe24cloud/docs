---
description: 오토스케일 사용 방법은 아래와 같습니다.
---

# 오토스케일 사용 방법

## 1. 오토스케일이란?

오토스케일(AutoScale)은 규칙에 따라 리소스를 자동으로 증가 및 감소시키는 기능입니다.

가상서버의 부하를 모니터링하여 운영자의 개입없이 탄력적인 인프라 운영을 할 수 있습니다.







## 2. 사전 작업하기

### (1) 가상서버 스냅샷 생성하기

<mark style="background-color:blue;">콘솔 > 서버 > 가상서버 스냅샷</mark>

오토스케일에 의해 자동으로 생성될 가상서버들의 기본 이미지를 생성합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/28/b2c3b98db070325285f512b3fa98ac6a_1601273169.jpg" alt=""><figcaption></figcaption></figure>





### (2) 로드밸런서 생성하기

<mark style="background-color:blue;">콘솔 > 네트워킹 > 로드밸런서</mark>

오토스케일에 사용할 연결된 가상서버가 없는 로드밸런서를 생성합니다.

[<mark style="color:blue;">\[로드밸런서 생성 방법\]</mark>](../../network/loadbalancer/create.md)을 참고해 주세요.

오토스케일로 생성된 가상서버들은 해당 로드밸런서의 멤버가 됩니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/28/8b7ec3778245eca927a933802618da5e_1601273516.jpg" alt=""><figcaption></figcaption></figure>







## 3. 오토스케일 생성하기

<mark style="background-color:blue;">콘솔 > 서버 > 오토스케일</mark>

오토스케일 생성 과정은 다음과 같습니다.

### (1) 이미지 선택하기

오토스케일로 생성될 가상서버들의 기본 이미지를 선택합니다.

이미지에 블록 스토리지 스냅샷이 연결되어 있다면 해당 블록 스토리지도 함께 생성됩니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/23/7b275878a6690e9b318f246534b576b2_1600841060.jpg" alt=""><figcaption></figcaption></figure>





### (2) 로드밸런서 선택하기

2-(2)번에서 생성한 연결된 가상서버가 없는 로드밸런서를 선택합니다.

로드밸런서에 연결된 가상서버의 동작을 모니터링하게 됩니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/24/f7739a4c77047a21cad8667c5d913dd3_1600932019.jpg" alt=""><figcaption></figcaption></figure>





### (3) 방화벽 선택하기

오토스케일에 적용할 방화벽을 선택합니다. [<mark style="color:blue;">\[방화벽 설정 방법\]</mark>](../../security/security/config.md)을 참고해 주세요.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/06/15/1b380a15fb77fa157e4d3f9d5550c5ab_1655251930.jpg" alt=""><figcaption></figcaption></figure>





### (4) 서버 및 기준 설정하기

키페어와 오토스케일의 이름을 입력한 후, 아래의 서버 및 기준 설정 값을 입력합니다.

① **하드웨어 사양**

오토스케일로 만들어질 가상서버의 하드웨어 사양을 선택합니다.

선택된 하드웨어 사양의 가상서버가 생성되며, 사용한 만큼 시간당 과금됩니다.

② **최대 서버수**

<mark style="color:blue;">최대 서버수는 20대까지 제공하고 있으며, 증축이 필요하신 경우 1:1 문의를 부탁드립니다.</mark>

③ **쿨다운**

쿨다운 시간은 오토스케일링으로 인한 부하 분산 효과가 나타나기 전에, 가상서버를 추가하거나 감축하지 않도록 하는 시간입니다.

따라서 가상서버 생성 후 자체적인 준비 시간 없이 로드밸런서에 연결되어 발생하는 에러를 방지합니다.

쿨다운 시간은 5분 \~ 60분 사이로 설정 가능합니다.

부하에 따른 오토스케일링의 반응 속도를 빠르게 하고 싶을 경우, 해당 시간을 낮추는 것으로 해결할 수 있습니다.

④ **증가 기준 / 감소 기준**

CPU와 RAM의 경우, 증가 기준과 감소 기준을 정하여 오토스케일링을 할 수 있습니다.

예를 들어 CPU의 증가 기준을 80%로 설정하면, CPU의 부하가 80%에 도달했을 때 서버 scale-out을 하게 됩니다.

같은 원리로 CPU의 감소 기준을 40%로 설정하면, CPU의 부하가 40%에 도달했을 때 서버 scale-in을 하게 됩니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/23/f97038aed54d7e0f58431677aa878f3e_1600842204.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
<mark style="color:red;">**주의사항**</mark>

오토스케일은 가상서버의 OS에서 측정되는 CPU, Memory 사용률을 기반으로 동작하지 않습니다.

카페24 클라우드의 인프라 구조에 따라 좀 더 정확한 방식으로 Host 서버에서의 해당 가상서버의 사용률을 측정하며, 이는 top 명령어 등으로 조회하는 수치와 차이가 있습니다.
{% endhint %}





### (5) 자동 스크립트 작성하기

오토스케일로 생성된 가상서버에 적용할 외부 스크립트를 입력합니다.

해당 부분은 선택사항이며, [<mark style="color:blue;">\[자동 스크립트 적용 방법\]</mark>](../server/use/script.md)을 참고해 주세요.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2022/08/31/30d9e07e5dba6a4cdf00a94ea41c09ae_1661904499.png" alt=""><figcaption></figcaption></figure>





### (6) 오토스케일 생성 확인하기

#### a. 상세 정보

생성된 오토스케일의 상세 정보를 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/24/17af20af7329dc10673f59d94494f433_1600932187.jpg" alt=""><figcaption></figcaption></figure>



#### b. 현재 서버 현황

오토스케일 설정 값에 따라 생성된 서버들을 확인할 수 있습니다.

오토스케일 생성 직후 서버는 1대가 생성되며, 이후 오토스케일의 설정 값에 따라 서버가 자동으로 증가 및 감소합니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/24/561fe6ea854a6976145240490faaa26b_1600932251.jpg" alt=""><figcaption></figcaption></figure>



#### c. 서버 사용 내역

오토스케일의 설정  값에 따라 삭제된 가상서버의 내역을 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/10/07/8a38da42b7e7ee46304617128303abc2_1602028988.jpg" alt=""><figcaption></figcaption></figure>







## 4. 가상서버 접속하기

오토스케일로 생성된 가상서버는 사설IP만 할당받기 때문에 스냅샷 생성 대상 가상서버에서 SSH 접속을 해야 합니다.

<mark style="color:red;">부하 모니터링에 따라 가상서버가 무작위로 자동 삭제될 수 있기 때문에 오토스케일로 생성된 가상서버에서 직접 작업하는 것을 지양합니다.</mark>

오토스케일로 증설된 가상서버에 대한 수정 사항은 스냅샷 생성 대상 가상서버를 통해 배포해 주세요.

### (1) 스냅샷 생성 대상 가상서버에 접속하기

스냅샷 생성 대상 가상서버의 공인IP로 SSH 접속을 합니다.

해당 가상서버의 정보는 오토스케일 설정 보기에서 확인할 수 있습니다.

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2020/09/30/839288e08e4109ef64b7aebb86a006f9_1601447181.jpg" alt=""><figcaption></figcaption></figure>

예를 들어, 사용된 demoServer1 가상서버의 공인 ip가 211.183.1.15일 경우, ssh 접속 방법은 다음과 같습니다.

해당 서버 정보에 맞게 입력해 주세요.

```shell
$ ssh -i keypair.pem centos@211.183.1.15
```





### (2) 오토스케일로 생성된 가상서버 접속하기







## 5. 오토스케일 테스트하기 (선택)

