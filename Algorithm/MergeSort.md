# 합병정렬(MergeSort)
- 합병정렬이란?
  - 과정
  - 장단점
- 구현

## You Can Answer
- 합병정렬이란 무엇인가요?
- 합병정렬의 작동 방식은 어떻게 되나요?
- 합병정렬의 시간복잡도는 어떻게 되나요?

## 합병정렬이란?
합병정렬이란 전형적인 분할 정복 알고리즘의 하나로 주어진 리스트를 최소 단위까지 나눈 뒤(분할), 다시 그들을 합치면서 정렬하는 방법이다.

## 합병정렬의 과정
1) 리스트를 두 개의 리스트로 분할한다.
2) 분할한 리스트를 정렬한다.
3) 정렬된 리스트를 하나의 리스트로 병합한다.


![MergeSort](./img/merge_sort_example.gif)


왼쪽과 오른쪽 리스트의 원소를 하나씩 비교하는데, 더 작은 것을 결과 리스트에 넣는다. 그리고 결과 리스트에 들어가지 못한 리스트의 인덱스를 가리키는 것은 그대로 두고, 결과 리스트에 들어갔던 리스트의 인덱스를 가리키는 것은 한 칸 증가시킨다. 그리고 그 두 숫자를 다시 비교한다. 이 과정을 반복하면, 결과 리스트에 정렬되어 값이 들어가게 된다.

## 합병정렬의 장단점
- 단점
  - 레코드를 배열로 구현할 경우 임시 배열이 필요하다.
  - 레코드들의 크기가 큰 경우 이동 횟수가 많으므로 시간적 낭비가 크다.
- 장점
  - 입력 데이터가 무엇이든 간에 정렬되는 시간은 동일하다(O(nlog₂n)로 동일).


## 구현
```
void merges(int left, int right) {
	int mid = (left + right) / 2;
	int i = left;
	int j = mid + 1;
	int k = left;

	while (i <= mid && j <= right) {
		if (arr[i] > arr[j]) {
			result[k++] = arr[j++];
		}
		// 양쪽 리스트에서 최솟값을 비교했는데 오른쪽 리스트가 더 컸을 경우
		// 그대로 왼쪽 리스트의 최솟값이 결과배열에 내려오면 됩니다.
		else {
			result[k++] = arr[i++];
		}
	}

	// 오른쪽 리스트에 아직 결과배열에 들어가지 못한 값이 있으면 그대로 넣어줍니다.
	if (i > mid) {
		while (j <= right) {
			result[k++] = arr[j++];
		}
	}
	else { // 그림의 (6)번 과정
		while (i <= mid) {
			result[k++] = arr[i++];
		}
	}

	// 다시 원래 배열에 옮겨담는 작업
	for (int a = left; a <= right; a++) {
		arr[a] = result[a];
	}
}

// 두개로 분할하고, 병합하는 과정
void partition(int left, int right) {
	int mid;
	// 병합 함수 merges를 보면 알 수 있듯이, while문 등으로 정렬하면서 병합하게 된다.
	if (left < right) {
		mid = (left + right) / 2;
		partition(left, mid);
		partition(mid + 1, right);
		merges(left, right);
	}
}
```

## Reference
- [알고리즘 - 병합정렬의 개념](https://chanhuiseok.github.io/posts/algo-48/)
- [[알고리즘] 합병정렬이란](https://gmlwjd9405.github.io/2018/05/08/algorithm-merge-sort.html)
