# 컴퓨터를 키면 무슨 일이 일어날까?



## You can answer
- 컴퓨터를 켜면 무슨 일이 일어나나요?
- 부팅과정은 어떻게 되나요?


프로그램이 실행되지 않는 컴퓨터는 그저 전자제품일 뿐입니다.
컴퓨터가 켜질 때 가장 먼저 해야하는 것은 운영체제(이하 OS)라고 부르는 특별한 프로그램을 시작하는 것입니다. 결국 운영체제도 디스크에 저장된 하나의 프로그램일 뿐!
OS의 일은 컴퓨터의 하드웨어의 세부사항을 제어해 다른 프로그램이 수행되도록 돕는 것 입니다.


# 컴퓨터 부팅과정

---


![booting](https://user-images.githubusercontent.com/70083982/115982119-9156cf00-a5d3-11eb-911c-645d86e62f4c.jpeg)


1. 전원 공급
- 본체의 전원 스위치를 누르면 전원이 파워 서플라이에 전달
- 파워 서플라이에 전달된 전원은 컴퓨터 내부에서 사용되는 전압으로 바뀌어 CPU로 전달되어 부팅 작업 시작
 
 
2. 공급되는 전원 확인
- 파워서플라이 안에는 몇 개의 반도체 칩이 들어있음, 이 반도체에 전달되는 전압이 정상이고 안정적인지 진단
- 올바른 전압이면 내장된 타이머 칩으로 "Power good signal" 신호 발송
 
 
3. CPU온
- 타이머 칩은 CPU에 보내던 리셋 신호를 중지
- CPU안에 남아있던 불필요한 내용들을 제거
- 리셋 시그널이 없어지지 않으면 전원은 들어오지만 화면은 나오지않음
 
 
4. 바이오스 읽기
- CPU는 바이오스에서 데이터를 읽어 온다. POST(Power on self(test))진행
- 바이오스 오류가 있다면 역시 전원은 들어오지만 화면은 나오지 않습니다.
 
 
5. POST 진행
- 컴퓨터 본체와 하드웨어에 정상적인 작동을 하는지 검사하는 과정 (메인보드 연결, 그래픽카드, 메모리, 키보드, 하드디스크, USB등 외부단자)
- 오류가 발생하면 비프음을 내거나 화면에 오류 내용을 출력
 (시스템 버스의 정상적 작동유무, 그래픽카드 데스트, 바이오스 검색 및 테스트, 메모리 이상 유무 테스트, 메인보드에 연결된 장치들의 시스템 자원 확인 등 250가지)
 

- Post에 이상이 없으면 MBR (Master Boot Record) 실행, 부트로더의 실행으로 보조기억장치에 저장된 운영체제가 시스템 파일을 램으로 이동 => 부트로더가 OS를 메모리에 로드!

 
6. OS부팅
- 전원이 켜질 때 ROM(Read Only Memory)에 저장된 초기프로그램을 실행합니다.
- ROM은 오직 읽기만 할 수 있기 때문에 항상 똑같은 프로그램만 실행할 수 있습니다.
- init 은 메모리, CPU레지스터 등 초기화시켜 컴퓨터가 새로운 연산을 할 수 있는 상태를 만듭니다.
- 이후 운영체제를 메모리에 올림과 동시에 첫 시작프로세스를 실행하고 인터럽트가 발생합니다.
 
 
## 요약

---

1. 컴퓨터의 전원을 누르면 ROM(또는 플레시 메모리)에 있는 BIOS가 로드됩니다.
2. BIOS는 하드웨어가 정상적인지 확인하는 POST를 실행합니다. 이 과정에서 문제가 없다면, MBR에서 부트로더(하드 디스크의 가장 낮은 주소에 위치함)를 로드합니다. (부팅 프로그램을 주기억 장치에 로딩합니다.)
3. 부트로더는 OS를 실행합니다. (운영체제를 주기억 장치에 로딩합니다.)
4. OS의 초기화를 진행하는 init을 실행합니다.
5. 운영체제 명령에 의해 CPU가 프로그램을 실행합니다.



