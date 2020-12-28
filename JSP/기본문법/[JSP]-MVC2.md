---
sort: 2
---

# MVC 구조로 웹 프로젝트 구성하기 (1)

## 초기 셋팅

0. 프로젝트 생성
- web.xml 추가하기

1. 패키지, 폴더 제작
2. Model Interface제작
- 클래스 유형이 다르면 조건문으로 제작하면 되지만, 모든 클래스 유형이 동일하면 interface로 제작한다.
- 클래스를 모아서 한 번에 관리할 때 사용한다.

3. Model.java class제작
- add Model하면 아래 코딩과 같이 Model 인터페이스가 오버라이딩 됨

```java
package com.sist.model;

import javax.servlet.http.HttpServletRequest;

public class ListModel implements Model {

	@Override
	public String execute(HttpServletRequest request) {
		// TODO Auto-generated method stub
		return null;
	}

}
```

4. View.jsp 파일 제작
- model과 매칭되어야 한다.

5. Controller.java (Servlet) 제작


## web.xml
- 메모리 할당 위치 설정하기

```XML
<servlet>
  	<servlet-name>mvc</servlet-name>
  	<servlet-class>com.sist.controller.Controller</servlet-class>
</servlet>
```

- URL 주소를 어떻게 설정할 것인지 설정해두기

```XML
<servlet-mapping>
  	<servlet-name>mvc</servlet-name>
  	<url-pattern>/Controller</url-pattern>
</servlet-mapping>
```

## Controller.java
- 입력될 요청값 배열로 모아두기
  - 배열 Array에서의 인덱스는 값에 대한 유일무이한 식별자이다.

```java
	String[] strCmd= {
			"list",
			"detail",
			"insert"
	};
```

- 연결되는 Model 배열로 모으기

```java
String[] strClass= {
			"com.sist.model.ListModel",
			"com.sist.model.DetailModel",
			"com.sist.model.InsertModel"
	};
```

- map으로 담기

```java
private Map clsMap = new HashMap();
```

- 메모리 할당 후 해시맵에 값 넣기
  - 리플렉션 : 클래스명을 받아서 메모리 할당하기
  - 리플렉션은 모든 정보(메소드, 생성자, 멤버변수)를 제어한다.
- `Class.forName()` : ?????

```java
	public void init(ServletConfig config) throws ServletException {
		try {
			for(int i=0;i<strClass.length;i++)
			{
				Class cls = Class.forName(strClass[i]);
				Object obj=cls.newInstance(); // 메모리 할당(리플렉션)
				clsMap.put(strCmd[i], obj);
			}
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
```


- URI에 jsp 파일 이름 매칭하기
  - 앞서 해시맵에 등록해둔 key를 찾는 과정이다.
  - URI는 URL에서 앞의 서버 정보를 제외한 것이다. 

```java
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String cmd=request.getRequestURI();
		System.out.println("cmd:"+cmd);
		cmd=cmd.substring(request.getContextPath().length()+1,cmd.lastIndexOf("."));
		System.out.println("cmd2:"+cmd);
}
```

- model, java.util import하기

```java
import java.util.*;
import com.sist.model.*;
```

- 클래스 찾기

```java
Model model =(Model) clsMap.get(cmd);
```

- 클래스를 찾아서 메소드 호출 후 기능(요청사항) 처리하기

```java
String jsp=model.execute(request);
```

- request에 값을 담아서 JSP로 전송하기
```java
RequestDispatcher rd= request.getRequestDispatcher(jsp);
rd.forward(request, response);
```


## ListModel.java
- list.jsp에 msg라는 키에 "게시판 목록"이라는 문자열을 매칭해서 request에 실어서 넘겨준다. 
  - `request.setAttribute("msg","게시판 목록");` : request에 값을 실어주기
  - `return "board/list.jsp";` : 받는 쪽 설정

```java
@Override
	public String execute(HttpServletRequest request) {
		request.setAttribute("msg","게시판 목록");
		return "board/list.jsp";
	}
```

## 모델이 추가된다면?
- Model.java제작해서 JSP에 값을 넣어 보내준다. 
- jsp(View)에서 Model이 넘겨준 값을 받는다
- Controller에 추가된 Model 정보를 작성한다.

```java
	String[] strCmd= {
			"list",
			"detail",
			"insert",
			"update"
	};
	String[] strClass= {
			"com.sist.model.ListModel",
			"com.sist.model.DetailModel",
			"com.sist.model.InsertModel",
			"com.sist.model.UpdateModel"
	};
```

## Controller.java 구동하기
- 설정한 것과 같이 URL 맨 뒤에 `.do`가 붙어서 실행된다.
- jsp 파일에서 구동하면 URL 맨 뒤에 `.jsp`가 붙어서 실행된다. 
