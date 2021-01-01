---
sort: 8
---

# [Java]예외처리

## 예외처리란?

> 에러가 날때 어떻게 처리할 것인가?

- 정의 : 사전에 예상되는 에러를 대비하는 코드를 만드는 것
- 목적 : 비정상 종료를 방지하고 정상 상태를 유지하기 위함 => 견고한 프로그램

- 자바의 모든 소스에는 예외처리가 있다.(생략 가능 or 생략 불가능)
- 프로그램을 실행하는 과정에서 오작동, 비정상으로 종료되는 것 => 에러/오류
  - 윈도우 : 쓰레드의 충돌, 주소값 중복
  - 오작동 , 비정상 수행
    1. 사용자 실수 : 입력값 오류 => 예외처리
    2. 프로그래머 실수 : 클래스 내 형변환 오류, 배열을 초과, 0으로 나누기 등

## 실제 에러 종류
> 예외(Exception) : 프로그래머가 소스 안에서 처리가 가능한 것

- 수정이 가능한 에러, 가벼운 에러
- 아이디중복체크, 경로명, 파일명, 오라클주소 오류 등

> 에러(Error) : 프로그래머가 소스 안에서 처리가 불가한 것

- 메모리 부족(재부팅 필요), 시스템 오류 => 고칠 수 없음

## 예외처리 종류
1. 컴파일 에러(CheckException) 
	- 문법상의 에러 => 컴파일러가 예외처리했는지 확인하기 => 반드시 예외처리 해야함
2. 실행시 에러(UnCheckException) 
	- 사용자 입력 오류 등 => 컴파일러가 예외처리를 확인하지 않음 => 필요시에는 예외처리 생략 가능
3. 논리적 에러
	- 실행은 되는데 동작이 이상하게 되는 경우 -> 프로그램의 순서가 틀린 경우

||CheckException | UnCheckException|
|-|-----|------|
|예외처리|반드시 예외처리|생략가능(필요시에는 반드시 예외처리)|
|종류|IOException(파일입출력)|RunTimeException(실행에러:생략가능)|
||InteruptedException(Thread)|0으로 나눌경우|
||MalformedURLException(URL)|배열의 크기 초과|
||SQLException(데이터베이스)|**NumberFormException(정수변환 오류)**|
||ClassNotFoundException(클래스 이름)|NullPointException(메모리할당 안함)|
|||ClassCastException|

## 예외처리의 계층구조

![image](https://user-images.githubusercontent.com/66978721/103265228-704ceb80-49f0-11eb-9715-00a61ade803e.png)

![image](https://user-images.githubusercontent.com/66978721/103265243-7cd14400-49f0-11eb-9b64-715b40e4d2cd.png)



### RuntimeException의 종류
> RunTimeException은 생략이 가능하다!

- ArithmeticException:  정수를 0으로 나누었을 경우
- ArrayStoreException: 배열 유형이 허락하지 않는 객체를 객체를 배열에 저장하려는 경우
- ArrayIndexOutOfBoundsException: 배열을 참조하는 인덱스가 잘못된 경우
- ClassCastException: 적절치 못하게 Class를 형 변환하는 경우
- NullPointerException: 널(null) 객체를 참조했을 경우
- NegativeArraySizeException: 배열의 크기가 음수인 경우
- NoClassDefFoundException: 클래스를 찾을 수 없는 경우
- OutOfMemoryException: 사용 가능한 메모리가 없는 경우
- IndexOutOfBoundsException: 객체의 범위를 벗어난 색인(Index)를 사용하는 경우
- IllegalArgumentException: 메서드에 유형이 일치하지 않는 매개변수를 전달하는 경우
- IllegalMonitorStateException: 스레드가 스레드에 속하지 않는 객체를 모니터 하려고 기다리는 경우
- IllegalStateException: 적절하지 않은 때에 메서드를 호출하는 경우

## 예외처리의 방법
|예외복구 | 예외회피 |임의발생 | 사용자정의|
|-----|-----|------|------|
|try-catch|throws|throw|ExtendsException|
|직접처리|간접처리|테스트용|거의 사용X|
|예외처리 순서 중요|예외처리 순서X|||

- **1. 예외복구 : 점프 , 프로그래머가 예외가 발생한 경우 직접 처리하여 정상상태를 유지하는 것(직접처리)**
- **2. 예외회피 : 다른메소드로 전송해서 시스템에 의해서 처리하는 것(간접처리)**  
- 3. 임의발생 : 테스트용  
- 4. 사용자 정의 예외 : 자바라이브러리에서 전체 에러를 지원하지 않는 경우

## 1. 예외복구

```java
try{
  실행가능한 평상시 코드
}
catch(예상되는 에러){}
finally{에러가 발생하든, 발생하지 않든 무조건 수행되는 문장 => 생략가능}
```

### 예외복구의 형식
> 1) 전체 예외처리

```java
try
{
  for(int i=0;i<10;i++)
  {
    i==3 error => i=0,1,2
  }
}catch(Exception e){}

```

> 2) 부분적 예외처리

```java
for(int i=0;i<10;i++)
{
  try
  {
    i==3 error => i=0,1,2 ==> i=4,5...
  }catch(Exception e){} ==> i++로 이동
}
```

### 예외복구의 다중처리

```java
try{
  실행가능한 평상시 코드
}
catch(예상되는 에러1){에러처리}
catch(예상되는 에러2){에러처리}
catch(예상되는 에러3){에러처리}
finally{에러가 발생하든, 발생하지 않든 무조건 수행되는 문장 => 생략가능}
```

- 예상되는 에러가 여러개 있는 경우
- catch를 여러개 사용 시에는 순서가 존재함
- 예외처리의 계층구조를 알고 있어야 함



## 2. 예외회피

> 간접적으로 예외를 처리하는 방식

### 예외회피의 사용 목적
- 에러에 대한 예측이 가능하게 만든다.
- 프로그래머가 예외처리를 각자 프로그램에 맞게 처리를 유도한다.
  - 대부분의 프로그램은 직접 처리(try~catch)한다.

### 예외회피의 형식
- 1. 순서없이 나열이 가능하다.  
- 2. 예외처리는 호출하는 메소드에서 처리한다.

```java
public void display() throws Exception, ArrayIndexOfBoundsException,NumberFormatException
{
  
}
```

- (예시)

```java
public class MainClass {
	public int div(int a,int b) throws ArithmeticException
	{
		return a/b;
	}

	public static void main(String[] args) { // RuntimeException 관련 부분은 생략 가능
		MainClass m = new MainClass();
		int res = m.div(10, 2);
		System.out.println(res);
	}

}
```
