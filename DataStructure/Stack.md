# 스택 (Stack)

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 스택이란 무엇인가요?
- 스택은 언제 사용되나요?
- 스택은 어떻게 사용하나요?

---

## 스택이란?
* 한 쪽 끝에서만 자료를 넣고 뺄 수 있는 LIFO(Last In First Out) 형식의 자료 구조
* 즉, 선형 구조로 책을 쌓는 것처럼 차곡차곡 쌓아 올린 형태의 자료구조를 말한다.


### 1) 스택의 사용 사례
* 재귀 알고리즘
  + 재귀적으로 함수를 호출해야 하는 경우에 임시 데이터를 스택에 넣어준다.
  + 재귀함수를 빠져 나와 퇴각 검색(backtrack)을 할 때는 스택에 넣어 두었던 임시 데이터를 빼 줘야 한다.
  + 스택은 이런 일련의 행위를 직관적으로 가능하게 해 준다.
  + 또한 스택은 재귀 알고리즘을 반복적 형태(iterative)를 통해서 구현할 수 있게 해준다

* 웹 브라우저 방문기록 (뒤로 가기) : 가장 최근에 열린 페이지부터 다시 보여준다.
* 역순 문자열 만들기 : 가장 최근에 입력된 문자부터 출력한다.
* 후위 표기법 계산
* 실행 취소 (undo) : 가장 최근에 실행한 명령어를 취소한다.


### 2) java 라이브러리 스택 관련 메서드
* push(E item)
  + 해당 item을 Stack의 top에 삽입
  + Vector의 addElement(item)과 동일
* pop()
  + Stack의 top에 있는 item을 삭제하고 해당 item을 반환
* peek()
  + Stack의 top에 있는 item을 삭제하지않고 해당 item을 반환
* empty()
  + Stack이 비어있으면 true를 반환 그렇지않으면 false를 반환
* search(Object o)
  + 해당 Object의 위치를 반환
  + Stack의 top 위치는 1, 해당 Object가 없으면 -1을 반환


### 3) 스택의 구현  (java)
* 스택의 구현 방법은 배열을 사용하는 것과 연결 리스트를 사용하는 것 두가지가 있으면 각각 장단점이 존재한다.
* 배열은 데이터들이 순차적으로 나열되어 있기 때문에 구현이 쉽고, 원하는 데이터의 접근 속도가 빠르다는 장점을 갖고 있다.
하지만 단점으로는 데이터 최대 개수를 미리 정해야 하고, 데이터의 삽입과 삭제에 있어 매우 비효율적이다.
* 연결 리스트의 구조는 배열과 다르게 데이터들이 순차적으로 나열되어 있지 않고, 노드로 연결리스트를 구성한다.
이러한 구조 때문에 데이터의 최대 개수가 한정되어 있지 않고, 데이터의 삽입 삭제가 용이하다는 장점이 있다.
하지만 단점으로는 배열과 달리 한번에 원하는 데이터의 접근이 불가능하다.

1. 배열을 통한 스택 구현
```java
public class ArrStack {
	private int top; //마지막 위치 (삽입,삭제가 이루어질 위치)
	private int maxSize;
	private Object[] stackArray;

	//생성자 : 프로퍼티 초기
	public ArrStack(int size){
		this.top = -1;
		this.maxSize = size;
		this.stackArray = new Object[maxSize];
	}

	//스택이 비어있는지 확인
	public boolean isEmpty(){
		return (top==-1);
	}

	//데이터 삽입
	public void push(Object item){
		//스택에 가득찼을 때 예외처리
		if(top >= maxSize-1) throw new ArrayIndexOutOfBoundsException((top+1)+">="+maxSize);
		stackArray[++top] = item;
	}

	//데이터 읽기
	public Object peek(){
		//스택이 비어있을 때 예외처리
		if(isEmpty()) throw new ArrayIndexOutOfBoundsException(top);
		return stackArray[top];
	}

	//데이터 삭제 : 삭제된 데이터 반환(백업용도)
	public Object pop(){
		Object item = peek();
		top--;
		return item;
	}

}
```
&nbsp;&nbsp;  클래스의 생성자를 통해 각 인스턴스 변수 등을 초기화한다.
&nbsp;&nbsp;  배열의 특성을 고려하여 isEmpty, push, peek, pop과 같은 각 메서드들을 구현한다.
<br>

2. 연결 리스트를 통한 스택 구현 - 연결리스트로 사용 할 노드 class
```java
public class Node {
	private Object data;
	private Node nextNode;

	public Node(Object data){
		this.data = data;
		this.nextNode = null;
	}

	//해당 노드를 원하는 노드(Node top)와 연결해주는 메소드
	public void linkNode(Node top){
		this.nextNode = top;
	}

	//해당 노드의 데이터를 가져오는 get메소드
	public Object getData(){
		return this.data;
	}

	//해당 노드와 연결된 노드를 가져오는 get메소드
	public Node getNextNode(){
		return this.nextNode;
	}

}
```
&nbsp;&nbsp;  data와 다음 node를 가리키는 주소로 구성되어있는 node를 구현
&nbsp;&nbsp;  스택 메서드를 구현할 때 필요한 노드와 관련된 메서드를 구현한다.

3. 연결 리스트를 통한 스택 구현 - 스택 메서드를 구현한 class
```java
public class ListStack {
	private Node top;

	public ListStack(){
		top = null;
	}

	public boolean isEmpty(){
    return (top==null);
	}

	public void push(Object item){
		Node newNode = new Node(item);
		newNode.linkNode(top);
    top = newNode;
	}

	public Object peek(){
		return top.getData();
	}

	public Object pop(){
		if(isEmpty()) throw new ArrayIndexOutOfBoundsException();
    else{
			Object item = peek();
			top = top.getNextNode();
      return item;
		}
	}

}
```
&nbsp;&nbsp;  배열로 구현된 스택과 같이 연결리스트의 특성을 고려하여 isEmpty, push, peek, pop과 같은 각 메서드들을 구현한다.


## Reference
- [자료구조] 스택(Stack)이란(https://gmlwjd9405.github.io/2018/08/03/data-structure-stack.html)
- [알고리즘] 2.1. 자료구조 : 스택(Stack) 이해하기(https://monsieursongsong.tistory.com/4)
