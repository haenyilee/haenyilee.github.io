---
sort: 2
---

# JSP 내장객체

## request : 사용자가 보내준 데이터를 받는 경우 

- ```getParameter()``` : 단일 값을 받을 경우 사용함

- ```getParameterValues()``` : 다중 값을 받을 경우에 사용함
  - 주로 체크박스에서 많이 사용한다.

- ```setCharacterEncoding()``` : 디코딩할 경우 주로 사용함
S
## response : 서버에서 화면 이동할 때 
- ```setHeader()``` 
  - 실제 데이터를 보내기 전에 전송하는 데이터를 Header라고 한다.
  - header에는 IP나 Method방식이 담긴다.
  - 주로 다운로드 시에 보낸다.

- ```sendRedirect()``` : 서버에서 사용자가 요청한 파일로 이동할 때 사용한다.
  - 무조건 get방식을 이용한다.
  - 데이터를 보낼 때 항상 ?를 써야한다.

## out
- ```println()``` : 화면에 출력할 때 사용한다.
  - ```<%= %>```
- 기본 메모리 크기를 8kb를 가지고 있다.

## pageContext
- ```include()``` : 특정부분에 다른 jsp를 포함할 때 사용한다.
  - ```<jsp:include>```와 같다.

- ```forward()``` : request를 잃지 않게 해줄때 사용한다.
  - 모든 JSP는 request를 따로 가지고 있다. 
  - 화면이 변경되면 request가 초기화가 되는데, 기존에 보냈던 모든데이터를 잃게 된다.
  - 이때, 기존 데이터를 잃지 않게 만드는 방식이 바로 forward이다.
  - MVC나 spring에서 자주 사용하는 기법이다.

## application

- ServletContext의 객체명
- 서버와 관련된 서버에 대한 정보를 가지고 있음

#### 1. 서버 관련 메소드
- ```getServerInfo()``` : 서버의 이름을 가지고 올때 사용함
- ```getMajorVersion()``` : Dynamic web version 3.0 의 "3"이 Major
- ```getMinorVersion()``` : Dynamic web version 3.0 의 "0"이 Minor

#### 2. 디렉토리에 대한 정보를 가지고 있음 
- ```getRealPath()``` : 실제 톰캣이 읽어가는 경로명을 출력할 때 사용한다.
- [C:\webDev\20200921-jsp2...] 형식의 경로 프로그래머가 편집을 가능하게 해주는 가상 주소이다.
- 모든 그림파일이 여기에 존재하게 된다.


#### 3. 초기화 관련 파라미터
- 서버 파일을 읽을 수 있는 권한을 가지고 있음(web.xml) : 서블릿 등록 , 에러 등록
- ```getInitParameter()``` : web.xml에 등록된 내용을 읽어오기
  
  ```xml
  <!-- web.xml에 저장되어 있는 내용 --> 
  <context-param>
      <param-name>driver</param-name>
      <param-value>oracle.jdbc.driver.OracleDriver</param-value>
  </context-param>
  ```
#### 4. 로그에 대한 정보를 가지고 있음
- ```log()```
-  콘솔에 출력됨
- 모든 서버에서 로그파일 기록



## session VS cookie
- cookie는 내장객체가 아님
- 로그인 , 장바구니 , 예매하기




## config , page , exception
