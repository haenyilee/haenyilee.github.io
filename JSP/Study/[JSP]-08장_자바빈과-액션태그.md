---
sort: 6
---

# 자바빈과 액션태그

## 1. 액션태그란?

- 자바 문법을 태그형으로 제작한 것이 액션태그이다.
- 형식 : ```<jsp: >```
- 아래 3가지 종류만 기억하면 된다. <br>
(1) ```<jsp:include page="첨부할 jsp파일명">``` <br>
(2) ```<jsp:useBean id="dao" class="MemberDAO">``` <br>
(3) ```<jsp:setProperty name="객체명" property="">```


## 2. 액션태그의 종류
### 2.1 ```<jsp:include page="첨부할 jsp파일명">```





### 2.2 ```<jsp:useBean id="dao" class="MemberDAO">```
- id는 객체명이 된다.
- 자바에서의 코딩으로 구현하면 아래와 같으며, 메모리 할당을 하는 용도로 사용된다.

```JAVA
MemberDAO dao = new MemberDAO();
```

- 아래 자바코딩처럼 메모리 할당하지 말고, 

```java
MemberVO vo=new MemberVO();
```

- 아래 JSP 액션 태그를 써야 한다.

```jsp
<jsp:useBean id="vo" class="MemberVO">
```


### 2.3 ```<jsp:setProperty name="객체명" property="변수명" value="값">```
- name : id명칭
- property : 변수명
- setProperty는 setXxx()에 값을 채워주는 역할을 담당한다.


- 자바에서의 아래 코딩이

```java
vo.setName("홍길동");
vo.setNo(1);
```
- jsp에서는 아래 액션태그로 대체된다.

```jsp
<jsp:setProperty name="vo" property="no" value="1">
<jsp:setProperty name="vo" property="name" value="홍길동">
```

- *을 주게 되면 전체 변수에 값을 한 번에 채울 수 있다.

```jsp
<jsp:setProperty name="vo" property="no" value="*">
```

- 폼에 입력한 값을 자바 객체에 저장할때 유용하게 사용된다



## 3. 액션태그의 목적은?
- 싱글턴
- ```<% %>``` 형식의 자바 코딩을 제거하기 위함이다.

```jsp
	String name=request.getParameter("name");
	String no = request.getParameter("no");
	String subject=request.getParameter("subject");
	String content=request.getParameter("content");
	String pwd=request.getParameter("pwd");
	
	DataBoardVO vo= new DataBoardVO();
	vo.setNo(Integer.parseInt(no));
	vo.setName(name);
	vo.setSubject(subject);
	vo.setContent(content);
	vo.setPwd(pwd);
```

- 위의 코딩과 같이 vo에 값을 채워주는 과정을 하나로 줄여주기 위해 사용한다.







## 4. useBean/setProperty 액션 태그 연습하기

### 4.1 MemberBeans.java 만들기

### 4.2 input.jsp만들기
- form태그에 담아서 output.jsp로 보내기

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
		<h1>회원정보</h1>
		<form method=post action="output2.jsp">
		이름:<input type=text name=name size=15><br>
		성별:<input type=radio name=sex value="남자" checked>남자
			<input type=radio name=sex value="여자">여자<br>
		나이:<input type=text name=age size=15><br>
		주소:<input type=text name=addr size=30><br>
		전화:<input type=text name=tel size=30><br>
			<input type=submit value=전송>
		</form>
</body>
</html>
```

### 4.3 output.jsp
- 기존에 배웠던 방식으로 값을 받는 방식임
- 사용자가 보내준 데이터 받기
  - 받은 데이터 한글로 변환하기 
- 받은 데이터 한 개의 클래스로 묶어서 관리하기

```jsp
<%@page import="com.sist.temp.MemberBean"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%
	// 사용자가 보내준 데이터 받기
	request.setCharacterEncoding("utf-8");
	String name=request.getParameter("name");
	String sex=request.getParameter("sex");
	String age=request.getParameter("age");
	String addr=request.getParameter("addr");
	String tel=request.getParameter("tel");
	
	MemberBean bean = new MemberBean();
	bean.setName(name);
	bean.setSex(sex);
	bean.setAge(Integer.parseInt(age));
	bean.setAddr(addr);
	bean.setTel(tel);
	
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	이름:<%= bean.getName() %><br>
	성별:<%= bean.getSex() %><br>
	나이:<%= bean.getAge() %><br>
	주소:<%= bean.getAddr() %><br>
	번호:<%= bean.getTel() %><br>

</body>
</html>
```

### 4.4 input.jsp만들기
- form태그에 담아서 output2.jsp로 보내는 것으로 변경하기

### 4.5 output2.jsp
- 새로운 방식으로 값을 받기
- jsp:useBean : 메모리 할당하는 기능을 함
- jsp:setProperty : 값을 채우는 역할을 한다.
  - 정수가 있는 경우, 자동으로 Integer.parseInt() 를 한다. 
- 값을 모아서 받는 것이 아니라 한 개의 데이터를 받는 경우에는 request를 이용해서 받는 것이 좋다.
  - 액션태그는 사용자가 보내주는 데이터가 많은 경우에 사용한다.
  
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="com.sist.temp.*"%>
<%
	request.setCharacterEncoding("utf-8");
%>
<jsp:useBean id="bean" class="com.sist.temp.MemberBean">
	<jsp:setProperty name="bean" property="*"/>
</jsp:useBean>


<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>jsp액션태그로 받기</h1>
	이름:<%= bean.getName() %><br>
	성별:<%= bean.getSex() %><br>
	나이:<%= bean.getAge() %><br>
	주소:<%= bean.getAddr() %><br>
	번호:<%= bean.getTel() %><br>

</body>
</html>
```
