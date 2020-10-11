---
sort: 2
---

# JSP 기초

## JSP란

- Java Server Page : 서버에서 실행되는 자바파일
- Java + HTML : HTML 기반에 Java가 첨부되는 것

- 자바영역 : ```<% %>```

```jsp
<html>
  <body>
    <h1>게시판</h1>
    <ul>
    <%
      for(BoardVO vo:list)
      {
    %>
        <li> 번호 - 제목 - 이름 - 작성일 - 조회수 </li>
    <%
      }
    %>
    </ul>
  </body>
</html>
```

- 화면 출력 영역 : ```<%= %>``` = ```out.println()```

```jsp
<body>
	<%
		Date date = new Date();
		SimpleDateFormat sdf= new SimpleDateFormat("yyyy-MM-dd");
		String today=sdf.format(date);
	
	%>
	오늘 날짜는 <%=today %>입니다. <br>
	오늘 날짜는 <% out.println(today); %>입니다.

</body>
```


## JSP 구동방식

- 톰캣이 JSP를 .java로 변환함 
- 컴파일해서 .class로 변환
- 한줄씩 읽어서 출력함
- 한줄씩 번역
- 메모리 내용(HTML)을 브라우저에서 읽어서 출력

## JSP의 구성요소
### 1. 지시자
- JSP의 시작부분

#### page : JSP 파일에 대한 정보
- 지정된 속성에 값을 채워야 한다. ```jsp<%@ page 속성="값",속성="값"...%>```

```JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="java.util.*,java.text.*"%> 
```

- contentType
  - 브라우저에 HTML을 전송한다. (브라우저에서 HTML 파싱 준비)
  - contentType="text/html" => 화면에 출력
  - contentType="text/xml" => 문서저장

- charset
  - default는 영문 : ISO-8859_1 
  - 한글 : UTF-8
  - response.setContentType("text/html;charset=euc-=kr")

- import
  - 이미 만들어진 클래스를 읽어올 때 사용 (라이브러리 로드)
  - pageEncoding 뒤에 써도 되고, 아래처럼 page 태그 뒤에 써도 됨

```jsp
<%@ page import="java.util.*" %>
<%@ page import="java.text.*" %>
```

- errorPage="jsp파일 등록"
  - 에러가 나면 이동하는 파일임

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="java.util.*,java.text.*" errorPage="error.jsp"%> 
```

- buffer
  - 임시 저장장치
  - html이 출력되는 장소
  - 크기가 커지면 버퍼 수정


#### tablib : 태그로 자바의 문법을 만들어줌 
  - 제어문 : ```<c:forEach>``` , ```<c:if>```

```jsp
<html>
  <body>
    <h1>게시판</h1>
    <ul>
      <c:forEach items="list">
        <li> 번호 - 제목 - 이름 - 작성일 - 조회수 </li>
      </c:forEach>
    </ul>
  </body>
</html>
```

#### include : 특정 JSP안에 JSP를 첨부
- 조립식 프로그램이 시작됨
- 여러개 파일을 나눠서 하나에 묶어서 처리하는 프로그램 설계를 위해 나온 것이 include임
- 스프링에서 사용됨

### 2. 자바 코딩 부분
- 스크립트릿 ```<% %>``` : 일반 자바 코딩
- 표현식 ```<%= %>``` : out.println() , 화면에 값을 출력
- 선언식 ```<%! %>``` :  전역변수 , 메소드를 만들 경우 (사용빈도


### 3. 내장객체 (8개 지원)
- 미리 객체를 생성해 놓고 필요시마다 사용가능하게 만들어주는 것
- request : 사용자의 요청값을 받는 것
- response : 서버에서 처리가되면 응답을 해줘야 하는데, 응답해주는 것을 response라고 함
- session
- pageContext
- page
- config
- exception
- application


### 4. 액션태그
#### include 
- 조립식 프로그램이 시작됨
- 여러개 파일을 나눠서 하나에 묶어서 처리하는 프로그램 설계를 위해 나온 것이 include임
- 스프링에서 사용됨

#### useBean

#### setProperty


### 5. 표현식 (EL , JSTL)

### 6. MVC 

### server.xml
- 경로 : 
- ```<Context>```뒤에 ``` / ```를 지우고 ```</Context>```를 추가한다
- ```<Resource>```태그를 활용해서 DBCP을 작성한다.

### 커넥션 풀이란?
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

####
- - 웹프로그램은 항상 오라클 연결해서 데이터를 가지고 온다
- 방식 : JDBC , DBCP , ORM(MyBatis)


#### ORM
- 마이바티스와 하이버네이트같은 프레임워크를 ORM이라고 한다.
- Connection
- PreparedStatement , Resultset
 
