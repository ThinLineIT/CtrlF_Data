# N-Queens Problem
<!--Table of Contents-->
- What is N-Queens algorithm
    - Explanation
    - Example : 8-Queens Problem
- Pseudo-code
    - queens
    - promising
    
<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- Understand N-Queens Problem's Pseudo-code

<!--Contents-->

---
## What is N-Queens algorithm
### Explanation
    [N x N] 체스판 위에 임의의 퀸(1)이 올려져 있다고 가정해보자
    퀸(2)는 퀸(1)이 이동할 수 있는 위치를 제외한 공간에 위치할 수 있고,
    퀸(3)은 퀸(1), 퀸(2)이 이동할 수 있는 위치를 제외한 공간에 위치할 수 있다
    마찬가지로 퀸(4)은 퀸(1), 퀸(2), 퀸(3)이 이동할 수 있는 위치를 제외한 공간에 위치할 수 있다
    계속해서 퀸(N)은 퀸(1) - 퀸(N-1)이 이동할 수 있는 위치를 제외한 공간에 위치 할 수 있다
    N Queens algorithm은 이렇게 [N x N] 체스판 위에 N개의 퀸이 서로를 공격할 수 없는 위치에 
    위치시키는 경우의 수를 구하는 문제이다

    
### Example : 8-Queens Problem
![이미지 1](https://user-images.githubusercontent.com/70050038/117534088-ec081600-b02a-11eb-98e6-732691603e52.png)

    ☆예시는 1행부터 8행까지 순서대로 하나의 행에 하나의 퀸을 배치하는 흐름으로 진행된다☆

    0) 먼저 퀸(0)은 3열에 배치가 되었다고 가정해보자

    1) 퀸(1)은 퀸(0)이 공격할 수 있는 위치를 제외한 (0, 1, 5, 6, 7)열에 위치할 수 있다
    즉, 퀸(1)이 (2, 3, 4)열에 위치하는 경우는 고려하지 않는다 -> Backtracking    
    퀸(1)은 6열에 배치되었다고 가정

    2) 퀸(2)는 퀸(0), 퀸(1)이 공격할 수 있는 위치를 제외한 (0, 2, 4)열에 위치할 수 있다
    즉, (1, 3, 5, 6, 7)열에 위치하는 경우는 고려하지 않는다 -> Backtracking
    퀸(2)은 2열에 배치되었다고 가정

    3) 퀸(3)은 퀸(0, 1, 2)이 공격할 수 있는 위치를 제외한 (5, 7)열에 위치할 수 있다
    즉, (0, 1, 2, 3, 4, 6)열에 위치하는 경우는 고려하지 않는다 -> Backtracking
    퀸(3)은 7열에 배치되었다고 가정

    4) 퀸(4)은 퀸(0, 1, 2, 3)이 공격할 수 있는 위치를 제외한 (4)열에만 위치할 수 있다
    즉, (0, 1, 2, 3, 5 , 6, 7)열에 위치하는 경우는 고려하지 않는다 -> Backtracking

    ...

    8) 퀸(8)은 퀸(0, 1, 2, 3, 4, 5, 6, 7)이 공격할 수 있는 위치를 제외한 5열에만 위치할 수 있다

    ※만약, 퀸(N)을 위치시킬 때, 퀸(0, 1, 2, ... , N - 1)이 퀸(N)이 위치할 수 있는 모든 위치를 공격한다면, Backtracking   




## Pseudo-code
### queens
```
public static void queens(index i) {
    index j;

    if (promising(i)) {
        if (i == n) // i = N 이면 N Queens Problem 조건 성립 -> 결과 출력
            print : N element of result set
        else {
            for (j = 1; j <= n; j++) {
                column[i + 1] = j;
                queens(i + 1);  // i = N 일때까지 증가
            }
        }
    }
}
```
### promising
```
public static boolean promising (index i) {
    index k; boolean switch;

    k = 1;
    switch = true;

    while (k < i && switch) {   // i번째 퀸이 1번째 퀸 ~ (i - 1)번째 퀸과 겹치는지 확인    
        if (column[i] == column[k] || abs(column[i] - column[k]) == i - k)
        // 같은 열에 놓였는가 || 대각선 라인에 놓였는가
            switch = false;
        k++
    }

    return switch;
}
```

---
## Reference
- [이미지 1](https://callumfrance.github.io/n-Queens/)

