---
sort: 2
---

# CURD게시판 - 자유게시판

# 자유게시판 셋팅

## table 생성하기
```
CREATE TABLE movie_board4(
     no NUMBER,
     name VARCHAR2(34) CONSTRAINT mb4_name_nn NOT NULL,
     subject VARCHAR2(1000) CONSTRAINT mb4_subject_nn NOT NULL,
     content CLOB CONSTRAINT mb4_content_nn NOT NULL,
     pwd VARCHAR2(10) CONSTRAINT mb4_pwd_nn NOT NULL,
     regdate DATE DEFAULT SYSDATE,
     hit NUMBER DEFAULT 0,
     CONSTRAINT mb4_no_pk PRIMARY KEY(no)
);
```
- 시퀀스 만들기 
```
CREATE SEQUENCE mb4_no_seq
    START WITH 1
    INCREMENT BY 1
    NOCYCLE
    NOCACHE;
```
- 데이터값 집어넣기
```    
INSERT INTO movie_board4 VALUES (
    mb4_no_seq.nextval,
    '홍길동','8장','8장','1234',SYSDATE,0
);
INSERT INTO movie_board4 VALUES (
    mb4_no_seq.nextval,
    '홍길동','8장','8장','1234',SYSDATE,0
);
INSERT INTO movie_board4 VALUES (
    mb4_no_seq.nextval,
    '홍길동','8장','8장','1234',SYSDATE,0
);
INSERT INTO movie_board4 VALUES (
    mb4_no_seq.nextval,
    '홍길동','8장','8장','1234',SYSDATE,0
);
INSERT INTO movie_board4 VALUES (
    mb4_no_seq.nextval,
    '홍길동','8장','8장','1234',SYSDATE,0
);
INSERT INTO movie_board4 VALUES (
    mb4_no_seq.nextval,
    '홍길동','8장','8장','1234',SYSDATE,0
);
INSERT INTO movie_board4 VALUES (
    mb4_no_seq.nextval,
    '홍길동','8장','8장','1234',SYSDATE,0
);
INSERT INTO movie_board4 VALUES (
    mb4_no_seq.nextval,
    '홍길동','8장','8장','1234',SYSDATE,0
);
COMMIT;
```

## BoardVO 제작
- 목적 :  필요한 데이터 여러개를 모아서 한 번에 브라우저로 보내기 위해 직접 제작한다.
- ~Bean , ~DTO, ~VO 라는 이름으로 쓰인다.

## BoardDAO 제작
- date는 java.utill의 메서드임
- sql 컬럼명과 vo의 변수명이 불일치하면 마이바티스가 둘을 매칭하지 못해서 꼭 같게 설정해줘야함

## mapper 제작하기
- 새로 생성한 mapper파일은 xml에 등록해둬야 마이바티스가 읽어갈 수 있다.


# 자유게시판 목록 출력하기
## [freeboard]폴더에 list.jsp만들기
- 모양만 잡아둔다.

## main.jsp , nav.jsp와 연결시키기

## VO에 dbday 변수 추가하기
- 날짜를 simple-date 형식으로 변경해서 저장하기 위함임

## board-mapper에 SQL문장 작성하기
- 다른 매퍼 파일의 id와 같은 것을 주는 경우에도 오류가 발생하므로, <BR>
ID명칭을 다르게 줘야 한다.
  - 앞에 테이블 명을 붙여주면 충돌없이 ID를 지을 수 있다.
  - 마이바티스와 스프링은 먼저 XML을 다 읽기 때문에 XML이 틀릴 경우에는 구동이 안된다.

## DAO 작성


## [freeboard/list.jsp]
(1) Java
- 사용자가 요청(전송)한 데이터는 톰캠이 모든 데이터를 묶어서 request라는 객체에 첨부한다.
- 모든 데이터가 request를 통해서 들어오는데, 이때 이 값을 받는 메소드를 `request.getParameter()`라고 한다.

- ```list.jsp?page=1```의 값을 받게 되면 ```request.getParameter("page");```의 결과값이 1이다.
- ```list.jsp``` 의 값을 받게 되면 ```request.getParameter("page");```의 결과값이 null이다.
  - ```if(strPage==null)```
