# LRU Cache(Least Recently Used)
  
<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- LRU 캐시 알고리즘이란?
- 어떤 자료구조를 이용해서 이를 구현할 수 있을까?

<!--Contents-->

---


## Cache 개념
캐시는 데이터나 값을 미리 복사해 놓는 임시 장소를 가리킨다. 캐시는 접근 시간에 비해 원래 데이터를 접근하는 시간이 오래 걸리는 경우나 값을 다시 계산하는 시간을 절약하는 경우 사용한다. 캐시에 데이터를 미리 복사해 놓으면 계산이나 접근 시간 없이 빠른 속도로 데이터에 접근할 수 있다. (캐시가 사용하는 리소스의 양도 제한이 있다.)

## LRU Cache 개념
LRU는 Least Recently Used의 약자로 OS의 페이지 교체 알고리즘의 하나로 최근에 가장 오랫동안 사용하지 않은 페이지를 교체하는 알고리즘이다. 캐시에 공간이 부족하면 가장 최근에 사용하지 않은 항목을 제거한다.

## LRU Cache 구현

<img width="792" alt="스크린샷 2021-08-09 오전 1 06 00" src="https://user-images.githubusercontent.com/70083982/128638274-18952b88-571b-42c8-bb97-5723c756edde.png">

LRU Cache 구현은 Doubly Linked List를 통해 구현한다. head에 가까운 데이터일수록 최근에 사용한 데이터이고, tail에 가까울수록 가장 오랫동안 사용하지 않은 데이터로 간주하여 새로운 데이터를 삽입할 때, 가장 먼저 삭제되도록 한다.

삽입된 데이터를 사용하면 head로 옮겨 우선순위를 높이게 되고, 삭제될 우선순위에서 멀어지게 된다.


## 파이썬으로 구현해보기

    class LRUCache:
        def __init__(self, max_size: int):
            self.max_size = max_size
            self.dic = {}

        def get(self, key: int) -> int:
            if key in self.dic:
                value = self.dic.pop(key)
                self.dic[key] = value # 이렇게 pop 이후 다시 입력하면 가장 뒷 순서로 배치됩니다.
                return self.dic[key]

            else: 
                return -1

        def put(self, key: int, value: int) -> None:
            if key in self.dic:
                self.dic.pop(key)
            elif len(self.dic) == self.max_size:
                del self.dic[next(iter(self.dic))] # 가장 먼저 나온 (쓴 지 오래된) 값을 지웁니다.

            self.dic[key] = value