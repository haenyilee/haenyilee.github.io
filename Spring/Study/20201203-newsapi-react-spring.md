# 20201203-newsapi-react-spring

## react

### App.js

- function 단위 state를 사용이 가능하게 만드는방법(react-hooks) : `const  [chef,setChef]=useState([]);`
  - 요즘은 class단위보다 hooks를 사용하는 것을 권장함
  - [chef,setChef] : hef를 생성함과 동시에 set메소드로 값을 채워줌
  - state는 자동호출되게 해주는 함수이다.
  
```js
function App() {
  const  [chef,setChef]=useState([]);
  const  [page,setPage]=useState(1);
  const  [totalpage,setTotalpage]=useState(0);
}
```


- useEffect : componentWillMount()를 대체함

```js
// 데이터 읽기
    useEffect(()=>{
      
    },[])
```

## openApi
- news api : https://newsapi.org/
- key : b90ec673d2724353a8cd018741bf1c4b

## spring
