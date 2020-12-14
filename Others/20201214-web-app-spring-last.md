# 20201214-web-app-spring-last

## spring
### web.xml
- tomcat이 인식하는 파일
- servlet 파일을 실행해주는 역할을 한다.
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
4. 수행된 결과를 DispatcherServlet로 다시 전송
5. 결과값을 Model에 담아서 ViewResolver로 전송
6. ViewResolver가 Model에 담긴 것을 request에 담아서 해당 JSP로 전송한다.
7. 받은 JSP에서 화면 출력을 한다
```
