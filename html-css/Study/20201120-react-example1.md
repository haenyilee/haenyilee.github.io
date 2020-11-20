# 20201120-react-example1

## webstorm
- react app 프로젝트 생성

### package.json

- 라이브러리  등록하는 파일
- `"axios": "^0.21.0"` 추가

### app.js ,  index.xml
- src폴더에 app.js `<div></div>` 코드가 public폴더에 index.xml 내 `<div id="root"></div>`에 들어가게 됨
- index.js에서 app.js을 호출해서 root에 넣으라고 명령을 내려줌
- index.js가 main임
- 함수 호출해서 리턴값 받기

```react
ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

```javascript
let html=App();
$('#root').html(html)
```

- src 폴더내에 javascript 파일 생성 : 제목 - app2

###

1. 생성 : constructor()
  - 변수 선언
    - props : 변경이 안되는 변수
    - state : 변경이 되는 변수
    - ref : input을 참조할 때 사용
  - 이벤트 등록할 때 사용한다.
2. 렌더 전 : componentWillMount()
  - 외부 서버에서 데이터를 가지고 올 때 사용한다.(node.js,spring)
3. 렌더 : render() => 실행할 데이터를 모아서 html에 출력
4. 렌더 후 => 브라우저 실행
  - componentDidMount() => window.onload
    - `$function(){}`과 같음
    - jquery 연동할 때 많이 사용한다.
    - 제어를 해주는 부분
  
### JSX형식 => JavaScript + XML
- 계층형 구조로 만들어야 한다.
