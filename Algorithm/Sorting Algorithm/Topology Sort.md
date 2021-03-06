# 위상 정렬 (Topology Sort)
<!--Table of Contents-->
- 위상 정렬이란?
- 위상 정렬 알고리즘
- 위상 정렬의 특징
- 위상 정렬 알고리즘 소스코드 (Python)
- 위상 정렬의 시간 복잡도

## You Can Answer
- 위상 정렬이란 무엇인가요?
- 위상 정렬 알고리즘을 설명해보세요.
- 위상 정렬의 시간 복잡도는 어떻게 될까요?
-----------------------

## 위상 정렬이란?
- 위상 정렬(Topology Sort)은 정렬 알고리즘의 일종이다. 위상 정렬은 순서가 정해져 있는 일련의 작업을 차례대로 수행해야 할 때 사용할 수 있는 알고리즘이다. **위상 정렬**이란 **방향 그래프의 모든 노드를 방향성에 거스르지 않도록 순서대로 나열하는 것**이다.

## 위상 정렬의 진입 차수
- 위상 정렬 알고리즘을 알기 전에 진입차수(Indegree)를 알아야한다. 진입차수란 특정한 노드로 '들어오는' 간선의 개수를 의미한다.

## 위상 정렬 알고리즘
1.진입차수가 0인 노드를 큐에 넣는다.</br>
2.큐가 빌 때까지 다음의 과정을 반복한다.</br>
   ⅰ.큐에서 원소를 꺼내 해당 노드에서 출발하는 간선을 그래프에서 제거한다.</br>
   ⅱ.새롭게 진입차수가 0이 된 노드를 큐에 넣는다.

- 위 알고리즘을 이용하여 간단하게 위상 정렬을 수행할 수 있다. 큐가 빌 때까지 큐에서 원소를 계속 꺼내서 처리하는 과정을 반복한다. 이때 모든 원소를 방문하기 전에 큐가 빈다면 사이클이 존재한다고 판단할 수 있다. 다시 말해 큐에서 원소가 V번 추출되기 전에 큐가 비어버리면 사이클이 발생한 것이다.(사이클이 존재하는 경우 사이클에 포함되어 있는 원소 중에서 어떠한 원소도 큐에 들어가지 못하기 때문.)

</br>

## 위상 정렬의 특징
- 위상 정렬의 답안은 여러 가지가 될 수 있다는 점이 특징이다. 만약에 한 단계에서 큐에 새롭게 들어가는 원소가 2개 이상인 경우가 있다면, 여러 가지의 답이 존재하게 된다.

</br>

## 위상 정렬 알고리즘 소스코드 (Python)
```
#위상 정렬 소스코드

from collections import deque

#노드의 개수와 간선의 개수를 입력받기
v,e = map(int,input().split())

#모든 노드에 대한 진입차수는 0으로 초기화
indegree = [0]*(v+1)

#각 노드에 연결된 간선 정보를 담기 위한 연결 리스트(그래프) 초기화
graph = [[] for i in range(v+1)]

#방향 그래프의 모든 간선 정보를 입력받기
for _ in range(e):
    a,b = map(int,input().split())
    graph[a].append(b) # A->B로 이동 가능

    #진입차수를 1 증가
    indegree[b]+=1

#위상 정렬 함수
def topology_sort():

    result = []
    q = deque()

    #처음 시작할 때는 진입차수가 0인 노드를 큐에 삽입
    for i in range(1,v+1):
        if indegree[i]==0:
            q.append(i)
    
    #큐가 빌 때까지 반복
    while q:
        #큐에서 원소 꺼내기
        now = q.popleft()
        result.append(now)

        #해당 원소와 연결된 노드들의 진입차수에서 1빼기
        for i in graph[now]:
            indegree[i]-=1

            #새롭게 진입차수가 0이 되는 노드를 큐에 삽입
            if indegree[i]==0:
                q.append(i)
                
    #위상 정렬을 수행한 결과 출력
    for i in result:
        print(i,end=' ')
        
topology_sort()
```
<br/>

## 위상 정렬의 시간 복잡도
- 위상 정렬의 시간 복잡도는 O(V+E)이다.

<br/>

## Reference
- 이것이 취업을 위한 코딩 테스트다 with 파이썬 (나동빈 저자)

