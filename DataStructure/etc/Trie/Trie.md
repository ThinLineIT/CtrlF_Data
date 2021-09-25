# 트라이(Trie) 자료구조

- 트라이 자료구조란?
- 과정
- 특징
- 구현

## You Can Answer

- 트라이 자료구조란 무엇인가요?
- 트라이 자료구조의 특징은 무엇인가요?

## 트라이 자료구조란?
“접두사”를 검색하거나 “단어 자체”를 검색하는 데에 특화된 문자열 집합 자료구조로 트리 형태다. 중복되는 “접두사”들에 대응되는 노드들이 서로 연결된 트리다.

## 과정

![Trie](./img/Trie.png)

위 그림에서 his를 검색한다면, her과 his 둘 다 각각 대조해 볼 필요 없이 자연스럽게 h -> i -> s 이렇게 노드를 타고 내려오면 된다.

## 특징
- 트라이에 포함된 다른 문자열의 개수와 상관없이 시간 복잡도는 O(문자열의 길이)이다.
- 포인터 배열로 인해 필요로 하는 메모리가 크다.
  - 최종 메모리는 O(포인터 크기 * 포인터 배열 개수 * 트라이에 존재하는 총 노드의 개수) 다.
- 루트 노드는 항상 길이 0 인 문자열에 대응된다.
  - 루트 노드는 대응되는 문자가 없는 상태이다. 루트의 자식부터 첫 문자에 대응
- 깊이가 깊어질 때마다 대응되는 문자열의 길이는 1 씩 늘어난다.


## 구현
```java
const int NUM_ALPHABETS = 26;
int toIndex(char ch) { return ch - 'a'; } // 알파벳을 아스키코드로 변환

struct TrieNode {
	TrieNode* children[NUM_ALPHABETS];
	bool isEnd;

	TrieNode() : children(), isEnd(false) {}

	void Insert(string& key, int index) {
		if (index == key.length() - 1)
			isEnd = true;
		else {
			int next = toIndex(key[index]);
			if (children[next] == nullptr)
				children[next] = new TrieNode;
			children[next]->Insert(key, index + 1);
		}
	}

	bool Find_1(string& key, int depth) {   // 접두사로서 검색 되더라도 true 를 리턴하게끔 한 함수
		if (depth == key.length() - 1)
			return true;
		int next = toIndex(key[depth]);
		if (children[next] == nullptr)  // 해당 알파벳이 있는지 확인
			return false;
		return children[next]->Find_1(key, depth + 1);
	}

	bool Find_2(string& key, int depth) {  // 완전히 일치하는 단어 단위로만 찾고 true 를 리턴하게끔 한 함수
		if (depth == key.length() - 1 && isEnd)
			return true;
		if (depth == key.length() - 1 && !isEnd)
			return false;
		int next = toIndex(key[depth]);
		if (children[next] == nullptr)  // 해당 알파벳이 있는지 확인
			return false;
		return children[next]->Find_2(key, depth + 1);
	}
};

int main() {
	vector<string> words = { "what", "when", "yours", "apple", "her", "his", "you" };

	TrieNode root;
	for (string word : words)
		root.Insert(word, 0);

	string search = "wh";

	if (root.Find_1(search, 0)) cout << search << "가 검색 결과에 있습니다." << '\n';
	else cout << search << "가 검색 결과에 없습니다." << '\n';

	if (root.Find_2(search, 0)) cout << search << "가 검색 결과에 있습니다." << '\n';
	else cout << search << "가 검색 결과에 없습니다." << '\n';
}
```


출력 결과

```
wh가 검색 결과에 있습니다.
wh가 검색 결과에 없습니다.
```
