# AOP 

## AOP란
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F994AA3335C1B8C9D28D24B)

- AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다. 
- 관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것이다. 
  - 여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말한다. 
- AOP에서 각 관점을 기준으로 로직을 모듈화한다는 것은 코드들을 부분적으로 나누어서 모듈화하겠다는 의미다. 
- 이때, 소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들을 발견할 수 있는 데 이것을 흩어진 관심사 (Crosscutting Concerns)라 부른다.

- [출처](https://engkimbs.tistory.com/746)
- 스프링이라는 프로그램은 항상 2개로 나뉜다.
  - 공통 모듈 : 공통 관심사 (스프링에서 담당하는 부분) 
    - AOP : 반복을 제거하는 프로그램 (트랜젝션,로그파일,보안)
    - CallBack 함수 : 시스템에 의해 자동 호출되는 함수
  - 핵심 모듈 : 핵심 관심사 (프로그래머가 필요한 부분)

## 기초 셋팅

### 프로젝트 생성
- 패키지 생성 : com.sist.aspect


### 자바 패킷 버전 변경
- 1.8버전으로 변경하기

### pom.xml
- 라이브러리 추가하기
  - 라이브러리가 없는 경우엔 
  
```xml
<!-- https://mvnrepository.com/artifact/javax.validation/validation-api
		유효성검사 -->
		<dependency>
			<groupId>javax.validation</groupId>
			<artifactId>validation-api</artifactId>
			<version>2.0.1.Final</version>
		</dependency>
<!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-validator 
		메세지와 객체 검색-->
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-validator</artifactId>
			<version>6.1.6.Final</version>
		</dependency>
```

### web.xml
- 한글 인코딩 추가

### application-context.xml 설정
- AOP

  - setter가 필요하거나 값이 있을 때는 bean을 활용해야 한다.
  - 메모리할당은 가능하지만 어노테이션에서 값을 주입할 수 없다.
  - 값을 주입할 때는 xml 혹은 자바에서 값 주입이 필요하다.
  - c를 사용하면 생성되자마자 값을 주입할 떄 사용한다 
  
```tip
c는 생성자에 값 주입 vs p는 setter에 값을 주입할 때 사용한다.)
```

```tip
- 메모리 할당만 하느냐?
  - 어노테이션 사용
- 메모리 할당해서 값을 주입하느냐?
  - xml을 사용해서 코딩
```


- beans
- context


### 자바(Model, DAO , VO) 파일 제작
- com.sist.dao : EmpVO , EmpDAO , MyDataSource


### EmpDAO.java
- @Repository : 메모리 할당
- @Autowired : application-context.xml에서 설정한 ds값 채워주기
  - MyDataSource.java : application-context.xml 의 ds의 요소들을 받아올 값들을 정의한 클래스
- getConnection , disConnection 의 경우 AOP를 활용해서 반복 구간을 제거하는 프로그램을 만들 수 있다.

### JSP(View) 폴더 제작




## AOP

### DBAspect.java
- `@Aspect` : 공통모듈임을 스프링에게 알린다.
  - DAO에서 공통으로 사용하는 내역을 모아준다.
  - 메모리 할당은 하지 않는다.
- `@Component` : 메모리 할당한다.

- AOP가 어떻게 활용될까?
  - 반복 코딩되는 부분은 자동호출되도록 만들어서 핵심 코딩만 작성하도록 할 때 사용된다.
  - 마이바티스도 AOP가 적용된 프로그램이기 때문에 핵심문장(쿼리문장)만 작성하면 된다. 

- AOP 관련 어노테이션
  - @Before : 쿼리문장 실행 전에 작동되는 코딩
  - @AfterThrowing : 쿼리문장 실행 후 예외 처리 부분을 자동 호출
  - @After : 쿼리문장 실행 전에 작동되는 코딩
  - @Around : 메소드 호출 전/후에 실행할 수 있음
  	- 핵심 코딩 전에 많이 사용하는 메소드 : setAutoCommit(false)
	- 핵심 코딩 후에 많이 사용하는 메소드 : Commit()
  - @AfterReturning


```JAVA
public List<EmpVO> empListData()
{
	try {
		getConnection();_ // @Before("") : 쿼리문장 실행 전 이 부분을 자동호출
		/*
		 * 핵심코딩(SQL => 전송 => 결과값 => List,VO...)
		 */
		
	} catch (Exception e) {
		e.printStackTrace(); // @After-Throwing : 쿼리문장 실행 후 예외 부분을 자동 호출
	} finally {
		disConnection(); // @After("") : 쿼리문장 실행 후 이 부분을 자동호출
	}
}
```

- 자동 호출을 사용하기 위해서는
  - PointCut : 언제 호출할지 호출 시기를 지정해야 한다.
  - JoinPoint : 어디서 호출할지 호출 지점을 지정해야 한다.
  - Advice : PointCut + JoinPoint
  - Aspect : Advice + Advice + ...

- `@Before("execution(* com.sist.dao.EmpDAO.emp*(..))")`
  - `* ` : 리턴형으로 어떤 것이 와도 상관 없다는 뜻이다.
  - `com.sist.dao.EmpDAO.emp*` : 해당 경로에서 emp로 시작하는 이름의 메서드에서만 실행하라는 뜻이다.
    - 만약 메소드간의 이름 규칙이 없다면, `||` (or문자)로 연결해서 여러 개 작성해주면 된다.
  - `(..)` : 매개변수로 몇 개가 오던, 어떤 데이터형이 오던 관계없이 적용된다는 뜻이다.


### AOP 동작원리 : Proxy 패턴
- 새로운 클래스를 만들어서 실행되는 것이다.
- invoke로 호출된다.

```java
public class A
{
	public void display()
	{
		System.out.println("A Call...");
	}
}
public class Proxy
{
	A a;
	public Proxy(A a)
	{
		this.a=a;
	}
	public void Display()
	{
		System.out.println("A Call...");
		a.display();
		System.out.println("A Call...");		
	}
}

A a= new A();
Proxy p = new Proxy(a);
p.display();
```


  
## 전체 목록

### EmpDAO.java
- empListData


```java
public List<EmpVO> empListData()
	{
		List<EmpVO> list = new ArrayList<EmpVO>();
		// @Before
		try {
			/*
        핵심코딩
      */
		} catch (Exception e) {
			// @After-Throwing
		} finally {
			// @After
		} return list; // @After-Returning
	}
```

- 스프링이라는 프로그램은 항상 2개로 나뉜다.
  - 공통 모듈 : 공통 관심사 (스프링에서 담당하는 부분) 
    - AOP : 반복을 제거하는 프로그램 (트랜젝션,로그파일,보안)
    - CallBack 함수 : 시스템에 의해 자동 호출되는 함수
  - 핵심 모듈 : 핵심 관심사 (프로그래머가 필요한 부분)


### EmpController.java

```java
@GetMapping("emp/list.do")
	public String emp_list(Model model)
	{
		List<EmpVO> list= dao.empListData();
		model.addAttribute("list",list);
		return "emp/list";
	}
```
  
### list.jsp

## 상세 보기

### EmpDAO.java
- empDetailData

### EmpController.java

```tip
**Spring MVC 컨트롤러 메서드**
- @GetMapping
- @PostMapping : POST (form , ajax)
- @RequestMapping : @GetMapping + @PostMapping
  - get과 post 방식 전체를 찾아야 하기 때문에 속도가 더 느리다.
```

- get방식으로 요청값이 넘어가기 때문에 GetMapping으로 값을 받는다.
  - list.jsp : `<a href="detail.do?empno=${vo.empno}">`
  

```java
@GetMapping("emp/detail.do")
	public String emp_detail(int empno,Model model)
	{
		EmpVO vo = dao.empDetailData(empno);
		model.addAttribute("vo",vo);
		return "emp/detail";
	}
```


### detail.jsp


## 부서 생성

### EmpDAO.java : deptInsert
- deptInsert는 emp로 시작하는 이름이 아니기 때문에 AOP 적용을 받지 못한다.
	- getConnection,disConnection이 자동호출되지 못한다.

- INSERT를 두 번 실행 시, deptno가 중복값으로 두 번 들어가기 때문에 오류 발생할 것이다.
	- 첫 번째 INSERT는 수행되고, 두 번째 INSERT는 실행되지 않을 것이다.
	- 그래서 둘 다 실행 안되거나 되거나, 두 INSERT가 한 번에 이뤄지도록 하는 것이 트랜잭션 프로그램이다.
	
- 트랜잭션 프로그램으로 짜는 방법
	- COMMIT을 두 개가 모두 시행된 다음에 배치한 뒤, catch절에서 ROLLBACK을 시킨다.
	- `executeUpdate`에 COMMIT이 포함되어 있다.
	- EmpController - emp_detail 메서드에서 `dao.deptInsert(90, "자재부", "인천");` 코딩을 작성해 값이 제대로 들어가는지 확인해 볼 수 있다.
	
- 
