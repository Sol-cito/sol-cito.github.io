---
layout: post
title: "[Network] OSI 3계층 - 네트워크 계층"
subtitle: "IP, Subnet, Router "
background: '/img/bg_technology.jpg'
categories: technology-network
---

## [Network] 네트워크 계층 (3계층)

> 네트워크 계층은 서로 다른 네트워크끼리 정보를 주고받을 수 있도록 하는 계층이다.

<br>

2계층에서는 같은 네트워크 내 다른 물리장비로 데이터를 보낼 때 **MAC주소**를 사용했었다.

그러나, 다른 네트워크로 데이터를 보낼 때는 다른 정보가 필요한데, 그것이 바로 **IP주소**이다.

IP주소는 **Internet Protocol address**의 약자로, **어떤 네트워크의 어떤 컴퓨터인지 구분**할 수 있도록 하는 주소다.

다른 네트워크로 데이터를 보내려면 ip주소로 목적지를 지정하고, **어떤 경로로 보낼지**를 결정해야 한다.

이러한 과정을 **라우팅(Routing)**이라 하며, 라우팅을 하는 장비를 **라우터(Rounter)**라고 한다.

라우터는 **라우팅 테이블**이 있어 경로정보를 여기에 등록하고 관리한다.

<br>

#### IP

IP에 대해서 좀 더 자세히 알아보자.

IP는 네트워크 계층의 하나의 **프로토콜**이다(Internet Protocol address 라는 이름만 봐도 알 수 있다).

프로토콜이므로, 데이터링크 계층의 이더넷 프로토콜과 마찬가지로 타겟 데이터에 **IP 헤더**를 붙인다.

IP헤더에는 '출발지 IP주소 / 도착지 IP주소 / 헤더 길이 / 서비스 유형'...등의 정보가 들어가며,

이렇게 IP헤더가 붙어 캡슐화된 것을 **IP패킷**이라 한다(2계층에서는 **프레임**이라고 했었다).

IP주소는 기본적으로 **인터넷 서비스 제공자(통신사)**가 부여하는데, 

서비스 제공자가 부여하는 주소는 **공인 IP주소**이다.

공인 IP가 부여되면 **라우터에 할당되고, **해당 네트워크 LAN 내 컴퓨터들에는 **사설 IP주소**가 할당된다.

즉, **공인 IP주소 하나로 LAN 내부 컴퓨터들이 모두 인터넷 사용이 가능하도록** 만들 수 있다.

IP는 기존엔 IPv4를 사용하였으나, **IPv4는 32비트**로 되어있어서 2^32, 약 43억개의 주소 부여가 가능했다.

그러나 43억개는 턱없이 부족하기 때문에, 현재는 **128비트로 된 IPv6**를 혼용해서 사용하고 있다.

(2011년에 32비트 공인 IP주소가 모두 고갈되었다고 함.)

32비트 IP주소(IPv4)는 아래와 같이 **8비트씩 구분(octet이라 함)**되어 구성된다.

```c
11000000 / 10101000 / 00000001 / 00001010
```

각 옥텟을 10진수로 변환하면

```c
192 / 168 / 1 / 10
```

이 된다. 우리는 여기서 한 옥텟의 10진수 범위가 **0~255**까지임을 알 수 있는데, 2^8 = 256이기 때문이다.

IP주소는 **네트워크 ID / 호스트 ID**의 두 Partition으로 분리가 가능하며,

이 Partion의 각 크기에 따라 **네트워크 클래스**가 결정된다.

**네트워크ID**란 **"어떤 네트워크인지"**를 나타내는 고유값이며, 

**호스트ID**는 **"해당 네트워크의 어느 컴퓨터인지"**를 나타내는 고유값이다.

공인 IP의 네트워크 클래스의 종류 및 Partion의 크기는 아래와 같다.

![image](https://www.tech-faq.com/wp-content/uploads/IP-Address-Classes.jpg)

(출처 : https://www.tech-faq.com/ip-address-classes.html)

- **A클래스** : 대규모 네트워크 - 8비트 네트워크ID / 24비트 호스트ID
- **B클래스** : 중형 네트워크 - 16비트 네트워크ID / 16비트 호스트ID
- **C클래스** : 소형 네트워크 - 24비트 네트워크ID / 8비트 호스트ID
- **D클래스** : 멀티캐스트 주소
- **E클래스** : 연구 및 특수용도 주소

즉 클래스 별로 공인IP주소의 범위가 다르며, 그에 따른 사설IP주소의 범위도 다르다.

일반 가정집에서는 보통 C클래스의 IPv4를 사용하므로, 위 표 **First Octet Range**에 나와있는 것 처럼

192~223 사이의 주소로 시작하는 범위를 사용한다 (일반적으로는 192.168.xxx.xxx 임).

이 IP주소에는 **네트워크 주소**와 **브로드캐스트 주소**가 존재한다.

- 네트워크 주소 : 전체 네트워크에서 작은 네트워크를 식별하는데 사용되는 주소로, **호스트ID가 10진수로 0 이면 그 네트워크 전체를 대표하는 주소가 됨**. 즉, **전체 내트워크의 대표 주소**.
  - Ex) 192.168.1.0 네트워크에 속한 컴퓨터들의 네트워크 주소는 192.168.1.1 / 192.168.1.2 /... 등이 있다.
- 브로드캐스트 주소 : 한 네트워크 안의 **전체 컴퓨터에 한 번에 데이터를 전송하는데 사용되는 IP주소**
  - Ex) 192.168.1.255 의 주소로 데이터를 전송하면 192.168.1.0 네트워크 내에 위치한 모든 컴퓨터에 데이터 수신 가능.

