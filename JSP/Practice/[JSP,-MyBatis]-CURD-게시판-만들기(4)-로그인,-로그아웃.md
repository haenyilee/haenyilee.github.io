---
sort: 2
---

# CURD게시판 - 로그인, 로그아웃
# 1. 기초 셋팅
## (1) 멤버 테이블 생성
```
DROP TABLE member;
CREATE TABLE member4(
    id VARCHAR2(20),
    pwd VARCHAR2(10) CONSTRAINT mem_pwd_nn NOT NULL,
    name VARCHAR2(34) CONSTRAINT mem_name_nn NOT NULL,
    email VARCHAR2(1000),
    birthday VARCHAR2(20) CONSTRAINT mem_bd_nn NOT NULL,
    post VARCHAR2(10) CONSTRAINT mem_post_nn NOT NULL,
    add1 VARCHAR2(200) CONSTRAINT mem_add1_nn NOT NULL,
    add2 VARCHAR2(100),
    tel VARCHAR2(20),
    content CLOB CONSTRAINT mem_cont_nn NOT NULL,
    CONSTRAINT mem_id_pk PRIMARY KEY(id),
    CONSTRAINT mem_email_uk UNIQUE(email),
    CONSTRAINT mem_tel_uk UNIQUE(tel)
);
```

```
ALTER TABLE member4 ADD admin CHAR(1) CHECK(admin IN('y','n'));
```

```
INSERT INTO member4 VALUES('hong','1234','홍길동','hong@naver.com','2000-01-01','000-000',
'서울시 강남구 역삼동','','010-0000-0000','나는 admin입니다','y');
COMMIT;
```

## (2) 폴더 및 JSP 생성하기
- webcontent안에 member폴더 만들기
- member폴더 안에 login.jsp, login_ok.jsp , logout.jsp , logout_ok.jsp 만들기

## (3) VO만들기
```java
package com.sist.dao;

public class MemberVO {
	private String id;
	private String pwd;
	private String name;
	private String admin;
	private String msg;
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getPwd() {
		return pwd;
	}
	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getAdmin() {
		return admin;
	}
	public void setAdmin(String admin) {
		this.admin = admin;
	}
	public String getMsg() {
		return msg;
	}
	public void setMsg(String msg) {
		this.msg = msg;
	}
}

```

# 2. 로그인 처리하기
## main.jsp
(1) Java
- 세션에 저장된 id값 받기
  - 세션에 저장이 되면 데이터 값이 존재하지만, 저장이 안된 경우에는 null값을 가지고 있다. 
  - getAttribute메서드는 반드시 Object로 값을 받기 때문에 반드시 형변환을 해야한다.
```jsp
<%
String id=(String)session.getAttribute("id");
%>
```
- 로그인 된 경우, 안된경우 나눠서 화면 전환 처리하기
```jsp
String log_jsp="";
   	if(id==null)
   	{
   		log_jsp="../member/login.jsp";
   	}
   	else{
   		log_jsp="../member/logout.jsp";
   	}
```

(2) HTML : 화면 전환 결과 출력하기
- log_jsp include하기
```jsp
<jsp:include page="<%=log_jsp %>"></jsp:include>
```

## login.jsp
- <form> 태그로 값보내기
  - method : get/post(보안) 중에서 선택하기
  - action : 데이터를 받을 파일명 작성하기
  - ```</form>```의 위치 : 전송할 내용의 다음부분
```jsp
<form method=post action="post">
  <table>
   /* 중략 */
  </table>
</form>
```

- 일반 버튼으로 되어 있는 로그인 버튼을 데이터 보내는 유형으로 변경하기
```jsp
<input type=submit value="로그인" class="btn btn-sm btn-success">
```

## login_ok.jsp
- 사용자가 보낸 id와 pwd받기
  - login.jsp의 <input>태그에서 보낸 값을 받는 것임
  - <input>태그에서 name과 getParameter뒤에 값이 같아야 값을 받을 수 있음
```jsp
<%
	String id=request.getParameter("id");
	String pwd=request.getParameter("pwd");
%>
```

> 자바에서 값 받을 때 : name사용 <br>
> css제어 할 때 : id사용


## member-mapper.xml에서 쿼리문장 작성하기
(1) member-mapper.xml 파일 만들기
> 매퍼를 기능 별로 따로 만드는 이유는? <br>
> : 재사용을 편하게 하기 위해서 

(2) id 존재하는지 확인하기
- 사용자가 입력한 id와 일치하는 id의 갯수가 몇 개인가?
  - 0이면 존재x, 1이면 존재o
```xml
<select id="memberIdCheck" resultType="int" parameterType="String">
		SELECT COUNT(*) FROM member
		WHERE id=#{id}
</select>
```

(3) 로그인 세션정보 확인하기
```xml
<select id="memberGetInfoData" resultType="com.sist.dao.MemberVO" parameterType="String">
		SELECT pwd,id,name,admin FROM member
		WHERE id=#{id}
</select>
```

## Config.xml에 매퍼 등록하기
```xml
<mapper resource="com/sist/dao/member-mapper.xml"/>
```
- 실제 프로젝트 할때는 패키지를 용도별로 vo, mapper,dao 따로 만들어서 mapper패키지를 한 번에 Config.xml에 등록하기


## MemberDAO.java

(1) 자바 파일 생성하기
(2) XML읽어서 데이터를 저장하는 객체 만들기 (공통모듈)
```java
	private static SqlSessionFactory ssf;
	static {
		try {
			Reader reader = Resources.getResourceAsReader("Config.xml");
			ssf=new SqlSessionFactoryBuilder().build(reader);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
```

