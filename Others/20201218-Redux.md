# 20201218-Redux

## Redux란?

### WebStrom 삭제 방법
- uninstall에서 체크버튼 두 개 선택해서 삭제하고 다시 깔면 다시 30일 무료 이용 가능함


### recipeReducer.js
- import : FETCH_NEWS_LIST

- 저장하기

```react
const initailState={
  recipe:[],
  chef:[],
  news:[]
}
```

- 처리 : dispatch로 받은 데이터를 state에 저장
  - 스코프 연산자 => 배열에 있는 모든 데이터를 복사할 경우에 사용
  
```react
export default function (state = initalState , action){
  switch (action.type){
    case FETCH_RECIPE_LIST:
      return {
        ...state,
        recipe: action.payload
      }
     default:
      return state
   }
}
```

### index.js
- import
- export

