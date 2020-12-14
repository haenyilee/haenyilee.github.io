# 20201214-web-app-spring-last

## spring
### web.xml
- tomcat이 인식하는 파일
- servlet 파일을 실행해주는 역할을 한다.
  - Controller를 등록한다.
  
```note
**servlet 동작과정(생명주기)**
1. 생성자 호출 : 메모리 할당 (톰캣이 담당)
2. init() : 파일을 읽어서 멤버변수의 초기화 , 필요한 데이터 읽어오기
  - xml파일 읽어서 처리
3. service() : 클라이언트마다 따로 호출하는 함수 (쓰레드에서 호출되는 함수)
  - 클라이언트 요청 받아서 응답을 하는 메소드
  - 요청시마다 service()를 호출
4. 화면 변경되거나, 새로고침 , 이동 시 : 자동으로 메모리에서 해제
  - destroy()
```

- 에러가 났을 경우에 해당 페이지로 이동한다.
- servlet 등록 : 스프링 컨트롤러가 서블릿을 제작한다.
- 한글 변환 코드 등록
- 에러 처리

```note
**스프링 MVC가 전송하는 과정**
1. 사용자 요청 : `.do`
2. DispatcherServlet : 사용자 요청을 받는다.
3. 요청을 처리할 클래스 찾기 : `@RequestMapping` , `@GetMapping` , `@PostMapping`
  - DispatcherServlet => HandlerMapping => invoke()가 메소드 호출
  - 메모리할당 , 데이터 첨부(DI) , 반복 기능(AOP)
  - DI , AOP 사용시 반드시 XML, Annotation을 사용해야 한다.
4. 수행된 결과를 DispatcherServlet로 다시 전송
5. 결과값을 Model에 담아서 ViewResolver로 전송
6. ViewResolver가 Model에 담긴 것을 request에 담아서 해당 JSP로 전송한다.
7. 받은 JSP에서 화면 출력을 한다
```

- 스프링에서 request를 가급적이면 사용금지 시킨다.
  - 왜냐하면 접속한 클라이언트의 정보(IP,PORT)를 가지고 있기 때문에 보안에 불리하기 때문이다.(파밍)
  
  
### MainController
- Main 페이지에서 서비스 역할

#### `@Controller` vs `@RestController`
- `@Controller`
  - forword, sendRedirect만 담당한다.
  - forword를 위해서는 jsp 파일명을 보내야 한다.
    - return "경로명/jsp 파일명";
    - forward는 request를 유지하기 위해서 사용한다.
  - sendRedirect를 할 때는, .do를 전송한다.
    - return "redirect:경로명/.do";
    - requset를 기억하고 있을 필요가 없는 경우에 사용한다.
    - request가 저장되지 않는다.
    
- `@RestController`
  - 해당 jsp로 문자열을 전송할 때 사용한다.
  - json, xml을 전송할 때 주로 사용한다.
  - React , AJax , 코틀린에게 데이터만 전송할 때 사용한다.
  


### FoodDAO

- 메모리할당을 하는 어노테이션을 보면 구분해서 사용함
  - 일반클래스 : @Componenet (JSOUP,OPENAPI)
  - 데이터베이스 관련 : @Repository
  - DAO가 여러개일때 묶어서 사용 : @Service
  - MVC구조의 Model을 만들 때 : @Controller ,  @RestController
  
- 스프링에서 구현된 클래스를 받는다.
  - 
  
  
### application-context

- 사용자 정의 클래스의 메모리를 할당해준다
-  viewResolver : jsp를 찾을 수 있게 경로명과 확장자를 넘겨줘야 한다 


### application-datasource
- 마이바티스 , 몽고디비 연결

- DBCP : database connection pool
  - 커넥션을 미리 생성 , 저장해서 사용하기 때문에 속도가 빠르다
  - 커넥션의 갯수를 제한할 수 있다.(기본 maxActive=8 , maxIdle)

- ORM : 마이바티스 , 아이바티스
  - 통일화 ,표준화 , 사용하기 편리하다
  - 연결, 해제를 자동으로 처리해주기 때문에 편리하다
  - 핵심 SQL만 작성하면 된다는 것이 장점이다.
  - 소스코딩이 간결해진다.
  
### FoodMapper(Interface) ,FoodDAO , MainController

### MainRestController
- 코틀린이 데이터를 전송함

- model을 이용하지 않음

- result에 json 형식의 데이터를 담아서 전송한다.

- 한글이 깨지지 않게 하기 위해선 requestMapping에 설정해준다
  - `@RequestMapping(value="kotlin_list.do",produces="text/plain;charset=UTF-8")`
