# TCP vs UDP
## 목차
- 전송계층
- TCP
    - TCP 란?
    - TCP 특징
- UDP
    - UDP 란?
    - UDP 특징

## You can answer
- TCP는 무엇인가요?
- UDP는 무엇인가요?
- TCP와 UDP의 차이는 무엇인가요?



---
## 전송계층(Transport Layer)
### 전송계층 이란 ?
<img src = "img/TransportLayer.jpeg" width = "70%"/>

    OSI 7계층 모델의 4 계층에 해당되며 양단간 어떤 종류의 망이 사용 되었는지를 의식하지 않고, 쌍방 응용 프로세스 간에 투명하고 신뢰성 있게 양단 간에 논리적인 통신을 이루는 계층.

    즉, 목적지에 신뢰할 수 있는 데이터를 전달하기 위해 필요한 계층이다.


## TCP
### TCP 란?
- TCP(Transmission Control Protocol) :
장치들 사이에 논리적인 접속을 성립하기 위하여 연결을 설정하여 신뢰성을 보장하는 __연결형 서비스__ 통신 프로토콜.

- TCP는 네트워크에 연결된 컴퓨터에서 실행되는 프로그램 간에 일련의 옥텟(데이터, 메세지, 세그먼트 블록단위)를 안정적으로, 순서대로,에러없이 데이터를 교환할 수 있게 한다.

### TCP 특징
#### 1. 연결형 서비스
연결형 서비스로 가상 회선 방식을 제공한다.
- 3-way handshaking 과정을 통해 __연결 설정__.
>3-way-handshking : 데이터를 전송하기 전 먼저 정확한 전송을 보장하기 위해 수신 컴퓨터와 사전에 세션을 수립하는 과정.(SYN 은 동기화, ACK 는 확인을 의미.)

<img src = "img/3-way-Handshaking.png">

1. Client > Server : TCP SYN
>접속을 요청하는 SYN 패킷을 보낸다. Client는 응답을 기다리는 SYN_SENT 상태로 변경.
2. Server > Client : TCP SYN ACK
>Server는 SYN요청을 받고 요청을 수락한다는 ACK와 SYN flag가 설정된 패킷을 Client 로 보내고 ACK으로 Client가 응답하기를을 기다린다. 이때 Server는 SYN_RECEIVED상태가 된다.
3. Client > Server : TCP ACK
>Client는 Server에게 ACK을 보내고 이후로부터는 연결이 이루어지고 데이터가 오가게 된다. 이때 Server의 상태는 ESTABLISHED 이다.


- 4-way handshaking 을 통해 __연결 해제__.

<img src = "img/4-way-Handshaking.png">

1. Client > Server : TCP FIN
> Client가 연결을 종료하겠다는 FIN 플랠그를 전송.
2. Server > Client : TCP ACK
> Server는 확인 메세지를 보내고 자신의 통신이 끝날때까지 기다린다. 이때 TIME_WAIT 상태.
3. Server > Client : TCP FIN
> Server가 통신이 끝났으면 연결이 종료되었다고 Client에 FIN 플래그를 전송.
4. Client > Server : TCP ACK
> Client는 확인 메세지를 보냄.

#### 2. 흐름제어(Flow Control)
송신하는 곳에서 감당이 안되게 많은 데이터를 빠르게 보내면 수신하는 곳에서 문제가 일어나는것을 방지한다.
- Stop and Wait 방식 - 매번 전송한 패킷에 대해서 응답을 받아야만 다음 패킷을 전송할 수 있게 하는 방법.
<img src = "img/StopandWait.png">



- Sliding Window 방식 - 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인응답 없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 방법.

<img src = "img/SlidingWindow.png">


#### 3. 혼잡제어(Congestion Control)
네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지
- 만약 한 라우터에 데이터가 몰릴 경우, 자신에게 오는 정보의 소통량이 과다하면 패킷을 조금만 전송하여 혼잡 붕괴 현상이 일어나는것을 막는다.
#### 4. 신뢰성 높은 전송(Reliable transmission)
TCP Header에는 Checksum 이라는 필드가 있다. 이 필드에서 알고리즘을 통해 세그먼트가 손상되었는지를 판단할 수 있다. 손상 된 경우, 수신자는 ACK FLAG를 0으로 reset해서 보내고, 정상적으로 수신 됐을경우, ACK FLAG를 1로 설정하고 해당 세그먼트의 sequence number + 1 한 값을 ACK number에 저장해서 전송한다. (이는 TCP 세그먼트의 순서를 보장하기 위함이다.)

## UDP
### UDP 란?
<img src = "img/UDP.png" width = "70%">
- UDP(User Datagram Protocol) : 컴퓨터들 간에 데이터들이 교환될 때 제한된 서비스만을 제공하는 __비연결형 서비스__ 통신 프로토콜.

- TCP와 달리 수신측에서 데이터를 재조립하는 등의 서비스를 제공하지 않고, 특히 도착하는 데이터 패킷들의 순서를 제공하지 않는다.

### UDP 특징
#### 1. 비연결형 서비스
UDP에는 연결자체가 없어서 연결 없이 통신이 가능하다.
#### 2. 신뢰성 없는 데이터 전송 및 빠른 전송속도
<img src = "img/TCPvsUDP.jpeg" width = "70%">

UDP는 비연결형 서비스이기 때문에 TCP와 다르게 시간이 걸리는 확인작업을 일일이 하지 않는다. 효율성을 중요하게 여기는 프로토콜이라서 신뢰성과 정확성을 요구하게 되면 효율이 떨어지게 된다. 빠르게 데이터를 수신해야하는 스트리밍 방식으로 전송하는 동영상 서비스 같은곳에서는 UDP 통신을 이용한다.



---
## Reference
- [흐름제어](https://jwprogramming.tistory.com/36)
- [TCP vs UDP](https://devlog-wjdrbs96.tistory.com/288)
