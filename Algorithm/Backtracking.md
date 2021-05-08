# Backtracking against Greedy algorithm
<!--Table of Contents-->
- What is the Backtracking
    - explanation
- Compare to Greedy algorithm
    - Similar to Greedy algorithm
    - Different from Greedy algorithm
- Example
    - N-Queens Problem
    - Graph Coloring (m - coloring problem)
    - Hamiltonian Circuit Problem
    - 0-1 Knapsack Problem
    
<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- Explain Backtracking's flow
- When to use Backtracking

<!--Contents-->

---
## What is the Backtracking
### explanation
![이미지 1](https://user-images.githubusercontent.com/70050038/117528665-3dee7300-b00e-11eb-8e61-3d4ae68c7cb9.png)

    백트래킹의 큰 틀은 깊이 우선 탐색(DFS), 완전탐색이다
    즉, 해를 구할 때 까지 모든 가능한 경우를 시도하는 것이다. 단 제약 조건이 있다
    탐색을 하는 과정에서 조건에 충족되지 않는 노드의 가지들은 탐색을 하지 않고, 이전의 분기로 되돌아 간다
    가지치기를 하는 과정으로 인해 완전탐색에 비해 경제적으로 해에 도달할 수 있다
    모든 노드를 검사했을 때, 해가 없다면 해당 문제의 해가 없는 것이다


## Compare to Greedy algorithm
### Similar to Greedy algorithm
    백트래킹과 욕심쟁이 알고리즘 모두 최적의 해를 구하기 위해 특정 집합의 원소를 계속해서 선택해가는 과정이다
### Different from Greedy algorithm
    욕심쟁이 알고리즘은 순간의 최선을 마지막까지 추적하는데 반해, 백트래킹은 재추적을 사용한다

## Example
### N-Queens Problem
    [N x N]의 체스판에서 N개의 퀸이 서로 공격할 수 없도록 퀸을 배치하는 문제
    퀸은 체스판에서 상, 하, 대각선 으로 움직일 수 있다 (움직일 수 있는 길이는 최대 N)
### Graph Coloring (m - coloring problem)
    비방향 그래프에서 인접한 노드들을 서로 다른 색으로 칠하는 모든 방법을 찾는 문제
### Hamiltonian Circuit Problem
    연결된 그래프에서 모든 경로를 한번씩만 지나는 경로를 찾는 문제

## Answer
### Explain Backtracking's flow
    Depth First Search including pruning
### When to use Backtracking
    When you must check all case

---
## Reference
- [이미지 1](https://cinux.tistory.com/14)

