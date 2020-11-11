---
sort: 1
---

# 스프링으로 서브쿼리 적용해보기

## 사원정보 출력 Project

### EmpController.java

- Service 사용자 요청을 받아서 데이터베이스 연동하고 JSP로 결과값 전송하기

```tip
**MVC**
- Model (java) : Service
  - Spring에서는 Controller라는 이름이다.
  - 요청을 받아서 기능을 제어하는 프로그램이다.
- View (JSP) : 화면출력
- Controller : Model+jsp
  - Spring에서는 DispatcherServlet : Front Controller이다.
  - 요청을 받아서 Model+JSP를 연결해주는 역할이다.
```


- `@Controller` 어노테이션 : Model

### EmpVO.java

```java
package com.haeni.dao;
import java.util.*;

public class EmpVO {
	private int empno;
	private String ename;
	private String job;
	private Date hiredate;
	private int sal;
	private int deptno;
	private String dname;
	private String loc;
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
	public Date getHiredate() {
		return hiredate;
	}
	public void setHiredate(Date hiredate) {
		this.hiredate = hiredate;
	}
	public int getSal() {
		return sal;
	}
	public void setSal(int sal) {
		this.sal = sal;
	}
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

### interface EmpMapper

- empListData : JOIN을 대체하는 서브쿼리

```java
@Select("SELECT empno,ename,job,hiredate,sal,deptno,"
			+ "(SELECT dname FROM dept d WHERE e.deptno=d.deptno) as dname,"
			+ "(SELECT loc FROM dept d WHERE e.deptno=d.deptno) as loc"
			+ "FROM emp e")
	public List<EmpVO> empListData(); // 마이바티스에서 getConnection, disConnection 자동 구현 해줌
```

- empGroupData : 클릭된 이름의 같은 부서 사람들 정보 가져오기

```java
@Select("SELECT empno,ename,job,hiredate,sal,deptno "
			+ "FROM emp "
			+ "WHERE deptno=(SELECT deptno FROM dept WHERE ename=#{ename})")
	public List<EmpVO> empGroupData(String ename);
```

```note
**SubQuery**
- SQL문장을 하나 묶어서 사용한다. 
- JOIN , 복잡한 쿼리 문장이 있는 경우 , SQL 문장이 많이 수행되는 경우 사용한다.
- 여러 쿼리문장을 작성하는 것보다 하나의 서브쿼리 문장을 전송하는 것이 속도가 더 빠르다.
- 종류 : 
  1. 단일행 서브쿼리 : 조건문에서 WHERE 뒤에 많이 사용한다.
  2. 다중행 서브쿼리 : 조건문에서 WHERE 뒤에 많이 사용한다.
  3. 스칼라 서브쿼리 : 컬럼을 대체하는 프로그램
  4. 인라인 뷰(TOP-N) :  테이블을 대체하는 프로그램이다.
- 실행 과정
  1. 서브쿼리가 먼저 실행되고 결과값을 메인쿼리에 전송한다. (`( )` 우선 처리)
  2. 
- ALL / ANY = MIN /MAX
  - ALL : 전체 중에 가장 큰것/작은 것보다 큰것/작은것
  - ANY : 전체 중에 가장 큰것/작은 것
```


### EmpDAO

- `@Autowired` : 스프링에서 구현된 객체 받기

```java
package com.haeni.dao;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;

public class EmpDAO {
	@Autowired
	private EmpMapper mapper;
	public List<EmpVO> empListData()
	{
		return mapper.empListData();
	};
	public List<EmpVO> empGroupData(String ename)
	{
		return mapper.empGroupData(ename);
	};
}
```

### EmpController.java

- `@Autowired` : 스프링에서 생성된 객체 EmpDAO 가져오기
	- `@Autowired`가 있으면 config xml 파일에서 등록된 id를 찾아서 메모리 할당된 것을 찾아준다.
	- 어디서 호출하더라고 똑같은 주소를 찾아준다. (한 사람 당, DAO는 한 번만 생성됨 => 싱글턴)

- `emp/list.do`를 호출하면 `/emp/list.jsp` 반환하기

```java
@RequestMapping("emp/list.do")
	public String emp_list(Model model)
	{
		List<EmpVO> list=dao.empListData();
		model.addAttribute("list",list); // request.setAttribute("list",list)
		return "list";
	}
