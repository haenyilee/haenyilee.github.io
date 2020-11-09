---
sort:
---

# 영화 입장권 통합전산망 메인페이지 만들기

## 데이터 파싱

### Model : MovieController.java

- JSP에서 HTML과 Java가 합쳐져 있던 것을 분리하여 Java부분만 별도로 코딩한 부분이 바로 Model이다.
  - HTML은 View에 해당된다.
- Controller는 스프링 안에서 이미 만들어져 있다.
- 메뉴얼을 만들 때 사용하는 것이 XML이다.
- 이 클래스는 모델 역할이라고 스프링에 알려주는 것이 바로 `@Controller` 어노테이션이다.


### movie.json
- http://www.kobis.or.kr/kobis/business/main/searchMainDailyBoxOffice.do 에서 복사해온 데이터 임시 저장용 파일이다.


### MovieVO.java

### MovieManager.java
- 일반 클래스일 경우에 `@Component` 로 메모리 할당한다.

- `[]` : JSONArray
- `{}` : JSONObject => javascript 객체
- JSON 파싱 방법
  - 자바 파싱
  - 자바스크립트 파싱
  
### MovieController.java
- 매개변수 no는 int보다 String으로 받는 것이 좋다.

### application-context.xml
- ViewResolver(경로명, 확장자)
  - 데이터를 JSP로 전송


### main.jsp

### index.jsp
