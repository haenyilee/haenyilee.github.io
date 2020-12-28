---
sort: 2
---

# MVC

## MVC란?
- Model
- View
- Controller
- Model과 View는 항상 Controller를 거쳐야 한다.
- Model과 View의 갯수는 일치해야 한다. 

## MV구조와 다른점
- JSP에 작성했던 아래의 자바코딩이
```JSP
<%
    String no=request.getParameter("no");
    BoardDAO dao=new BoardDAO();
    BoardVO vo=dao.boardUpdateData(Integer.parseInt(no));
%>
```
- Controller.java파일로 대체된다.

## Model 1 vs Model 2
![model 1 vs model 2](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile21.uf.tistory.com%2Fimage%2F25538C3959703C862C0BDD)

- 장단점 비교

||장점|단점|
|----|:------|:------|
|**모델1**|기능과 JSP의 직관적인 연결이 가능하다|로직 코드와 뷰 코드가 혼합되어 <br> 코드가 복잡해지고 유지보수가 어렵다.|
|**모델2**|컨트롤러 서블릿에서 <br> 집중적인 작업 처리가 가능하고 확장이 용이하다|작업량이 많다.|


## MVC 구조로 웹페이지 만들기
- M/C : Java로 구성할 예정이다.
  - Controller를 Spring처럼 제작해서 사용할 예정이다.
- V : JSP로 구성할 예정이다.

## 프로젝트 제작
- JavaResources - Library - JRE System Library - properties - Workspace defaultJRE
- web.xml 추가


## Java 패키지 제작
- model : 데이터 읽기, 사용자 요청을 처리하는 기능을 담당한다.
- controller : 사용자 요청을 받아서 model에 연결해 데이터를 가지고 온 뒤, jsp로 전송하는 기능을 담당한다.
  - 스프링에서 지원하는 기능이기 때문에, 차후에는 만들 필요 없다.
  - 브라우저에서 들어오는 값을 받을 때는 JSP/Servlet 둘 중에 하나만 사용해야 한다.
  - JSP는 소스가 공개되는 단점이 존재하기 때문에 대부분 Servlet을 사용한다.
  - Servlet코드는 화면 출력용이 아니고, 연결과 보안용이다. 

## Web 폴더 제작
- view : controller로부터 결과값을 전송 받아서 화면에 출력하는 기능을 담당한다.
  - JSP로 제작된다.

## 파일 제작
- List/Detail/FindModel.java 제작
- list/detail/find.jsp 제작
- Controller.java 서블릿 제작
  - 생성자, doget, dopost 체크해제
  - init , service 체크



#### Servlet `init()`
- XML파싱, 파일읽기
- 생성자 역할을 담당한다.
- `ServletConfig` : config를 web.xml에 등록하는데, 이 데이터를 읽을 수 있는 기능을 담당한다.

#### Servlet `service()`
- 사용자가 전송하는 방식에는 GET/POST로 나뉘어지는데 이 방식에 따라서 메소드 처리가 달라진다. 
  - GET : `doGet()`
  - POST : `doPost()`
- 두 메서드를 동시에 처리하는 것이 바로 `service()`이다.



# 목록 출력하기
## web.xml (Servlet)
- 프로그래머가 구동하는 것이 아니라 톰캣이 구동하기 때문에 web.xml에 미리 등록해야 한다.
  - 스프링은 web.xml에 등록하는 과정이 어노테이션으로 만들어져 있다.
- web.xml은 WAS가 최초 구동될 때 WEB-INF 디렉토리에 존재하는 web.xml을 읽고, <br> 그에 해당하는 웹 애플리케이션 설정을 구성한다. 
- 한마디로 각종 설정을 위한 설정파일이다.

#### web.xml태그 (출처 : [web.xml](https://devpad.tistory.com/24))
- `<servlet>`
  - DispatcherServlet을 구현하기 위해 어떤 클래스를 이용해야 할지와 초기 파라미터 정보를 포함하고 있다. 
- `<servlet-name>`
  - 해당 서블렛의 이름을 지정하면 이 지정된 이름을 가지고 다른 설정 파일에서 해당 서블릿 정보를 참조한다. 
- `<servlet-class>`
  - 어떤 클래스를 가지고 DispatcherServlet을 구현할 것인지를 명시하고 있다. 
- `<init-param>`
  - 초기화 파라미터에 대한 정보. servlet에 대한 설정 정보가 여기에 들어간다. 
  - 만약 초기화 파라미터에 대한 - 정보를 기술하지 않을 경우 <br> 스프링이 자동적으로 appServlet-context.xml을 이용하여 스프링 컨테이너를 생성한다. 
