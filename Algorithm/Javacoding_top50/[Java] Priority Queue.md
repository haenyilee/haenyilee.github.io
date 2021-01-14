# [Java] Priority Queue

## Priority Queue란?

- PriorityQueue는 먼저 들어온 순서대로 데이터가 나가는 것이 아닌 우선순위를 먼저 결정하고 그 우선순위가 높은 엘리먼트가 먼저 나가는 자료구조이다.

- 우선순위 큐는 힙을 이용하여 구현하는 것이 일반적이다. 

- 데이터를 삽입할 때 우선순위를 기준으로 최대힙 혹은 최소 힙을 구성한다. 

- 데이터를 꺼낼 때는 루트 노드를 얻어낸 뒤 루트 노드를 삭제할 때는 빈 루트 노드 위치에 맨 마지막 노드를 삽입한 후 아래로 내려가면서 적절한 자리를 찾아서 옮긴다.

- 내부 요소는 힙으로 구성되어 이진트리 구조로 이루어져 있다.
- 내부구조가 힙으로 구성되어 있기에 시간 복잡도는 O(NLogN)이다.

- 응급실과 같이 우선순위를 중요시해야 하는 상황에서 쓰인다.


## 메소드


### 선언

```java
import java.util.PriorityQueue; //import

//int형 priorityQueue 선언 (우선순위가 낮은 숫자 순)
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();

//int형 priorityQueue 선언 (우선순위가 높은 숫자 순)
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(Collections.reverseOrder());

//String형 priorityQueue 선언 (우선순위가 낮은 숫자 순)
PriorityQueue<String> priorityQueue = new PriorityQueue<>(); 

//String형 priorityQueue 선언 (우선순위가 높은 숫자 순)
PriorityQueue<String> priorityQueue = new PriorityQueue<>(Collections.reverseOrder());
```


### 추가

```java
priorityQueue.add(1);     // priorityQueue 값 1 추가
priorityQueue.add(2);     // priorityQueue 값 2 추가
priorityQueue.offer(3);   // priorityQueue 값 3 추가
```

### 삭제

```java
priorityQueue.poll();       // priorityQueue에 첫번째 값을 반환하고 제거 비어있다면 null
priorityQueue.remove();     // priorityQueue에 첫번째 값 제거
priorityQueue.clear();      // priorityQueue에 초기화
```

### 루트 노드값 출력

```java
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();//int형 priorityQueue 선언
priorityQueue.offer(2);     // priorityQueue에 값 2 추가
priorityQueue.offer(1);     // priorityQueue에 값 1 추가
priorityQueue.offer(3);     // priorityQueue에 값 3 추가
priorityQueue.peek();       // priorityQueue에 첫번째 값 참조 = 1
```

## 출처
- [https://coding-factory.tistory.com/603](https://coding-factory.tistory.com/603)
