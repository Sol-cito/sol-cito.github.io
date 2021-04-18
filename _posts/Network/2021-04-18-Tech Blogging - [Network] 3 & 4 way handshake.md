---
layout: post
title: "[Network] 3 & 4 way handshake"
subtitle: "Trustworthy communication"
background: '/img/bg_technology.jpg'
categories: technology-network
---

전송계층의 프로토콜 종류는 TCP, UDP 2가지가 있다.

TCP는 통신 노드들 사이의 신뢰있는 통신을 보장하는데, **신뢰**를 보장하기 위해서는 기술적 방법이 필요하다. 

*"내가 너를 믿을 수 있니? - 응 믿을 수 있어"* 와 같은 대화를 통해 신뢰를 보장하는 것이다 (말이 좀 웃기지만..)

TCP에서 첫 연결을 할 때 신뢰를 보장하는 기술이 **3 way handshake**,

연결을 종료할 때 하는 것이 **4 way handshake**이다.

---

#### 3 Way handshake

TCP헤더에는 아래와 같은 6비트의 **코드비트**가 존재한다.

| URG   | ACK   | PSH   | RST   | SYN   | FIN   |
| ----- | ----- | ----- | ----- | ----- | ----- |
| 1비트 | 1비트 | 1비트 | 1비트 | 1비트 | 1비트 |

우리가 살펴봐야 할 비트는 **ACK, SYN** 두 비트이다. 이 두 비트로 3 way handshake가 일어나기 때문이다.

- **SYN** : Syncronization, 즉 상대방과의 '연결 동기화'를 요청하는 비트이다.
- **ACK** : Acknowledgement, 즉 상대방과의 연결을 '승인'하는 비트이다.

이 두 비트로 이루어지는 3 way handshake의 절차는 아래와 같다.

![image](https://ars.els-cdn.com/content/image/3-s2.0-B9780128024379000059-f05-08-9780128024379.jpg)

(출처 : [https://www.sciencedirect.com/topics/computer-science/three-way-handshake](https://www.sciencedirect.com/topics/computer-science/three-way-handshake))

<br/>

(1) 연결을 요청하는 클라이언트가 **SYN비트를 1로 활성화**하고 TCP헤드에 담아 연결 요청을 보낸다.

| URG  | ACK  | PSH  | RST  | SYN   | FIN  |
| ---- | ---- | ---- | ---- | ----- | ---- |
| 0    | 0    | 0    | 0    | **1** | 0    |

(2) 서버는 연결을 승인함(**ACK**)과 동시에 마찬가지로 클라이언트와의 연결 동기화를 요청(**SYN**)한다.

| URG  | ACK   | PSH  | RST  | SYN   | FIN  |
| ---- | ----- | ---- | ---- | ----- | ---- |
| 0    | **1** | 0    | 0    | **1** | 0    |

(3) 클라이언트 또한 연결을 승인(**ACK**)한다.

| URG  | ACK   | PSH  | RST  | SYN  | FIN  |
| ---- | ----- | ---- | ---- | ---- | ---- |
| 0    | **1** | 0    | 0    | 0    | 0    |

이렇게 세 번 통신하게 되므로, 이 과정을 **3 way handshake**라고 부르는 것이다.

<br/>

#### 4 way handshake

연결된 통신을 종료하기 위해서는 4번의 과정이 필요하고, 이를 4 way handshake라 부른다.

4 way handshake에서 역할을 하는 비트는 **SYN, ACK, FIN**이다.

**FIN**은 말 그대로 '연결 종료 요청 비트'이다. 아래의 그림을 보자.

![image](https://swapnilshevate.files.wordpress.com/2016/08/tcp-4-way-handshake.png)

(출처 : [https://swapnilshevate.wordpress.com/tcp-vs-udp/https://swapnilshevate.wordpress.com/tcp-vs-udp/](https://swapnilshevate.wordpress.com/tcp-vs-udp/))

(1) 연결종료를 요청하는 클라이언트가 **FIN비트를 1로 활성화**하고 TCP헤드에 담아 연결 요청을 보낸다.

| URG  | ACK  | PSH  | RST  | SYN  | FIN   |
| ---- | ---- | ---- | ---- | ---- | ----- |
| 0    | 0    | 0    | 0    | 0    | **1** |

(2) 서버는 연결종료를 승인(**ACK**)하는 요청을 보내고, **연결이 종료될 때까지 기다린다 (CLOSE_WAIT 상태)**.

| URG  | ACK   | PSH  | RST  | SYN  | FIN  |
| ---- | ----- | ---- | ---- | ---- | ---- |
| 0    | **1** | 0    | 0    | 0    | 0    |

(3) 연결이 종료되면, 서버 또한 연결종료 요청(**FIN**)을 보낸다.

| URG  | ACK  | PSH  | RST  | SYN  | FIN   |
| ---- | ---- | ---- | ---- | ---- | ----- |
| 0    | 0    | 0    | 0    | 0    | **1** |

(4) 클라이언트 또한 연결종료를 승인(**ACK**)한다.

| URG  | ACK   | PSH  | RST  | SYN  | FIN  |
| ---- | ----- | ---- | ---- | ---- | ---- |
| 0    | **1** | 0    | 0    | 0    | 0    |

<br/>

FIN 비트 전송 후 연결이 종료될 때 까지 **CLOSE_WAIT**시간차가 존재하므로, 

서버는 ACK와 FIN을 한꺼번에 전송할 수 없다. 

우선 ACK를 전송하고 연결이 종료되면 FIN을 송신하므로 **절차가 한 번 더 필요하다**.

<br/>

자 그런데...

**만약 서버가 FIN패킷을 보내기 전에 클라이언트에 다른 패킷들을 전송했는데,**

**지연으로 인해 클라이언트가 서버로부터 FIN패킷을 다른 패킷들보다 먼저 받게 된다면?**

이 경우, 만약 클라이언트가 FIN패킷을 받자마자 연결을 종료해버리면 **후속 도착한 패킷은 유실될 것이다.**

따라서 클라이언트는 서버로부터 FIN을 수신하더라도 **그 이후 default 240초의 대기시간을 갖는다.** 

이 시간을 **TIME_WAIT**라고 하며, 이 시간이 지나면 비로소 세션을 만료하고 연결을 종료하여 **CLOSE**상태가 된다.