- `<load-on-startup>`
  - 서블릿이 로딩될 때 로딩 순서를 결정하는 값. 
  - 톰캣이 구동되고 서블릿이 로딩되기 전 해당 서블릿에 요청이 들어오면 서블릿이 구동되기 전까지 기다려야 한다. 
  - 이 중 우선순위가 높은 서블릿부터 구동할 때 쓰이는 값이다.
- `<servlet-mapping>`
  - 서블렛이 `<url-pattern>`에서 지정한 패턴으로 클라이언트 요청이 들어오면 <br> 해당 `<servlet-name>`을 가진 servlet에게 이 요청을 토스하는 정보를 기술한다. 


```note
**톰캣의 역할**
- 서블릿 구동
- 에러처리
```

```note
**URL의 역할**
- Controller가 사용자 요청을 받는다.
- Controller가 요청을 처리하기 위해서 Model을 찾는다.
- Model에서 요청처리 결과값을 Controller가 받는다.
- Controller가 결과값을 request/session에 담아준다.
- request가 해당 JSP로 결과값을 전송한다.
- JSP가 구동됨과 동시에 실행된 HTML을 JSP로 전송한다.
```

1. 서블릿 등록

```xml
  <servlet>
  	<servlet-name>mvc</servlet-name>
  	<servlet-class>com.sist.controller.Controller</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>mvc</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
```

- MVC에서는 Controller를 찾아야 결과값을 받을 수 있기 때문에 이를 찾는 과정이 중요하다.
- 다른 서블릿과 URL 충돌을 막기 위해 `.do`를 확장자를 주면 Controller를 찾을 수 있다.
- MVC구조에서는 URL주소가 주로 `*.do`로 끝난다.
  - list.jsp찾기 위해서는 list.do 라는 URL 주소를 찾으면 된다.
  - find.jsp를 찾기 위해서는 find.do 라는 URL 주소를 찾으면 된다.
- `<url-pattern>` 뒤에 올 수 있는 확장자는 사용자가 지정할 수 있다.
  - 보통 회사 명칭을 많이 준다. (ex. *.naver , *.daum)


## Controller가 필요한 이유
- 처리 후 결과값이 담겨있는 request값을 받아서 JSP에 넘겨주는 역할을 한다.
- 각 model.java에 아래 코딩을 추가하고

```java
public void execute(HttpServletRequest request)
	{
		String msg="FindModel=>find.jsp로 값을 전송";
		request.setAttribute("msg", msg);
	}
```

- JSP에서 아래 코딩을 실행하면 결과값을 받지 못한다. 

```jsp
<body>
	<center>
		<h1>list.jsp영영</h1>
		<h1>${msg }</h1>
	</center>
</body>
```

- 그 이유는 결과값이 담겨있는 request를 호출하지 않았기 때문이다. 
- 그래서 각 JSP 파일 내에 아래 코딩을 추가하면 request값을 받을 수 있다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="com.sist.model.*"%>
<%
	ListModel model=new ListModel();
	model.execute(request);
%>
```

- 하지만 이런 방식은 모든 파일을 일일히 찾아서 변경해야 하기 때문에 유지보수에 불리하다.
- 이 기능을 Controller에 모아두면 내용을 한 번만 수정해주면 된다는 장점이 존재한다. 

## Controller로 대체하는 방법
- Controller?cmd=list => ListModel => list.jsp

1. model.java import하기 : `import com.sist.model.*;`
2. `service()` 메소드에서 request값 받기
3. request값을 JSP(View)로 전송하기
- RequestDispatcher로 dispatch(배달)한다.


## Controller에서 URI를 받는 방식
- <url-pattern>/Controller</url-pattern>로 설정한 뒤, 
Controller에서 IF문을 돌려서 URI값을 받아올 수도 있지만, 소스량이 늘어난다면 계속해서 If문을 추가하는 것은 비효율적이다. 
- 그래서 <url-pattern>*.do</url-pattern>로 설정한 뒤 해시맵으로 값을 받는 편이 유지보수가 편리하다.

```note
**스프링의 필요성**
- 하나의 클래스에 많은 메소드를 모아둬서 결합도(의존성)을 낮춘다.
- LooseComplition
- 한 클래스에서 오류났을 때 다른 클래스에서도 연쇄적으로 오류가 발생하면 <br> 각 클래스를 일일히 찾아서 수정해야 하기 때문에 유지보수가 어렵다.
```
