---
sort: 2
---

# JSP 구동방식

- 톰캣이 JSP를 .java로 변환함 
- 컴파일해서 .class로 변환
- 한줄씩 읽어서 출력함
- 한줄 변역
- 메모리 내용(HTML)
- 브라우저에서 읽어서 출력

# JSP에서 자바 출력
- <% 일반자바 %>
- <%= 출력(변수,데이터) %>
- 

# Directive
#### 1. page : JSP페이지에 대한 정보를 지정
- 문서타입 : ```contentType```
- 사용할 클래스 : ```import``` : 라이브러리 추가할 때 사용함
- 에러페이지 정보 지정 : ```errorPage```
#### 2. include : 다른 문서를 포함
#### 3. taglib : 태그 라이브러리를 지정

# 스크립트요소

# 스크립트릿
```<%= %>```

```<%  %>```

# 내장객체(implicit object)
- request
- response
- out
- session
- application

# 액션태그
## include 
- 조립식 프로그램이 시작됨
- 여러개 파일을 나눠서 하나에 묶어서 처리하는 프로그램 설계를 위해 나온 것이 include임
- 스프링에서 사용됨

## useBean

## setProperty

