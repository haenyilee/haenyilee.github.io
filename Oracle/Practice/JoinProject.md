---
sort: 3
---

# DeptVO
## dept테이블 컬럼에 맞게 객체 생성 & 캡슐화
```java
package com.sist.join;

public class DeptVO {
		private int deptno;
		private String dname;
		private String loc;
		
		public int getDeptno() {
			return deptno;
		}
		public void setDeptno(int deptno) {
			this.deptno = deptno;
		}
		public String getDname() {
			return dname;
		}
		public void setDname(String dname) {
			this.dname = dname;
		}
		public String getLoc() {
			return loc;
		}
		public void setLoc(String loc) {
			this.loc = loc;
		}		
}

```


# EmpVO
## 1. emp 테이블 컬럼에 맞게 객체생성 & 캡슐화
```java
package com.sist.join;

import java.util.Date;

public class EmpVO {
	// 컬럼명대로 변수만들기(객체생성?)
	private int empno;
	private String ename;
	private String job;
	private int mgr; // 사번
	private Date hiredate;
	private double sal;
	private double comm;
	private int deptno;
	
	// 캡슐화
	public int getEmpno() {
		return empno;
	}
	public void setEmpno(int empno) {
		this.empno = empno;
	}
	public String getEname() {
		return ename;
	}
	public void setEname(String ename) {
		this.ename = ename;
	}
	public String getJob() {
		return job;
	}
	public void setJob(String job) {
		this.job = job;
	}
	public int getMgr() {
		return mgr;
	}
	public void setMgr(int mgr) {
		this.mgr = mgr;
	}
	public Date getHiredate() {
		return hiredate;
	}
	public void setHiredate(Date hiredate) {
		this.hiredate = hiredate;
	}
	public double getSal() {
		return sal;
	}
	public void setSal(double sal) {
		this.sal = sal;
	}
	public double getComm() {
		return comm;
	}
	public void setComm(double comm) {
		this.comm = comm;
	}
	public int getDeptno() {
		return deptno;
	}
	public void setDeptno(int deptno) {
		this.deptno = deptno;
	}
	
}

```


## 2. DeptVO 클래스 가져오기
```java
package com.sist.join;

import java.util.Date;

public class EmpVO {
	
	// Emp테이블 컬럼들의 객체 생성 & 캡슐화

	// deptVO가져오기 = 포함클래스 JOIN
	private DeptVO dvo= new DeptVO();
	
	public DeptVO getDvo() {
		return dvo;
	}
	public void setDvo(DeptVO dvo) {
		this.dvo = dvo;
	}
}

```

- ArrayList에 DeptVO의 정보, EmpVO의 정보를 따로따로 가져와서 담을 수 없어서, 하나의 VO에 모든 정보를 담아줘야함
  - ArrayList의 형식 : ```public ArrayList<EmpVO>```이므로,  
