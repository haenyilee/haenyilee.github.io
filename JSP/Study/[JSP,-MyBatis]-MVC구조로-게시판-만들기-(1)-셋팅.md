---
sort: 2
---

# MVC구조로 게시판 만들기 (1) 셋팅

## 1. 공통모듈 제작하기

### [Model] 인터페이스

```java
package com.sist.model;

import javax.servlet.http.HttpServletRequest;

public interface Model {
	
	public String handlerRequest(HttpServletRequest request);

}
```


- 요청 처리를 위해서는 확장(재사용)이 필요하기 때문에 일반 자바로 제작한다.
- 모든 기능의 Model이 사용할 인터페이스 파일을 제작한다.
- `HttpServletRequest` : 톰캣 메소드

```note
#### 매개변수 전송 방식
- Call by Value : 다른 메모리에 값을 복사
- Call by Reference : 메모리 주소를 복사해서 원본 데이터가 공유됨
```



### [applicationContext.xml]

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- applicationContext.xml
     사용자 요청 => list.do 
           => ListModel클래스를 찾을 수 있게 만든다 (매칭)
           
   Model클래스 등록 => Controller가 찾아서 요청 처리가 가능하게 만드는 파일
 -->
<beans><!-- 스프링 Bean(Model클래스) -->
  <bean id="list.do" class="com.sist.model.ListModel"/>
  <bean id="detail.do" class="com.sist.model.DetailModel"/>
  <bean id="insert.do" class="com.sist.model.InsertModel"/>
  <bean id="insert_ok.do" class="com.sist.model.InsertOkModel"/>
  <!-- 
         map.put("list",new ListModel())
   -->
</beans>
```

- Model클래스를 등록하고, Controller가 찾아서 요청 처리가 가능하게 만드는 파일이다.
  - list.do와 ListModel클래스를 찾을 수 있게 만드는 매칭 기능을 담당한다.

- 자바로 구현했을 시, 아래 코딩과 같다.

```java
map.put("list",new ListModel())
```






### [web.xml]

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>20201012-MVC1</display-name>
  <servlet>
  	<servlet-name>mvc</servlet-name>
  	<servlet-class>com.sist.controller.Controller</servlet-class>
  </servlet>
  <init-param>
  	<param-name>contextConfigLocation</param-name>
  	<param-value>C:\webDev\20201012-MVC1\WebContent\WEB-INF\applicationContext.xml</param-value>
  </init-param>
  <servlet-mapping>
  	<servlet-name>mvc</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
</web-app>
```

### [Controller] 제작
- 서블릿으로 제작한다.
- Controller를 수정하기 위해서 소스를 수정하면 전체 사이트를 구동할 수 없게 된다. 
  - 이러한 단점을 보완하기 위해 파일을 이용한다.(properties,xml)
- Controllers는 모델과 뷰를 매칭하는 역할을 수행한다.
  - 연결해야 하는 모델과 뷰가 무엇인지 알고 있다.
  - list.do라는 요청이 들어오면 ListModel을 찾고 list.jsp를 전송해준다.


- **서블릿에서 사용하는 메서드**
![서블릿의 라이프사이클](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwsTd6%2FbtqBXUJ4fM2%2FtIM6vSTDW7OaakgwgHuuck%2Fimg.png)
- 1. `init()` : 메모리할당(생성자 호출)이 되면, 파일 읽기와 서버연결을 담당하는 메소드이다.
    - xml데이터를 읽어서 map에 저장한다.
    - 프로그래머가 호출하는 것이 아닌 시스템에 의해서 자동 호출되는 Callback함수이다.

```note
#### Callback함수
대표적인 Callback함수는 `main()`이다.
자바에서는 Callback함수을 프로그래머가 만드는 기능이 없기 때문에, 반드시 호출을 해줘야 한다.
```

```java
	public void init(ServletConfig config) throws ServletException {
		
	}
```



  - 2. `service()` : 사용자가 요청이 들어오면, 처리하는 메소드이다.
    - doGet과 doPost의 내용이 동일하기 때문에, 둘을 동시에 처리하는 service메소드를 사용한다.
    - 프로그래머가 호출하는 것이 아닌 시스템에 의해서 자동 호출되는 Callback함수이다.

