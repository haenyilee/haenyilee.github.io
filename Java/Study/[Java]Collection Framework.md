---
sort: 11
---

# [Java]Collection Framework : 자료구조


## Collection이란?
> 자료구조 : 가변형이기 때문에 배열을 대처하는 기능을 함

- 장점
  - 라이브러리이기 때문에 표준화되어 있다.
  - 데이터를 모아서 처리하게 쉽게 제공한다.
  - 메모리 저장해서 사용시 편리하다.

- 단점
  - 메모리(프로그램 종료하면 이전에 입력했던 내용이 사라짐) => 대안 : 파일과 연동해서 영구저장
  - 모든 데이터형을 마구잡이로 저장 가능해서, 형변환이 어려움 => 대안 : 제네릭스 프로그램으로 일관되게 입력

- 사용되는 프로그램 : 입출력 
  - CURD프로그램 : 읽기 get(), 저장 add(), 삭제 remove(), 수정 set(), 저장갯수 가져오기 size()

### 컬렉션의 종류
![image](https://user-images.githubusercontent.com/66978721/103323613-10edea80-4a87-11eb-8e2d-cd4b5c53a011.png)



## 1. List

> List : 순서가 있고 중복된 데이터 추가

- index번호로 값을 찾음 
- 데이터베이스에 자주 사용됨
- 종류(아래 5가지 메소드는 동일)
  - ArrayList : 비동기화 , 인스턴스 메소드(new로 메모리 공간 생성)
  - Vector : 동기화
  - LinkedList
  - Queue
  - Stack
  
  
```tip
**동기화 vs 비동기화**
- 동기화(synchronized) : 한놈들어가고 나오면 다른놈들어가는것 
- 비동기화 : 여러놈이 들어가서 쓰는 것 (동기화보다 빠름)
```

- 인터페이스이기 때문에 인터페이스를 구현하고 있는 클래스를 이용해서 메모리 할당을 한다.
- 순서가 있다.
- 중복된 데이터를 사용할 수 있다.



### 1.1 ArrayList
![image](https://user-images.githubusercontent.com/66978721/103323875-2fa0b100-4a88-11eb-9570-93dcce13aa8b.png)

- 비동기화
- 맨 뒤에 붙일 땐 속도 빠름
- 중간 삽입은 번호를 새로 매겨야 해서 속도 느림
- Vector(동기화) , LinkedList(속도 가장 빠름)와 동일

- 인스턴스 메소드(new로 메모리 공간 생성) : ```ArrayList list = new ArrayList();```

- 기본형이 Object이기 때문에 꼭 형변환 시켜서 값 읽어줘야 함  
  - ```int a=(int)list.get(0);```, ```double d= (double) list.get(1);```
- 데이터 형을 맞춰서 입력해줘야 편함
- 데이터형이 뒤섞여있으면 for-each사용 불가

- 특정 index위치에 데이터 삽입 가능함 ```list.add(1,"강강강")```
  - 원래 있던 데이터는 추가된 만큼 index번호가 뒤로 밀림
- 리스트에 데이터 추가 : ```list.add("홍길동");```
- 리스트 크기 확인 : ```list.size();```
- 리스트 수정 : ```names.set(2, "둘리");```
- 리스트 전체 삭제 : ```names.clear();```


### 1.2 LinkedList
![image](https://user-images.githubusercontent.com/66978721/103323886-3e876380-4a88-11eb-8098-315023532743.png)
- LinkedList는 데이터를 저장하는 각 노드가 이전 노드와 다음 노드의 상태만 알고 있다고 보면 된다.

- ArrayList와 같이 데이터의 추가, 삭제시 불필요한 데이터의 복사가 없어 데이터의 추가, 삭제시에 유리하다
- 반면 데이터의 검색시에는 처음부터 노드를 순회해야 하기 때문에 성능상 불리하다.


### 1.3 Queue
![image](https://user-images.githubusercontent.com/66978721/103323934-79899700-4a88-11eb-98e3-8b2180856d7a.png)

- 메서드
![image](https://user-images.githubusercontent.com/66978721/103323971-a473eb00-4a88-11eb-9b1b-25c677e59086.png)

### 1.4 Stack
![image](https://user-images.githubusercontent.com/66978721/103323927-72628900-4a88-11eb-9dc7-dfeac7aac5ec.png)

- 메서드
![image](https://user-images.githubusercontent.com/66978721/103323958-945c0b80-4a88-11eb-9613-789674647660.png)


## 2. Set
> Set : 순서가 없고 중복이 없는 데이터 추가

- 인터페이스이다.
- 순서가 없다
- 중복된 데이터를 사용할 수 없다.
- 탐색기에 자주 사용됨
- 종류
  - HashSet
  - TreeSet

### 2.1 HashSet vs TreeSet
- 구현방식

 - HashSet은 해싱을 이용해 구현

 - TreeSet은 이진탐색트리를 이용해 구현



- 속도

 - HashSet > TreeSet

 - 해싱이 이진탐색트리보다 빠름



- 정렬 기능

 - HashSet < TreeSet

 - 이진탐색트리를 이용했기 때문에 데이터 정렬이 가능



## 3. Map

> Map : Key,Data 두 개를 동시에 저장

- 클래스 관리에 자주 쓰임(spring)
- 종류
  - Hashtable
  - HashMap
- 키와 값을 나눠서 저장할 수 있는 공간이다.
- 키는 중복될 수 없다.
- 값은 중복될 수 있다.
- 클래스를 미리 메모리할당하고 키를 이용해서 할당된 주소를 찾아서 사용할 때 많이 사용한다.
- 웹에서 사용되는 request,response,session,cookie 들이 Map방식으로 저장된다.

#### 3.1 HashMap
- put() : 값 입력할때
- get() : 값 자져올때
- clear()

#### 3.2 Hashtable
- 일반적으로 동기화가 필요 없다면 HashMap을, 동기화 보장이 필요하다면 Hashtable을 사용하면된다.
- HashMap과 Hashtable은 동기화를 보장하느냐 하지 않느냐는 측면 이외에는 차이가 거의 없다.