- ```list.jsp?page=``` 의 값을 받게 되면 ```request.getParameter("page");```의 결과값이 ""(공백)이다.
  - ```if(strPage.equals(""))```

```jsp
	String strPage=request.getParameter("page");
	if(strPage==null)
		strPage="1";
```

- 현재 페이지 받기
```jsp
int curpage=Integer.parseInt(strPage);
```

- 사용자가 요청한 페이지의 첫번째 데이터와 마지막 데이터의 번호를 map으로 받아서 리스트로 묶어두기
```jsp
	Map map = new HashMap();
	int rowSize=10;
	int start=rowSize*(curpage-1)+1; // 오라클
	int end=rowSize*curpage;
	map.put("start", start);
	map.put("end", end);
	List<BoardVO> list = BoardDAO.freeBoardListData(map);
```
#### cf. List
```
- ArrayList는 비동기화(섞여서 값을 가져오기 때문에 속도는 빠르지만 순서가 뒤섞여져서 값을 가져옴) 
이미 저장되어 있는 데이터를 사용할때 주로 사용함(오라클)
- Vector : 동기화(다 읽어서 저장이 된 후에 값을 가져옴)방식임, 속도가 느림
- LinkedList() : C언어 호환할 때 사용함
```

(2) HTML


# 자유게시판 새글 추가하기

## list.jsp에서 새글 추가 버튼 만들기
- 클릭하면 mode 10번인 insert.jsp로 넘어가야 한다.
```html
<a href="../main/main.jsp?mode=10" class="btn btn-sm btn-primary">새글</a>
```


## main.jsp에 mode10번 include하기
```jsp
<%
case 10:
 jsp="../freeboard/insert.jsp";
 break;
%>
```

## freeboard/insert.jsp 만들기
- 기존 것 복붙하기
- 첨부파일 기능 삭제
- <form method="post" action="../freeboard/insert_ok.jsp">로 변경하기

## freeboard/insert_ok.jsp 만들기
- 역할 : 사용자가 보내준 데이터를 받아서 DAO와 연결하고 list.jsp로 이동하기
- 사용자가 입력한 값 한글로 변환하기
```jsp
<%
	request.setCharacterEncoding("utf-8");
%>
```

- 메모리 할당하기
```JSP
<jsp:useBean id="vo" class="com.sist.dao.BoardVO">
</jsp:useBean>
```

- vo에 사용자가 보내준 값을 채우라고 명령 내리기
  - useBean의 id와 setProperty의 name이 꼭 일치해야함
```jsp
<jsp:setProperty name="vo" property="*"/>
```

- 자바코드로 변환하면 아래와 같음
```java
vo.setName(request.getParameter("name"));
vo.setNo(request.getParameter("no"));
// 이하 생략
```

- DAO에 값 넘겨주기
  - DAO에서 freeboardInsert를 만들어 줘야함
```JSP
<%
	BoardDAO.freeboardInsert(vo);
%>
```

## board-mapper에 SQL 쿼리문장 작성하기
```
<insert id="freeBoardInsert" parameterType="com.sist.dao.BoardVO">
 		INSERT INTO movie_board4 VALUES(
 			mb_no_seq.nextval,
 			#{name},
 			#{subject},
 			#{content},
 			#{pwd},
 			SYSDATE,
 			0
 )
</insert>
```

## DAO에서 freeBoardInsert만들기
```java
public static void freeBoardInsert(BoardVO vo)
	{
		SqlSession session=null;
		try {
			session=ssf.openSession(true);
			session.insert("freeBoardInsert",vo);
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if(session!=null)
				session.close();
		}
	}
```

## freeboard/insert_ok.jsp에서 화면 이동시키기
```jsp
<%
response.sendRedirect("../main/main.jsp?mode=9");
%>
```


# 자유게시판 글 상세보기

## list.jsp 에서 제목 클릭하면 detail.jsp로 넘어가게 만들기
- <a> 태그 사용하기
- mode11을 detail.jsp로 설정
- no값도 반드시 넘겨줘야함 : 사용자가 클릭한 게시글이 뭔지 no로 넘겨줘야 함
```jsp
<td class="text-left" width=45%>
<a href="../main/main.jsp?mode=11&no=<%=vo.getNo()%>"><%=vo.getSubject() %></a>
</td>
```

