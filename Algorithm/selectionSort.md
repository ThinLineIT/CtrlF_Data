# 선택 정렬(selection sort)
<!--Table of Contents-->
- 선택정렬 이란?
  - 정의
  - 복잡도  
- 구현



<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 선택정렬이란 무엇인가요 ? 
- 선택정렬은 어떻게 구현할 수 있나요 ? 

<!--Contents-->

---
## 선택 정렬(selection sort) 란?
* 기본 정렬 알고리즘 중 하나로 다음과 같은 순서를 반복하며 정렬하는 알고리즘
    1) 주어진 데이터 중 최소값을 찾는다
    2) 해당 최소값을 데이터 맨 앞에 위치한 값과 바꾼다
    3) 그 다음 데이터도 같은 방식으로 최소값과 교체한다.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
![selection_sort_gif](https://upload.wikimedia.org/wikipedia/commons/9/94/Selection-Sort-Animation.gif)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
By <a href="https://en.wikipedia.org/wiki/User:Joestape89" class="extiw" title="en:User:Joestape89">Joestape89</a>, <a href="http://creativecommons.org/licenses/by-sa/3.0/" title="Creative Commons Attribution-Share Alike 3.0">CC BY-SA 3.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=3330231">Link</a>

* 시간 복잡도  
  + 두 개의 for 루프의 실행 횟수 
    + 외부 루프 :  (n-1) 번
    + 내부 루프 : n-1, n-2, ... , 2, 1 번
  + 교환 횟수 = 외부 루프의 실행 횟수 (즉, 상수 시간 작업)  
  
 ![수식](https://latex.codecogs.com/gif.latex?%7B%5Cdisplaystyle%20C_%7Bmin%7D%3DC_%7Bave%7D%3DC_%7Bmax%7D%3D%5Csum%20_%7Bi%3D1%7D%5E%7BN-1%7D%7BN-i%7D%3D%7B%5Cfrac%20%7BN%28N-1%29%7D%7B2%7D%7D%3DO%28n%5E%7B2%7D%29%7D)

## 구현
### 데이터가 두 개 일 때,
    예 ) data_list = [5,1]
        data_list[0] > data_list[1] 이므로 값 서로 교환 

### 데이터가 네 개 일 때,
    예 ) data_list = [8,6,5,1]
        첫 번째 실행 -> [1,6,5,8]
        두 번째 실행 -> [1,5,6,8]
        세 번째 실행 -> 변화 없음
### 구현 (파이썬)
    ''' python
    def selection_sort(data):
    for stand in range(len(data) - 1):
        lowest = stand
        for index in range(stand + 1, len(data)):
            if data[lowest] > data[index]:
                lowest = index
        data[lowest], data[stand] = data[stand], data[lowest]
    return data
    '''
### 구현 (C 언어)
    ''' c
    void selectionSort(int *list, const int n)
    {
    int i, j, indexMin, temp;

    for (i = 0; i < n - 1; i++)
    {
        indexMin = i;
        for (j = i + 1; j < n; j++)
        {
            if (list[j] < list[indexMin])
            {
                indexMin = j;
            }
        }
        temp = list[indexMin];
        list[indexMin] = list[i];
        list[i] = temp;
        }
    } 
    '''
## 구현 (JAVA)
    '''{.java}
    void selectionSort(int[] list) {
    int indexMin, temp;

    for (int i = 0; i < list.length - 1; i++) {
        indexMin = i;
        for (int j = i + 1; j < list.length; j++) {
            if (list[j] < list[indexMin]) {
                indexMin = j;
            }
        }
        temp = list[indexMin];
        list[indexMin] = list[i];
        list[i] = temp;
    }
    }
    '''

---
## Reference
- [Selection sort](https://en.wikipedia.org/wiki/Selection_sort)
- [선택 정렬](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC)
