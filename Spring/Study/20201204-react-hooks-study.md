# 20201204-react-hooks-study


# spring

### ReactController
- 리액트는 View의 역할밖에 수행하지 못한다.
- 리액트 : 데이터 관리 프로그램이라고 한다(state)
- 서버 프로그램 : Spring , NodeJS
- 두 서버 프로그램이 충돌되면 안된다.
- NodeJS는 Chat이나 실시간 데이터 전송을 할 때 주로 사용한다.
- Spring은 데이터를 보낼 때 사용한다.
- 사용자가 볼 수 있는 것 : Front
- 사용자가 볼 수 없는 것 : Back


# react


### React 사용형식

- React는 Javascript기반이기때문에 객체지향 프로그램이다.
  - `let a={name:"",sex:""}=>{} Object`
  - `a.name` , `a.sex` => JSON (자바스크립트에서 객체를 사용할 때 쓰는 표현)

- 리액트는 두 가지 형식을 가지고 있다.
  - class기반
    - 멤버변수
  - function기반
    - 지역변수 
    - 전역변수의 개념이 없기 때문에 멤버변수 처럼 사용이 가능한 변수가 State이다. (Hooks)
  
- function기반
  - useState : 멤버변수 역할
    - `render()` : 화면 출력
    - `useEffect(componentWillMount)` : 화면 출력 전에 데이터 사용할 때 사용
    - `useMemo` , `useCallback` , `useReduce` , `useDispatch`
    - `useReduce` , `useDispatch`만을 가지고 Redux를 만들어서 MVC 구조를 구축할 수 있다.
    - `Context-API` : 전역에서 사용할 수 있는 변수를 만들 때 사용함
    
- class기반
  - costructor => componentWillMount => render => componentDidMount
  - render : 변경시에 `render()` 함수가 호출되어야 html이 변경된다.
  - 이벤트가 발생 (버튼, 검색어 입력) 했을 때 새로운 데이터를 가져오게 되는데, 
  이때 `render()`에서 새로운 데이터에 해당하는 html을 변경해야한다.
  - 사용자가 직접 `render()`를 호출할 수 없기 때문에 이벤트 발생 시(=데이터 변경 시) `render()` 호출이 가능하게 만든 것이 `setState`이다.
  
  - function단위에서도 render를 호출 할 수 있도록 return값을 받은 후에 화면이 변경된다.
  - 하지만 function에는 setState가 존재하지 않는다. 
  - 그래서 const [recipe,setRecipe]를 만들어두고 setRecipe라는 함수를 호출하면 자동으로 return이 다시 수행된다. 
  - 이렇게 만든 것을 hooks라고 한다.
  - Hooks는 화면이 갱신되지 않는다.
  
  


### App.js


#### 값 받아오기

#### 출력하기

#### 이벤트 처리

    
