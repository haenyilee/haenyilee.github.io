---
sort: 9
---

# [Java]java.lang패키지와 유용한 클래스



## 자바 API
![](https://4.bp.blogspot.com/-GxunHEBiBmI/U6bst9SZh3I/AAAAAAAAAWY/LmU09gJrXu8/s1600/package+in+java.png)


## 1. java.lang

![](https://t1.daumcdn.net/cfile/tistory/2255EF495696F9372F)

- 자바모든 클래스가 상속받는 클래스이기 때문에 import붙이지 않음

- 종류 
	- Object
	  - clone() : 복제
	  - finalize() : 소멸자
	  - toString() : 객체를 문자열로 변환
	- String (StringBuffer)
	  - substring() : 문자 추출
	  - indexOf(), lastIndexOf() : 문자 찾는 경우(MVC)
	  - trim() : 로그인처리 , 사용자 입력값
	  - equals() : 실제 저장된 문자열을 비교(로그인처리, 우편번호)
	  - length() : 문자갯수
	  - valueOf() : 모든 데이터형을 문자열로 변환(유일한 static함수)
	  - join() : 변환
	  - StringBuffer() : append() : 문자열 결합
	- Wrapper : 클래스 형변환 , 문자열을 다른 데이터형으로 변경
	  - Integer : parseInt() : 정수형 변환
	  - Double : parseDouble() : 실수형 변환
	  - Boolean : parseBoolean() : "true" => true , "false" => false
	- Math
	  - random() , ceil()
	- System
	  - gc() , exit()
	  * web에서는 자동으로 메모리 회수(톰캣)



### 1.1 Object 

#### Object의 특징
- 모든 데이터형 포함하는 최상위 클래스
  - 데이터형 : 기본형 , 참조형(클래스,배열)

```java
class A
Object obj=new A();
Object obj=10.5;
Object[] obj = {new A(), 10.5, 100, 'A',"Hello"};
double[] d={10.5,'A',100,10.5F};
```

- 모든 클래스에 상속을 내리는 클래스
  - Object가 가지고 있는 모든 기능을 사용할 수 있음


#### clone()
> 복제할 때 사용

- spring에서는 prototype
https://readystory.tistory.com/122


[얕은복사 vs 깊은 복사](https://m.blog.naver.com/PostView.nhn?blogId=jihoon8912&logNo=220213557201&proxyReferer=https:%2F%2Fwww.google.com%2F)



1. 새로운 메모리 제작 : new()
- 처음부터 시작, 중간에 값이 변해도 처음값을 가져옴
```java
class A
{
  int aa=10;
}

A a = new A();
```

2. 기존 것을 참조
- 똑같은 메모리 주소를 공유하는 것(별칭을 주는 행위라고 볼 수 있음)
- 클래스를 매개변수로 전송, 메소드를 통해서 전송할 때 자주 나옴 
- Call By reference
- 얕은 복사

```java
class A
{
  int aa=10;
}

A a = new A();
A b = a; // 동일한 저장 장소 , 동일한 값을 가지게 됨
```

3. 기존 것을 복제
- 똑같은 메모리가 하나 더 생기는 것
- 변경된 것부터 시작됨 , 초기값이 아닌 변경된 값을 가져옴
- 매개변수 : Object clone()
- Call by value
- 깊은 복사

```java
class A
{
  int aa=10;
}

A a = new A();
A e=c.clone(); // 다른 저장 장소 , 동일한 값을 가지게 됨
```


- (참고) 상속의 2가지 방법
1. extends : 클래스로부터 상속 , 단일상속
2. implements : interface로 부터 상속 , 다중상속

- **this와 super**

```java
class A
{
	int a=10;
}

class B extends A
{
	// A를 상속 : a값 갖게됨
	public B()
	{
		this.a=100; // class B의 int a;
		super.a=200; // class A의 int a;
	}
}
```






#### toString()
> 클래스 객체를 문자열로 변환할 때 사용

- 매개변수 : ```string toStirng()```
- ```(String) 변수타입 변수명``` 형변환과 동일 기능



#### finalize()
> 소멸자 함수
> 누적된 메모리가 해제되었는지 확인하는 기능
> 메모리 최적화 작업관련

```java
class C
{
	public void display()
	{
		System.out.println("C:display() Call...");
	}

	@Override
	protected void finalize() throws Throwable {
		System.out.println("메모리 해제");
	}
	
}
public class MainClass5 {

	public static void main(String[] args) {
		// 메모리 저정
		C c =new C();
		c.display();
		// 소멸하라고 명령 내리기 , 가비지컬렉션(gc)
		c=null;
		System.gc();

	}

}
```






### 1.2. String

#### String클래스의 특징
- String클래스 : char[]를 조작해서 사용하기 쉽게 만들어진 클래스
  - ```"Hello"```는 ```char[] c={'H','e','l','l','o'};```와 같다.

- ```String s = "Hello";```
  - s에는 메모리 주소가 저장(stack) , 메모리에는 "Hello"가 저장됨(heap)

- 문자열이 같으면 같은 주소에 저장된다.

```java
String s1 = "Hello";
String s2 = "Hello";
String s3 = new String("Hello");

System.out.println(s1==s2); // true. s1과 s2는 메모리 주소가 같다.(stack은 별도 , heap은 공통)
System.out.println(s1==s3); // false. s3는 새로운 메모리 공간에 저장됨(주소가 다름)

System.out.println(s1.equals(s2)); // true. 값이 같음
System.out.println(s1.equals(s3)); // true. 값이 같음
```

> 문자열은 주소다.

- 상속되지 않으면 크기 비교 불가

#### String의 주요 메소드
|no|메소드명|메소드 기능|
|---|---------|----------|
|1| length() | 문자의 갯수|
|2| trim()|좌우의 공백제거|
|3| substring()|문자분해|
|4|intdexOf()  lastIndexOf()|문자의 위치비교|
|5|equals()|저장된 문자를 비교|
|6-1|contains()|포함찾기|
|6-2|startsWith()|시작찾기|
|6-3|endsWith()|끝찾기|
|7|valueOf()|static메소드,모든 기본형을 문자열로 변환,공통으로 사용가능|

```java
String s1="Hello";
s1.length();  // 문자열별로 사용
String s2="Hello Java";
String.valueOf(); // static이기 때문에 클래스 사용 가능
```


#### trim()
> 좌우 공백 제거

```java
String s = "background-image: url(' https://mp-seoul.jpg');";

String temp= s.substring(s.indexOf("'")+1,s.lastIndexOf("'"));
System.out.println(temp.trim()); // https://mp-seoul.jpg
```

#### substring()
> 문자열 자르기

```java
String s="0123456789";

String temp = s.substring(3,5);
System.out.println(temp); // 34
```

#### String.length()
#### String.indexOf()
#### String.lastIndexOf()

```java
String s = "background-image: url('https://mp-seoul.jpg');";

String temp= s.substring(s.indexOf("'")+1,s.lastIndexOf("'"));
System.out.println(temp); // https://mp-seoul.jpg
```

#### equals()

|equals|equalsIgnoreCase|
|------|-----------------|
|대소문자 구분|대소문자 구분 없음|
|로그인|검색|


#### String.valueOf()
> 모든 데이터형을 문자로 변환

- ```String.valueOf(int)``` 
- 윈도우, 웹, 모바일에서는 문자열만 취급함

-  유일한 static메소드임

#### contains()
> [찾기1] 문자열이 포함

#### startsWith()
> [찾기2] 시작 문자열

#### endWith()
> [찾기3] 끝 문자열

#### String.replace()




### 1.3. StringBuffer
> 빠른 속도로 문자열 결합시켜주는 함수

#### 문자열 결합 방법

1. StringBuffer : 동기화 , 안정성 높음
```java
StringBuffer data = new StringBuffer();
data.append(String.valueOf((char)i));
```

2. StringBuilder : 비동기화, 가장 빠름

```java
StringBuilder data= new StringBuilder();
data.append(String.valueOf((char)i));
```

3. 

```java
String data="";
data+=(String.valueOf((char)i));
```


#### append()
#### join()

```java
String[] arr= {"홍길동","고길동","둘리","심청이","박문수"};
System.out.println(String.join("|",arr));

// 출력 : 홍길동|고길동|둘리|심청이|박문수
```
#### format()

```java
String str=String.format("%d - %d = %d", 10,2,10-2);
System.out.println(str);

// 출력 : 10 - 2 = 8
```




### 1.4. System
#### System.println()
> 화면 출력
#### System.exit()
> 프로그램 종료
#### System.gc
> "가비지컬렉션" 호출
> 메모리를 회수
#### System.currentTimeMillis()
> 현재 시간 불러오기




### 1.5. Math
> 수학에 관련된 함수

#### Math.random()
> 0.0 ~ 0.99의 수를 랜덤으로 추출

#### Math.ceil()
> 올림함수
- 총 페이지 수를 구할 때 자주 사용됨
```java
Math.ceil(10.5); // 11
```


### 1.6. Wrapper
> 기본형에 대한 클래스 
> 모든 데이터형을 사용하기 쉽게 클래스로 변경해줌

#### 유형
- Double => double
- Byte => byte
- Integer => int
- Boolean => boolean

- parseInt()
- parseDouble()
- parseBoolean()
- parseLong()

#### 박싱과 언박싱

- 박싱 : Primitive 자료형 -> Wrapper 클래스

```java
Integer i = 10;
```

- 언박싱 : Wrapper 클래스 -> Primitive 자료형

```java
Integer i = new Integer(100);
int k=i;
```

#### Integer()

```
int i= (int)"10"; // Error! : 문자열은 정수로 형변환 불가
```

- 계산기에서 숫자 입력 받을 때는 문자열로 받고, 계산할 때는 정수로 계산함

#### Double()
#### Boolean()
#### Long()



### 1.7. Thread
#### Thread.start()
#### Thread.run()




## 2.java.util
- 프로그램에서 자주 사용하는 클래스의 집합
- 반드시 import를 사용한다.

- 종류 
	- StringTokenizer : 문자를 분해 
	  - nextToken() : 한 개의 구분문자를 자를 때
	  - hasMoreTokens() : 자른 갯수만큼 루프
	- Date , Calendar
	  - Date : 시스템의 시간 , 날짜 읽기
	  - Calendar : 요일구하기, 달의 마지막 날짜 읽기
	    * 월 : month-1 , 요일 : week-1
	- Collection
	  - ArrayList : 비동기화
	  - Vector : 동기화 , 네트워크에서 사용자 관리
	  - HashMap : 키(일반문자열),값(클래스 주소) , 클래스관리
	  * 단점 보완 : 제너릭스 프로그램 , <원하는 데이터형> : 데이터형의 특징




### 2.1 Random()
> 난수 , 임의의 수를 컴퓨터가 결정

1. static이 아니다.```Random.nextInt()```형식으로 사용하지 않음  (static : **제어할 대상이 따로 없기 때문에** 코딩하자마자 저장 => 모두 동일하게 작동) 
2. 인스턴스임 (저장 공간을 따로 만들어줘야함 , 각자 다르게 작동)  

```java
Random r = new Random();
r.nextInt()
```

#### int nextInt()
- 0~21억 4천 사이의 난수 발생(int 범위 내)

#### int nextInt(int n)
- 지정된 범위의 수가 나온다.  
- (예) ```int nextInt(100)```은 ```0 ~ 99```사이의 난수 발생

- 특징 : 오버로딩(같은 형식, 매개변수만 다르게 사용)


### 2.2 StringToknizer
> 단어별로 잘라주는 기능

- split과 유사
  - 배열을 잘라주는 것이 아님




#### nextToken()

- 범위 초과시 에러 발생함

```java
String msg="Red,Green,Blue,Black,Yellow";
StringTokenizer st = new StringTokenizer(msg,",");
String color1=st.nextToken();
String color2=st.nextToken();
String color3=st.nextToken();
String color4=st.nextToken();
String color5=st.nextToken();
```

#### hasMoreTokens()

```java
String msg="Red,Green,Blue,Black,Yellow";
StringTokenizer st = new StringTokenizer(msg,","); // ","부분이 생략되어 있으면 공백(" ")을 기준으로 자름
while(st.hasMoreTokens())
{
  String color = st.nextToken();
}
```


### 2.3 regex

|구분기호|기능|예시|
|-|--|--|
|^|시작문자 |(예) 한글로 시작하는 모든 데이터: ```^[가-힇]```|
|||한글을 제외한 모든 데이터 : ```[^가-힇]```|
|$|끝문자|A$|
|.|임의의 한글자(모든글자)|\\.|
|\\||선택||
|?|있으면 출력 , 없으면 미출력||





### 2.4 Scanner
- 도스창에서만 사용
- 문법 처리할 때만 사용



### 2.5 reflect







