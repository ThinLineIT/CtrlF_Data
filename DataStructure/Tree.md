# Tree
## 목차
- 트리(Tree)의 개념 및 사용
- 트리(Tree)의 용어정리
- 트리(Tree)의 특징
- 트리(Tree)의 종류


## You can answer
- 트리가 무엇인가요?!
- 트리(Tree)의 장점이 뭔가요?!
- 어디에 트리(Tree)가 사용되나요?!

---
## Tree의 개념 및 사용
### 트리(Tree)
    나무는 하나의 뿌리에서 여러 가지로 뻗어나가는 구조이다.
    CS에서의 Tree란 그래프의 일종으로, 어떤 노드들의 집합으로 노드들은 각 서로 다른 자식을 가지며 이 때 각 노드는 재사용 되지 않는 구조이다.
    또한 앞서 설명한 Stack, Queue, LinkedList, Array는 선형적인 구조이지만 Tree는 비선형 구조이다.

### 트리의 사용
- 주요 용도 : 데이터 검색(탐색)
- 장점 : 탐색 속도를 개선할 수 있음.

BinarySearchTree 와 정렬된 Array의 탐색 속도 비교
![CompareTime](./img/CompareTime.gif)



## Tree용어 간단정리
1. 노드(Node) : 트리는 노드들의 집합으로 트리를 구성하는 것은 보통(value)값과 부모 자식의 정보를 가지고 있다.
2. 엣지(Edge) : 엣지는 노드들을 연결하는 간선으로 부모 노드와 자식 노드를 연결하게 된다.
3. 루트(Root Node) : 가장 상위 노드(부모를 가지지 않는다.)
4. 리프(Leaf Node) : 가장 하위 노드(자식을 가지지 않는다.)
5. 자식,부모 노드(Child,Parent) : 하위(자식), 상위(부모) 노드
6. 형제 노드(Sibling,Brother) : 깊이가 같은 등급의 노드
7. 깊이(Depth) : 루트에서 해당 노드까지의 경로의 갯수
8. 차수(Degree) : 어떠한 노드의 자식의 갯수.

![TermsOfTree](./img/TermsOfTree.png)

## Tree의 특징
### Tree구조에는 몇가지 특징들이 존재한다.
1. Tree의 서로다른 임의의 두 노드에 대해 두 노드를 연결하는 경로는 유일하다.
2. Tree에는 사이클을 가지는 노드 집합이 존재하지 않는다.
3. Tree는 반드시 하나의 Root가 존재한다.(부모 노드가 존재하지 않는 노드)


## Tree의 종류
### 이진트리(Binary Tree) - 자식노드가 최대 두 개인 노드들로 구성된 트리.
+ 정 이진트리(Full Binary Tree) : Leaf노드를 제외한 __모든 노드가 자식노드를 2개 또는 0개를 가짐.__

+ 완전이진트리(Complete Binary Tree) : 부모->왼쪽자식->오른쪽자식 순으로 채워지는 트리. 마지막 레벨을 제외하고 모든 노드가 가득차 있어야 한다. 또한 마지막 레벨의 노드에서도 왼쪽으로 몰려있어야 한다.

+ 포화이진트리(Balanced Binary Tree) : 모든 Leaf노드의 레벨이 동일하고 모든 레벨이 가득 채워져 있는 이진트리.

![SortOfBinaryTree](./img/SortOfBinaryTree.png)

### 이진탐색트리(Binary Search Tree)
    각 노드는 유일한 값을 가지고 있으며, 해당 노드의 왼쪽 서브트리는 그 노드보다 작은 값을 가진 노드들로, 오른쪽 서브트리는 그 노드의 값보다 큰 값을 가진 노드들로 구성된 트리.

+ 균형이진탐색트리(Balanced Binary Search Tree) : 완전 균형 트리의 형태를 취하고 있는 이진탐색트리.
+ 힙(Heap) : 부모 자식간의 대소 관계는 정의되어 있으나 형제간의 대소관계는 정의 되어있지 않은 완전 이진트리.

![SortOfBinarySearchTree](./img/SortOfBinarySearchTree.png)

## BONUS
??? : 공돌이가 좋아하는 랜덤게임!! 무슨 게임 게임 스타트!!!

공돌이 : 소주뚜껑 숫자(1~50) 맞추기!

### Case1

??? : 25!! __(BinarySearchTree 를 아는사람)__

__결과__ :  ???=정신멀쩡

### Case2

??? : 1!! __(Array 탐색을 좋아하는 사람)__

__결과__ : ???=만취

BinarySearchTree는 술에 취하지 않는 방법 중 하나입니다!! 기억해 두세요^^

---
## Reference
- [트리의 개념](https://smujihoon.tistory.com/153)
- [트리의 속도비교 및 코드](https://devraphy.tistory.com/45)