(3) 로그인 처리
- 로그인 처리가 완료되면 세션에 저장해야 하는데, 세션에 저장할 때 3개를 저장해야 한다.
   - id, name, admin여부가 세션에 저장되어야 한다.
- new Connection()이 여러 개 있는데 사용 중이면 true, 사용 중 아니면 false이다. 
  - 커넥션을 close하면 false로 변경되어 사용 가능한 상태로 변한다. 

> <JSP실행 시, 구동되는 메서드>
> - _jspInit() : 환경 설정
> - _jspService() : 사용자가 요청한 파일을 실행하기
> - _jspDestroy() : 메모리 해제


- id몇 개 존재하는지 확인하기
```java
int count=session.selectOne("memberIdCheck",id);
```

- id가 존재하지 않으면 NOID라는 메세지 보내기
```java
if(count==0) 
{
	vo.setMsg("NOID");
}		
```

- id 존재하면 member정보 가져오기
```JAVA
else
{
    vo=session.selectOne("memberGetInfoData",id);
}
```
- 가져온 member정보의 pwd와 사용자가 입력한 pwd 비교하기
```JAVA
if(pwd.equals(vo.getPwd()))
{
      vo.setMsg("OK");
}
else
{
      vo.setMsg("NOPWD");
}
```

## login_ok.jsp
- DAO보낸 값 받기
```jsp
<%
    MemberVO vo=MemberDAO.memberLogin(id, pwd);
%>
```
- id 틀린 경우 로그인 창으로 이동
```jsp
        if(vo.getMsg().equals("NOID"))
	{
%>
		<script>
		alert("ID가 존재하지 않습니다!")
		history.back();
		</script>
<%
		
	}
```

- id 존재, pwd 틀린 경우 로그인 창으로 이동
```jsp
        else if(vo.getMsg().equals("NOPWD"))
	{
%>
		<script>
		alert("비밀번호가 일치하지 않습니다!")
		history.back();
		</script>
<%
	}
```
- id 존재, pwd 맞는 경우 id,name,admin여부를 서버에 저장하고 main.jsp로 이동하기
  - 사용 중인 모든 jsp안에서 세션에 등록된 모든 데이터를 사용할 수 있다.
```jsp
	else
	{
		session.setAttribute("id", vo.getId());
		session.setAttribute("name", vo.getName());
		session.setAttribute("admin", vo.getAdmin());
		response.sendRedirect("../main/main.jsp");
	}
%>
```
# 3. 로그아웃 처리하기
## logout.jsp
- HTML 모양 잡기

- 관리자일 때와 일반유저일때 나눠서 처리하기
```jsp
<tr>
       <td>
         <%
         	String admin=(String)session.getAttribute("admin");
         	if(admin.equals("y"))
         	{
         %>
         		<b><font color=blue>관리자</font></b>
         <%
         	}
         	else
         	{
         %>
         		<b><font color=blue>일반</font></b>
         <%
         	}
         %>
       </td>
</tr>
```

- 로그아웃 버튼에 <form>태그 주기
  - 로그아웃 클릭하면 logout_ok.jsp로 이동하기

## logout.jsp
(1) session에 등록된 데이터 전체 삭제하기
```jsp
<%			
	session.invalidate();
%>
```

(2) main.jsp로 이동하기
```jsp
<%	
	response.sendRedirect("../main/main.jsp");
%>
```

# 4. 로그인/로그아웃 및 권한별 메뉴 흐름 제어하기
- 회원, 마이페이지


## nav.jsp table-hover적용하기
- ```<head>``` 부터 ```<nav>```까지 적용하기

- 로그인되면 => 회원가입, 아이디찾기, 비밀번호 찾기 대신 회원수정, 회원탈퇴 메뉴 활성화하기
  - id=null이면 로그인되지 않은 상태임
```jsp
 <%
 	String id=(String)session.getAttribute("id");
 
 %>
```
```jsp
 <ul class="dropdown-menu">
        <%
        	if(id==null)
        	{
        %>
	          <li><a href="#">회원가입</a></li>
	          <li><a href="#">아이디찾기</a></li>
	          <li><a href="#">비밀번호찾기</a></li>
        <%
        	}
        	else
        	{
        %>
        		<li><a href="#">회원수정</a></li>
         		<li><a href="#">회원탈퇴</a></li>
        <%
        	}
        %>
</ul>
```

- 로그인 안된 상태면, 영화 예매 기능 비활성화 하기
```jsp
      <%
      	if(id!=null)
      	{
      %>
	      <li><a href="#">영화추천</a></li>
	      <li><a href="#">영화예매</a></li>
	      <li><a href="#">마이페이지</a></li>
      <%
      	}
      %>
```

- 어드민 계정 정보도 받아오기
```jsp
String admin=(String)session.getAttribute("admin");
```

- 어드민 계정이면 마이페이지 대신 예매 현황이 보이도록 하기
```jsp
      <%
      	if(id!=null)
      	{
      %>
	      <li><a href="#">영화예매</a></li>
	      <li><a href="#">영화추천</a></li>
	      <%
	      	if(admin.equals("n"))
	      	{
	      %>
	      		<li><a href="#">마이페이지</a></li>
	      <%
	      	}
	      	else
	      	{
	      %>
	      		<li><a href="#">예매 현황</a></li>
	      <%
	      	}
	      %>
```