## main.jsp에서 detail.jsp include하기
```jsp
case 11:
  jsp="../freeboard/detail.jsp";
  break;
```

## detail.jsp만들기
- board/detail.jsp와 동일하지만, 파일 첨부 기능만 지워주기
- 이 부분에 들어갈 내용은 board-mapper.xml에서 SQL문장을 작성해서 DAO에서 값을 가지고 와야 한다.


## board-mapper.xml에서 쿼리문장 작성하기
(1) 조회수 증가시키기
- parameterType : 게시물의 번호를 대입해줄 것이므로 int라고 작성한다.
```xml
<update id="freeBoardHitIncrement" parameterType="int">
 		UPDATE movie_board SET 
 		hit=hit+1
 		WHERE no=#{no} 
</update>
```

(2) 게시글 내용 데이터 읽기
```xml
<select id="freeBoardDetailData" resultType="com.sist.dao.BoardVO" parameterType="int">
 		SELECT no,name,subject,content,TO_CHAR(regdate,'YYYY-MM-DD') as dbday,hit
 		FROM movie_board4
 		WHERE no=#{no}
</select>
```

## DAO작성하기
- 반환타입 작성하기
```JAVA
public static BoardVO freeBoardDetailData(int no)
{
	BoardVO vo = new BoardVO();
	return vo;
}
```

- 조회수 증가시키기
```java
session.update("freeBoardHitIncrement",no);
```

- 게시글 상세 데이터 받기
```java
vo=session.selectOne("freeBoardDetailData",no);
```


## detail.jsp에서 화면에 출력하기
(1) Java
- list.jsp → main.jsp를 통해 전송된 no값 받기
```jsp
<%
String no=request.getParameter("no");
%>
```
- no값에 해당하는 vo 데이터 받아오기
```jsp
<%
BoardVO vo=BoardDAO.freeBoardDetailData(Integer.parseInt(no));
%>
```

(2) HTML
- 첨부파일 내용 빼고 기존 detial.jsp와 동일
- 목록으로 돌아가기 버튼 만들기
```jsp
<a href="../main/main.jsp?mode=9" class="btn btn-xs btn-danger">목록</a>
```



# 자유게시판 수정하기
- update.jsp : mode=12
## detail.jsp에 수정 버튼 만들기
```jsp
<a href="../main/main.jsp?mode=12&no=<%=vo.getNo() %>" class="btn btn-xs btn-primary">수정</a>
```

## main.jsp에 update.jsp include하기
```jsp
<%
switch(index)
{
  case 12:
   jsp="../freeboard/update.jsp";
   break;
}
%>
```

## board-mapper에서 쿼리문장 작성하기
- 수정할 게시글 내용 불러오는 쿼리문장은 필요 없음
  - freeBoardDetailData와 동일하기 때문에 재사용이 가능하기 때문이다.

## BoardDAO 작성하기
- board-mapper.xml에 이미 만들어진 SQL문장이 있는 경우 재사용이 가능하다.
- freeBoardUpdateData 만들기
```java
//<select id="freeBoardDetailData" resultType="com.sist.dao.BoardVO" parameterType="int">
	public static BoardVO freeBoardUpdateData(int no)
	{
		BoardVO vo = new BoardVO();
		SqlSession session=null;
		return vo;
	}
```
- 쿼리문장 돌려서 나온 결과값 vo에 받기
```java
try
{	session=ssf.openSession(true);
	vo= session.selectOne("freeBoardDetailData",no);
} catch (Exception e) {
	e.printStackTrace();
} finally {
	if(session!=null)
		session.close();
}
```
## update.jsp에서 결과값 받아서 화면에 출력하기
(1) Java
```jsp
<%
	String no=request.getParameter("no");
	BoardVO vo= BoardDAO.freeBoardDetailData(Integer.parseInt(no));
%>
```
(2) HTML
- 기존 update.jsp 파일 내용과 유사하기 때문에 거의 복붙!
- submit action 태그 경로 수정하기
```jsp
<form method="post" action="../freeboard/update_ok.jsp">
```


