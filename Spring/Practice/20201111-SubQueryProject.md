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
