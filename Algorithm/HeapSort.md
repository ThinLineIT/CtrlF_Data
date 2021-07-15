# Heap Sort(힙 정렬)

## 목차
- Heap Sort란?
- Heap Sort의 정렬 과정
- Heap Sort의 장단점
- Heap Sort 구현

## You Can Answer
- Heap Sort란 무엇인가요?
- Heap Sort의 장단점은 무엇인가요?
- Heap Sort의 정렬 과정은 어떻게 되나요?

### Heap Sort란?
힙은 각 노드의 값이 자신의 자식 노드 값보다 크거나(최대 힙) 작은(최소 힙) 완전 이진 트리로 구성돼 있다. 힙 정렬은 이러한 힙의 특징을 만족하도록 재귀적으로 트리 구조를 만들어 정렬하는 방법이다.

### 과정
최소 힙(부모 노드 < 자식 노드)을 예시로 설명하겠다. 인덱스는 마지막 노드부터 시작한다.
1. 해당 노드의 값과 자식 노드들과의 값을 비교한다.</br>1-1. 자식 노드가 2개라면 2개 중 작은 값을 부모 노드와 비교한다. 부모 노드의 값이 더 클 경우 해당 자식 노드와 값을 바꾼다.
2. 바꾼 자식 노드에 자식 노드가 존재할 경우 1번을 과정으로 되돌아 간다.

![heapsort1](./img/HeapSort1.png)

idx가 5,4,3일 때는 자식 노드가 없으므로 pass

![heapsort2](./img/HeapSort2.png)

idx=2일 때 바꾼 자식 노드에 자식 노드가 추가로 없으므로 값을 바꾸는 과정을 한 번만 한다.

![heapsort3](./img/HeapSort3.png)

idx=1 일 때 바꾼 자식 노드에 자식 노드가 추가로 있으므로 한 번 더 비교한다.

### 장단점
- 장점
  - 추가적인 메모리가 필요하지 않다.
  - 항상 O(NxlogN)의 시간 복잡도를 가진다.
- 단점
  - 다른 정렬 방법들에 비해 조금 느린 편이다.
  - 데이터의 순서가 바뀌므로 unstable한 알고리즘이다.


### 구현
```python
def heapify(li, idx, n):
  left = idx * 2
  right = idx * 2 +1
  s_idx = idx
  if(left <= n and li[s_idx] > li[left]):
    s_idx = left
  if(right <= n and li[s_idx] > li[right]):
    s_dix = right
  if s_idx != idx:
    li[idx], li[s_idx] = li[s_idx], li[idx]
    return heapify(li, s_idx, n)

def heap_sort(v):
  n = len(v)
  # index를 1부터 시작하기 위해서  
  v = [0]+v

  # min heap 생성
  for i in range(n,0,-1) :
    heapify(v, i, n)


heap_sort([5,3,4,2,1])
```

### Reference
- [파이썬으로 힙 정렬 구현하기](https://leedakyeong.tistory.com/entry/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9C%BC%EB%A1%9C-%ED%9E%99-%EC%A0%95%EB%A0%AC-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-heap-sort-in-python)
- [힙 정렬(Heap Sort) 알고리즘](https://velog.io/@ssuda/%ED%9E%99-%EC%A0%95%EB%A0%ACHeap-Sort-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-cik536v5ls)
