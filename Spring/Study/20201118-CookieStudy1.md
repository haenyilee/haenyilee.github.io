# 20201118-CookieStudy (1)

## 쿠키를 활용해서 최근 본 영화 출력하기

```note
- Spirng Container의 종류 : 사용자정의 클래스를 관리(생성,소멸 담당)
	- ApplicationContext
	- WebApplicationContext
	- AnnotationConfigApplicationContext
- 생성시에 필요한 데이터 전송하기
	- DI : setter , 생성자의 매개변수
	- 	p:		c:
- AOP : 중복제거해서 자동호출하는 프로그램
	- 시점 : JoinPoint
	- 메소드 호출 : PointCut

```

### MovieMapper
- 페이지 안나누고 20개 출력하는 쿼리문장 작성


### MovieDAOy

### application-context.xml
- aop
- beans
- context
- p
- mvc : `<mvc:annotation-driven/>`
  - RedirectAttributes , addFlashAttribute 를 사용하기 위해서 등록이 필요함
- mybatis

- 매퍼 등록
  - 1개 짜리일때 : `<bean id="mapper" class="org.mybatis.spring.mapper.MapperFactoryBean" p:mapperInterface="com.sist.dao.MovieMapper" p:sqlSessionFactory-ref="ssf" />`
  - 여러개일때 : `<mybatis-spring:scan base-package="com.sist.dao" factory-ref="ssf"/>`
  
- 인터페이스도 스프링이 메모리 할당을 해야할지 안해야할지 어노테이션으로 구분함
  - mapper는 `@Mapper`라는 어노테이션을 사용함

### MovieController
- 쿠키는 respons를 무조건 설정해야 함
- RedirectAttributes , addFlashAttribute : redirect:detail.do?no=xxx...를 post방식으로 값을 전송하는 기능이 필요할 때 사용한다.
  - application-context.xml에 `<mvc:annotation-driven/>`가 등록되어 있어야 함
  
  
  
### list.jsp

```html
<a href="detail_before.do?no=${vo.no }"> <img src="${vo.poster }" alt="Lights"
							style="width: 100%" class="img-rounded">
```


### MovieController (**쿠키**)
- movie_detail_before
  - 쿠키의 키, 값 설정? : `Cookie cookie = new Cookie("m"+no, String.valueOf(no));`
    - movie_detail_before에서 쿠키의 키가 중복되면 덮어써지기 때문에 앞에 m을 붙여서 구분해줘야 한다.
    - 쿠키는 문자열만 저장 가능하기 때문에 no를 String으로 형변환해줘야 한다.
  - 쿠키의 저장기간 설정 : `cookie.setMaxAge(60*60*24);`
  - 쿠키를 클라이언트에 저장 : `response.addCookie(cookie);`
- movie_detail
- movie_list
  - 최신 쿠키부터 출력하기 위해 for문을 역순으로 진행함 : `for(int i=cookies.length-1;i>=0;i--)`
  
### list.jsp 

### detail.jsp
