---
sort: 3
---

# CRUD 준비과정
- 프로젝트 생성
- 자바 리소스 라이브러리 변경
  - Java Resources > Libraries > JRE System Library > Properties > Workspace default JRE(jdk 1.8.0_251)


# 오라클 데이터 컬럼 선언
- 경로 : [com.sist.dao] - BoardVO

#### BoardVO
- 데이터형, 변수명 선언

```java
package com.sist.dao;

import java.util.Date;

public class BoardVO {
	private int no;
	private String name;
	private String subject;
	private String content;
	private String pwd;
	private Date regdate;
	private int hit;
}
```

- 캡슐화

```java
package com.sist.dao;

import java.util.Date;

public class BoardVO {
	private int no;
	private String name;
	private String subject;
	private String content;
	private String pwd;
	private Date regdate;
	private int hit;
	public int getNo() {
		return no;
	}
	public void setNo(int no) {
		this.no = no;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getSubject() {
		return subject;
	}
	public void setSubject(String subject) {
		this.subject = subject;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}
	public String getPwd() {
		return pwd;
	}
	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	public Date getRegdate() {
		return regdate;
	}
	public void setRegdate(Date regdate) {
		this.regdate = regdate;
	}
	public int getHit() {
		return hit;
	}
	public void setHit(int hit) {
		this.hit = hit;
	}
	
}
```

# 오라클 연결
- 경로 : [com.sist.dao] - [BoardDAO] - BoardDAO() , getConnection() , disConnection()

#### BoardDAO()

```java
package com.sist.dao;

import java.util.*;
import java.sql.*;

public class BoardDAO {
         // 연결 
	private Connection conn; // 오라클 연결 클래스

	 // SQL문장을 전송 
	private PreparedStatement ps; 

	 // 오라클 주소 첨부 
	private final String URL="jdbc:oracle:thin:@localhost:1521:XE";

	 // 연결 준비 
	 // 드라이버 등록 
	public BoardDAO()
	{
		try
		{
			Class.forName("oracle.jdbc.driver.OracleDriver");
		}catch(Exception ex)
		{
			System.out.println(ex.getMessage());
		}
	}

}
```

#### getConnection()

```java
package com.sist.dao;

import java.util.*;
import java.sql.*;

public class BoardDAO {

	public BoardDAO()
	{
	    // 생략
	}

	// 연결/닫기 반복 => 기능이 반복일 경우 => 메소드로 처리
	public void getConnection()
	{
		try
		{
			conn=DriverManager.getConnection(URL,"hr","happy");
		}catch(Exception ex) {}
	}
	
}
```

#### disConnection()

```java
package com.sist.dao;

import java.util.*;
import java.sql.*;

public class BoardDAO {

	public BoardDAO()
	{

	}
	// 연결/닫기 반복 => 기능이 반복일 경우 => 메소드로 처리
	public void getConnection()
	{

	}
	public void disConnection()
	{
		try
		{
			if(ps!=null) ps.close();
			if(conn!=null) conn.close();
		}catch(Exception ex) {}
	}
}

```



# 게시물 목록
### 1. 오라클 데이터와 게시글 목록 연결
- 경로 : [com.sist.dao] - [BoardDAO] - boardListData()

```java
package com.sist.dao;

import java.util.*;
import java.sql.*;

public class BoardDAO {

	public BoardDAO()
	{

	}
	// 연결/닫기 반복 => 기능이 반복일 경우 => 메소드로 처리
	public void getConnection()
	{

	}
	public void disConnection()
	{

	}

	// 기능 
	// 1. 목록 (게시판) => SELECT
	public ArrayList<BoardVO> boardListData()  // 리턴형이 ArrayList이고 <BoardVO>클래스를 받는 메서드
	{
		ArrayList<BoardVO> list=
				new ArrayList<BoardVO>();
		try
		{
			// 연결
			getConnection();
			// SQL문장 전송 
			String sql="SELECT no,subject,name,regdate,hit FROM freeboard ORDER BY no DESC";
			          // 최신 등록된 게시물 먼저 출력
			          // ORDER BY => 단점 (속도가 늦다) => INDEX를 사용권장
			ps=conn.prepareStatement(sql);

			// SQL실행후에 결과값 받기
			/* <executeQuery()> 
			   1. 수행결과로 ResultSet 객체의 값을 반환합니다.
			   2. SELECT 구문을 수행할 때 사용되는 함수입니다.
			*/

			/* <resultset> 은 원래 recordset이었음
			   근데 ms에서 먼저써서 클래스 이름 바뀜
			*/

			ResultSet rs=ps.executeQuery();

			// 결과값을 => ArrayList에 첨부
			/* next() : sql record 한줄씩 읽어오는 클래스임
			   - rs.next() : 출력한 첫번쨰줄부터 마지막줄까지 읽어온다
			*/
			while(rs.next()) // 출력한 첫번째줄부터 마지막줄까지 읽어 온다
			{
				BoardVO vo=new BoardVO();
				vo.setNo(rs.getInt(1));
				vo.setSubject(rs.getString(2));
				vo.setName(rs.getString(3));
				vo.setRegdate(rs.getDate(4));
				vo.setHit(rs.getInt(5));
				
				list.add(vo);
			}
			rs.close();
		}catch(Exception ex)
		{
			System.out.println(ex.getMessage());
		}
		finally
		{
			disConnection();
		}
		return list;
	}
}
```


