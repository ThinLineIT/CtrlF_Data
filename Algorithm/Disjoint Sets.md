# 서로소 집합 알고리즘 (Disjoint Sets)
- 서로소 집합의 정의
- 서로소 집합 자료구조
- 서로소 집합 알고리즘 소스코드 (Python)
- 서로소 집합을 활용한 사이클 판별
- 서로소 집합을 활용한 사이클 판별 소스코드 (Python)


## You can answer
- 서로소 집합이 무엇인가?
- 서로소 집합 자료구조는 무엇인가?
- 서로소 집합을 활용한 사이클 판별의 소스코드를 작성해보아라.
-----------------------

## 서로소 집합의 정의
- 수학에서 서로소 집합이란 공통 원소가 없는 두 집합을 의미한다. 예를 들어 집합 {1,2}와 집합{3,4}는 서로소 관계이다. 반면에 집합{1,2}와 집합{2,3}은 2라는 원소가 두 집합에 공통적으로 포함되어 있기 때문에 서로소 관계가 아니다. 
서로소 집합 자료구조란 **서로소 부분 집합들로 나누어진 원소들의 데이터를 처리하기 위한 자료구조**라고 할 수 있다. 서로소 집합 자료구조는 union과 find 이 2개의 연산으로 조작할 수 있다.
서로소 집합 자료구조는 union-find(합치기 찾기) 자료구조라고 불리기도 한다. 

- union(합집합) 연산: 2개의 원소가 포함된 집합을 하나의 집합으로 합치는 연산.
- find(찾기) 연산: 특정한 원소가 속한 집합이 어떤 집합인지 알려주는 연산.

</br>

## 서로소 집합 자료구조
- 서로소 집합 자료구조를 구현할 때는 **트리 자료구조**를 이용하여 집합을 표현한다. 

- 서로소 집합 계산 알고리즘</br>
1.union(합집합) 연산을 확인하여, 서로 연결된 두 노드 A,B를 확인한다.</br>
ⅰ. A와 B의 루트 노드 A',B'를 각각 찾는다.</br>
ⅱ. A'를 B'의 부모 노드로 설정한다 (B'가 A'를 가리키도록 한다 = 부모 노드로 설정한다)</br>
2.모든 union(합집합) 연산을 처리할 때까지 1번 과정을 반복한다.

- union 연산들은 그래프 형태로 표현될 수도 있다. 각 원소는 그래프에서의 노드로 표현되고, '같은 집합에 속한다'는 정보를 담은 union 연산들은 간선으로 표현된다.
다른 두 원소에 대해 union을 수행해야 할 때는 각각 루트 노드를 찾아서 더 큰 루트 노드가 더 작은 루트 노드를 가리키도록 하면 된다. 

</br>

## 서로소 집합 알고리즘 소스코드 (Python)
```Python
#서로소 집합 알고리즘 소스코드
def find_parent(parent,x):
    if parent[x]!=x:
        parent[x]=find_parent(parent,parent[x])
    
    return parent[x]
#두 원소가 속한 집합을 합치기
def union_parent(parent,a,b):
    a=find_parent(parent,a)
    b=find_parent(parent,b)
    if a<b:
        parent[b]=a
    else:
        parent[a]=b
    
#노드의 개수와 간선(union 연산)의 개수 입력받기
v,e=map(int,input().split())
parent=[0]*(v+1) #부모 테이블 초기화
#부모 테이블상에서, 부모를 자기 자신으로 초기화
for i in range(1,v+1):
    parent[i]=i
#union 연산을 각각 수행
for _ in range(e):
    a,b = map(int,input().split())
    union_parent(parent,a,b)
#각 원소가 속한 집합 출력
print('각 원소가 속한 집합: ',end='')
for i in range(1,v+1):
    print(find_parent(parent,i),end=' ')
print()
#부모 테이블 내용 출력
print('부모 테이블: ',end='')
for i in range(1,v+1):
    print(parent[i],end=' ')
```
- 서로소 집합 알고리즘 실행 과정
![DisjointSets_1](.\img\DisjointSets_1.png)

- 이 알고리즘에서 유의할 점은 union 연산을 효과적으로 수행하기 위해 '부모 테이블'을 항상 가지고 있어야 한다는 점이다. 또한 루트 노드를 즉시 계산할 수 없고, 부모 테이블을 계속해서 확인하며 거슬러 올라가야 한다.

<br/>

## 서로소 집합을 활용한 사이클 판별
- 서로소 집합은 다양한 알고리즘에 사용될 수 있다. 특히 서로소 집합은 무방향 그래프 내에서의 사이클을 판별할 때 사용할 수 있다는 특징이 있다. 방향 그래프에서의 사이클 여부는 DFS를 이용하여 판별할 수 있다.

</br>

## 서로소 집합을 활용한 사이클 판별 소스코드 (Python)

- 이 알고리즘은 간선에 방향성이 없는 무방향 그래프에서만 적용 가능하다. 
```
# 서로소 집합 알고리즘 소스코드
def find_parent(parent,x):
    if parent[x]!=x:
        parent[x]=find_parent(parent,parent[x])
    return parent[x]
def union_parent(parent,a,b):
    a = find_parent(parent,a)
    b = find_parent(parent,b)
    if a<b:
        parent[b]=a
    else:
        parent[a]=b
v,e = map(int,input().split())
parent = [0]*(v+1)
for i in range(1,v+1):
    parent[i]=i
cycle = False
for _ in range(e):
    a,b=map(int,input().split())
    
    if find_parent(parent,a)==find_parent(parent,b):
        cycle = True
        break
    else:
        union_parent(parent,a,b)
    
if cycle:
    print("사이클이 발생했습니다.")
else:
    print("사이클이 발생하지 않았습니다.")
        
```

<br/>

## Reference
- 이것이 취업을 위한 코딩 테스트다 with 파이썬 (나동빈 저자)
- [서로소 집합 알고리즘 실행 과정 이미지 출처](https://travelbeeee.tistory.com/369)