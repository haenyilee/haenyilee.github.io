# 20201202-newsAPI

## 개요
- 사용기술 : redux , hooks

## API 키 얻기
- news api
- key : b90ec673d2724353a8cd018741bf1c4b

## Spring

### ReactController.java
- `@RestController`
- react는 json 밖에 받지 못함
  - arraylist로 보내면 안됨
  
- 한글이 깨지지 않도록 변환해야함

- chef_list : page 요청값을 받아서 [{},{},{}] 형태의 json으로 값을 받기

## React

- create-react-app 에서 라이브러리가 깔린 부분으로 선택해야함


### React가 강력한 이유
- 리액트는 가상dom

![리액트의 실행 과정](https://cdn-images-1.medium.com/max/1600/1*48mwTh2nPA-_owlgwFK6Ew.png)

- HTML이 늦어지는 이유는?
  - 새로 고침을 하게되면 아래 과정을 다 거쳐서 Display를 띄운다.
  - 리액트는 이 과정을 가상으로 실행하여 Display를 띄우기 때문에 빠르다.
  
![html의 실행과정](https://www.hanumoka.net/images/20180815-web-virtual-dom_1.png)  
  

### package.json

```json
"react-router": "^5.2.0",
"react-router-dom": "^5.2.0",
"axios": "^0.21.0"
```

- 위의 코드 추가하고 run npm install 실행하기

### App.js
- `model.addAttribute("main_jsp","파일명")` 이 코딩을 대체하는 것이 Route임

### ChefList.js
- react에서 요청하면 Spring에서 요청을 처리하고 React(클래스 전체, JS 전체에서 사용 : state에 사용)로 데이터 전송
- 페이지 변경  : `setState()` (데이터 바뀌고)  => `render()` (html 호출) => `componentDidMount()` (화면에 출력)

```NOTE
**react의 함수**
- `constructor()` : 클래스 생성 
- `componentWillMount()` : 메모리 저장 전 => 데이터 받기용
- `render()` : html 만들고 메모리에 올리기 => 메모리 올리기용
- `componentDidMount()` : 메모리에 있는 내용을 브라우저에 출력 => 출력용 
- `setState()` : state의 함수
```

- 한글 변환해서 받는 방법


## Spring

### RecipeDAO

```java
public int chefTotalPage()
{
	BasicQuety query= new BasicQuery("{}");
	List<ChefVO> list = mt.find(query,ChefVO.class,"chef");
	int count=list.size();
	int total=(int) (Math.ceil(count/20.0));
	return total;
}
```

### ReactController

```java
@RequestMapping("react_chef/totalpage.do")
public String chef_total()
{
	int total=0;
	try{
		total=dao.chefTotalPage();
	}
	catch(Exception ex){
	}
	return String.valueOf(total);
}
```

## React

### ChefList
- NavLink : a태그와 같은 기능이다.

- post에는 render 호출이 포함되어 있다. 
  - 그래서 this.setstate를 설정하게 되면 render가 두 번 호출되기 때문에 화면 변경이 두 번 이뤄지게 된다.

```js
prev()
    {
        this.state.page=this.state.page>1?this.state.page-1:this.state.page;
        this.post();
    }
```

### ChefDetail
- 다른 서버를 연결해서 데이터를 가져오는 법 : axios , fetch , request 
  - async, await
- setstate가 있어야 render를 새로 호출할 수 있다.
  - 요청값을 변경해서 페이지를 변경해줄 때 사용함
  
![리액트 데이터 갱신](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FAKb5w%2FbtqCUcQ5U36%2FZgc2EzbPHuZ8en320sPMF1%2Fimg.jpg)

- state를 갱신하면 화면이 바뀜
  - render가 호출되지 않으면 데이터가 아무리 바뀌어도 화면이 갱신되지 않음
 
![](https://user-images.githubusercontent.com/6733004/45586846-2a131180-b938-11e8-9655-c9dd20f74ef5.png)

- `constructor` : 생성자는 변수선언과 이벤트 등록밖에 안함, 값을 받는 역할은 수행하지 않음, 필요한 멤버변수 설정만 한다.

- `componentWillMount()` : 출력할 데이터를 저장해둠

- `render` : 저장된 데이터를 출력함

- `post` : 페이지 나눌 경우 디폴트 페이지를 설정하기 위해 사용함

- `componentWillMount` , `render`는 자동호출되는 함수이다.

- `Fragment` : 임시용 root를 만들때 사용함

- `this.findBtnClick=this.findBtnClick.bind(this)` : 이벤트 등록
- `e.target.value` : 텍스트에서 입력한 값 받기


## Spring

### ReactController.java
- chef_find

## 배포하기

## React

### webpack

### package.json


## spring

### webapp
- root 경로에 recipe 폴더 만들어서 chef.js와 img 파일 첨부하기

### chef_list.jsp
- 리액트로 출력할 부분 주석 처리 후, 스크립트 추가하기

```jsp
<div class="container" id="root">
        <%-- <div class="row">
          <c:forEach var="vo" items="${list }">
           <table class="table">
             <tr>
               <td width=35% class="text-center" rowspan="2">
                 <a href="../recipe/chef_product.do?chef=${vo.chef }"><img src="${vo.poster }" width=180 height=80></a>
               </td>
               <td colspan="4"><font color="orange"><a href="../recipe/chef_product.do?chef=${vo.chef }">${vo.chef }</a></font></td>
             </tr>
             <tr>
               <td class="text-center">${vo.mc1 }</td>
               <td class="text-center">${vo.mc3 }</td>
               <td class="text-center">${vo.mc7 }</td>
               <td class="text-center">${vo.mc2 }</td>
             </tr>
           </table>
          </c:forEach>
</div> --%>
        
      <!-- 리액트로 출력하는 부분 -->
      <script src="..chef.js"></script>
      </div>
```