EmpVO와 DeptVO에서 각자 값을 가져와 하나의 ArrayList에 담을 수 없음
- class를 첨부해서 가져와야함
- [new연산자와 메모리](https://steps-for-developer.tistory.com/entry/%EB%A9%94%EC%86%8C%EB%93%9C%EC%99%80-%EB%A9%94%EB%AA%A8%EB%A6%AC%EA%B5%AC%EC%A1%B0)
- 힙메모리 : new 연산자로 만든 객체나 배열이 저장되는 메모리
  - new할때마다 heap 메모리에 데이터크기만큼 새롭게 할당받고  
해당 메모리의 주소를 받고 메모리주소가 해당변수에 저장된다.

![](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99C5CB4D5BF2CCDC2F)

# EmpDAO
## 1. 오라클 연결
```java
package com.sist.join;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;

public class EmpDAO {


	// 연결 객체
	private Connection conn;

	// 쿼리 객체
	private PreparedStatement ps;
	
	// 오라클 주소 객체
	private final String URL="jdbc:oracle:thin:@localhost:1521:XE";
	
	// 드라이버 설치 메소드
	public EmpDAO()
	{
		// 생성자의 역할 (1) 멤버변수의 초기화 (2) 네트워크 서버 연결 -> 시작하자마자 서버 연결위함
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
	

	// 오라클 연결 메소드	
	public void getConnection() {
		try {
			// 연결 명령어 : conn hr/happy
			conn=DriverManager.getConnection(URL,"hr","happy");
		} catch (Exception e) {}
	}
	
	// 오라클 연결 종료 메소드
	public void disConnection() {
		try {
			if(ps!=null) ps.close();
			if(conn!=null) conn.close();
		} catch (Exception e) {}
	}
	
	
}

```
## 2. empDeptJoinData()클래스 생성
- EmpVO와 DeptVO의 객체를 담기위해 ArrayList 생성해야함

```java
package com.sist.join;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.util.ArrayList;

public class EmpDAO {

	// 연결 객체
	// 쿼리 객체
	// 오라클 주소 객체
	// 드라이버 설치 메소드

	// 오라클 연결 메소드	
	// 오라클 연결 종료 메소드
	
	// EmpVO와 DeptVO의 객체를 담을 ArrayList 생성
	public ArrayList<EmpVO> empDeptJoinData()
	{
		ArrayList<EmpVO> list=
				new ArrayList<EmpVO>();
		
		return list;
	}
```
## 3. 쿼리 작성 & 전송
```java
package com.sist.join;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.util.ArrayList;

public class EmpDAO {

	// 연결 객체
	// 쿼리 객체
	// 오라클 주소 객체
	// 드라이버 설치 메소드

	// 오라클 연결 메소드	
	// 오라클 연결 종료 메소드
	
	// EmpVO와 DeptVO의 객체를 담을 ArrayList 생성
	public ArrayList<EmpVO> empDeptJoinData()
	{
		ArrayList<EmpVO> list=
				new ArrayList<EmpVO>();
		try {
			getConnection();
			
			// 쿼리 작성
			String sql="SELECT empno,ename,job,hiredate,sal,dname,loc "
					+ "FROM emp,dept "
					+ "WHERE emp.deptno=dept.deptno"; 
					// 자바에서는 쿼리 맨 뒤에 ; 작성하면 오류남
			
			// 쿼리 전송
			ps=conn.prepareStatement(sql);
			
			// 쿼리 전송 후 받은 결과데이터 rs 메모리에 받아두기
			ResultSet rs = ps.executeQuery();
			
			// EmpVO는 한개에 대한 값만 저장하고 있으므로, 행의 갯수만큼 반복해서 각 행에 데이터 넣어주기
			while(rs.next())
			{
				EmpVO vo=new EmpVO();
				vo.setEmpno(rs.getInt(1));
				vo.setEname(rs.getString(2));
				vo.setJob(rs.getString(3));
				vo.setHiredate(rs.getDate(4));
				
				// DeptVO의 객체들에 rs값 담아주기
				vo.setSal(rs.getInt(5));
				vo.getDvo().setDname(rs.getString(5));
				vo.getDvo().setLoc(rs.getString(rs.getString(6)));
				
				// 리스트 vo에 값 추가
				list.add(vo);				
			}
			
			
		} catch (Exception e) {
			
		} finally {
			disConnection();
		}
		return list;
		return list;
	}
```


# EmpServlet
- Servlet으로 들어온 요청을 파악해서 클라이언트에게 응답 보내기
```java
package com.sist.join;

// import 부분 생략

@WebServlet("/EmpServlet")
public class EmpServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	// doGet() : get방식으로 호출시 호출되는 메서드
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
	throws ServletException, IOException {
		
		// 응답할 문서 타입과 인코딩 방식을 지정한다.
		response.setContentType("text/html;charset=EUC-KR");

		// 응답 문서에 출력하기 위한 PrintWriter객체를 가지고 온다.
		PrintWriter out=response.getWriter();
		
		EmpDAO dao=new EmpDAO();
		
		ArrayList<EmpVO> list = dao.empDeptJoinData();
		out.println("<html>");
		out.println("<body>");
		out.println("<center>");
		out.println("<h1>사원정보</h1>");
		out.println("<table border=1 width=700>");
		
		out.println("<tr>");
		out.println("<th>사번</th>");
		out.println("<th>이름</th>");
		out.println("<th>직위</th>");
		out.println("<th>입사일</th>");
		out.println("<th>급여</th>");
		out.println("<th>부서번호</th>");	
		
		out.println("<th>부서명</th>");
		out.println("<th>근무지</th>");
		out.println("</tr>");
		
		
		// 14개의 row에 대한 데이터 출력하기
		for(EmpVO vo:list)
		{
			// EmpVO의 값
			out.println("<tr>");
			out.println("<th>"+vo.getEmpno()+"</th>");
			out.println("<th>"+vo.getEname()+"</th>");
			out.println("<th>"+vo.getJob()+"</th>");
			out.println("<th>"+vo.getHiredate()+"</th>");
			out.println("<th>"+vo.getSal()+"</th>");
			out.println("<th>"+vo.getEmpno()+"</th>");
			
			// DeptVO의 값
			out.println("<th>"+vo.getDvo().getDname()+"</th>");
			out.println("<th>"+vo.getDvo().getLoc()+"</th>");
			out.println("</tr>");
		}
			
		out.println("</center>");
		out.println("</body>");
		out.println("</html>");
	}
}

```

## 1. 응답할 문서 타입과 인코딩 방식을 지정하기
```java
package com.sist.join;

// import 부분 생략

@WebServlet("/EmpServlet")
public class EmpServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	// doGet() : get방식으로 호출시 호출되는 메서드
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
	throws ServletException, IOException {
		
		// 응답할 문서 타입과 인코딩 방식을 지정한다.
		response.setContentType("text/html;charset=EUC-KR");
	}
}

```

## 2. 응답 문서에 출력하기 위한 PrintWriter객체를 가지고 오기
  - 응답 스트림에 텍스트를 기록하기 위해 response.getWriter() 호출하기
  - Servlet으로 들어온 요청은 대체로 텍스트(HTML)로 응답을 보내기 때문에  
```PrintWriter out=response.getWriter();```형식으로 응답으로 내보낼 출력 스트림을 얻어낸 후  
 ==> ```out.println("<script>");``` 형태로 스트림에 텍스트를 기록하게됨

```java
package com.sist.join;

// import 부분 생략

@WebServlet("/EmpServlet")
public class EmpServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	// doGet() : get방식으로 호출시 호출되는 메서드
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
	throws ServletException, IOException {
		
		// 응답할 문서 타입과 인코딩 방식을 지정한다.

		// 응답 문서에 출력하기 위한 PrintWriter객체를 가지고 온다.
		PrintWriter out=response.getWriter();
	}
}

```

## 3. 응답으로 내보낼 html스크립트 작성하기


#### 3.1 EmpDAO에서 생성했던 empDeptJoinData()를 새로운 list에 담기위해 메모리 공간 만들기
```java
package com.sist.join;

// import 부분 생략

@WebServlet("/EmpServlet")
public class EmpServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	// doGet() : get방식으로 호출시 호출되는 메서드
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
	throws ServletException, IOException {
		
		// 응답할 문서 타입과 인코딩 방식을 지정한다.

		// 응답 문서에 출력하기 위한 PrintWriter객체를 가지고 온다.

		// new연산자로 새로운 EmpDAO 할당받고 메모리주소 저장하기
		EmpDAO dao=new EmpDAO();

		// dao.empDeptJoinData()의 값들을 arraylist로 묶어주기	
		ArrayList<EmpVO> list = dao.empDeptJoinData();

	}
}

```

#### 3.2 list에 값 넣어주고 HTML스크립트 완성하기
```java
package com.sist.join;

// import 부분 생략

@WebServlet("/EmpServlet")
public class EmpServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	// doGet() : get방식으로 호출시 호출되는 메서드
	protected void doGet(HttpServletRequest request, HttpServletResponse response) 
	throws ServletException, IOException {
		
		// 응답할 문서 타입과 인코딩 방식을 지정한다.

		// 응답 문서에 출력하기 위한 PrintWriter객체를 가지고 온다.
		
		// EmpDAO에서 생성했던 empDeptJoinData()를 새로운 list에 담기위해 메모리 공간 만들기

		// list에 값 넣어주고 html 스크립트 완성하기
		out.println("<html>");
		out.println("<body>");
		out.println("<center>");
		out.println("<h1>사원정보</h1>");
		out.println("<table border=1 width=700>");
		
		out.println("<tr>");
		out.println("<th>사번</th>");
		out.println("<th>이름</th>");
		out.println("<th>직위</th>");
		out.println("<th>입사일</th>");
		out.println("<th>급여</th>");
		out.println("<th>부서번호</th>");	
		
		out.println("<th>부서명</th>");
		out.println("<th>근무지</th>");
		out.println("</tr>");
		
		
		// 14개의 row에 대한 데이터 출력하기
		for(EmpVO vo:list)
		{
			// EmpVO의 값
			out.println("<tr>");
			out.println("<th>"+vo.getEmpno()+"</th>");
			out.println("<th>"+vo.getEname()+"</th>");
			out.println("<th>"+vo.getJob()+"</th>");
			out.println("<th>"+vo.getHiredate()+"</th>");
			out.println("<th>"+vo.getSal()+"</th>");
			out.println("<th>"+vo.getEmpno()+"</th>");
			
			// DeptVO의 값
			out.println("<th>"+vo.getDvo().getDname()+"</th>");
			out.println("<th>"+vo.getDvo().getLoc()+"</th>");
			out.println("</tr>");
		}
			
		out.println("</center>");
		out.println("</body>");
		out.println("</html>");
	}
}

```




# 서블릿 추가자료
[서블릿 이클립스에서 작성하는 방법](https://private.tistory.com/19)

#### 서블릿이란?
  - 서블릿은 자바 플랫폼에서 동적인 웹을 개발할 때 사용하는 기반 기술로 웹에서 JAVA 프로그래밍을 할 수 있습니다.
  - 사용자에게 요청(Request)을 받아 요청한대로 처리해주는 (doGet() 또는 doPost()) 일을 처리한 후  
처리 결과를 사용자에게 응답(Response) 해줍니다.



#### 서블릿의 동작 과정

 1. 사용자의 URL 요청
```
- 어떤 사용자의 URL 요청이 서블릿 요청이라는 것을 웹 서버가 알기 위해서는  
사전에 웹 서버 측에 URL과 서블릿 클래스를 미리 매핑시켜 놓은 배포서술자가 필요합니다.

- 배포서술자(DD - Deployment Descriptor) : 웹서버가 알 수 있도록 적어놓은 파일(web.xml)
```
 2. request, response 객체 설정
```
- 웹 컨테이너는 지금 받은 요청을 처리하기 위해  
HTTP 요청을 처리하기 위한 Request 객체와  
HTTP응답을 위한 Response 객체를 생성합니다.
```


 3. 서블릿 인스턴스와 스레드 생성
```
- request, response 객체가 생성된 뒤  
사용자의 URL 요청이 어떤 서블릿 클래스를 필요로 하는지 배포서술자를 통해 알아냅니다. 

- 만일 그 클래스가 웹 컨테이너에서 한 번도 실행된 적이 없거나  
현재 메모리에 생성된 인스턴스(프로세스)가 없다면  
새로 인스턴스를 생성하고(메모리에 로드) init() 메서드를 실행하여 초기화 한 뒤 스레드를 하나 생성합니다.

- 이미 인스턴스가 존재할 경우에는  
새로 인스턴스를 생성하지 않고 기존의 인스턴스에 새로운 스레드만 하나 생성합니다.  
각 서블릿 인스턴스는 웹 컨테이너당 하나만 존재하기 때문에 init() 메서드는 각 서블릿 당 한번씩만 호출이 됩니다.
```


  4. service() 메서드 호출과 서블릿 클래스 실행
```
- 스레드가 생성되면 각 스레드에서 service() 메서드가 호출됩니다.

- service() 메서드가 호출되면  
HTTP요청 방식이 GET방식일 경우 서블릿 클래스의 doGet() 메서드가,  
POST 방식일 경우 doPost() 메서드가  
request, response 객체를 인자로 자동으로 호출됩니다.
```


  5. 응답과 스레드의 소멸
```
- doGet() 또는 doPost() 메서드가 호출되어 사용자의 요청에 따른 동적인 웹페이지를 생성하면  
그 결과물이 담긴 response객체를 웹 컨테이너가 HTTP응답(Response) 형태로 바꾸어 웹 서버로 전송하게 됩니다.  
그리고 사용이 끝난 Request와 Response 객체를 소멸시키고 스레드를 종료합니다.
```




#### 실행순서 정리

- 클라이언트 
1. URL 요청

- 웹서버
2. 요청된 서블릿 확인 후 컨테이너로 요청

- 컨테이너 
3. 컨테이너에서 request와 response 생성 후 web.xml(배포서술자)을 참조하여 해당 서블릿의 스레드 생성 후 service 메서드 호출

4. service() 메서드에서는 요청 방식에 따라 doGet() 또는 doPost() 메서드 호출

5. doGet() 또는 doPost() 메서드에서 응답 생성

