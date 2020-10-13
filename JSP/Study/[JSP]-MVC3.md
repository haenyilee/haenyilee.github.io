---
sort: 2
---

# URI

- 방법1 : http://localhost/mvc/(list).do
- 방법2 : http://localhost/mvc/Controller?cmd=list


- `.jsp`, `.asp`, `.aspx`, `.php` 이외의 확장자는 mvc구조로 만들어졌다고 보면 됨

## MVC구조의 request 전송과정
![](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F2405DC46577868AF13E0FE)
  - 클라이언트 : request요청
  - Controller : request 전달
  - Model (↔ DAO) : 결과값을 request에 담음
  - Controller : request 결과값
  - JSP : request 출력
  - JSP : 요청
  - Controller
  - Model : 요청처리
  - Controller : 결과값
  - JSP 출력


## web.xml
- 톰캣이 읽어가는 부분이다. 
- Servlet(= Controller)을 등록할 때 사용한다.
- Servlet 등록하기
  - 메모리 할당하기
  - `map.put("mvc",com.sist.controller.Controller)`와 같은 코딩이라고 볼 수 있다. 
```xml
  <servlet>
  	<servlet-name>mvc</servlet-name>
  	<servlet-class>com.sist.controller.Controller</servlet-class>
  </servlet>
```

- URI 패턴 지정하기
  - `http://localhost/mvc/(list).do` 형식으로 코딩하려면 `*.do`를 설정하면 된다.
```xml
  <servlet-mapping>
  	<servlet-name>mvc</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
```


#### web.xml없이 코딩하려면?
- controller.java(Servlet)에 어노테이션 등록하기 : `@WebServelt("*.do")`
- 하지만 이는 스프링에 없기 때문에 web.xml을 제작하는 방식이 더 스프링에 적합하다.
```java
@WebServlet("*.do")
public class Controller extends HttpServlet {
	private static final long serialVersionUID = 1L;

	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
	}

	@Override
	public void init(ServletConfig config) throws ServletException {
		
	}    
```

