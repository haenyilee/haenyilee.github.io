---
sort: 2
---

# request VS response

#### 1. request(내장객체 => 미리 생성된 객체)
= HttpServletRequest request
= 주요기능
1) 브라우저의 정보
- 사용자의 IP
- 사용자 포트번호 

#### 2. 사용자 요청 정보 : 사용자가 보낸 모든 값을 받을 수 있는 기능
- 단일값 : ```getParameter("보낸변수명");```
```
list.jsp?page=1 => getParameter(""page");
<input type=text name=no> => getParameter("no")
```


- 다중값 : checkbox , select
=> ```getParameterValues()```


- 한글처리 : 한글은 2바이트씩 읽어오기 때문에 인코딩과 디코딩의 과정을 거쳐야함
=> ```setCharacterEncoding("UTF-8")```