```java
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
	}

```


  - 3. `destroy()` : 할당된 메모리를 회수하는 메소드이다.
    - 새로고침과 화면이동을 하면 destroy 메소드가 호출되어 메모리가 회수된다.
    - 다시 원상복귀되는 것은 기존 메모리가 호출되는 것(new)이 아닌 새로운 서블릿과 JSP를 생성하는 것이다.
    - 프로그래머가 호출하는 것이 아닌 시스템에 의해서 자동 호출되는 Callback함수이다.



-  **Controller의 기능**
  - 1. 요청을 받는다.

```java
String cmd=request.getRequestURI();
// cmd : (list).do
```


  - 2. 모델을 찾는다.

```java
List Model
```

  - 3. 모델에서 넘겨준 결과값을 request, session에 담는다.

```java
request.setAttribute()
```

  - 4. JSP를 찾아서 request, session을 넘겨준다.

```java
forward(request,response)
```

- **xml파싱해서 읽는 과정** : `init`이 담당한다.

- 1. Model과 View정보 담을 HashMap 생성
  - 객체 `clsMap`에는 Model과 View정보가 키, 값 형식으로 담겨있다.

```java
private Map clsMap = new HashMap();
```

- 2. web.xml에 접근해서 데이터 읽기
  - `init`의 매개변수인 `ServletConfig config`에서 `ServletConfig`는 web.xml에 설정된 데이터에 접근하는 인터페이스이다.


```java
String path=config.getInitParameter("contextConfigLocation");
```


- 3. xml읽기

```java
DocumentBuilderFactory dbf=DocumentBuilderFactory.newInstance();
```



- 4. xml파싱기 역할을 담당하는 클래스 생성

```java
DocumentBuilder db=dbf.newDocumentBuilder();
```

- 5. xml 읽어서 메모리에 저장하기 (저장공간 :Document)

```java
Document doc=db.parse(new File(path));
```

- 6. 최상위 태그 읽어서 트리 형태로 저장하기
  - applicationContext.xml에 저장된 `<beans>`
```java
Element root=doc.getDocumentElement();
```

- 7. 같은 태그를 묶을 때 NodeList 를 사용한다.

```java
NodeList list=root.getElementsByTagName("bean");
```

  - 8. for문 돌려서 등록한 bean 태그 정보 전부 읽어오기

```java
			for(int i=0; i<list.getLength();i++)
			{
				Element bean = (Element)list.item(i);
			}
```

  - 9. 파싱한 클래스를 메모리 할당 한 다음에 키, 주소 주기

```java
			for(int i=0; i<list.getLength();i++)
			{
				Element bean = (Element)list.item(i);
				String cmd=bean.getAttribute("id");
				String cls=bean.getAttribute("class");
				Class clsName=Class.forName(cls);
				Object obj = clsName.newInstance();
			}
```


  - 10. map으로 저장하기
```java
			for(int i=0; i<list.getLength();i++)
			{
				Element bean = (Element)list.item(i);
				String cmd=bean.getAttribute("id");
				String cls=bean.getAttribute("class");
				Class clsName=Class.forName(cls);
				Object obj = clsName.newInstance();

				clsMap.put(cmd,obj);
			}
```


- **사용자 요청 시, 처리되는 과정** : `service`가 담당한다.
- 1. 사용자 요청을 받기

```java
String cmd=request.getRequestURI();
```

  - 2. Model클래스 받기 : clsMap에 저장된 클래스를 찾아주기
    - ContextPath : `http://localhost/20201012-MVC1/*.do` 에서 `http://localhost/20201012-MVC1` 부분

```java
cmd=cmd.substring(request.getContextPath().length()+1);
Model model=(Model)clsMap.get(cmd);
```

  - 3. Model을 통해서 요청 처리하기 >> 결과값을  request,session에 담기

```java
String jsp=model.handlerRequest(request);
```

  - 4. JSP파일 찾기

  - 5. JSP로 request 전송하기
    - RequestDispatcher : request를 해당 JSP로 전송하는 클래스

