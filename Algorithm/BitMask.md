# BitMask

## 목차
- 비트마스크란?
- 비트 연산자 종류
- 예제
- 장점
- 구현

## You Can Answer
- 비트마스크란 무엇인가요?
- 비트마스크의 장점은 무엇인가요?

</br>

### 비트마스크란?
비트는 데이터를 나타내는 최소 단위로 이진수의 한 자리인 0 또는 1의 값을 가진다. 비트마스크란 컴퓨터의 언어인 이진수를 사용하여 연산이 빠른 점을 이용해 어떠한 정수를 이진수 형태로 표현하는 기법이다.

</br>

### 비트 연산자 종류
- AND연산  a & b
두 개의 비트가 모두 1일 때만 1

![AND](./img/AND.png)

- OR연산 a | b
두 개의 비트 중 하나라도 1이면 1

![OR](./img/OR.png)

- NOT연산 ~a
비트가 1이라면 0, 0이라면 1

![NOT](./img/NOT.png)

- XOR연산 a ^ b
비트가 서로 다르다면 1, 같으면 0

![XOR](./img/XOR.png)

- shift연산 a << b, a >> b
정수 a의 비트를 왼쪽(<<) 혹은 오른쪽(>>)으로 b만큼 움직임

![SHIFT](./img/SHIFT.png)

</br>

### 예제

예를 들어 원소 A, B, C, D 가 있다고 가정한다.
- 집합에 포함 : 1
- 집합에 미포함 : 0
집합S에 아무런 원소가 없다면 아래와 같이 표현이 가능하다.

![INIT](./img/INIT.png)

S = (0000)₂ = 0

</br>

**위 상태에서 A원소를 추가해보자.**
- 원하는 원소를 가리키기 위해서는 shift 연산이 필요하고
- 이를 집합 S에 포함하기 위해서는 or 연산이 필요하다.

1. 원소 가리키기
n 번째 원소를 가리킨다면 아래와 같이 표현할 수 있다.
    - 1 << n

    위 연산을 해석하면 (0001)₂를 n만큼 왼쪽으로 이동하라는 뜻이다. 만약, 1<<3 이라면 왼쪽으로 3칸 이동하여 (1000)₂이 될 것이다.


  2. 집합에 포함시키기
가리킨 원소를 집합에 포함시키기 위해서는 or연산으로 가능하다.

![OR2](./img/OR2.png)

집합에 원소를 포함시키는 방법은 아래와 같다.
포함하고자 하는 원소가 n번째 위치에 있다면 이와 같이 표현 가능하다.
- S |= (1 << n)

</br>

**원소의 포함 여부를 알고 싶다면 어떻게 해야 할까?**
이 방법은 AND 연산을 해야 한다.
집합 S와 알고 싶은 원소(1 << n)를 AND연산을 통해 유무를 판단한다.
- if(S & (1 << n))

</br>

**원소를 삭제하기 위해서는 어떻게 해야 할까?**
1. 삭제하고자 하는 원소를 가리킨다(Shift).
    - (1 << n)
2. 해당 비트를 뒤집는다(NOT).
    - ~(1 << n)
3. 집합 S와 AND 연산을 해준다.
    - S &= ~(1 << n)

AND연산은 둘 다 참일 때만 참이므로 위의 식을 사용한다면 원하는 원소만 0이 되므로 이와 같이 원소를 삭제할 수 있다.

</br>

### 비트마스크의 장점
- 장점
  - 연산이 O(1)에 구현되는 것이 많기 때문에 연산 속도가 빠르다.
  - 코드가 간결하다.
  - 메모리 사용량이 적다.

</br>

### 구현
``` C++
#include <iostream>
#include <bitset>

using namespace std;

int main() {
    std::bitset<16> bs1("1100100011110010");
    std::bitset<16> bs2(0xfff0);
    cout << "bs1        : " << bs1 << endl;
    cout << "bs2        : " << bs2 << endl;
    cout << "NOT bs1    : " << ~bs1 << endl;
    cout << "bs1 AND bs2: " << (bs1 & bs2) << endl;
    return 0;
}
```

출력
```
bs1        : 1100100011110010
bs2        : 1111111111110000
NOT bs1    : 0011011100001101
bs1 AND bs2: 1100100011110000
```

</br>

### Reference
- [비트마스크](https://wtg-study.tistory.com/83)
- [비트마스크란](https://noahlogs.tistory.com/31)