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
- TCP UDP는 어디에 사용되나요?


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
1. 연결형 서비스
연결형 서비스로 가상 회선 방식을 제공한다.
- 3-way handshaking 과정을 통해 연결 설정.

    3-way-handshking : 데이터를 전송하기 전 먼저 정확한 전송을 보장하기 위해 수신 컴퓨터와 사전에 세션을 수립하는 과정.

    Client > Server : TCP SYN
    Server > Client : TCP SYN ACK
    Client > Server : TCP ACK



- 4-way handshaking 을 통해 연결 해제.


2. 흐름제어(Flow Control)
3. 혼잡제어(Congestion Control)
4. 신뢰성 높은 전송(Reliable transmission)


## UDP
### UDP 란?
- UDP(User Datagram Protocol) : 컴퓨터들 간에 데이터들이 교환될 때 제한된 서비스만을 제공하는 __비연결형 서비스__ 통신 프로토콜.

- TCP와 달리 수신측에서 데이터를 재조립하는 등의 서비스를 제공하지 않고, 특히 도착하는 데이터 패킷들의 순서를 제공하지 않는다.

### UDP 특징




---
## Reference
- [Example Site1](www.google.com)
- [Example Site2](www.google.com)
