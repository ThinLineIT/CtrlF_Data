# 동적 계획법(Dynamic Programming)

- 동적계획법이란?
- 특징
- 장단점
- 분할 정복(Divide and Conquer)과의 차이점
- 종류
- 구현

## You Can Answer
- 동적계획법이란 무엇인가요?
- 동적계획법의 특징에 대해 설명해보세요.
- 분할 정복(Divide and Conquer)과 다른 점이 무엇인가요?
-----------------------

## 동적계획법이란?
큰 문제를 작은 문제로 나누어 작은 부분 문제들을 해결한 것을 가지고 큰 부분 문제들을 해결하여 최종적으로 전체 문제를 해결하는 알고리즘


## 특징
- 상향식 접근법
가장 작은 문제들로부터 답을 구해가며 전체 문제의 답을 찾는 방식
- Memoization 기법 사용
  - Memoization이란?
  동일한 계산을 반복할 때 이전에 계산한 값을 메모리에 저장함으로써 동일한 계산의 반복 수행을 제거하는 기술
- 부분 문제 중복

## 장단점
- 장점
  - 프로그램을 구현할 때 필요한 모든 가능성을 고려하여 항상 최적의 결과를 얻을 수 있다.
  - 메모리에 저장된 값을 사용하므로 빠른 속도로 문제를 해결할 수 있다.
- 단점
  - 모든 가능성에 대한 고려가 불충분할 경우 최적의 결과를 보장할 수 없다.
  - 다른 방법론에 비해 많은 메모리 공간을 요구한다.

## 분할 정복(Divide and Conquer)과의 차이점
큰 문제를 작은 문제로 나누는 부분에 있어서는 공통점이 있지만 다이나믹 프로그래밍은 작은 문제(동일한 계산)가 반복적으로 나타나고 분할정복은 반복이 일어나지 않는다.

## 종류
- 탑다운(Top-Down) 방식
  - 큰 문제를 해결하기 위해 작은 문제를 호출하는 방식으로 '하향식'이라고도 한다.
- 바텀업(Bottom-Up) 방식
  - 가장 작은 문제들부터 답을 구해가며 전체 문제의 답을 찾는 방식으로 '상향식'이라고도 한다.

## 구현(파이썬)
### 탑다운 방식
```python
# 메모이제이션하기 위한 리스트 초기화
memoization = [0] * 100


# 피보나치 함수를 재귀함수로 구현 (Top-down DP)
def fibo(x):
    if x == 1 or x == 2:
        return 1
    # 이미 계산한 적 있으면 그대로 반환
    if memoization[x] != 0:
        return memoization[x]
    # 계산한 적 없으면 점화식에 따라 피보나치 결과 반환
    memoization[x] = fibo(x - 1) + fibo(x - 2)
    return memoization[x]


print(fibo(6))
```
fibo(6)을 구하는 과정은 아래 그림과 같다.
![DP_image](./img/DP_image.png)
메모이제이션을 사용하여 색칠된 노드만 방문하게 된다.

### 바텀업 방식
```python
dp = [0] * 100
dp[1] = 1
dp[2] = 1
n = 99

# 피보나치 수열 반복문으로 구현(Bottom-Up DP)
for i in range(3, n + 1):
    dp[i] = dp[i - 1] + dp[i - 2]

print(dp[n])
```
---
## Reference
- [다이나믹 프로그래밍(Dynamic Programming)](https://velog.io/@kimdukbae/%EB%8B%A4%EC%9D%B4%EB%82%98%EB%AF%B9-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-Dynamic-Programming)
- [알고리즘 - Dynammic Programming(동적프로그래밍)이란?](https://galid1.tistory.com/507)
