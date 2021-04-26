# 메모리 구조(Memory Structure)
- 프로그램 실행 순서
- 메모리 구조
    - code
    - data
    - stack
    - heap
- Stack vs Heap
- 오버플로우(Overflow)

## You can answer
- 메모리 구조
- Stack vs Heap 비교
- Stack, Heap Overflow?

---
# 프로그램 실행 순서

![1program](https://user-images.githubusercontent.com/22022393/116100838-44b0e800-a6e8-11eb-9beb-6354663e648a.png)


- 먼저 그림을 통해 프로그램이 실행되는 과정을 볼 수 있는데 이 때 OS는 메모리에 공간을 할당하는데 이는 4가지의 공간으로 볼 수 있다.


---

# 메모리 구조(Memory Structure)

![2memory](https://user-images.githubusercontent.com/22022393/116101220-9e191700-a6e8-11eb-88be-3f95ad1a83a5.png)


# 1. 코드(code) 영역
- 작성과 실행할 프로그램의 코드가 저장되는 영역이며 텍스트(text) 영역이라고도 부른다.
프로그램이 시작하고 끝날 때까지 메모리에 계속 남아 있으며 실행 파일을 구성하는 명령어들이 올라가는 메모리 영역으로 함수, 제어문, 상수 등이 지정된다.
CPU는 코드 영역에 저장된 명령어를 하나씩 가져가서 처리하게 된다.


---

# 디스크 스케줄링 종류
1. FCFS(First Come First Served), FIFO(First In First Out)
- First Come First Served 즉, 큐에 가장먼저 요청이 온 순서대로 서비스한다.
- 장점 : 알고리즘이 다른 기법보다 단순하며, 공평하게 요청을 처리
- 단점 : 비용이 많이 발생되어, 비효율적임

![1FCFS](https://user-images.githubusercontent.com/22022393/116054648-d869c080-a6b6-11eb-9f10-466413e2d411.png)


2. SSTF(Shortest Seek Time First)
- 현재 헤드에서 가장 가까운 트랙의 요청을 먼저 처리한다. 즉 현재 헤드셋을 처리하고, 다음 요청 중에 이동거리가 가장 적은거리에 있는 트랙을 처리한다.
- 장점 : Seek Time 이 적다. 즉 트랙을 찾는 시간을 최소화 할 수 있고, 처리량(Throughput)을 극대화 할 수 있다.
- 단점 : 안쪽 및 바깥쪽에 있는 요청들은 기아 현상이 발생할 수 있다. 도한, 응답 시간 편차가 크다.

![2SSTF](https://user-images.githubusercontent.com/22022393/116055573-cccac980-a6b7-11eb-9c59-fb989bdd2180.png)


3. SCAN
- 헤드셋의 진행방향에 있는 요청을 처리하고, 다시 반대 방향으로 틀어 반대방향에 있는 요청들을 처리한다. 마치 엘레베이터가 동작하는 원리가 같아서 엘레베이터 기법이라고도 한다. 헤드가 앞으로 스캔할 때(번호가 작은 실린더 방향)와 뒤로 스캔할 때(번호가 큰 실린더 방향) 선택하는 실린더가 서로 다르다. 즉 관성을 고려하여 한방향으로 쭉 가다가 끝이면 반대 방향으로 돈다.
- 장점 : SSTF의 바깥쪽 트랙의 기아가능성을 제거할 수 있고, 응답시간의 편차를 줄일 수 있다.
- 단점 : 양 쪽 끝 트랙 가운데 위치한 트랙보다 대기 시간이 길어진다. 엘레베이터로 비유하자면, 맨 꼭대기 층이 중간층보다 응답시간이 늦어질 수 있다는 뜻이다.

![3SCAN](https://user-images.githubusercontent.com/22022393/116055606-d5bb9b00-a6b7-11eb-811f-424920e7e4b3.png)


4. C-SCAN
- 항상 한쪽 방향에서 반대방향으로 진행하며 트랙의 요청을 처리한다. 물론 오른쪽에서 왼쪽 끝으로 갈 때 한 바퀴를 돌기 때문에 움직이는 거리는 더 길어질 수 있지만 다시 처음 위치로 되돌아갈 때는 데이터를 읽지 않으므로 더 빠른 속도로 움직일 수 있다.
- 장점 : 응답시간의 편차가 매우 적음, SCAN보다 시간균등성이 좋음
- 단점 : 안쪽이나, 바깥쪽으로 처리할 요청이 없어도 헤드셋이 끝까지 이동하기 때문에 비효율적

![4C-SCAN](https://user-images.githubusercontent.com/22022393/116055628-dce2a900-a6b7-11eb-9f36-668ff53b3f56.png)


5. LOOK, C-LOOK
- LOOK 과 C-LOOK 은 SCAN과 C-SCAN을 보완하기 위한 스케쥴링 기법이다. SCAN과 C-SCAN의 비효율적인 부분은 끝단에 요청이 없어도 끝단까지 도달하는 것이다. 따라서 요청이 진행방향에서 더 이상 없다면, 끝단까지 가지 않고 반대방향으로 가던가(SCAN), 다시 같은 방향으로 진행(C-SAN)하게 한다.
- 장점 : 불필요한 헤드 이동시간을 제거
- 단점 : 끝단까지 가야할지 말아야할지 판단하는데 있어서 오버헤드 발생

![5LOOK](https://user-images.githubusercontent.com/22022393/116055671-ea982e80-a6b7-11eb-8001-1d544a12e40d.png)
