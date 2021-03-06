# 20201109-MVCstudy (6) - 스프링 구조

## 스프링 컨테이너
![](https://t1.daumcdn.net/cfile/tistory/2236F14757BBD25119)


## Spring Component
![](data:image/png;base64,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)
![](https://lh3.googleusercontent.com/proxy/WDtrrzznBLkQXin8OYEOq_aWchtufDDDAJAPv_7w3ip0Ul_zUM_iuoZ9kb6E1WbvmrY4dialCE1Rnc6HmhgalOyvwCvNX22HR5iJsnPrPfsaOjXGY93UU2XIy5swXKY3rrYFb1mIUsr3sRZ3OPoJjDV4onfeP6MCzkotO_7mdh88Cmv8oUWKULcGmpFLQ_6NaRhPp65c6M8y_K0vxTAV1SLq3N3rHYb1BrN5M6QNMhNfxnPlW20z3JpFC6NMFLJ2tE0k82xFLUmLo46PLrva1yteP-5Luw)

## 자바 Reflection
- Reflection : 클래스의 정보를 읽어와서 처리
  - 리플렉션이란 객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법을 말한다.
  - 클래스의 구조를 개발자가 확인할 수 있고, 값을 가져오거나 메소드를 호출하는데 사용된다.
- Reflection을 사용해서 스프링에서는 런타임 시에 개발자가 등록한 빈을 애플리케이션에서 가져와 사용할 수 있게 되는 것이다.
  
  - class : ClassForName() 메모리 할당
  - 멤버변수 : 필요시에 변수를 초기화
  - 생성자 : 멤버변수 초기화(매개변수)
  - 메소드 : setter/getter 
  - 지역변수(X)
  - 기타 : 생성 시, 한번 혹은 소멸 시, 한 번
- 스프링을 통해서 값을 넣어주는 것을 DI라고 한다.

## 스프링이 필요한 이유는?
- 메소드가 20개면 스프링이 알아서 20개를 배치해주는 것이 아니다.
  - 맨 처음 생성 한번, 소멸할 떄 한 번만 스프링이 담당한다.
  - 나머지는 프로그래머가 직접 배치해야한다.
  - 90%는 프로그래머가 담당하고 10%만 스프링이 담당한다.
  - 스프링을 사용한다고 소스가 작아지는 것이 아니다
  
- 스프링의 장점은 핵심코딩만 하게 해준다는 것이다.
  - 생성, 소멸은 스프링이 담당하기 때문이다.  
- 스프링의 좋은 점은 소멸의 관리를 스프링이 담당한다는 것이다.
  - 생성을 하지 않는 것이 장점이다.
    - 스프링을 사용하지 않고 new를 사용하면 메모리 할당이 커진다.
  - 스프링에서 값을 넣는 방법은 setter와 생성자 둘 뿐이다.

- 스프링에는 MVC가 없었다.
  - 웹이 발전하면서 MVC가 라이브러리로 발전한 것이다.
  - DispatcherServlet이 잘 만들어져 있다.
  
- 스프링에서는 DI와 AOP가 핵심이다.
  - 면접 TIP : OOP(객체지향 프로그램)답게 만든 것이 AOP이다?
  - OOP에는 자동호출 기능이 없다.
  - 콜백함수처럼 자동호출하게 만드는 것이 AOP이다.
  - 중복을 제거할 때, AOP는 호출을 안해도 자동으로 만들게 해주는 것이다.
  - OOP를 더 보완한 것이 AOP이다.
  


## IoC 적용을 위한 기법
![](https://mblogthumb-phinf.pstatic.net/MjAxNzA0MTdfNzcg/MDAxNDkyNDI1MzgzNzQy._W91cz5Ariw5noo7_DvFGKh2dJW17zjWZin4gP1NLk8g.crAuNPuwICooiwGXGgmkQpWe6e3DG_s3INFETFrf4NQg.JPEG.wwwkang8/3.JPG?type=w800)
- IoC(Inversion of Controll) : 제어의 역행
  - 기존의 제어방식은 필요로 하는 측에서 만들어서 썼다. 제어의 역행을 통해서 이루고자 하는 것은 낮은 결합도, 유지보수성 향상이다.
- DL(Dependency Lookup) : `getBean()` , 클래스 관리자
- DI(Dependency Injection) : 주입 , 필요한 데이터를 첨부
  - setter DI
    - 멤버변수 값 주입
      - `p:name=""`
  - 생성자 매개변수에 값 주입
     - `c:name=""`
  - 메소드 DI
    - init-method
    - destroy-method
- 스프링은 결합성이 낮은 프로그램을 지향한다. (POJO방식)
  - 클라이언트 쪽에는 에러가 나지 않도록 만드는 것이 스프링이다.
  - 클라이언트에게 서버 정보가 새지 않는 이유는 스프링이 컨테이너를 통해 제작되었기 때문이다.
  
## 스프링 2.5 VS 5.0

- 스프링 2.5~4.XX의 구성
  - 자바
  - XML(XML, Annotaion)
- 스프링 5.0
  - 자바 전용(Annotation)
  
  
## 스프링 어노테이션 종류
  
```note
**할당**
- @Component
- @Repository
- @Service
- @Controller
- @RestController
- @ControllerAdvice
- @Configuration
	
**주입**
- @Required
- @Autowired
- @PostrConstruct
- @PreDestroy
- @Resource
```

## 순서
- `.do` => DispatcherServlet => HandlerMapping => Model(@Controller) => DAO(Manager) => Model(@Controller) => DispatcherServlet =>  ViewResolver => View(JSP) 


- Model(RequestMapping) => DAO(Manager) => `request.setAttribute()` => `model.addAttribute()`

- Tiles

