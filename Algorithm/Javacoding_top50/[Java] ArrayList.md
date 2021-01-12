# [Java] ArrayList
-  List 인터페이스를 상속받은 클래스로 크기가 가변적으로 변하는 선형리스트이다.

- 일반적인 배열과 같은 순차리스트이며 인덱스로 내부의 객체를 관리한다는점등이 유사하다. 

- 한번 생성되면 크기가 변하지 않는 배열과는 달리 ArrayList는 객체들이 추가되어 저장 용량(capacity)을 초과한다면 자동으로 부족한 크기만큼 저장 용량(capacity)이 늘어난다. 


## ArrayList란?

## ArrayList의 메소드

### add() : 추가

- ArrayList에 값을 추가하려면 ArrayList의 `add(index, value)` 메소드를 사용하면 된다. 

- index를 생략하면 ArrayList 맨 뒤에 데이터가 추가된다.

- index중간에 값을 추가하면 해당 인덱스부터 마지막 인덱스까지 모두 1씩 뒤로 밀려난다. 

```java
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(3); //값 추가
list.add(null); //null값도 add가능
list.add(1,10); //index 1뒤에 10 삽입
```

```java
ArrayList<Student> members = new ArrayList<Student>();
Student student = new Student(name,age);
members.add(student);
members.add(new Member("홍길동",15));
```


### remove(), clear() : 값 삭제

- ArrayList에 값을 제거하려면 ArrayList의 remove(index) 메소드를 사용하면 된다. 

- remove()함수를 사용하여 특정 인덱스의 객체를 제거하면 바로 뒤 인덱스부터 마지막 인덱스까지 모두 앞으로 1씩 당겨진다. 

- 모든 값을 제거하려면 clear() 메소드를 사용하면 된다.

```java
ArrayList<Integer> list = new ArrayList<Integer>(Arrays.asList(1,2,3));
list.remove(1);  //index 1 제거
list.clear();  //모든 값 제거
```

### size() : 크기 구하기

```java
ArrayList<Integer> list = new ArrayList<Integer>(Arrays.asList(1,2,3));
System.out.println(list.size()); //list 크기 : 3
```


### get() : index값 가져오기

- ArrayList의 get(index) 메소드를 사용하면 ArrayList의 원하는 index의 값이 리턴된다. 

- 전체출력은 대부분 for문을 통해서 출력을하고 Iterator를 사용해서 출력을 할수도 있다.


```java
ArrayList<Integer> list = new ArrayList<Integer>(Arrays.asList(1,2,3));

System.out.println(list.get(0));//0번째 index 출력
		
for(Integer i : list) { //for문을 통한 전체출력
    System.out.println(i);
}
```

### Iterator, ListIterator : 출력하기

- Iterator<E> 인터페이스

  - 자바의 컬렉션 프레임워크는 컬렉션에 저장된 요소를 읽어오는 방법을 Iterator 인터페이스로 표준화하고 있습니다.

  - Collection 인터페이스에서는 Iterator 인터페이스를 구현한 클래스의 인스턴스를 반환하는 iterator() 메소드를 정의하여 각 요소에 접근하도록 하고 있습니다.

  - 따라서 Collection 인터페이스를 상속받는 List와 Set 인터페이스에서도 iterator() 메소드를 사용할 수 있습니다.

 

- 연결 리스트를 반복자(iterator)를 사용하여 순회하는 예제

```java
LinkedList<Integer> lnkList = new LinkedList<Integer>();

lnkList.add(4);
lnkList.add(2);
lnkList.add(3);
lnkList.add(1);

Iterator<Integer> iter = lnkList.iterator();

while (iter.hasNext()) {
    System.out.print(iter.next() + " ");
}
```

- ListIterator

  - ListIterator 인터페이스는 Iterator 인터페이스를 상속받아 여러 기능을 추가한 인터페이스입니다.

  - Iterator 인터페이스는 컬렉션의 요소에 접근할 때 한 방향으로만 이동할 수 있습니다.

  - 하지만 JDK 1.2부터 제공된 ListIterator 인터페이스는 컬렉션 요소의 대체, 추가 그리고 인덱스 검색 등을 위한 작업에서 양방향으로 이동하는 것을 지원합니다.

  - 단, ListIterator 인터페이스는 List 인터페이스를 구현한 List 컬렉션 클래스에서만 listIterator() 메소드를 통해 사용할 수 있습니다.


- 리스트 반복자를 사용하여 리스트의 모든 요소를 각각 순방향과 역방향으로 출력하는 예제

```java
LinkedList<Integer> lnkList = new LinkedList<Integer>();

lnkList.add(4);
lnkList.add(2);
lnkList.add(3);
lnkList.add(1);

ListIterator<Integer> iter = lnkList.listIterator();

while (iter.hasNext()) {
    System.out.print(iter.next() + " "); // 4 3 2 1 
}

while (iter.hasPrevious()) {
    System.out.print(iter.previous() + " "); // 1 2 3 4
}
```


- ListIterator의 메소드

|메소드|설명|
|-----|-----|
|void add(E e)	|해당 리스트(list)에 전달된 요소를 추가함. (선택적 기능)|
|boolean hasNext()	|이 리스트 반복자가 해당 리스트를 순방향으로 순회할 때 다음 요소를 가지고 있으면 true를 반환하고, 더 이상 다음 요소를 가지고 있지 않으면 false를 반환함.|
|boolean hasPrevious()	|이 리스트 반복자가 해당 리스트를 역방향으로 순회할 때 다음 요소를 가지고 있으면 true를 반환하고, 더 이상 다음 요소를 가지고 있지 않으면 false를 반환함.|
|E next()	|리스트의 다음 요소를 반환하고, 커서(cursor)의 위치를 순방향으로 이동시킴.|
|int nextIndex()	|다음 next() 메소드를 호출하면 반환될 요소의 인덱스를 반환함.|
|E previous()	|리스트의 이전 요소를 반환하고, 커서(cursor)의 위치를 역방향으로 이동시킴.|
|int previousIndex()	|다음 previous() 메소드를 호출하면 반환될 요소의 인덱스를 반환함.|
void remove()	|next()나 previous() 메소드에 의해 반환된 가장 마지막 요소를 리스트에서 제거함. (선택적 기능)|
void set(E e)	|next()나 previous() 메소드에 의해 반환된 가장 마지막 요소를 전달된 객체로 대체함. (선택적 기능)|



## 출처
- [[Java] 자바 ArrayList 사용법 & 예제 총정리](https://coding-factory.tistory.com/551)

- [Iterator와 ListIterator](http://www.tcpschool.com/java/java_collectionFramework_iterator)
