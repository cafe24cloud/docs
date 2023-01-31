---
description: 가상서버에서 Docker를 정상적으로 사용하기 위해서는 아래와 같은 설정이 필요합니다.
---

# Docker 구성 방법

## 1. Docker란?

<figure><img src="https://filesystem.cafe24.com/hosting/cloud_service/2021/03/12/36e485be398b33887ecefe6aa2688f3f_1615538493.PNG" alt=""><figcaption></figcaption></figure>

Docker는 가상화된 공간에서 애플리케이션을 개발, 제공, 실행할 수 있는 플랫폼입니다.

Docker Container를 사용하여 하나의 VM(host machine)위의 격리된 환경에서 애플리케이션을 관리할 수 있습니다.

Docker를 사용하면 경량화된 이미지로 컨테이너를 빠르게 배포할 수 있으며, 가상화된 공간을 사용할 때의 성능 또한 손실이 거의 없습니다.

Docker의 장점 및 특징은 다음과 같습니다.

* 애플리케이션의 개발과 배포가 편해집니다.
* 애플리케이션의 독립성과 확장성이 높아집니다.







## 2. Docker 설치하기

운영체제에 따른 Docker Engine 설치 방법은 다음과 같습니다.

* **CentOS** : [<mark style="color:blue;">Install Docker Engine on CentOS</mark>](https://docs.docker.com/engine/install/centos/#install-using-the-repository)<mark style="color:blue;"></mark>
* **Ubuntu**  : [<mark style="color:blue;">Install Docker Engine on Ubuntu</mark>](https://docs.docker.com/engine/install/ubuntu/)<mark style="color:blue;"></mark>







## 3. Docker MTU 설정하기

가상서버에 Docker를 설치하면 가상서버와 Docker의 MTU 수치가 달라 통신이 불가능합니다.

가상서버에서 MTU 설정 정보는 다음과 같이 확인할 수 있습니다.

가상서버에 설정된 MTU는 1450(eth0)이고, Docker 서비스가 사용하는 MTU는 1500(docker0)입니다.

```shell-session
$ ip a | grep 'eth0|docker0'
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.1.42/24 brd 192.168.1.255 scope global dynamic eth0
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    inet 172.17.0.1/16 scope global docker0
```

따라서 Docker에서 가상서버로 넘어오는 Packet에 Packet loss가 발생하여 통신 오류가 생기게 됩니다.

아래의 2가지 방법으로 해당 문제를 해결할 수 있으며, 리눅스 사용이 익숙하지 않은 사용자의 경우 좀 더 간단하게 해결할 수 있는 (1)번 방법을 권장합니다.

### (1) Host Network 사용 설정하기

docker run을 할 때 **--network** 옵션을 사용하여 해당 컨테이너가 Host Network를 사용하도록 설정합니다.

컨테이너가 Host Network를 사용하면 별도의 네트워크 인터페이스가 생성되지 않아 MTU 변경 없이 사용 가능합니다.

```shell-session
$ sudo docker run --rm -d --network host --name my_nginx nginx
```





### (2) Docker MTU 설정 변경하기

#### a. Docker MTU를 1450으로 변경하기

```shell-session
$ cat << EOF | sudo tee /etc/docker/daemon.json
{
  "mtu": 1450
}
EOF
```



#### b. Docker 데몬 재시작하기

```shell-session
$ sudo systemctl restart docker
```



#### c. 변경된 MTU 확인하기

```shell-session
$ sudo docker network inspect bridge | grep mtu
            "com.docker.network.driver.mtu": "1450"
```



#### d. Docker  Container의 정상 동작 확인하기

테스트 용으로 nginx 컨테이너 하나를 생성합니다.

```shell-session
$ sudo docker run --rm -d --name my_nginx nginx
```

생성한 my\_nginx 컨테이너의 shell에 접속하고, curl로 외부 web의 정보를 정상적으로 가져오는지 확인합니다.

```shell-session
$ sudo docker exec -it my_nginx bash

root@2a17da973db2:/# curl --head https://hosting.cafe24.com/
HTTP/2 200
server: NWS
content-type: text/html; charset=UTF-8
cache-control: no-cache, no-store, must-revalidate
pragma: no-cache
p3p: CP="CAO DSP CURa ADMa TAIa PSAa OUR LAW STP PHY ONL UNI PUR FIN COM NAV INT DEM STA PRE"
x-frame-options: DENY
x-xss-protection: 1; mode=block
strict-transport-security: max-age=63072000; includeSubdomains
referrer-policy: unsafe-url
date: Mon, 15 Mar 2021 04:27:13 GMT
content-length: 0
```
