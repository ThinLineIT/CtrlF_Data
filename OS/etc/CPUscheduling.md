# CPU Scheduling
<!--Table of Contents-->
    
<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- OS 의 CPU 스케줄링에는 어떤 방법이 있나요?
- 각각을 설명해보세요.

<!--Contents-->
<br>

---

<br>

## CPU 스케줄링이란?

    CPU가 하나의 프로세스 작업이 끝나면 다음 프로세스 작업을 수행해야 한다. 이때 다음 프로세스가 어느 프로세스인지를 선택하는 알고리즘을 CPU Scheduling 알고리즘이라고 한다. 
    
    간단히 생각해보면 먼저 온 프로세스가 먼저 실행되는 것이 가장 좋을 것이라 생각할 수 있다. 하지만 여러 상황에서 사용되는 컴퓨터 환경에서 꼭 이러한 방법이 좋다고 할 수 없다. 그러므로 CPU 스케줄링에는 여러가지 방법이 존재한다.

<br>
<br>

## 1. Preemptive VS Non-preemptive
<br>

### 1-1. Preemptive (선정)

Preemptive(선점)은 프로세스가 CPU를 점유하고 있는 동안 I/O나 인터럽트가 발생한 것도 아니고 모든 작업을 끝내지도 않았는데, 다른 프로세스가 해당 CPU를 강제로 점유 할 수 있다. 즉, 프로세스가 정상적으로 수행중인 가운데 다른 프로세스가 CPU를 강제로 점유하여 실행할 수 있는 것이다.


    한 마디로, 프로세스가 CPU 를 강제로 점유하여 실행할 수 있는 것!

<br>

### 1-2. Non-Preemptive (선정)

Non-preemptive(비선점)은 말 그대로 preemptive의 반대이다. 한 프로세스가 한 번 CPU를 점유했다면, I/O(프로세스 상태가 실행 -> 대기로 변경되는 경우) 또는 프로세스가 종료될 때까지 다른 프로세스가 CPU를 점유하지 못하는 것이다.

<br>
<br>
<br>

## 2. Scheduling criteria

<br>

Scheduling criteria(척도)는 스케줄링의 효율을 분석하는 기준들이다.

<br>

- CPU Utilization(이용률, %): CPU가 수행되는 비율
- Throughput(처리율, jobs/sec): 단위시간당 처리하는 작업의 수(처리량)
- Turnaround time(반환시간): 프로세스의 처음 시작 시간부터 모든 작업을 끝내고 종료하는데 걸린 시간이다.(CPU, waiting, I/O 등 모든 시간을 포함한다.) 반환시간은 짧을 수록 좋다.
- Waiting time(대기시간): CPU를 점유하기 위해서 ready queue에서 기다린 시간을 말한다.(다른 큐에서 대기한 시간은 제외한다.)
- Response time(응답시간): 일반적으로 대화형 시스템에서 입력에 대한 반응 시간을 말한다.

<br>
<br>

## 3. CPU Scheduling Algorithms

<br>

### 1. First come, First out (FCFS)
<br>
FCFS는 먼저 온 프로세스가 먼저 CPU를 점유하는 방식이다. 이는 매우 단순하고 많이 사용하는 방법이지만, 모든 부분에서 효율적인 것은 아니다.
문제점은 Convey 효과로, 처리 기간이 긴 프로세스가 오랫동안 CPU 를 점유하면 다음 프로세스의 대기시간이 늘어나는 문제점이 있다. 

<br>
<br>


<img width="566" alt="스크린샷 2021-06-27 오후 7 52 32" src="https://user-images.githubusercontent.com/70083982/123541781-38e1b080-d781-11eb-886b-82a5b9849623.png">


<br>
<br>
<br>

### 2. Shortest-Job-First (SJF)
<br>

SJF는 이름에서도 나타나듯이 가장 짧게 수행되는 프로세스가 가장 먼저 수행되는 것을 말한다. FCFS에서 보았듯이 수행 시간이 짧은 프로세스가 먼저 오는 것이 평균 대기시간이 짧은 것을 알 수 있었다. 이를 이용한 것이 SJF이다. 콘보이 효과를 해결할 수 있지만 문제점으로는

    1. 운영체제가 프로세스의 종료시간을 정확히 예측하기 힘들다. 
    

<br>


<img width="559" alt="스크린샷 2021-06-27 오후 7 56 19" src="https://user-images.githubusercontent.com/70083982/123541911-c02f2400-d781-11eb-9a20-2a308d9d971b.png">

<br>
<br>
<br>

### 3. Priority
<br>
Priority 스케줄링은 말그대로 우선순위가 높은 프로세스가 먼저 선택되는 스케줄링 알고리즘이다. 운영체제에서 일반적으로 우선순위는 정수값으로 나타내며, 작은 값이 우선순위가 높다.(Unix/Linux 기준)

