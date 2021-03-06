# Heap이란?
  - 개념 및 종류
    - 힙(heap)의 개념
    - 힙(heap)의 종류
  
  - 데이터 처리 과정
    - 데이터 삽입 
    - 데이터 삭제 
  
  ---
  ## You can answer
  - 힙이란 무엇인가?
  - 힙에서 데이터 처리는 어떻게 이루어지는가?
  
  ---
    ## 개념 및 종류
    ### 힙(heap)의 개념
    1. 완전 이진 트리의 일종으로 우선순위 큐를 위해 만들어진 자료구조이다.
    2. 여러개의 값들 중에서 최댓값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조이다.
    3. 단순히 부모노드가 자식노드보다 우선순위가 높다
    4. 중복된 값을 허용한다.
    5. 힙 기반 데이터 저장/삭제의 시간복잡도는 O(logN)이다.
   
  ### 힙(heap)의 종류
#### 1. 최소힙(Min-Heap)
     부모 노드의 값이 자식 노드 값보다 작거나 같은 완전 이진 트리
<img src="heap/min_heap.png" width='50%' height='100%'/>

#### 2. 최대힙(Max-Heap)
     부모 노드의 값이 자식 노드 값보다 크거나 같은 완전 이진 트리
<img src="heap/max_heap.png" width='50%' height='100%'/>

---
## 데이터 처리 과정
### 데이터 삽입
    1. 힙에 새로운 요소가 들어오면, 힙의 마지막 노드에 이어서 삽입한다.
    2. 새로운 노드를 부모노드와 교환하여 힙의 성질을 만족시킨다.

예시) 최소힙(Min-heap)에 '1'을 삽입하는 과정
<img src="heap/min_heap_add_ex.png" width='100%' height='100%'/>

### 데이터 삭제
    1. 힙에 우선순위가 가장 높은 노드가 삭제된다.
    2. 삭제된 노드 위치에는 힙의 마지막 노드를 가져온다.
    3. 우선순위가 높은 자식노드와 교환하여 힙의 성질을 만족시킨다.

예시) 최소힙(Min-heap)에서 '1'을 삭제하는 과정
<img src="heap/min_heap_del_ex.png" width='100%' height='100%'/>

---
## 배열을 이용한 힙
### 힙의 데이터를 배열로 표현하면 아래 그림과 같다.
<img src="heap/heap_array.png" width="100%"/>


### Tip. 부모노드와 자식노드 간의 관계
    1. 왼쪽 자식 노드(left child)의 인덱스 : 부모노드 인덱스 * 2
    2. 오른쪽 자식 노드(right child)의 인덱스 : 부모노드 인덱스 * 2 + 1
    3. 부모 노드의 인덱스 : 자식 노드 인덱스 / 2

---
# Reference
- [힙이란 무엇인가?](https://gmlwjd9405.github.io/2018/05/10/data-structure-heap.html)
- [파이썬 힙 라이브러리 사용법](https://hocheon.tistory.com/70)