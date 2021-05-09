# Graph Coloring
<!--Table of Contents-->
- What is M-coloring Problem
    - Explanation
    - Example
- Pseudocode
    - m-coloring
    - promising
    
<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- You can write M-coloring Problem code

<!--Contents-->

---
## What is M-coloring Problem
### Explanation
![](https://user-images.githubusercontent.com/70050038/117547313-f09fef00-b069-11eb-94b2-cae4d188522d.png)

    M-coloring Problem은 그림에서 인접한 공간을 모두 구별되는 색으로 칠하는 것이다
    나뉘어진 공간을 노드, 인접한 부분을 간선으로 표현해 비방향 그래프를 그릴 수 있다
    즉, 왼쪽의 그림을 오른쪽 그래프와 같이 표현할 수 있는 것이다
    
    기억해야할 것은 간선이 이어진 노드끼리는 반드시 다른 색으로 칠해져야 하는 것이다
### Example
![](https://user-images.githubusercontent.com/70050038/117547344-14633500-b06a-11eb-9ef7-8929107f12a1.png)

    3 Coloring Problem에 관한 예시를 보자
    
    먼저 V(1)에는 3가지 색 모두 들어 갈 수 있다
    V(1) : Red라 가정하자

    V(2)는 V(1, 3)이 가진 색을 제외한 색을 가질 수 있다 -> Yellow, Blue
    if (V(2) = Red) 판별하지 않는다 -> Backtracking 
    V(2) : Yellow라 가정하자

    V(3)는 V(1, 2, 4)가 가진 색을 제외한 색을 가질 수 있다 -> Blue
    if (V(3) = Red, Yellow) 판별하지 않는다 -> Backtracking
    V(3) : Blue

    V(4)는 V(1, 3)가 가진 색을 제외한 색을 가질 수 있다 -> Yellow
    if (V(4) = Red, Blue) 판별하지 않는다 -> Backtracking
    V(4) : Yellow

## Pseudocode
### m-coloring
```
※Top level call : m_coloring(1);

Vector<> Vcolor;

public static void m_coloring(index i) {
    int color;

    if (promising(i)) {
        if (i == n)
            print( vcolor[1] ... vcolor[n] ); // 결과 출력
        else {
            for (color = 1; color <= m; color++) {  // 1 color ~ m color 까지
                vcolor[i+1] = color;
                m_coloring(i+1);
            }
        }
    }
}
```
### promising
```
public static boolean promising(index i) {
    index j; bool switch;

    switch = true;
    j = 1;
    
    while (j < i && switch) {
        if (W[i][j] &&  vcolor[i] == vcolor[j]) // i와 인접한 노드 중 같은 색의 노드가 있는지 체크
            switch = false;
        j++;
    }

    return switch;
}
```