<br>
<br>

    Priority 스케줄링의 문제점은 Starvation(기아)가 있다. Starvation은 말 그대로 CPU의 점유를 오랫동안 하지 못하는 현상을 말한다. 

Priority 스케줄링 방식에서 우선순위가 매우 낮은 프로세스가 ready queue에서 대기하고 있다고 가정해보자. 이 프로세스는 아무리 오래 기다려도 CPU를 점유하지 못할 가능성이 매우 크다. 실제 컴퓨터 환경에서는 새로운 프로세스가 자주 ready queue에 들어온다. 이러한 프로세스가 모두 우선순위가 높은 상태라면 이미 기다리고 있던 우선순위가 낮은 프로세스는 하염없이 기다리고만 있는 상태로 남아있을 수 있다.

=> 이를 해결하는 방법 중 하나는 aging이 있다. 이 방식은 ready queue에서 기다리는 동안 일정 시간이 지나면 우선순위를 일정량 높여주는 것이다. 그러면 우선순위가 매우 낮은 프로세스라 하더라도, 기다리는 시간이 길어질수록 우선순위도 계속 높아지므로 수행될 가능성이 커진다.


<br>
<br>

### 4. Round - Robin
<br>
<br>

Round-Robin은 원 모양으로 모든 프로세스가 돌아가며 스케줄링하는 것을 말한다. 이는 시분할 시스템에서 주로 사용하는 방식이다. 일정 시간을 정하여 하나의 프로세스가 이 시간동안 수행하고 다시 대기 상태로 돌아간다. 그리고 다음 프로세스가 역시 같은 시간동안 수행한 후 대기한다. 이러한 작업을 모든 프로세스가 돌아가면서 하며, 마지막 프로세스가 끝나면 다시 처음 프로세스로 돌아와서 반복한다.

위에서 말한 일정 시간을 Time Quantum(Time Slice)이라 부른다. Time Quantum은 일반적으로 10 ~ 100msec 사이의 범위를 갖는다. Round-Robin은 기본적으로 preemptive 이다. 한 프로세스가 종료되기 전에 time quantum이 끝나면 다른 프로세스로 CPU를 넘겨주기 때문이다.

<br>


<img width="561" alt="스크린샷 2021-06-27 오후 8 04 30" src="https://user-images.githubusercontent.com/70083982/123542145-e3a69e80-d782-11eb-9581-7d1f29c29fe3.png">

<br>

    문제점은 Time Slice 의 크기를 적절하게 해야 효율을 유지할 수 있다.  


<br>
<br>

### 5. Multilevel Queue
<br>
<br>

Multilevel Queue를 살펴보기 전에 프로세스 그룹에 대해 살펴보자. 프로세스는 기준에 따라 여러 그룹으로 나눌 수 있다.

- System processes: 운영체제 커널 수준의 프로세스
- Interactive processes: 유저 수준의 대화형 프로세스
- Interactive editing processes
- Batch processes: 대화형 프로세스의 반대인 것으로 일정량을 한 번에 처리하는 프로세스(Ex, 컴파일러)
- Student processes
위와 같이 여러 성격에 따라 프로세스 그룹을 나눌 수 있는데 이를 하나의 큐에 사용하는 것은 비효율적이라고 판단하였다. 그래서 각 그룹에 따라 큐를 두어 여러 개의 큐를 사용하는 것이 Muitilevel Queue 방식이다.

<br>



위와 같이 여러 성격에 따라 프로세스 그룹을 나눌 수 있는데 이를 하나의 큐에 사용하는 것은 비효율적이라고 판단하였다. 그래서 각 그룹에 따라 큐를 두어 여러 개의 큐를 사용하는 것이 Muitilevel Queue 방식이다.

<br>

<img width="431" alt="스크린샷 2021-06-27 오후 8 07 40" src="https://user-images.githubusercontent.com/70083982/123542219-544dbb00-d783-11eb-8956-67205244511e.png">

<br>
<br>

위 그림은 각 그룹에 따라 큐를 나눈 것이다. 그리고 각 큐마다 다른 규칙을 지정할 수도 있다.


먼저, 큐마다 우선순위를 지정해줄 수 있다. 프로세스 그룹을 보면 System process는 커널 수준에서 중요한 작업이므로 우선순위가 높은 그룹이라 볼 수 있다. 위 그림에서 System process, Interactive process, Batch process 순으로 우선순위가 높은 순서이다. Batch 프로세스는 운영체제의 개입이 매우 적으므로 우선순위가 가장 낮다고 볼 수 있다.

위의 방식 이외에도 큐에 따라 여러 기준을 둘 수 있다. 큐마다 CPU 시간을 다르게 줄 수도 있고, 큐마다 다른 스케줄링 방식을 사용할 수도 있다.