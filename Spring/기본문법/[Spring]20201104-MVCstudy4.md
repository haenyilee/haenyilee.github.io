# 20201104-MVCstudy (4) : 자바로 SELECT 구현하기

## 기본 셋팅
1. New Spring Legacy Project 생성
  - Spring MVC Project Templates 선택
  - 패키지 등록
2. Project Facets java 1.8버전으로 변경
  - 스프링 5.0버전을 사용하기 위함이다.
3. pom.xml properties 변경
  - [https://jeong-pro.tistory.com/168](참고)
4. 불필요한 폴더 및 파일 삭제
5. web.xml
  - 컨트롤러 
  - 인코딩
  - 환경 설정
6. WEB-INF에 config 폴더 생성

```note
**스프링에서 필요한 설정 파일(config)**
1. 사용자 정의 클래스 등록
2. 반복적인 코딩이 있는 경우에 반복 코딩 제거하기 위한 콩통 모듈을 등록(AOP)

3. 데이터 베이스 관련
	- DataSource : 데이터 베이스 관련 정보를 모은다.
		- driver , username, password , url
	- Transaction처리
	- MyBatis의 SqlSessionFactory를 설정
		- getConnection , disConnection을 가능하게 하기 위함임
	- MapperFactoryBean : 인터페이스(구현하는 메소드) 등록

4. MongoDB 사용 시, 등록
5. 보안 설정
6. Websocket 등록
7. ViewResolver , Tiles등록
```

7. src/main/java에 dao 패키지 생성
  - dao패키지에는 데이터 관련 클래스가 들어감
  - web패키지에는 컨트롤러(모델)이 들어감

8. VO제작
9. config.xml과 mapper는 interface를 통해 만들 예정임


## 레시피 목록 출력하기


### RecipeMapper.java
- mapper 인터페이스 생성
- 레시피 데이터 조회하는 쿼리문장 작성하고 구현하는 메소드 지정하기
  - `@Select` 어노테이션을 사용한다.
  - resultType과 parameterType은 메소드에 따라 자동으로 구현된다.  

```java
@Select("SELECT no,title,chef,poster,num "
			+ "FROM (SELECT no,title,chef,poster,rownum as num "
			+ "FROM (SELECT no,title,chef,poster "
			+ "FROM recipe)) "
			+ "WHERE num BETWEEN #{start} AND #{end}")
	// 구현하는 메소드 지정
	public List<RecipeVO> recipeListData(Map map);
```
- 레시피 토탈 갯수 구하는 쿼리 문장 작성하고 구현하는 메소드 지정하기


### RecipeDAO.java


```warning
**면접 기출 : @Repository vs @Service**
- `@Repository` : DAO
  - 저장소(데이터베이스 관련)
  - DAO등록할 때 주로 사용한다.
  
- `@Service` : DAO+DAO+...
  - DAO 여러개를 조립해서 사용할 때 사용한다.
```

- `@Repository` : 어노테이션으로 메모리 할당 요청하기

```note
**스프링에서 어노테이션으로 메모리 할당 요청하기**
- `@Controller` : Model클래스
  - return시 반드시 jsp파일명, `.do`를 반환해야 함
  - 화면을 변경하고 이동할 때 사용함
  
- `@RestController` : Model클래스
  - 화면 변경이 아닌 경우 사용한다.
  - 스크립트, XML , JSON을 반환함
  - 리액트를 연결할 때 주로 사용한다.
  
- `@Repository` : DAO
  - 저장소(데이터베이스 관련)
  - DAO등록할 때 주로 사용한다.
  
- `@Service` : DAO+DAO+...
  - DAO 여러개를 조립해서 사용할 때 사용한다.
  
- `@Component` : 외부 데이터
  - JSOUP , 뉴스 , OpenAPI를 받아올 때 주로 사용한다.
```



- `@Autowired` : mapper는 할당된 메모리 주소 받기 

```note
**스프링에서 어노테이션으로 할당된 메모리 주소 받기**
- `@Autowired` : 스프링 내부에서 찾아 자동으로 주입한다.
- `@Resource` : 프로그래머가 요청
  - 반드시 id명을 사용한다.
```

```tip
- 스프링에서 어노테이션은 xml의 역할을 대체한다.
- 이를 통해 순수 자바를 통한 코딩 구현을 가능하게 한다.
```

```java
@Repository
public class RecipeDAO {
	
	@Autowired
	private RecipeMapper mapper;
	
	public List<RecipeVO> recipeListData(Map map)
	{
		return mapper.recipeListData(map);
	}
}
```

### application-context.xml

- config 폴더에 new java bean configuration File 등록

- 사용자 정의 클래스 등록하기 : `<context:component-scan base-package="com.haeni.*"/>`
	- `com.*`이라고 등록하면 다른 파일까지 전부 등록되므로, 잘 작성해야함

```note
**사용자 정의 클래스 등록하는 방법**
- `<bean>`
	- 클래스를 한 개씩 등록할 때 사용한다.
	- DI : setter 메소드에 값을 첨부할 때 사용한다.

- `<context:component-scan>`
	- 클래스를 패키지 단위로 동록할 때 사용한다.
	- DI(setter 메소드) 사용이 어렵다.
	- 반드시 어노테이션을 이용해서 처리한다.
```

- 데이터베이스 관련 정보 등록
	- 데이터 베이스 정보 모으기
	- SqlSessionFactory
	- Mapper

- JSP 찾기위해 ViewResolver 등록

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

	<!-- 1. 사용자 정의 클래스 등록 -->
	<context:component-scan base-package="com.haeni.*"/>
	
	<!-- 2. 데이터베이스 관련 등록 -->
	<!-- 2-1. 데이터베이스 정보 모으기 -->
	<bean id="ds" class="org.apache.commons.dbcp.BasicDataSource"
		p:driverClassName="jdbc:oracle:thin:@211.238.142.181.1521:XE"
		p:url="jdbc:oracle:thin:@211.238.142.181:1521:XE"
		p:username="hr"
		p:password="happy"
		p:maxActive="20"
		p:maxIdle="10"
		p:maxWait="-1"
	/>
	<!-- 2-2. SqlSessionFactory 처리하기 -->
	<bean id="ssf" class="org.mybatis.spring.SqlSessionFactoryBean"
		p:dataSource-ref="ds"
	/>
	<!-- 2-3. Mapper 구현하기(인터페이스) -->
	<bean id="mapper" class="org.mybatis.spring.mapper.MapperFactoryBean"
		p:sqlSessionFactory-ref="ssf"
		p:mapperInterface-ref="com.haeni.dao.RecipeMapper"
	/>
	
	<!-- 3. viewResolver 등록 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver"
		p:prefix="/"
		p:suffix=".jsp"
	/>
	
</beans>
```


```note
**Spring MVC 동작 순서**
1. 사용자 요청 (.do)
2. 요청 받기 (DispatcherServlet)
3. 요청 처리 (Model: @Controller)
	- 요청 처리 메소드 찾기 (HandelreMapping) 
	- HandelreMapping : 스프링에 할당해둔 클래스(@Controller가 붙은 것) 안에서 찾게 된다.
4. 요청 처리 메소드 호출 (Model : `invoke()`)
5. 결과값 담기 (request , session)
6. 결과값 전송 (ViewResolver : JSP찾아서 request 전송)
```

### RecipeController.java

- `@Controller` : 무조건 이 어노테이션이 붙어야 한다.
- 스프링에 생성된 DAO 객체 얻기

- 사용자 요청 받아서 처리하는 결과값 받기