#### mapper.xml 없이 코딩하려면?
- Mapper.java(interface)에서 어노테이션 사용하면 된다.
- 참고 : [MyBatis Mapper 인터페이스](https://bigstupid.tistory.com/23)
- 코딩으로 구현하면 아래와 같다.
```java
package com.sist.dao;

import org.apache.ibatis.annotations.Select;
import java.util.*;
import com.sist.dao.*;

public interface Mapper {
	@Select("SELECT * FROM emp")
	public List<RecipeVO> listAllData();

}
```

## Model.java (Interface)

- 인터페이스 : 클래스에 공통되는 부분을 하나로 묶어 서로 다른 클래스를 연결하는 것이다. 
- 일반 변수를 묶을 때는 배열을 사용한다.

#### 인터페이스
```JAVA
class A implements I
```
- `A a=new A();`도 가능하고 `I i=new A();`도 가능하다.
- 관련된 여러 개 클래스를 통합할 때 필요한 것이 인터페이스이다. 
- 기능은 없지만 설정만 해주기 때문에 리모콘에 비유된다. 
- 인터페이스의 단점은 기능 추가가 어렵다는 점이다. 
  - 그래서 스프링의 POJO (Adapter패턴)을 사용하게 된다. 
  - 참고 : [[구조패턴] Adapter Pattern](https://readystory.tistory.com/125)



## Controller.java (Servlet)
- 서블릿 파일 제작
- 필요한 메서드 Override
  - [Soure] - [Override/Implement Methods]
  - [HttpServlet] - service() 체크
  - [GenericServelt] - init() 체크


- import 추가
```java
import java.util.*;
import com.sist.model.*;
```

#### Model과 View 매칭시키기
- 예를 들어 `ListModel.java`와 `list.jsp`를 매칭시키는 클래스를 제작하는 과정이다.
- 클라이언트가 Controller에 어떤 페이지를 요청하면, 요청을 처리할 Model클래스를 찾는 것이 첫 번째 일이다.
  - Controller는 어떤 요청인지 알 수 없기 때문에, 인식할 수 있도록 명령을 내려줘야 하기 때문에 이 과정이 필요하다.
- Model정보와 View정보를 매칭해서 저장할 수 있는 구조인 Map에 값을 넣는다.
  - `init()`메소드에서 실행한다.
    - 서블릿 컨테이너는 서블릿 객체가 생성된 후, 단 한번만 init() 메소드를 호출한다.
    - new를 사용하면 계속 새로운 메모리가 생성되고 회수(GB)가 바로바로 되지 않기 때문에 버벅인다. 
    - 싱글턴 패턴을 이용하면 값을 딱 한 번만 생성하지 때문에 메모리를 효율적으로 사용할 수 있다. 

    - 서블릿은 init() 메소드가 완료되어야 서비스할 수 있다. 
      - init() 메소드 완료 전의 요청은 블록킹된다.
    - init() 메소드가 호출될 때 ServletConfig 인터페이스 타입의 객체를 매개변수로 전달받는데, <br> 
만약 web.xml 에서 서블릿 초기화 파라미터를 설정했다면, <br> 
전달받은 ServletConfig 에는 web.xml에서 설정했던 서블릿 초기화 파라미터 정보를 가지고 있게 된다.
    - 초기화 파라미터가 있다면 init() 메소드에 서블릿의 초기화 작업을 수행하는 코드가 있어야 할 것이다.
    - 출처 : [Servlet인터페이스](https://cofs.tistory.com/48)

```java
public class Controller extends HttpServlet {
	private static final long serialVersionUID = 1L;
	
	private Map clsMap = new HashMap();

	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
	}

	@Override
	public void init(ServletConfig config) throws ServletException {
		clsMap.put("list", new ListModel());
		clsMap.put("detail", new DetailModel());
	}
}
``` 


#### 사용자가 보낸 값 확인하기
- URL 중에서 URI받아온다.
  - URI 읽어오는 메서드 : `getRequestURI()`
    - `http://localhost/mvc/Controller?cmd=list`처럼 `?`뒷부분은 읽어오지 않는다.
    - request에 담겨서 받아지는 값이기 때문이다. 
```
String cmd=request.getRequestURI();
```

- URI에서 `/`부터 `.do`앞 부분까지만 substring하기
  - 사용자가 요청한 요청내용을 확인하기 위함이다.
  - `getContextPath()`의 메소드 (출처:[getContextPath](https://chogoon.tistory.com/entry/requestgetContextPath%EC%99%80-requestgetRequestURI%EC%9D%98-%EC%B0%A8%EC%9D%B4))
    - `request.getContextPath()`는 프로젝트 path만 얻어온다.
      - 요청 : http://localhost/ZESTINE/test.jsp 경우 → /ZESTINE 경로만 얻는다
    - `request.getRequestURI()`는 프로젝트와 파일 경로까지 얻어온다.
      - 요청 : http://localhost/ZESTINE/test.jsp 경우 → /ZESTINE/test.jsp 까지 얻어온다.

```java
String cmd=request.getRequestURI();
```


- 클래스 찾기 시작하기
```java
Model model=(Model)clsMap.get(cmd);
```

- 요청 처리하기
  - request안에 결과값을 실어줬기 때문에 JSP로 전송이 가능하다.
  - `handlerRequest()`
    - 스프링에서 사용자 요청 처리할 때 사용하는 메서드이다. 
    - `HttpServletRequest request` 매개 변수를 넣으면 `"board/list.jsp";`을 반환한다?
```java
String jsp= model.handlerRequest(request);
```

- JSP로 전송하기
  - `RequestDispatcher`
  - `forward()`

  - (출처 : [RequestDispatcher로 forward() 하기](https://dololak.tistory.com/502) )
```java
RequestDispatcher rd=request.getRequestDispatcher(jsp);
rd.forward(request, response);
```


## ListModel.java
- 리스트에 값 넣어서 `board/list.jsp`로 넘겨주기
```java
public class ListModel implements Model{

	@Override
	public String handlerRequest(HttpServletRequest request) {
		List<String> list=new ArrayList<String>();
		list.add("홍길동1");
		list.add("홍길동2");
		list.add("홍길동3");
		list.add("홍길동4");
		list.add("홍길동5");
		request.setAttribute("list", list);
		return "board/list.jsp";
	}


}
```

## Controller.java (Servlet)에서 구동하기
