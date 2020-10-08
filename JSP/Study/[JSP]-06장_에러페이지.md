## error처리 방법
1. 에러 페이지로 이동시키기
- 모든 에러가 같은 파일을 이용해서 처리된다.
- ```<%@ page errorPage="error.jsp"%>```
- 지정된 파일이 없는 경우에는 서버자체에서 처리하는 에러에 대한 파일이 전송된다.

2. 종류별로 처리하기
- web.xml 파일을 여러개 만들어서 오류코드(500, 404)별로 다르게 처리한다.
- ```<welcome-file-list>```위에 작성한다.
```xml
<error-page>
  	<error-code>404</error-code>
  	<location>/jsp/error404.jsp</location> <!-- error404.jsp 파일은 미리 만들어둬야 한다. -->
  </error-page>
  <error-page>
  	<error-code>500</error-code>
  	<location>/jsp/error500.jsp</location> <!-- error500.jsp 파일은 미리 만들어둬야 한다. -->
  </error-page>
```

## 에러의 종류

- 404 : 파일이 존재하지 않는 경우
- 500 : 소스에 컴파일, 인터프리터에러
- 415 : 한글 변환 코드 오류
- 400 : 