```note
### 화면을 이동하는 방법
1. sendredirect : request가 초기화되는 상태이다.

`return "redirect:list.do";`

2. forward : request를 전송하기 때문에, jsp에서 request에 담은 데이터를 받아서 출력할 수 있다.

`return "board/list.jsp";`

```




```java
		if(jsp.startsWith("redirect"))
		{
			response.sendRedirect(jsp.substring(jsp.indexOf(":")+1));
		}
		else
		{
			RequestDispatcher rd=request.getRequestDispatcher(jsp);
			rd.forward(request, response);
		}
```



### [View] 제작
- Model과 매칭되는 JSP파일이다.
- 요청 처리된 결과값을 출력한다.

### [VO]

```JAVA
package com.sist.dao;

import java.util.*;

public class BoardVO {
	private int no;
	private String name;
	private String subject;
	private String content;
	private String pwd;
	private Date regdate;
	private int hit;
	public int getNo() {
		return no;
	}
	public void setNo(int no) {
		this.no = no;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getSubject() {
		return subject;
	}
	public void setSubject(String subject) {
		this.subject = subject;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}
	public String getPwd() {
		return pwd;
	}
	public void setPwd(String pwd) {
		this.pwd = pwd;
	}
	public Date getRegdate() {
		return regdate;
	}
	public void setRegdate(Date regdate) {
		this.regdate = regdate;
	}
	public int getHit() {
		return hit;
	}
	public void setHit(int hit) {
		this.hit = hit;
	}
	
}
```

### [Config.xml]

```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
   PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
   "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <properties resource="db.properties"/>
  <typeAliases>
    <typeAlias type="com.sist.dao.BoardVO" alias="BoardVO"/>
    
  </typeAliases>
  <environments default="development">
    <environment id="development">
     
       <transactionManager type="JDBC"/>
       <dataSource type="POOLED">
          
           <property name="driver" value="${driver}"/>
           <property name="url" value="${url}"/>
           <property name="username" value="${username}"/>
           <property name="password" value="${password}"/>
          
       </dataSource>
    </environment>
  </environments>

  <mappers>
    <mapper resource="com/sist/dao/board-mapper.xml"/>
  </mappers>
</configuration>
```

### [db.properties]

```SCCS
driver=oracle.jdbc.driver.OracleDriver
url=jdbc:oracle:thin:@211.238.142.183:1521:XE
username=hr
password=happy
```


### [mapper.xml (Mybatis)]

```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.sist.dao.board-mapper">
</mapper>
```

### [DAO]

- 파서기
  - SqlSessionFactory : 

```java
private static SqlSessionFactory ssf;
```


```note
### 초기화 블록
- static{} : static
- {} : instance
```

```tip
- Mybatis는 예외처리 안해도 컴파일 오류가 아니다. 
- IO는 파일경로 , 파일명이 틀릴 경우에 컴파일 에러이기 때문에 반드시 예외처리를 해야한다.
```


- Config.xml 파일 읽어오기

```java
Reader reader= Resources.getResourceAsReader("Config.xml");
```

```note
- classpath : 자동으로 인식하는 경로
- Mybatis와 Spring의 classpath는 src이다.
```

- xml 파싱하기
  - SqlSessionFactoryBuilder : 





## 2. 처음 실행하는 화면 설정하기

### View
- index.jsp 만들기

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

</body>
</html>
```

- list.do로 이동하는 스크립트 작성하기

```jsp
<script type="text/javascript">
location.href="list.do";
</script>
```

- 자바 코딩으로 구현하면 아래와 같음

```jsp
<%
	response.sendRedirect("list.do");

%> 
```

### web.xml
- welcome-file에 index.jsp 등록하기
  - `<welcome-file>` : url 주소에 파일명 없이 자동 실행 가능하게 만들어 주는 태그이다.
  - 프로젝트를 실행하자마자 출력되는 페이지를 등록하려면 아래 태그에 원하는 파일명을 추가해주면 된다.

```jsp
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
    <welcome-file>main.jsp</welcome-file>
  </welcome-file-list>
```