```

## config

### application-context.xml

- aop , beans, context , p , tx , util

- 어노테이션이 있는 클래스를 메모리 할당하도록 명령내리기
	- class찾기 => @Controller, @Repository ,@Service , @RestController , @Component 어노테이션을 기준으로...

```xml
<context:component-scan base-package="com.sist.*"/>
```

- 데이터베이스 정보를 마이바티스로 전송하기
	- 1. 정보를 모아서 처리하기 : BasicDataSource
	- 2. getConnection/disConnection
	- 3. mapper 인터페이스 구현

```xml
<bean id="ds" class="org.apache.commons.dbcp.BasicDataSource"
	p:driverClassName="oracle.jdbc.driver.OracleDriver"
	p:url="jdbc:oracle:thin:@localhost:XE"
	p:username="hr"
	p:password="happy"
/>
<bean id="ssf"
	class="org.mybatis.spring.SqlSessionFactoryBean"
	p:dataSource-ref="ds"
/>
<bean id="mapper"
	class="org.mybatis.spring.mapper.MapperFactoryBean"
	p:mapperInterface="com.haeni.dao.EmpMapper"
	p:sqlSessionFactory-ref="ssf"
/>
```


```tip
**데이터 구조**
- 데이터베이스(≒폴더) : XE , ora , orcl , pubs...
- 테이블(≒파일)
```


- ViewResolver : JSP를 찾아서 request를 전송하기
	- DispatcherServlet과 WebApplicationContext(Container)
	![](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile29.uf.tistory.com%2Fimage%2F99B16B375CC05ADD0D1A58)
	- WebApplicationContext
		- HandlerMapping : 클래스 찾기
		- ViewResolver : JSP찾기
		
		```
		**ViewResolver**
		(prefix)main/main(suffix)
		/mian/main.jsp
		```


```xml
<bean id="viewResolver"
class="org.springframework.web.servlet.view.InternalResourceViewResolver"
p:prefix="/emp/"
p:suffix=".jsp"
/>
```




```tip
**무료로 설치 가능한 버전**
- 오라클 : XE
- JAVA : openjdk 15
- STS : 3.9.14 
	- 스프링 5.0 (STS 4:Spring Boot)
```


## view

### list.jsp

- 리액트
	- ES 5.0 => text/javascript
	- ES 6.0 => text/babel => react (ES6) => jsx(javascript+XML)
		- jsx : 1. 대소문자 구분 , 2. 여는태그/닫는태그 동일 , 3. 태그를 사용할 경우 소문자로 시작

- 리액트는 문법 사항이 까다롭지만, ajax가 포함되어 있다는 장점이 있다. 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<style type="text/css">
.row {
   margin: 0px auto;
   width:900px;
}
</style>
<script src="https://unpkg.com/react@15/dist/react.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.34/browser.js"></script>
</head>
<body>
	<div class="container">
		<div class="row" id="root">
		
		</div>
	</div>
	<!-- 리액트 스크립트  -->
	<script type="text/babel">
		class Emp extends React.Component{
			render()
			{
				return (
					<table className="table table-striped">
						<thead>
							<tr className="sucess">
								<th>사번</th>
								<th>이름</th>
								<th>직위</th>
								<th>입사일</th>
								<th>부서명</th>
								<th>근무지</th>
							</tr>						
						</thead>
						<tbody>
							<c:forEach var="vo" items="${list}">
								<tr>
								<td>${vo.empno}</td>
								<td>${vo.ename}</td>
								<td>${vo.job}</td>
								<td><fmt:formatDate value="${vo.hiredate}" pattern="yyyy-MM-dd"/></td>
								<td>${vo.dname}</td>
								<td>${vo.loc}</td>
								</tr>
							</c:forEach>
						</tbody>
					</table>
				)
			}
		}
		ReactDOM.render(<Emp/>,document.getElementById('root'))
	</script>
</body>
</html>
```
