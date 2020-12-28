# 20201103-MVCstudy (3)

## 1.8로 바꾸는 이유
- 스프링 5.0이 1.8기준이기 때문이다.

## XML 파일 여러개를 사용하는 방식

### web.xml
- `<param-value>/WEB-INF/config/application-*.xml</param-value>`
  - `application-*.xml` : application으로 시작하는 xml파일 전부 읽어오기
- 사용자 정의 클래스 등록 : application-context.xml
- 데이터베이스 등록 : application-datasource.xml
- 보안
- 웹 소켓
- 몽고 디비

### WEB-INF : [config]폴더
- web.xml을 모아두는 폴더

### application-web.xml
- JSP찾기 : ViewResolver 파일 업로드
- 파일 업로드 : MultipartResolver
- Tiles : TilesView
  - include 자동 포함되어서 템플릿 제작에 주로 사용됨
  - 안정성이 높고, 화면 깜빡거림이 없다.

```xml
<bean id="viewResolver" 
		class="org.springframework.web.servlet.view.InternalResourceViewResolver"
		p:prefix="/"
		p:suffix=".jsp"
/>
```

- `viewResolver` : JSP찾아서 request를 전송함
  - 경로명 지정 :  `p:prefix="/"`
  - 확장자 지정 : `p:suffix=".jsp"`


### application-datasource.xml
- DataBase 관련 등록 : JDBC , ORM(MyBatis , Hibernate) , Spring-Data(MongoDB:NoSQL)
  - MongoDB가 Oracle보다 저장속도 , 저장용량이 빠름
  - 핵타바이트까지 저장 가능함
  - 오라클처럼 정형화된 규칙이 없다.
  
- MyBatis 연결
  - properties 파일 읽기
  - 데이터베이스 연결 정보 모으기
    - `p:driverClassName="#{db['driver']}"`
      - driver라는 키값을 가져옴?
  - 마이바티스로 데이터베이스 연결 정보 전송하기
    - ref는 id정보 전달


### db.properties
- DBCP : 데이터베이스 제한
  - maxActive=20
    - 커넥션 개체가 20개를 넘어갈 수 없다.
  - maxIdle=10
  - maxWait=-1

### Config.xml
- 마이바티스 DTD 작성

## 오라클 데이터값 불러오기

### recipe-mapper.xml
- 쿼리 문장 작성

### RecipeDAO
- 스프링에 메모리 할당
- datasource.xml에서 생성된 객체를 sessionfactory 에 넣어달라고 요청?

- `getSqlSession()`
  - `session=ssf.openSession()` + `session.close()`
  
  
### RecipeController
- Model
- `@Controller`
- `@Autowired` : 메모리 할당

- 사용자 요청이 있을 때 처리한다.
  - 웹에서 사용자 요청은 URL 주소로밖에 받을 수 없다.
  - URI : 서버주소를 제외하고 나머지
  - 요청 : *.do
  
- 페이지는 String으로 받는 것이 편하다
  - 첫 요청 시에는 null값이기 때문이다.
  
  
### list.jsp

- 화면출력 view

```note
- `@RequetMapping()` : 통합
- `@GetMapping()` : `<a>` , sendRedirect() , location.href
- `@PostMapping()` : ajax , form
```
