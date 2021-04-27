# Minimal Spanning Tree
- Minimal Spanning Tree의 사례
  - 도로건설
  - 통신망구축
- Definition
  - What is Minimal Spanning Tree?
  - Example
- Spanning Tree
  - What is Spanning Tree?
- Minimal Spanning Tree의 특징
  - 특징
- Minimal Spanning Tree의 구현 알고리즘
  - Prim's algorithm
  - Kruskal's algorithm

## You can answer
- Spanning Tree란 무엇인가
- Minimal Spanning Tree란 무엇인가
- Minimal Spanning Tree의 사례로 어떤 것이 있는가
---


## Minimal Spanning Tree의 사례
```
객체(node)들을 최소의 길이로 연결할 필요가 있는 곳에 사용
```
### 도로 건설
- 도시(node)들을 연결하여 도로 길이의 합이 최소가 되도록 한다
### 통신
- 통신선 길이의 합이 최소가 되도록 망을 구성한다, 사용자(= node)


## Definition
### What is minimal Spanning Tree?
```
Spanning Tree 中 가중치의 합이 가장 최소인 것
```


## Spanning Tree
### What is Spanning Tree
```
- 그래프의 모든 노드를 최소한의 간선으로 연결한다
- n개의 node를 가진 Spanning Tree는 반드시 (n - 1)개의 edge를 가진다
- 특징 : undirected, connected, acyclic
```
### Example
![MST](https://user-images.githubusercontent.com/70050038/116186570-e1ae6800-a75e-11eb-8940-910adaad9f89.png)

- G&#8321; 으로부터 G&#8323;, G&#8324;, G&#8325; 이라는 Spanning Tree가 파생 되었고, 그중 가중치가 가장 작은 G&#8324;, G&#8325; 이 Minimal Spanning Tree가 되었다


## Minial Spanning Tree의 특징
### 특징
```
1. Spanning Tree 中 간선의 가중치의 합이 최소이어야 한다
2. n 개의 node를 가지는 그래프에 대해 edge는 (n-1)개여야 한다
3. 그래프에 사이클이 포함되서는 안된다
4. 모든 정점은 연결되어 있어야한다
5. 방향성이 없어야 한다
```


## Minial Spanning Tree의 구현 알고리즘
- Prim's Algorithm
- Kruskal's Algorithm

---
Reference
- X