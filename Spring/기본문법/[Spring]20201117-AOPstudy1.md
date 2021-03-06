# AOP (1) : 마이바티스, Emp-list,detail , AOP 적용 전
- 마이바티스가 getConnection, disConnection을 연결하는 과정을 구현해 보았다.

## AOP란
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F994AA3335C1B8C9D28D24B)

- AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다. 
- 관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것이다. 
  - 여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말한다. 
- AOP에서 각 관점을 기준으로 로직을 모듈화한다는 것은 코드들을 부분적으로 나누어서 모듈화하겠다는 의미다. 
- 이때, 소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들을 발견할 수 있는 데 이것을 흩어진 관심사 (Crosscutting Concerns)라 부른다.

- [출처](https://engkimbs.tistory.com/746)


## 기초 셋팅

### 프로젝트 생성
- 패키지 생성


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
- beans
- context
- p


### 자바(Model, DAO , VO) 파일 제작
- com.sist.dao : EmpVO , EmpDAO , MyDataSource


### EmpDAO.java
- @Repository : 메모리 할당
- @Autowired : application-context.xml에서 설정한 ds값을 받아온다.
  - MyDataSource.java : application-context.xml 의 ds의 요소들을 받아올 값들을 정의한 클래스
- getConnection , disConnection 의 경우 AOP를 활용해서 반복 구간을 제거하는 프로그램을 만들 수 있다.

### JSP(View) 파일 제작


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
