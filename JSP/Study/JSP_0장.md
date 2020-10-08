# JSP란
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

- 화면 출력 : ```<%= %>``` = ```out.println()```
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


# JSP의 구성요소
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
- include : 특정 JSP안에 JSP를 첨부


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

### 4. 표현식 (EL , JSTL)

### 5. MVC 