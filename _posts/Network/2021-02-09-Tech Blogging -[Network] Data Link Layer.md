---
layout: post
title: "[Network] OSI 2계층 - 데이터링크 계층"
subtitle: "Ethernet Protocol, MAC, ARP"
background: '/img/bg_technology.jpg'
categories: technology-network

---

## [Network] 데이터링크 계층 (2계층)

> 데이터링크 계층은 '같은 네트워크' 의 '네트워크 장비'들 사이에서 신호를 주고받는 규칙을 정하며, 가장 많이 통용되는 규칙은 Ethernet Protocol 이다.

<br>

데이터링크 계층의 기능은 두 가지 키워드로 정리할 수 있다.

- **흐름제어** : 어떤 장비에서 어떤 장비로 데이터를 전송할지 제어 --> **이더넷 헤더**
- **오류제어** : 전송하려는 데이터에 오류는 없는지 확인, 제어 --> **이더넷 트레일러**

또한 데이터링크 계층은 **같은 네트워크(LAN)** 에서의 데이터 전달을 정의해주는 계층이므로,

**다른 네트워크(WAN)**로의 데이터 전달을 위해서는 **3계층(네트워크 계층)**의 도움이 필요하다.

<br>

2계층에서 사용하는 주소는 **'물리적 주소'**로 물리계층에서 살펴본 **MAC주소**를 의미한다.

MAC주소는 제조사번호 + 고유번호로 총 **48비트**로 구성되어있으며, 

1바이트는 8비트이므로, MAC주소는 **6바이트**로 구성되어 있다고도 볼 수 있다.

<br>

2계층에서 데이터(payload)에 **이더넷 헤더**와 **트레일러**가 추가된 데이터를 **Frame**이라 하며, 아래와 같이 구성된다.

