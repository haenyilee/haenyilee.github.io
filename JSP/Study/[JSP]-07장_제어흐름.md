---
sort: 2
---

# 제어 흐름

- 화면 이동
- 화면 조립

#### ```<jsp:include>``` : jsp파일 여러개를 묶어서 한 화면으로 만들어준다
- include : jsp안에 다른 jsp를 첨부해서, 원하는 위치에 출력할때 쓰는 형식이다.
- 즉, 기존 파일에 다른 파일의 내용을 붙여넣기 하는 것이다.
- spring에서도 사용하는 기능이다.
- 여러개의 jsp가 모여있는데 어떤 jsp든 상관없이 request를 공유할 수 있다.
- 전송 : 전체 include된 파일 전체를 가지고 있는 JSP전송해야 request가 항상 공유된다.

- <jsp:include> : jsp마다 따로 실행한 다음에 html만 묶어주는 역할 수행
- <%@include%> : jsp를 미리 묶어서 합쳐진 jsp를 컴파일 하는 것
  - 단점 : 변수가 동일하면 error를 발생시킨다. 

#### ```<jsp:forward>```
- 화면은 이동 : request를 사용할 수 있음
- 덮어쓰기의 개념에 가까운 기법이다.
- JSP만 가지고 짜는 Model1구조에서는 forward는 거의 사용하지 않는다
- spring framework인 MVC구조에서 많이 사용한다.
- 대소문자를 구분하니 주의하기
- ```<Resource />```
- 커넥션 풀 : 미리 Connection 객체를 만들어서 

#### ```<jsp:sendRedirect>```
- 화면 이동 , request가 초기화 된다.

## VO
- JSP : 새로운 데이터형 만들기 , 여러 데이터를 모아서 관리 (Bean)
- MyBatis : 데이터를 모아서 전송하는 것을 목적으로 함 (DTO)
- Spring : 값을 저장하는 클래스 (~VO)

## server.xml
- 경로 : 
- ```<Context>```뒤에 ``` / ```를 지우고 ```</Context>```를 추가한다
- ```<Resource>```태그를 활용해서 DBCP을 작성한다.

## 커넥션 풀이란?
- DBCP = DataBase Connection Pool
- 데이터베이스와 연결된 커넥션을 미리 만들어서 풀 속에 저장해 두고 있다가 <br>
필요할 때에 커넥션을 풀에서 가져다 쓰고 다시 풀에 반환하는 기법
- 커넥션을 생성하는데 드는 연결 시간이 소비되지 않는다.
- 커넥션을 재사용하기 때문에 생성되는 커넥션 수가 많지 않다.
- maxActive="10" : Connection의 총 갯수
- maxIdle="10" : 현재 사용 중에 있는 Connection
- maxWait="-1" : 10명이 동시에 사용할 Connection
  - 10명이 동시에 사용할 때, 사용이 가능할때까지 기다리는 시간
  - -1은 무한대 기다리라는 의미임
- ConnectionPool을 활용하면 Connection 객체를 열때만 달라지게 된다
  - 기존 : 직접 생성
  - 변경 후 : 미리 만들어져 있기 때문에 만들어진 주소(name)만 얻어오면 된다. ==> jdbc/oracle

```xml
<Resource 
           driverClassName="oracle.jdbc.driver.OracleDriver"
           username="hr"
           password="happy"
           url="jdbc:oracle:thin:@211.238.142.181:1521:XE"
           // 윗부분은 데이터베이스 정보임 : 오라클과의 연결을 시도하는 것임
           auth="Container"
           name="jdbc/oracle"
           type="javax.sql.DataSource"
           maxActive="10"
           maxIdle="10"
           maxWait="-1"
/>
```

##
- - 웹프로그램은 항상 오라클 연결해서 데이터를 가지고 온다
- 방식 : JDBC , DBCP , ORM(MyBatis)


#### ORM
- 마이바티스와 하이버네이트같은 프레임워크를 ORM이라고 한다.
- Connection
- PreparedStatement , Resultset
