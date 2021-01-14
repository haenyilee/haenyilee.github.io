---
comments: true
---

# React로 Recipe 출력하기

- 20201117-AOPstudy3 프로젝트에 제작함

## React
- 배포를 하면 소스가 없는데 동작을 하는 프로그램?
  - 보안이 뛰어나다.
- 17버전으로 올라가면서 react hook로 바뀜
  - function단위로 만드는 것이 hook이다.
  - 이전 버전에서는 function이다.
- MVC 구조도 적용할 수 있어야 한다.

## RecipeVO

## interface RecipeMapper
- recipeListData
- recipeTotalPage
- @Controller : 화면 전환 용 컨트롤러이다.
- @RestController : 데이터를 읽어오는 프로그램이다. 다른 프로그램에 데이터 전송 가능하다.
  - 모든 언어에 포함되는 파일 형식이 JSON, XML이다.
  - 파싱하기 더 용이한 형식은 JSON이다.

## application-context.xml
- recipemapper 추가 등록
  - 매퍼를 패키지 단위로 등록하면 일일히 추가할 필요가 없다.
 
 
## RecipeDAO
- @Repository
- @Autowired

## ReactController
- @RestController
- @Autowired
- @RequestMapping("recipe/list.do")

- JSONArray arr = new JSONArray();  
  - []을 만들어 준다.
- JSONObject obj = new JSONObject();
  - {}을 만들어 준다.
  
## webstorm
- jetbranin - bin - webstorm64
- React 편집기
- react app
- ~\appdate로 변경
- 이름 변경
- create
- thiswindow

## public - index.html

```
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<div class="container-fluid" id="root">
  <div class=
</div>
```

## src - app.js
- import : 라이브러리 읽어오기
- componentDidMount : SPRING으로부터 데이터를 받아오기
  - state : 값이 갱신될때 받아오기
- render : html 출력
- `{m.poster}` : 값 받아올 떄 형식

## 실행
- 포트번호 바꾸기 : 스프링 - ReactController - @CrossOrigin("http://localhost:3000")
- terminal => npm start
 
