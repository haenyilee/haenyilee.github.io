---
sort: 2
---

# application객체

- ServletContext의 객체명
- 서버와 관련된 서버에 대한 정보를 가지고 있음

#### 1. 서버 관련 메소드
- ```getServerInfo()``` : 서버의 이름을 가지고 올때 사용함
- ```getMajorVersion()``` : Dynamic web version 3.0 의 "3"이 Major
- ```getMinorVersion()``` : Dynamic web version 3.0 의 "0"이 Minor

#### 2. 디렉토리에 대한 정보를 가지고 있음 
- ```getRealPath()``` = 톰캣이 읽어가는 실제 경로명
- [C:\webDev\20200921-jsp2...]은 프로그래머가 편집을 가능하게 해주는 가상 주소임

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
- 콘솔에 출력됨
