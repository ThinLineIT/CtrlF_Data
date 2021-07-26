# Two Pointer
<!--Table of Contents-->
## 목차
- Two Pointer란?
- Two Pointer의 장점
- Two Pointer 예시

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- Two Pointer의 알고리즘
- Two Pointer의 시간복잡도

<!--Contents-->

---
##  Two Pointer란?
Two Pointer 알고리즘이란 인덱스를 가리키는 두 개의 변수(포인터)를 선언하여 사용하는 특징이 있어 투 포인터라 불리며, 주로 배열 안에 있는 값들을 연속해서 더하거나 연산하는 경우에 사용됩니다.  
배열의 인덱스를 가리키는 left와 right를 생성한 후 특정 규칙에 의해 각 포인터를 움직여 배열을 탐색해 문제를 해결합니다.  

예를 들어 N개의 숫자가 들어있는 배열이 주어질 때, 부분 집합의 합이 특정 숫자인 M인 경우의 수를 구하는 문제에 적용한다면, 배열의 인덱스를 가리키는 left와 right를 생성하여 다음과 같은 규칙에 따라 움직입니다.  

1. left와 right가 첫번째 원소의 인덱스를 가리키도록 합니다.

2. 현재 부분 합이 M보다 크거나 같다면 start를 1 증가시킵니다.

3. 현재 부분 합이 M보다 작다면 right를 1 증가시킵니다.

4. 현재 부분 합이 M과 같다면 카운트합니다.

## Two Pointer의 장점
앞선 예시와 동일한 경우 완전 탐색을 적용하여 해결할 시 N만큼 반복문 2번을 돌리게 되므로 O(n²)의 시간 복잡도가 걸리게 됩니다.

하지만, 투 포인터를 사용한다면 한 번 답이 될 수 없다고 판명된 이후의 값들을 더 이상 탐색하지 않으므로 시간 복잡도가 O(n)이 되어 완전 탐색에 비해 효율이 훨씬 좋아집니다.

따라서, 시간제한이 넉넉하지 않아 완전 탐색을 적용하여 해결하지 못하는 문제일 경우, 투 포인터를 적용하여 해결할 수 있습니다.  

다만, 이 경우엔 배열의 원소가 자연수여야 하는 제약조건이 따르게 되는데 이는 right를 증가시키면 합이 증가되고 left를 증가시키면 합이 감소하기 때문에 이런 제약조건이 생기게됩니다. 그렇기 때문에 투 포인터를 사용할 땐 꼭 문제의 조건을 확인하고 사용해야 하며, 포인터를 움직이는 규칙 또한 문제에 맞게 변경하여 사용해야합니다.


## Two Pointer 예시
N개의 숫자가 들어있는 배열이 주어질 때, 부분 집합의 합이 특정 숫자인 M인 경우의 수를 구하세요.  
여기서 N = 5, M = 5인 경우의 예시 코드입니다.
```python
n = 5  # 데이터 개수
m = 5  # 찾고자 하는 부분합
data = [1, 2, 3, 2, 5]

count = 0
interval_sum = 0
right = 0

# start를 차례대로 증가시키며 반복
for left in range(n):
    # end를 가능한 만큼 이동시키기
    while interval_sum < m and right < n:
        interval_sum += data[end]
        right += 1
    # 부분합이 m일 때 카운트 증가
    if interval_sum == m:
        count += 1
    interval_sum -= data[start]

print(count)  # 3
```

---
## Reference
- [[Algorithm] 투 포인터](https://hellominchan.tistory.com/252)
- [[기타 알고리즘] 투 포인터](https://kom-story.tistory.com/132)
