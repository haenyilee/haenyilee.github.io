# 2. java.util
1. 프로그램에서 자주 사용하는 클래스의 집합
2. 반드시 import를 사용한다.


# 2.1 Random()
> 난수 , 임의의 수를 컴퓨터가 결정

### 사용방법
1. static이 아니다.```Random.nextInt()```형식으로 사용하지 않음  (static : **제어할 대상이 따로 없기 때문에** 코딩하자마자 저장 => 모두 동일하게 작동) 
2. 인스턴스임 (저장 공간을 따로 만들어줘야함 , 각자 다르게 작동)  
```java
Random r = new Random();
r.nextInt()
```

## 2.1.1 int nextInt()
### 범위
0~21억 4천 사이의 난수 발생(int 범위 내)

## 2.1.2 int nextInt(int n)
### 범위
지정된 범위의 수가 나온다.  
(예) ```int nextInt(100)```은 ```0 ~ 99```사이의 난수 발생

### 특징
오버로딩(같은 형식, 매개변수만 다르게 사용)


# 2.2 StringToknizer
> 단어별로 잘라주는 기능
- split과 유사
  - 배열을 잘라주는 것이 아님




## 2.2.1 nextToken()

### 특징
- 범위 초과시 에러 발생함

### 사용방법
```java
String msg="Red,Green,Blue,Black,Yellow";
StringTokenizer st = new StringTokenizer(msg,",");
String color1=st.nextToken();
String color2=st.nextToken();
String color3=st.nextToken();
String color4=st.nextToken();
String color5=st.nextToken();
```

## 2.2.2 hasMoreTokens()
> 사용방법
```java
String msg="Red,Green,Blue,Black,Yellow";
StringTokenizer st = new StringTokenizer(msg,","); // ","부분이 생략되어 있으면 공백(" ")을 기준으로 자름
while(st.hasMoreTokens())
{
  String color = st.nextToken();
}
```


# 2.3 regex

|구분기호|기능|예시|
|-|--|--|
|^|시작문자 |(예) 한글로 시작하는 모든 데이터: ```^[가-힇]```|
|||한글을 제외한 모든 데이터 : ```[^가-힇]```|
|$|끝문자|A$|
|.|임의의 한글자(모든글자)|\\.|
|\\||선택||
|?|있으면 출력 , 없으면 미출력||

# 2.4 Calendar 
> 요일, 마지막 날짜
### 사용방법
- new 사용해서 클래스 생성하지 않음 
- ```Calendar cal = Calendar.getInstance();```형식으로 클래스 생성
  - 싱글턴 패턴, 즉 메모리를 한 개만 생성하기 때문임
- 값 설정 : ``` cal.set(Calendar.DATE, 1);```
- 값 가져오기 : ```cal.get(Calendar.DATE);```



# 2.5 Date 
- 시스템 시간
- 날짜, 시간 변환
  -  ```import java.text.*;``` , ```SimpleDateFormat sdf = new SimpleDateFormat("yy-MM-dd");```
  - 년(yyyy yy) 월(M MM) 일(dd d) 시간(h hh H(0~23)) 분(mm) 초(s)

# 2.6 Scanner
- 도스창에서만 사용
- 문법 처리할 때만 사용


# 2.7 Collection
> 자료구조 : 가변형이기 때문에 배열을 대처하는 기능을 함
- 메모리 저장해서 사용시 편리함
- 사용되는 프로그램 : 입출력 
  - CURD프로그램 : 읽기 get(), 저장 add(), 삭제 remove(), 수정 set(), 저장갯수 가져오기 size()
- 단점
1. 메모리(프로그램 종료하면 이전에 입력했던 내용이 사라짐) => 대안 : 파일과 연동해서 영구저장
2. 모든 데이터형을 마구잡이로 저장 가능해서, 형변환이 어려움 => 대안 : 제네릭스 프로그램으로 일관되게 입력

### 컬렉션의 종류
![](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile8.uf.tistory.com%2Fimage%2F25068A3957A85E69226E25)

> List : 순서가 있고 중복된 데이터 추가
- index번호로 값을 찾음 
- 데이터베이스에 자주 사용됨
- 종류(아래 5가지 메소드는 동일)
ArrayList : 비동기화 , 인스턴스 메소드(new로 메모리 공간 생성)
Vector : 동기화
LinkedList
Queue
Stack

> Set : 순서가 없고 중복이 없는 데이터 추가
- 탐색기에 자주 사용됨
- 종류
HashSet
TreeSet

> Map : Key,Data 두 개를 동시에 저장
- 클래스 관리에 자주 쓰임(spring)
- 종류
Hashtable
HashMap


## 2.7.1 ArrayList
- 비동기화 , 맨 뒤에 붙일 땐 속도 빠름 , 중간 삽입은 번호를 새로 매겨야 해서 속도 느림
- Vector(동기화) , LinkedList(속도 가장 빠름)와 동일
- 인스턴스 메소드(new로 메모리 공간 생성) : ```ArrayList list = new ArrayList();```
- 기본형이 Object이기 때문에 꼭 형변환 시켜서 값 읽어줘야 함  
```int a=(int)list.get(0);```, ```double d= (double) list.get(1);```
- 데이터 형을 맞춰서 입력해줘야 편함, 데이터형이 뒤섞여있으면 for-each사용 불가
- 특정 index위치에 데이터 삽입 가능함 ```list.add(1,"강강강")```
  - 원래 있던 데이터는 추가된 만큼 index번호가 뒤로 밀림
- 리스트에 데이터 추가 : ```list.add("홍길동");```
  - Index번호 
- 리스트 크기 확인 : ```list.size();```
- 리스트 수정 : ```names.set(2, "둘리");```
- 리스트 전체 삭제 : ```names.clear();```



## 2.7.2 제너릭스 타입 
> 전체의 데이터형을 변경하는 것
- <T,E,V,K> : 임시변수 , 타입 임시 데이터형
```java
class Box<T>   // 임시로 데이터형을 T라고 지정
{
  T t;
  public void setT(T t)
  {
     this.t=t;
  }
  public T getT()
  {
     return t;
  }
}
Box<String> box = new Box<String>()   // 마지막에 데이터형을 String으로 결정
ArrayList<Integer> list = new ArrayList<Integer>(); // int는 wrapper클래스로 사용해야함을 유의!
```

# 2.7.3 HashMap
- put() : 값 입력할때
get() : 값 자져올때
clear()


# 2.8 reflect