![image](https://1.bp.blogspot.com/-KnkpRMiJ17Y/WBNl6umivlI/AAAAAAAABw0/kZUcs1_tEJ0XjvIOElJ-P9ypzuYJRa8aQCLcB/s640/header-trailer.png)

이더넷 프로토콜의 헤더는 총 **14바이트**로 구성되어 있다.

- 목적지 MAC주소 : **6바이트**
- 출발지 MAC주소 : **6바이트**
- 유형 : **2바이트**

여기서 '유형'이란, **상위 계층(3계층 - 네트워크 계층)의 프로토콜 종류**를 의미하며, 

**IPv4의 경우 16진수 번호 0800, IPv6의 경우 86DD, ARP의 경우 0806 이 들어간다**

이더넷 프로토콜의 입장에서는 현재 페이로드 내부의 3계층 프로토콜의 내용을 모르기 때문에,

3계층 프로토콜을 미리 알려주는 것이다.

<br>

이더넷 프로토콜의 트레일러는 **오류검출**을 위해 사용되는데, 

데이터의 손실, 손상이 발생하였는지를 알아내기 위해 bit의 개수를 짝수개로 맟춰주는 역할을 한다고 한다...(자세한 내용은 교재에서 다루지 않아서 모르겠다.)

<br>

#### ARP (Address Resolution Protocol)프로토콜이란 ? 

2계층에서 데이터를 전송하는데 전송지의 MAC주소를 모른다면, 

지정된 IP주소를 통해 목적지 컴퓨터의 MAC주소를 알아낼 수 있다. 

이처럼 **IP주소를 통해 목적지 컴퓨터의 MAC주소를 알아내는 3계층 프로토콜을 ARP라 한다.**

목적지 MAC주소를 모르는 출발지 컴퓨터는 네트워크에 **'ARP 브로드캐스트'**를 보내고, 

지정된 IP주소의 장비만 응답으로 본인의 MAC주소를 보낸다(ARP reply). 

Reply를 받으면, 출발지 컴퓨터는 응답 컴퓨터의 MAC주소와 IP가 맵핑된 **ARP Table**을 메모리에 올린다.

이후의 데이터 통신은 이 ARP Table의 정보를 통해 실행되는데,

목적지 컴퓨터의 IP주소가 바뀌면 이 정보도 변경되어야 하므로, ARP Table은 캐시로 지정되어 일정 시간이 지나면 삭제하고 다시 ARP request를 보낸다.

현재 윈도우 운영체제 컴퓨터의 ARP Table을 확인하려면 **arp -a** 명령어를 쳐보면 된다.

<br>

![image](https://github.com/Sol-cito/sol-cito.github.io/blob/master/img/posts/2021-02-09-Tech%20Blogging/ARP%20Table.png?raw=true)

ARP Table의 결과인데, 인터넷 주소(IP)와 물리적 주소(MAC address)가 맵핑되어 있음을 확인할 수 있다.

저 테이블의 첫 번째 IP주소는 현재 사용하고있는 공유기 IP인데, 

인터넷 공유기는 **고정IP가 아닌 유동IP를 사용하므로 유형이 '동적'이다.**

IP에 관한 내용은 네트워크 계층에서 더 자세히 다루어보도록 하자.

<br>

#### Wireshark 패킷 캡쳐로 Ethernet header 확인해보기

ping 으로 구글 (8.8.8.8)에 패킷을 보내고, 와이어샤크로 이더넷 패킷캡쳐를 해보자.

> ping은 ICMP(Intefnet Control Message Protocol) 프로토콜로, 목적지에 패킷을 보내고 응답이 오는지를 검사하는 역할을 한다.

<br>

![image](https://github.com/Sol-cito/sol-cito.github.io/blob/master/img/posts/2021-02-09-Tech%20Blogging/Ping%20cmd.png?raw=true)

위와 같이 ping을 8.8.8.8에 쐈고, **ping은 기본적으로 4번 보내는 것으로 세팅**이 되어있기에 4번의 응답이 표시된다.

와이어샤크에서 ICMP로 필터링을 하면 해당 패킷을 볼 수 있다.

<br>

![image](https://github.com/Sol-cito/sol-cito.github.io/blob/master/img/posts/2021-02-09-Tech%20Blogging/wireshark%20capture.png?raw=true)

보다시피 ping으로 **4번의 request, 4번의 reply**가 이루어졌으며,

사용된 프로토콜은 아래에서부터 각각 ICMP, IPV4, Ethernet 으로 이루어진 것을 볼 수 있다.

자, 여기서 Ethernet 부분만 보면,

![image](https://github.com/Sol-cito/sol-cito.github.io/blob/master/img/posts/2021-02-09-Tech%20Blogging/wireshark%20ethernet.png?raw=true)

위와 같이 16진수가 총 14개가 보인다.

첫 6개는 목적지의 MAC주소, 두 번째 6개는 내 컴퓨터의 MAC주소, 

나머지 2개는 상위 프로토콜의 유형이다.

Ping 패킷을 구글로 쏘면, 일단 데이터가 **내가 현재 쓰고있는 Wifi 공유기**로 향해야 한다.

구글 서버는 다른 네트워크에 있으므로, 일단 데이터링크 계층에서는 

**같은 네트워크에 위치한 다른 네트워크 장비인 인터넷 공유기**로 데이터를 보내야 하기 때문이다.

위 ARP테이블에서 확인했듯이, 현재 내 공유기의 물리적 주소는 **C8-3A-35-07-EC-10** 이며,

ipconfig -all 로 확인한 내 노트북 LAN카드의 MAC주소는 **10-63-C8-51-BB-FB**이다.

위 Ethernet 패킷에 나온 첫 12개 비트와 일치함을 확인할 수 있다.

유형은 **0800**인데, 이는 ping의 상위 네트워크 계층의 프로토콜이 IPv4임을 의미한다.

<br>

여기까지가 데이터링크 계층의 내용이다.

다음 포스팅에서는 드디어! IP주소를 통한 네트워크끼리의 통신을 제어하는 **3계층 - 네트워크 계층**에 대해 알아보도록 하자.