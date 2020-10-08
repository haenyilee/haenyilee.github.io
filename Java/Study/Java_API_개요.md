# 기본문법
- 변수
- 연산자
- 제어문
- 메소드
- 클래스(변수 메소드)
- 상속
- 캡슐화
- 다형성(오버라이딩)
- 인터페이스
- 익명의 클래스
- 예외처리

# 자바 API 공부해야 할 것
1. 매개변수
2. 결과값
3. 메소드명
4. 용도
5. 오버라이딩(내 프로그램에 맞게 변형)
# 자바 API
![](https://4.bp.blogspot.com/-GxunHEBiBmI/U6bst9SZh3I/AAAAAAAAAWY/LmU09gJrXu8/s1600/package+in+java.png)


### 1. java.lang
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

### 2. java.util
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

# import
> import사용하는 방법
```java
import java.util.*;
```
> import를 사용하지 않는 방법
```java
java.util.Date date=new java.util.Date();
javax.swing.JFrame f= new javax.swing.JFrame();
```





