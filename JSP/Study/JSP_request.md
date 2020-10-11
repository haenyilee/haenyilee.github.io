---
sort: 2
---

# WAS

## 1. 톰캣의 두 가지 기능

![](https://gmlwjd9405.github.io/images/web/webserver-vs-was1.png)

1. 웹서버 : 사용자의 요청을 받는 기능
2. 컨테이너 : 번역해주는 기능 
- request : map형식(key,값)으로 찾아서 보내줌 

### 1.1 톰캣 기능 예시 (1)
1. 사용자 : URL localhost/main/main.jsp 요청
2. 웹서버 : 사용자로부터 요청 받기
3. 컨테이너
    - main.jsp를 html로 변환
    - JSP , Servlet 실행
    - html만 출력
4. 웹서버 : 사용자에게 응답 보내기
5. 브라우저 : 화면 출력

### 1.2 톰캣 기능 예시(2)
1. 사용자 : main.jsp?page=1을 요청
2. 톰캣 
    - main.jsp파일에서 page가 1인 애들을 묶음 : ```request.setAttribute("page",1)```
    - 묶은 애들을 보내줌 : ```request.getParameter("page")```


## 2. JSP
- 자바와 HTML을 동시에 작성할 수 있게 함
- HTML코드는 서블릿에서 ```public doGet(){}``` 메소드 안에 작성하는 것과 같음
- 그래서 모든 변수는 메소드 내에 있는 것이기 때문에 지역변수임
- 클래스가 아닌 메소드에 작성하고 있음을 주의해야함

### 2.1 JSP 전송방법
1. ```<a href="">``` : 1~2개씩만 전송
  - get방식 : URL 뒤에 보이게
2. ```<form action="">``` : 한번에 전송
  - get방식 : URL 뒤에 보이게
  - post방식 : URL 뒤에 안보이게
  - form태그 액션 수행하는 버튼들(데이터 전송하는 버튼들)
     1. ```<input type=submit>```
     2. ```<button>```
     3. ```<input type=image>```

  - 파라미터 읽기