### 2. 오라클 연결된 목록 웹으로 출력
- 경로 : [com.sist.board] - [BoardList] - doget()

#### 게시글 목록 컬럼까지만 출력
- table-hover css활용  
(https://www.w3schools.com/bootstrap/tryit.asp?filename=trybs_table_contextual&stacked=h)

```java
package com.sist.board;

import java.io.*;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.*;//ArrayList
import com.sist.dao.*;

@WebServlet("/BoardList")
public class BoardList extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		// 브라우저에서 실행하는 화면 => HTML
		// 브라우저에 알림 => HTML문서를 전송할 것이다 
		response.setContentType("text/html;charset=EUC-KR");

		// HTML을 브라우저로 전송 시작 
		PrintWriter out=response.getWriter();

		out.println("<html>");
		out.println("<head>");
		out.println("<link rel=\"stylesheet\" href=\"https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css\">");
		out.println("<style type=text/css>");
		out.println(".row {margin:0px auto;width:700px}");
		out.println("h2 {text-align:center}");
		out.println("</style>");
		out.println("</head>");

		out.println("<body>");
		out.println("<div class=container>");
		out.println("<h2>자유게시판</h2>");
		out.println("<div class=row>");
		
		// 새글 작성하는 버튼
		out.println("<table class=\"table\">");
		out.println("<tr>");
		out.println("<td>");
		out.println("<a href=BoardInsert class=\"btn btn-sm btn-success\">새글</a>");
		out.println("</td>");
		out.println("</tr>");
		out.println("</table>");
		
		out.println("<table class=\"table table-hover\">");
		out.println("<tr class=danger>");
		out.println("<th class=text-center width=10%>번호</th>");
		out.println("<th class=text-center width=45%>제목</th>");
		out.println("<th class=text-center width=15%>이름</th>");
		out.println("<th class=text-center width=20%>작성일</th>");
		out.println("<th class=text-center width=10%>조회수</th>");
		out.println("</tr>");
		out.println("</table>");
		
		out.println("</div>");
		out.println("</div>");
		out.println("</body>");
		out.println("</html>");
		
	}

}
```

#### 게시글 목록 데이터까지 출력

```java
package com.sist.board;

import java.io.*;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.*;//ArrayList
import com.sist.dao.*;

@WebServlet("/BoardList")
public class BoardList extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		// 출력 
		BoardDAO dao=new BoardDAO(); // 출력할 데이터가 있는 클래스 객체 생성

		ArrayList<BoardVO> list=dao.boardListData(); // 출력할 데이터가 담긴 배열 배열 객체 생성

		for(BoardVO vo:list) // list 배열에 담긴 데이터 출력하는 for-each문
		{
			// 1. 게시글 번호
			out.println("<tr>");
			out.println("<td class=text-center width=10%>"+vo.getNo()+"</td>");

			// 2. 게시글 제목 : 제목을 클릭하면 게시글 상세 링크로 넘어감
			out.println("<td class=text-left width=45%>"
			+"<a href=BoardDetail?no="+vo.getNo()+">"
			/* a 태그 역할 : no라는 매개변수를 주겠다... 웹주소 끝에 글번호 삽입
			   실행결과 : http://localhost/20200814-CURDProject/BoardDetail?no=12
			*/
			+vo.getSubject()+"</a></td>");

			// 3. 게시글 작성자 
			out.println("<td class=text-center width=15%>"+vo.getName()+"</td>");

			// 4. 게시글 작성일 : Date 형식이라서 String으로 형변환 
			out.println("<td class=text-center width=20%>"+vo.getRegdate().toString()+"</td>");

			// 5. 게시글 조회수 
			out.println("<td class=text-center width=10%>"+vo.getHit()+"</td>");
			out.println("</tr>");
		}
		out.println("</table>");
		out.println("<hr>"); // 가로 경계선
		
	}

}
```

#### 추가 버튼 생성

```java
package com.sist.board;

import java.io.*;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.*;//ArrayList
import com.sist.dao.*;

@WebServlet("/BoardList")
public class BoardList extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		out.println("<table class=\"table\">");
		out.println("<tr>");

		// 게시글 찾기		
		out.println("<td class=text-left>");
		out.println("Search:");
		out.println("<select class=input-sm>");
		out.println("<option>이름</option>");
		out.println("<option>제목</option>");
		out.println("<option>내용</option>");
		out.println("</select>");
		out.println("<input type=text size=15 class=input-sm>");
		out.println("<input type=button value=찾기 class=\"btn btn-sm btn-danger\">");
		out.println("</td>");
		
		// 페이지 목록
		out.println("<td class=text-right>");
		out.println("<a href=BoardInsert class=\"btn btn-sm btn-primary\">이전</a>");
		out.println("0 page / 0 pages");
		out.println("<a href=BoardInsert class=\"btn btn-sm btn-primary\">다음</a>");
		out.println("</td>");
		
		out.println("</tr>");
		out.println("</table>");
		
		out.println("</div>");
		out.println("</div>");
		out.println("</body>");
		out.println("</html>");
		
	}

}

```



# 새글 글쓰기
### 1. 웹에서 요청받고 처리한 뒤 웹으로 출력
- 경로 : [com.sist.board] - [BoardInsert] - doGet() , doPost()
#### doGet()
- 화면 출력 폼

```java
package com.sist.board;

import java.io.*;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.sist.dao.*;

@WebServlet("/BoardInsert")
public class BoardInsert extends HttpServlet {
	private static final long serialVersionUID = 1L;


	// 폼작업(화면출력)
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
				// 브라우저에서 실행하는 화면 => HTML
				// 브라우저에 알림 => HTML문서를 전송할 것이다 
				response.setContentType("text/html;charset=EUC-KR");
				// HTML을 브라우저로 전송 시작 
				PrintWriter out=response.getWriter();

				out.println("<html>");
				out.println("<head>");
				out.println("<link rel=\"stylesheet\" href=\"https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css\">");
				out.println("<style type=text/css>");
				out.println(".row {margin:0px auto;width:500px}");
				out.println("h2 {text-align:center}");
				out.println("</style>");
				out.println("</head>");
				out.println("<body>");
				out.println("<div class=container>");
				out.println("<h2>글쓰기</h2>");
				out.println("<div class=row>");
				
				/* do post 호출 : dopost에 입력한 값을 보내기 
				 전송방식 (1) 감춰서 post 보안에 조음(2) 노출해서 get */
				out.println("<form method=post action=BoardInsert>");

				out.println("<table class=\"table\">");
				out.println("<tr>");
				out.println("<td width=15% class=text-right>이름</td>");
				out.println("<td width=8% class=text-right>");
				out.println("<input type=text size=15 class=input-sm name=name>");
				out.println("</td>");
				out.println("</tr>");
				
				out.println("<tr>");
				out.println("<td width=15% class=text-right>제목</td>");
				out.println("<td width=85% class=text-right>");
				out.println("<input type=text size=15 class=input-sm name=subject>");
				out.println("</td>");
				out.println("</tr>");
				
				out.println("<tr>");
				out.println("<td width=15% class=text-right>내용</td>");
				out.println("<td width=85%>");
				out.println("<textarea cols=50 rows=10 name=content></textarea>");
				out.println("</td>");
				out.println("</tr>");
				
				out.println("<tr>");
				out.println("<td width=15% class=text-right>비밀번호</td>");
				out.println("<td width=85% class=text-right>");
				out.println("<input type=password size=10 class=input-sm name=pwd>"); // name은 입력 받은 데이터들을 구분할 수 있게 별칭을 준 것임
				out.println("</td>");
				out.println("</tr>");
				
				// 글쓰기, 취소 버튼
				out.println("<tr>");
				out.println("<td colspan=2 class=text-center>");
				out.println("<input type=submit class=\"btn btn-sm dtn-danger\" value=글쓰기>");
				out.println("<input type=submit class=\"btn btn-sm dtn-info\" value=취소  oneclick=\"javascript:history.back()\">");
				out.println("</td>");
				out.println("</tr>");
				
				out.println("</table>");
				out.println("</form>");
				
				out.println("</div>");
				out.println("</div>");
				out.println("</body>");
				out.println("</html>");
	}
}
```

#### doPost()

```java
package com.sist.board;

import java.io.*;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.sist.dao.BoardDAO;
import com.sist.dao.BoardVO;


@WebServlet("/BoardInsert")
public class BoardInsert extends HttpServlet {
	private static final long serialVersionUID = 1L;

	// 데이터베이스 연결 => 요청처리
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		/* request에 사용자가 요청한 값이 담겨 있는데, 
		   여기서 값을 가져와야함
		   반대로 HTML 보낼떄는 response에 담아서 보내면됨~!
		*/
		
		// 한글은 디코딩해야함
		try {
			request.setCharacterEncoding("EUC-KR");
		} catch (Exception e) {
			// TODO: handle exception
		}

		// 사용자가 입력한 값을 String으로 받기		
		String name=request.getParameter("name");
		String subject=request.getParameter("subject");
		String content=request.getParameter("content");
		String pwd=request.getParameter("pwd");

		// 받아둔 값을 vo에 담기
		BoardVO vo= new BoardVO();
		vo.setName(name);
		vo.setSubject(subject);
		vo.setContent(content);
		vo.setPwd(pwd);
		
		/* DAO로 vo에 묶은 값들을 전달하고 
		  => INSERT하라고 SQL문장 전송해서
		  => 오라클 데이터에 쌓으면됨
		*/
		BoardDAO dao=new BoardDAO();
		dao.boardInsert(vo);
		
		// 데이터 쌓고나서 다시 목록 화면으로 이동하도록 처리
		response.sendRedirect("BoardList");

	}

}

```

### 2. 오라클 데이터와 새글 데이터 연결
- 경로 : [com.sist.dao] - [BoardDAO] - boardInsert(BoardVO vo)

#### boardInsert(BoardVO vo)

```java
package com.sist.dao;

import java.util.*;
import java.sql.*;

public class BoardDAO {
	// 3. 글쓰기    => INSERT
	public void boardInsert(BoardVO vo)
	{
		try {
			getConnection();
			String sql="INSERT INTO freeboard(no,name,subject,content,pwd) "
					+ "VALUES((SELECT NVL(MAX(no)+1,1) FROM freeboard),?,?,?,?)";
				         /* 새글쓰면 no는 자동으로 증가 (최대값에 +1시키기) 
				            null+1=null이 되므로 NVL로 null값을 1로 변환시키기
				         */
			ps=conn.prepareStatement(sql);
			
			ps.setString(1, vo.getName()); // 첫번째 물음표자리에 이름넣기
			ps.setString(2, vo.getSubject()); // 두번째 물음표 자리에 제목넣기
			ps.setString(3, vo.getContent()); // 세번째 물음표 자리에 내용넣기
			ps.setString(4, vo.getPwd());  // 네번째 물음표 자리에 비밀번호 넣기
			
			/* executequery와 차이점 : 
			   executeUpdate()은 autocommit!(insert,update,delete가 사용)
			/=*/
			ps.executeUpdate();
		} catch (Exception e) {
			
		} finally {
			disConnection();
		}
		
	}
}
```

# 게시글 상세보기
### 1. 오라클 데이터와 게시글 상세보기 연결
- 경로 : [com.sist.dao] - [BoardDAO] - boardDetail(int no)

```java
package com.sist.dao;
import java.util.*;
import java.sql.*;
public class BoardDAO {

	public BoardDAO()
	{

	}
	// 연결/닫기 반복 => 기능이 반복일 경우 => 메소드로 처리
	public void getConnection()
	{

	}
	public void disConnection()
	{

	}

	// 2. 내용보기  => SELECT (WHERE) ?no=1
	public BoardVO boardDetail(int no)
	{
		BoardVO vo = new BoardVO();
		try {
			getConnection();

			// 1. 조회수 증가
			String sql="UPDATE freeboard SET hit=hit+1 WHERE no=?";
			ps=conn.prepareStatement(sql);

			// 물음표에 값채우기
			ps.setInt(1, no);
			
			// 실행
			ps.executeUpdate();
			
			// 2. 내용볼 데이터를 가지고 온다
			sql="SELECT no,name,subject,content,regdate,hit FROM freeboard WHERE no=?";
			ps=conn.prepareStatement(sql);
			ps.setInt(1, no);
			
			ResultSet rs=ps.executeQuery();

			rs.next();
			vo.setNo(rs.getInt(1));
			vo.setName(rs.getString(2));
			vo.setSubject(rs.getString(3));
			vo.setContent(rs.getString(4));
			vo.setRegdate(rs.getDate(5));
			vo.setHit(rs.getInt(6));
			
			
		} catch (Exception e) {
			System.out.println(e.getMessage());
			
		} finally {
			disConnection();
		}
		
		return vo;
	}

}
```

### 2. 오라클 데이터와 게시글 상세보기 연결
- 경로 : [com.sist.board] - [BoardDetail] - doGet()

```java
package com.sist.board;

import java.io.*;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import com.sist.dao.*;


@WebServlet("/BoardDetail")
public class BoardDetail extends HttpServlet {
	private static final long serialVersionUID = 1L;


	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// 브라우저에서 실행하는 화면 => HTML
		// 브라우저에 알림 => HTML문서를 전송할 것이다 
		response.setContentType("text/html;charset=EUC-KR");
		// HTML을 브라우저로 전송 시작 
		PrintWriter out=response.getWriter();
		
		// 번호 받아서 boardDetail() vo에 넣어주기
		String no=request.getParameter("no");
		BoardDAO dao= new BoardDAO();
		BoardVO vo=dao.boardDetail(Integer.parseInt(no));
		
		// 화면출력		
		out.println("<html>");
		out.println("<head>");
		out.println("<link rel=\"stylesheet\" href=\"https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css\">");
		out.println("<style type=text/css>");
		out.println(".row {margin:0px auto;width:600px}");
		out.println("h2 {text-align:center}");
		out.println("</style>");
		out.println("</head>");
		out.println("<body>");
		out.println("<div class=container>");
		out.println("<h2>내용보기</h2>");
		out.println("<div class=row>");
		
		out.println("<table class=\"table\">");
		
		out.println("<tr>");
		out.println("<td class=\"success text-center\" width=25%>번호</td>");
		out.println("<td width=25% class=text-center>"+vo.getNo()+"</td>");
		out.println("<td class=\"success text-center\" width=25%>작성일</td>");
		out.println("<td width=25% class=text-center>"+vo.getRegdate()+"</td>");
		out.println("</tr>");
		
		out.println("<tr>");
		out.println("<td class=\"success text-center\" width=25%>이름</td>");
		out.println("<td width=25% class=text-center>"+vo.getName()+"</td>");
		out.println("<td class=\"success text-center\" width=25%>조회수</td>");
		out.println("<td width=25% class=text-center>"+vo.getHit()+"</td>");
		out.println("</tr>");
		
		out.println("<tr>");
		out.println("<td class=\"success text-center\" width=25%>제목</td>");
		out.println("<td colspan=3>"+vo.getSubject()+"</td>");
		out.println("</tr>");

		// 내용작성하기
		out.println("<tr>");
		// valign : top /bottom /middle 중에 어디서 시작할건지 결정
		out.println("<td colspan=4 heigth=200 valign=top></td>");
		out.println("</tr>");
		
		// 하단 버튼
		out.println("<tr>");
		out.println("<td colspan=4 class=text-right>");
		out.println("<a href=# class=\"btn btn-xs btn-success\">수정</a>");
		out.println("<a href=# class=\"btn btn-xs btn-warning\">삭제</a>");
		out.println("<a href=BoardList class=\"btn btn-xs btn-danger\">목록</a>"); // BoardList로 이동?
		out.println("</tr>");
		
		out.println("</table>");
		out.println("</div>");
		out.println("</div>");
		out.println("</body>");
		out.println("</html>");
	}

}

```

# 기타
- 브라우저랑 자바 연결하는 웹서버가 바로 톰캣임
- 웹서버, 오라클은 서버가 이미 만들어져 있기 때문에 둘을 연결만 시켜주면됨
- 그 연결을 자바 DAO(Database Access Object)가 함
- 웹서버와 컴퓨터 화면을 연결하는 것은 html임
  - ```out.println();```은 결과를 브라우저로 보내겠다는 뜻임
  - ```System.out.println();```은 내 시스템 도스창으로 결과를 보내겠다라는 뜻임

![](https://image2.slideserve.com/5105022/slide8-l.jpg)
