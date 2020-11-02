---
sort:
---

# MVC Study (1)

## Spring의 MVC 구조
- Model : VO , DAO , Manager , Controller
  - 분리하는 목적 : 다른 프로젝트에서 재사용하기 위해서이다.
  - 자바로 제작된 클래스는 대부분 Model이다.

- View : css , js , jsp , html
  -분리하는 목적 : 재사용하기 위해서이다.

- 서블릿 : DispatcherServlet
  - Model+View를 합쳐주는 역할을 담당한다.


## Spring에서 달라지는 MVC 과정

```note
                             request
사용자 요청 ▷ DispatcherServlet ▷ Model클래스 찾기      ↔ DAO
        request                  : @RequestMappin("*.do")
```

- DispatcherServelet 단계에서 JSP Model2방식과 다른 점이 있다.
  - Model클래스의 RequestMapping을 찾아주는 클래스가 하나 더 있는데, 이것이 바로 `HandlerMapping` 이다.
    - 라이브러리이기 때문에 직접 구현할 필요가 없다.
  - JSP를 찾아서 request 전송하는 클래스인 `ViewResolver`가 있다.
    - JSP의 경로명만 XML에서 전송해서 알려주면 된다.
    - JSP Model2방식에서는 직접 return했던 과정이다.
 
- Model클래스에는 `@Controller`를 올려주지 않으면 스프링에서 Model클래스를 찾지 못한다.
 
- 메인 컨트롤러 : JSP의 모델클래스 = 요청을 제어한다.
  - MVC 구조에서 Model클래스 역할을 담당
  - 요청을 처리하고 결과값을 전송한다. (Busiuness Logic / JSP는 View로직이라고 한다.)
  - 스트럭츠에서는 메인액션이라고 칭한다.
- 디스패처 : 프론트 컨트롤러 = 요청을 받는다

 
## 값을 받아서 출력하는 방법 (1) 단일변수
- 여러 방식 중 권장하는 방식을 사용해야 한다.

### MainController.java

- 스프링 2.5버전 이후부터는 보안을 중요시하기 때문에 request 사용을 권장하지 않는다.
  - request안에서 접속자의 ip를 확인 가능하기 때문에 보안에 약하다.

- 스프링 5.0버전 이상 부터는 클래스 
  - 클래스 파일을 볼 수 없으므로 소스를 공개하지 않을 수 있다.
- 데이터 처리를 하고 전송 JSP 
  - `return "폴더명/jsp명"` : forward
    - `.jsp`를 생략한다.
 - `return "redirect:main.do"` : sendRedirect

- 스프링 디폴트 폴더 : `src/main/java` , `webapp`

- request를 사용하지 않으려면 매개변수로 값을 받고, Model 인터페이스를 이용해서 전송한다.
- 매개 변수는 DispatcherServlet이 설정해준다.
  - 순서 상관 없다.
  
- Model의 addAttribute 메소드

```java
public class Model
{
  HttpServletRequest request;
  public void addAttribute(String key,Object obj)
  {
    request.setAttribute(key,obj)
  }
}
```



### input.jsp

```note
- `<input type=submit>`
- `<button>`
- `<input type=image>`
- ajax는 submit버튼 사용안하고 data 필드를 통해서 전송함
```


### application-context.jsp

- 패키지 단위 등록
- viewResolver 처리


## 값을 받아서 출력하는 방법 (2) VO단위

### EmpVO
- 변수 선언
- 은닉화, 캡슐화

### MainController
- 매개변수가 vo와 model이 됨
- `@GetMapping` : get방식으로 전송한다는 것을 표기하는 `@RequestMapping`이다.
- `@PostMapping` : post방식으로 전송한다는 것을 표기하는 `@RequestMapping`이다.
- `@RequestMapping`을 사용하지 않는 이유는?
  - 전체를 검색하면 loop를 돌리는 횟수가 증가하여 비효율적이기 때문이다.
  - 속도를 더 높이기 위해 post/get방식을 분리한 것이다. (최적화/퍼포먼스 측면)
  
- return값은 prefix 이후의 정보를 넣으면 된다.
- `@RequestMapping("main/")`을 클래스 위에 적어주면 `@GetMapping("emp.do")`만 적어도 `main/emp.do`를 가리킨다.
  - 반복적으로 들어가는 경로를 위에 뺴서 미리 선언할 수 있다.
  
### dispatcher-servlet.xml

