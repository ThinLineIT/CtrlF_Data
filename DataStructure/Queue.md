# Queue (Stack)

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 큐란 무엇인가요?
- 큐는 언제 사용되나요?
- 큐는 어떻게 사용하나요?

---

## 큐란?
* 먼저 집어 넣은 데이터가 먼저 나오는 FIFO (First In First Out) 형식의 자료구조이다.
* 선형구조로 큐과 달리 데이터가 들어온 순서대로 먼저 나오는 형태의 자료구조를 말한다.


### 1) 큐의 사용 사례
* 너비 우선 탐색(BFS, Breadth-First Search) 구현
  + 처리해야 할 노드의 리스트를 저장하는 용도로 큐(Queue)를 사용한다.
  + 노드를 하나 처리할 때마다 해당 노드와 인접한 노드들을 큐에 다시 저장한다.
  + 노드를 접근한 순서대로 처리할 수 있다.
* 캐시(Cache) 구현
* 우선순위가 같은 작업 예약 (인쇄 대기열)
* 선입선출이 필요한 대기열 (티켓 카운터)
* 콜센터 고객 대기시간
* 프린터의 출력 처리
* 윈도 시스템의 메시지 처리기
* 프로세스 관리


### 2) java 라이브러리 큐 관련 메서드
* boolean add(E item)
  + 해당 item을 Queue에 삽입
  + 삽입에 성공하면 true를 반환, 삽입할 공간이 없는 경우는 예외(IllegalStateException)를 발생
* E element()
  + Queue의 Head에 있는 item을 삭제하지않고 해당 item을 반환
  + 만약 Queue가 비어있으면 예외를 발생
* E remove()
  + Queue의 Head에 있는 item을 삭제하고 해당 item을 반환
  + 만약 Queue가 비어있으면 예외를 발생
* boolean offer(E item)
  + add(e)와 동일한 기능
  + 삽입에 성공하면 true를 반환, 실패하면 false를 반환
* E peek()
  + element()와 동일한 기능
  + 만약 Queue가 비어있으면 null을 반환
* E poll()
  + remove()와 동일한 기능
  + 만약 Queue가 비어있으면 null을 반환


### 3) 큐의 구현  (java)
* 큐의 구현 방법은 배열을 사용하는 것과 연결 리스트를 사용하는 것 두가지가 있으면 각각 장단점이 존재한다.
* 배열은 데이터들이 순차적으로 나열되어 있기 때문에 구현이 쉽고, 원하는 데이터의 접근 속도가 빠르다는 장점을 갖고 있다.
하지만 단점으로는 데이터 최대 개수를 미리 정해야 하고, 데이터의 삽입과 삭제에 있어 매우 비효율적이다.
* 연결 리스트의 구조는 배열과 다르게 데이터들이 순차적으로 나열되어 있지 않고, 노드로 연결리스트를 구성한다.
이러한 구조 때문에 데이터의 최대 개수가 한정되어 있지 않고, 데이터의 삽입 삭제가 용이하다는 장점이 있다.
하지만 단점으로는 배열과 달리 한번에 원하는 데이터의 접근이 불가능하다.

1. 배열을 통한 큐 구현
```java
public class ArrQueue {
	private int front;
	private int rear;
	private int size;
	private Object[] arrQueue;

	public ArrQueue(int size){
		this.front = 0;
		this.rear = -1;
		this.size = size;
		this.arrQueue = new Object[this.size];
	}

	//Queue 배열이 꽉 차있는지 확인
	public boolean isFull(){
		if(rear >= size-1) return true;
		else return false;
	}

	//Queue 배열이 비어있는지 확인
	public boolean isEmpty(){
		if(rear < front) return true;
		else return false;
	}

	//원하는 데이터를 추가하는 메서드
	public void enQueue(Object item){
		if(isFull()) throw new ArrayIndexOutOfBoundsException();
		this.rear++;
		arrQueue[rear] = item;
	}

	//front에 위치한 데이터가 무엇인지 확인하는 메서드
	public Object peek(){
		return arrQueue[front];
	}

	//Queue의 front에 위치한 데이터 삭제
	public Object deQueue(){
		if(isEmpty()) throw new ArrayIndexOutOfBoundsException();
		Object backUpItem = peek();
		this.front++;
		return backUpItem;
	}

}
```
&nbsp;&nbsp;  클래스의 생성자를 통해 각 인스턴스 변수 등을 초기화한다.
&nbsp;&nbsp;  배열의 특성을 고려하여 isEmpty(), isFull(), peek(), enQueue(), deQueue() 같은 각 메서드들을 구현한다.
&nbsp;&nbsp;  배열로 큐를 구현하게 되면 큐의 크기를 미리 정해야 하고, 그 크기를 넘어서면 에러가 발생한다.
<br>

2. 연결 리스트를 통한 큐 구현 - 연결리스트로 사용 할 노드 class
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
&nbsp;&nbsp;  큐 메서드를 구현할 때 필요한 노드와 관련된 메서드를 구현한다.

3. 연결 리스트를 통한 큐 구현 - 큐 메서드를 구현한 class
```java
public class ListQueue {
	private Node front;
	private Node rear;

	public ListQueue(){
		this.front = null;
		this.rear = null;
	}

	public boolean isEmpty(){
		if (front == null) return true;
		else return false;
	}

	public void enQueue(Object item){
		Node newNode = new Node(item);

		//추가하는 Node가 처음인지 아닌지
		//처음이면 rear,front모두 추가한 노드로 초기화
		//처음이 아니면 이전에 있는 노드와 새로 추가된 노드를 연결
		//rear를 새로 추가한 노드로 초기화
		if(isEmpty()){
			rear = newNode;
			front = newNode;
		}else{
			rear.linkNode(newNode);
			rear = newNode;
		}
	}

	public Object peek(){
		return front.getData();
	}

	public Object deQueue(){
		Object backUpItem = peek();

		//front를 삭제한 다음 Node로 초기화
		front = front.getNextNode();

		//삭제한 노드가 마지막 노드였을 경우 rear도 초기화
		if(front == null) rear = null;

		return backUpItem;
	}
}
```
&nbsp;&nbsp;  연결리스트의 특성을 고려하여 isEmpty(), isFull(), peek(), enQueue(), deQueue() 같은 각 메서드들을 구현한다.
&nbsp;&nbsp;  연결리스트로 큐를 구현하게 되면 큐의 크기를 미리 정하지 않아도 되고, 그에 따라 크기를 넘어섰을 때 생기는 에러 또한 생기지 않는다.


## Reference
- [자료구조] 큐(Queue)란(https://gmlwjd9405.github.io/2018/08/02/data-structure-queue.html)
- [알고리즘] 2.2.자료구조 : 큐(Queue) 이해하기(https://monsieursongsong.tistory.com/5?category=754667)
