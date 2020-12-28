---
sort: 5
---

# 페이지 모듈화와 요청 흐름 제어

- 페이지 모듈화와 요청 흐름 제어란?
1. 화면 이동
2. 화면 조립

## 1. ```<jsp:include>```
- jsp파일 여러개를 묶어서 한 화면으로 만들어준다
- 즉, include란 jsp안에 다른 jsp를 첨부해서 원하는 위치에 출력할때 쓰는 형식이다.
- 쉽게 생각하면 원하는 위치에만 다른 파일의 내용을 붙여넣기 하는 것과 같다.
- spring에서도 사용하는 기능이다.
- 수신 : 여러개의 jsp가 모여있는데 어떤 jsp든 상관없이 request를 공유할 수 있다.
- 전송 : 전체 include된 파일 전체를 가지고 있는 JSP전송해야 request가 항상 공유된다.

- <jsp:include> : jsp마다 따로 실행한 다음에 html만 묶어주는 역할 수행을 수행한다.
- <%@include%> : jsp를 미리 묶어서 합쳐진 jsp를 컴파일 하는 것이다.
  - 단점 : 변수가 동일하면 error를 발생시킨다. 

## 2. ```<jsp:forward>```
- 화면은 이동되지만 request를 받을 수 있음
- 덮어쓰기의 개념에 가까운 기법이다.
- JSP만 가지고 짜는 Model1구조에서는 forward는 거의 사용하지 않는다
- spring framework인 MVC구조에서 많이 사용한다.
- 대소문자를 구분하니 주의해야한다.
- ```<Resource />```
- 커넥션 풀 : 미리 Connection 객체를 만들어서 

## 3. ```<jsp:sendRedirect>```
- 화면 이동 , request가 초기화 된다.
