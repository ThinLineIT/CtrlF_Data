# CPU and I/O bound
<!--Table of Contents-->
- CPU, I/O burst
- CPU, I/O bound process

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- burst란?
- CPU, I/O bound process란?
- CPU, I/O bound process 성능 향상 방법은?

<!--Contents-->

---
## cpu, IO burst
### Burst
- 어떤 특정된 기준(criterion)에 따라 한 단위로서 취급되는 연속된 신호(signal) 또는 데이터의 모임, 어떤 현상이 짧은 시간에 집중적으로 일어나는 현상 또는 주기억 장치의 내용을 캐시 기억 장치에 블록 단위로 한꺼번에 전송하는 것을 의미 합니다.  

즉, CPU burst는 프로세스 내에서 CPU 명령작업이 연속된 작업을 의미하며 IO burst는 로컬 혹은 네트워크등의 I/O wait 작업이 연속되는 것을 의미합니다.

![image](https://user-images.githubusercontent.com/22022393/128832443-f147e904-4e98-467f-9926-79547fcfc953.png)

프로세스는 수행과정중에 CPU burst, I/O burst 가 계속 변경되면서 수행되며 프로세스 작업의 종류에 따라 CPU burst가 일어나는 시간이 달라집니다. 이때 일반적으로 CPU burst가 많이 일어나는 작업을 CPU bound process 라고 부르고 I/O burst가 많이 일어나는 작업을 I/O bound process라 부릅니다.

## CPU, I/O bound process

![image](https://user-images.githubusercontent.com/22022393/128834931-814ed853-774d-4e6c-958f-3e3c90821525.png)

- CPU bound process
CPU bound proces 는 말 그대로 CPU의 성능에 영향을 받는 작업이기 때문에 CPU의 성능을 높여(높은 클럭의 CPU 사용) 작업의 성능을 향상시킬 수 있습니다.  
또한, 단순히 클럭만 높이는 방법이 아닌 프로세서가 추가된 멀티코어 프로세서를 이용해 작업을 CPU 병렬적으로 처리해 성능을 더욱이 향상 시킬 수 있습니다.  
특히 일반적으로 CPU연산이 많이 발생하는 머신러닝 작업에서 Many-core processor인 GPU를 사용하는 이유도 병렬연산를 통해 성능을 향상시키려는 것과 같은 이유 입니다.  

- IO bound process
로컬 저장소에 대한 I/O wait가 많은 작업의경우 하드웨어 교체를 통해 (HDD -> SSD) 성능을 향상시켜 볼 수 있으나 로컬 저장소보다 빠른 memory를 활용 하는 것 더 좋은 방법 입니다.  
하지만, memory용량 에는 한계가 있고 네트워크에 대한 I/O wait 성능개선에는 큰 도움이 안될 수 있다는점이 존재합니다.  
외부 I/O의 성능을 향상시는것은 어쩌면 처리할수 없는 부분이기 때문에, Non-blocking I/O를 통해 처리율을 향상시키는 방법도 있을 것 입니다.  

---
## Reference
- [Taes-k DevLog](https://taes-k.github.io/2021/06/05/cpu-io-bound/)
- [양햄찌가 만드는 세상](www.google.com)