이 두 주소는 특별한 주소로 **컴퓨터나 라우터가 본인의 IP로 할당하지 못한다.**

<br>

#### Subnet, Subnet Mask

Subneting이란 글자 그대로 **한 network를 '분할'하여 하위 network(subnet)들을 만드는 것**을 의미한다.

왜냐하면 하나의 C클래스만 해도 호스트ID가 254개(256 - 네트워크주소 + 브로드캐스트 주소)이고, A클래스와 B클래스는

그 개수가 각각 16M, 64K개로 엄청나게 많다.

이러한 상황에서는 많은 수의 컴퓨터가 한꺼번에 브로드캐스트 패킷을 전송하거나 하면 네트워크가 혼잡해진다.

혹은, C클래스의 IP를 50개 정도만 사용하고 있다면 204개는 사용하지 않으므로 낭비가 발생하므로,

네트워크를 적절히 나누어 효율적으로 관리하고 싶을 것이다.

즉, **한 네트워크를 다시 여러개로 분할하고 싶을 때** 사용하는 것이 Subneting 이다.

(**브로드캐스트 도메인**을 나누는 것)

Subnet을 나누기 위해 **기존의 호스트ID에서 비트를 빌려 서브넷ID를 구성한다.**

하나의 클래스를 subneting하면 아래와 같다.

![image](https://upload.wikimedia.org/wikipedia/commons/1/14/Subnetting_Concept-en.svg)

(출처 : https://en.wikipedia.org/wiki/Subnetwork)

서브넷 ID로 네트워크를 subnet으로 나누었을 때, 

나누어진 네트워크에서 **네트워크ID를 식별**하는 데 사용하는 것이 **Subnet Mask**이다.

Subnet Mask는 0과 1의 비트로 이루어져 있는데, 특정 클래스의 IP주소와 **AND연산**을 하면 그 IP주소의 **네트워크ID**를 알 수 있다.

예를 들어, C클래스의 IP주소가 192.168.1.0 이라면, 기본 서브넷마스크는 255.255.255.0 이 된다.

두 주소를 2진수로 바꾸면

```c
ip주소 : 11000000 / 10101000 / 00000001 / 00000000 -> 192.168.1.0
Subnet : 11111111 / 11111111 / 11111111 / 00000000 -> 255.255.255.0
```

이고, 각 비트의 And연산 결과는 192.168.1.0 이므로, 해당 subnet의 네트워크 주소는 192.168.1.0임을 알 수 있다.

(AND연산에서는 두 비트 중 하나라도 0이면 0이 반환된다)

자, 그럼 서브네팅 된 IP주소가 207.46.230.0 이고, 서브넷 마스크가 255.255.255.0, 서브넷팅 결과가 255.255.255.224일 때,

우선 **서브넷 마스크 비트**는 서브넷팅 결과의 맨 끝자리 224를 2진수로 변환한 결과 **11100000**의 128 + 64 + 32 = 224 이므로

**3비트**임을 알 수 있다. 호스트ID에서 빌린 비트가 총 3비트이므로 Subnet ID는 3비트이다.

즉, **총 서브넷 개수는 2^3 = 8개**이며, 

**하나의 서브넷 당 가능한 호스트 개수는 총 2^(8 - 3) = 32개**임을 알 수 있다.

각 서브넷에서 네트워크ID와 브로드캐스트ID 는 사용할 수 없으므로,

**실제 할당 가능한 호스트 개수는 32-2 = 30개**임을 알 수 있다.

<br>

#### 라우터(Router)

서로 다른 네트워크끼리의 통신은 라우터가 관장한다.

한 컴퓨터 A가 다른 네트워크에 데이터를 전송하려면 우선 **라우터의 IP주소**를 설정해야 하고,

이 라우터의 IP를 **기본 게이트웨이(default gateway)**라 한다.

컴퓨터A가 보낸 데이터는 우선 기본 게이트웨이를 따라 라우터이 도착하고,

라우터는 **라우팅 테이블**을 참고하여 다른 네트워크의 IP주소로 이 데이터를 보낸다.

라우팅 테이블은 **네트워크 간 통신을 위핸 최적 경로 정보**가 담긴 테이블로, **직접설정/수동설정**의 방법이 있다.

<br>

---

여기까지가 네트워크 계층에 대한 내용으로,

우리는 여태까지 물리계층, 데이터링크계층, 네트워크 계층에서 데이터가 어떻게 캡슐화되어 전송되는지를 살펴보았다.

그런데, 앞의 세 계층은 전송 데이터가 손실되었는지, 목적지에 잘 도착했는지 등의 **유효성에 대해서는 아무 관심이 없다.**

이 다음 계층인 4계층 - 전송계층은 **데이터의 신뢰성, 유효성을 검사하고 최종 목적 Application을 식별**하는 역할을 한다.

다음 포스팅에서 **OSI Network 4계층 - 전송계층**에 대해 살펴보자.

<br>