## update_ok.jsp 만들기
- import
```jsp
<% page import="com.sist.dao.*"%>
```

- 한글변환해서 값 받기
```jsp
<%
	request.setCharacterEncoding("utf-8");
%>
```

- 메모리 할당하기
```JSP
<jsp:useBean id="vo" class="com.sist.dao.BoardVO">
</jsp:useBean>
```

- vo가 가지고 있는 setXxx()메서드에 모든 값을 채우기
```jsp
<jsp:setProperty name="vo" property="*"/>
```

- BoardDAO에 vo를 전송하기

- 수정 완료되면 detail.jsp로 이동하기
```jsp

```
## board-mapper에서쿼리문 작성하기
(1) 저장된 비밀번호 가져오기
- 수정 시, 입력한 비밀번호와 일치하는 지 확인하기 위함임
```xml
<select id="freeBoardGetPassword" resultType="String" parameterType="int">
 		SELECT pwd FROM movie_board4
 		WHERE no=#{no}
</select>
```

(2) 내용 수정하기
```xml
<update id="freeBoardUpdtae" parameterType="com.sist.dao.BoardVO">
 		UPDATE movie_board4 SET
 		name=#{name},
 		subject=#{subject},
 		content=#{content}
 		WHERE no=#{no}
 </update>
```

## DAO에서 쿼리 실행 후 값 모으기
(1) 비밀번호 검증 후, 데이터 받아오기
- bCheck : 비밀번호 맞으면 true, 틀리면 false를 반환함
```java
public static boolean freeBoardUpdate(BoardVO vo)
{
	boolean bCheck=false;
	
	SqlSession session = null;
	return bCheck;
}
```
- 연결할 커넥션 객체 얻어오기
```java
session=ssf.openSession(true);
```

- SQL 문장 실행 요청하기
  - 비밀번호 가지고 오기
```java
String db_pwd= session.selectOne("freeBoardGetPassword", vo.getNo());
```


- 비밀번호 일치할 때
```java
if(db_pwd.equals(vo.getPwd()))
{
	bCheck=true;
	session.update("freeBoardUpdtae",vo);
	session.commit();
}
```


- 비밀번호 일치하지 않을 때 
```java
else
{
	bCheck=false;
}
``` 


## update_ok.jsp
- DAO에서 비밀번호 검증 결과 받아오기
```jsp
<%
boolean bCheck= BoardDAO.freeBoardUpdate(vo);
%>
```
- 비밀번호 맞으면 mode=11로 이동하기
```jsp
<%
	if(bCheck==true)
	{
		response.sendRedirect("../main/main.jsp?mode=11&no="+vo.getNo());
	}
%>
```
- 비밀번호 틀리면 update.jsp로 다시 이동하기
```
{% raw %}
{{
<%
	else
	{
		<script>
		alert("비밀번호가 틀립니다");
		history.back();
		</script>
	}
%>
}}
{% endraw %}
```


# 자유게시판 삭제하기

## detail.jsp에 삭제 버튼 만들기
- 삭제버튼
```jsp
<input type="button" class="btn btn-xs btn-success" id="delBtn" value="삭제">
```

- 삭제하기 버튼 클릭하면 보이는 비밀번호 칸 만들기
```jsp
<tr id="del" style="display:none">
       	<td colspan="4" class="text-right">
       	비밀번호: <input type=password class="input-sm" size=10 id="pwd">
       		  <input type=submit value="삭제하기" class="btn-sm btn-danger">
       	</td>
</tr>
```

- 삭제하기 버튼 클릭하면 보이고, 클릭안하면 안보이도록 자바스크립트 작성하기
```jsp
{% raw %}
{{
<script type="text/javascript" srs="http://code.jquery.com/jquery.js"></script>
<script>
var i=0;
$(function(){
	$('#delBtn').click(function(){
		// 조건제시하기
		if(i==0){
			$('#del').show();
			$('#delBtn').val('취소');
			i=1;
		}
		else{
			$('#del').hide();
			$('#delBtn').val('삭제');
			i=0;
		}
			
	})
});
</script>
}}
{% endraw %}
```


## main.jsp에 delete.jsp include하기
```jsp
case 13:
 	jsp="../freeboard/delete.jsp";
 	break;
```
