# Dijkstra Algorithm
- 개념 및 특징
- 프로세스
- 시간 복잡도
- 코드(python)

## You can answer
- Dijkstra Algorithm이 무엇인지 알 수 있다.
- Dijkstra Algorithm의 구현 할 수 있다.

---
## 개념 및 특징
### Dijkstra Algorithm
    1. 그래프의 한 정점에서 부터 다른 모든 정점으로 최단 경로를 구하는 알고리즘이다.
    2. 시작 정점에서 부터 인접한 정점들을 하나씩 방문하며 해당 정점까지의 거리를 갱신한다.
    3. 음수 간선이 존재할 경우 최적의 해를 구할 수 없다.

## 프로세스
    1. 시작 정점을 제외한 모든 정점까지의 거리는 모두 무한(INF)으로 설정한다.
    2. 시작 정점을 우선순위 큐에 삽입한다.
    3. 우선순위 큐에서 정점을 하나씩 꺼낸 후 해당 정점에서 갈 수 있는 인접한 정점들을 확인한다.
        3-1. 인접 정점까지의 거리 < 이미 기록된 거리 => 갱신
        3-2. 인접 정점까지의 거리 > 이미 기록된 거리 => 무시
    4. 3-1에서 갱신한 정점들을 우선순위 큐에 삽입
    5. 우선순위 큐에 더이상 정점이 없어질 때까지 3~4번 과정을 반복한다.

## 시간 복잡도(Time Complex)

-   각 정점마다 인접한 간선들을 탐색하는 과정 + 우선순위 큐에서 [거리, 정점] 정보를 삽입/추출하는 과정
  
    - 각 정점마다 인접한 간선들을 탐색하는 과정
        
            각 노드는 최대 한번씩 방문하기 때문에 그래프의 모든 간선은 최대 한번 씩은 검사함.
            => O(E)
    - 우선순위 큐에서 [거리, 정점] 정보를 삽입/추출하는 과정
        
            최악의 경우 : 모든 노드를 방문할 때마다 우선순위 큐가 갱신/저장 되는 경우
            => E개의 간선을 검사할 때마다 우선순위 큐를 유지해야함.
            => O(ElogE)
    - 결론
  
            O(E) + O(ElogE) = O(ElogE)


## 코드(Python)
```Python
import heapq
import sys
input = sys.stdin.readline
INF = int(1e9)

# n: 노드 개수, m: 간선 개수
n,m = map(int, input().split())
start = int(input()) # 시작노드
graph = [[]for _ in range(n+1)] #노드의 개수 만큼 이차원 배열 생성
distance = [INF] * (n+1) # 각 노드의 거리는 무한으로 초기화

for _ in range(m):
    # a에서 b까지 가는 비용(거리)가 c이다.
    # a:출발 노드, b:도착 노드, c: 비용(거리)
    a,b,c = map(int, input().split())
    graph[a].append((b,c))

#Dijkstra 수행
dijkstra(start)


for i in range(1,(n+1)):
    # 노드의 거리가 무한일 때 => -1 출력
    if distance[i] == INF:
        print(-1)
    # 그렇지 않을 경우, 해당 거리 출력
    else:
        print(distance[i])



# Dijkstra Algorithm
def dijkstra(start):
    q = []

    # 시작 노드의 거리는 0으로 초기화
    # 우선순위큐에는 (거리, 노드) 정보로 값을 삽입 => 거리 기준으로 최소힙을 유지할 수 있음
    heapq.heappush(q,(0,start))
    distance[start] = 0

    
    while q:
        # 우선순위 큐에서 하나의 (거리, 노드) 정보 값 추출
        dist, now = heapq.heappop(q)   

        # 이미 기록된 거리 < 새롭게 알게 된 인접 정점까지의 거리 => 무시
        if distance[now] < dist:
            continue 
        
        # graph[now] => (노드, 거리)형식으로 저장되어있음
        for i in graph[now]:
            # 기존 계산된 거리와 새롭게 알게된 거리의 누적 합
            cost = dist + i[1] 

            # 이미 기록된 거리 > 새롭게 알게 된 인접 정점까지의 거리 => 갱신 후 우선순위 큐에 삽입
            if distance[i[0]] > cost:
                distance[i[0]] = cost
                heapq.heappush(q,(cost,i[0]))

```