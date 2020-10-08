---
sort: 2
---

# request : 사용자가 보내준 데이터를 받는 경우 

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
- ```getInitParameter()``` : web.xml을 읽을 경우 사용함

- ```log()``` : 모든 서버에서 로그파일 기록

- ```getRealPath()``` : 실제 톰캣이 읽어가는 경로명을 출력할 때 사용한다.
  - 모든 그림파일이 여기에 존재하게 된다.



## session VS cookie
- cookie는 내장객체가 아님
- 로그인 , 장바구니 , 예매하기




## config , page , exception
