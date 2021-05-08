# Algorithm(알고리즘)
## 목차
1. 알고리즘 이란?
2. 알고리즘의 조건.
3. 알고리즘의 분석 4단계.
4. 좋은 알고리즘의 분석기준!


## You Can Answer
- 알고리즘은 무엇인가요?!
- 좋은 알고리즘은 무엇이고, 이를 어떻게 비교해야 하나요?!

## Algorithm 이란?
- 수학과 컴퓨터 과학, 언어학 또는 관련 분야에서 어떤 문제를 해결하기 위해 정해진 일련의 절차나 방법을 공식화한 형태로 표기한것, 계산을 실행하기 위한 단계적 절차.
- CS(Computer Science)에서 Algorithm은 컴퓨터를 이용하여 주어진 특정 문제를 풀기 위해 짜여진 명령의 순서.


## 알고리즘의 조건.
__알고리즘은 다음조건을 만족해야 한다.__
- __입력__ : 외부에서 제공되는 자료가 0개 이상 존재한다.
- __출력__ : 적어도 2개 이상의 서로 다른 결과를 내어야한다.(즉 모든 입력에 하나의 출력이 나오면 안됨.)
- __명확성__ : 수행 과정은 명확하고 모호하지 않은 명령어로 구성되어야 한다.
- __유한성(종결성)__ : 유한 번의 명령어를 수행후(유한 시간 내)에 종료한다.
- __효율성__ : 모든 과정은 명백하게 실행 가능(검증 가능)한 것이어야 한다.


## 알고리즘의 분석 4단계.
1. __문제 정의__
    -해결하려는 문제가 컴퓨터가 수행할 수 있도록 입력과 출력의 형태로 해결할 수 있도록 문제 정의.
2. __알고리즘 설명__
    -컴퓨터가 수행해야 할 내용은 하나씩 차례대로 정의한 과정.
3. __정확성 증명__
    -입력에 따라 알고리즘이 정상적으로 작동하여 올바른 출력이 나오고 종료되는지 확인.
4. __성능 분석__
- 수행시간(Running Time)/사용공간(Space Consumption)
- 수행연산(Basic Operation)의 횟수를 비교하는 방식으로 성능을 분석함.
- 성능 분석 비교대상
    * 산술연산(add,multiply...)
    * 데이터 입출력(Copy,Move,Save...)
    * 제어 연산(if,while,register...)
- 점근적 표기법 : 공정하고 공평한 비교를 위해 점근적 표기법에 의해 기술.
    * O-notation(Big O 표기)
    * Ω-notation(Omega 표기)
    * θ-notation(Theta 표기)


## 좋은 알고리즘의 분석기준!
+ __정확성__ : 적당한 입력에 대해서 유한 시간내에 올바른 답을 산출하는가를 판단.
+ __작업량__ : 전체 알고리즘에서 수행되는 가장 중요한 연산들만으로 작업량을 측정. 해결하고자 하는 문제의 중요 연산이 여러개인 경우에는 각각의 중요 연산들의 합으로 간주하거나 중요 연산들에 가중치를 두어 계산
+ __기억 장소 사용량__ : 수행에 필요한 저장 공간
+ __최적성__ :그 알고리즘보다 더 적은 연산을 수행하는 알고리즘은 없는가? 최적이란 가장 '잘 알려진' 이 아니라 '가장 좋은'의 의미이다
+ __복잡도__.
    - 시간복잡도(Time Complexity) : 얼마나 시간이 소요되는지?!
    - 공간복잡도(Space Complexity) : 얼마나 공간을 차지하는지?!
    - (자세한 내용은 복잡도 페이지클릭!!)


## (BONUS!!)시간복잡도를 구하는 요령!!
<u>각 문제의 시간복잡도 유형을 빨리 파악할 수 있도록 아래 예를 통해 빠르게 알아 볼수 있다.</u>


- 하나의 루프를 사용하여 단일 요소 집합을 반복 하는 경우 : O (n)

[예시]

    for(int i = 0; i < n;i++){
        //loop job
    }

- 두 개의 다른 루프를 사용하여 두 개의 개별 콜렉션을 반복 할 경우 : O (n + m) -> O (n)

[예시]

    for(int i=0; i < __n__; i++){
        //first loop job
    }
    for(int i=0; i < __m__; i++){
        //second loop job
    }

- 두 개의 중첩 루프를 사용하여 단일 컬렉션을 반복하는 경우 : O (n²)

[예시]

    for(int i = 0; i < __n__; i++){
        for(int j = 0; j < __n__; j++){
            //loop job
        }
    }
- 두 개의 중첩 루프를 사용하여 두 개의 다른 콜렉션을 반복 할 경우 : O (n * m) -> O (n²)

[예시]

    for(int i = 0; i < __n__; i++){
        for(int j = 0; j < __m__;j++){
            //loop job
        }
    }


## Reference
https://blog.chulgil.me/algorithm/