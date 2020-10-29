---
sort:
---

# Model2 방식으로 웹사이트 만들기 (3) 영화 상세보기

## 영화 상세 정보

### total.jsp

- no값 넘겨주기

```note
**쿠키 저장 및 전송**
- 책 : 206page
- html을 보냄과 동시에 쿠키를 response에 담아서 함께 넘길 수 없음
- response는 한 가지 씩밖에 전송을 못함 
session/cookie/response / request.

- 쿠키는 클라이언트 브라우저에 저장됨 저장 내용을 서버에 전송해야함
- 세션 : 서버에 저장 용도임 , 저장을 받지 않기 때문에 response를 이용하지 않음
- 쿠키를 저장하고 클라이언트 컴퓨터에 저장해야하기 떄문에 response를 이용함
  - 파일을 올리면 서버에 저장됨
  - 쿠키도 서버에 저장됨
  - 내 컴퓨터에 저장하려면 서버에 요청받아야함
  - 보내줄때 쓰는게 response임
    - 여러번 응답 불가함
    - 하나의 jsp에서 request는 하나씩밖에 못받음
    - 한번에 처리하려면 ajax를 써야함
```

### DispatcherServlet.java

- Response를 받을 수 있도록 설정하기
  - 현재는 obj와 request만 매개 변수로 잡혀 있기 때문이다.
  
```tip
**response가 필요한 경우**
1. cookie
2. upload
```

```java
for(Method m:methods)
				{
					RequestMapping rm=m.getAnnotation(RequestMapping.class);
						String jsp="";
						if(rm.value().equals("/movie/detail_before.do"))
						{
							if(cmd.equals(rm.value()))
							{
                // response를 받아와라
								jsp=(String)m.invoke(obj, request,response);
							}
							else
							{
								jsp=(String)m.invoke(obj, request);
							}
						}
				}
```

### MovieModel.java
- 세션에저장할떄도 요청을 2번할 때redirect를보내줘야 함
 - .jsp를 보내면 해당 화면만 보내주게되니까
 - .dof를 보내야함
- uri에서 물음표 뒤에 부분은 request에 보내주는 부분이기 떄문에 .do까지만 보내고 ?뒤에 내용은 request에 담아서 보내주면 되나?


- 서버에는 쿠키 저장하는 공간이 하나라서, 키 명을 id를 줘서 각 사용자마다 다른 쿠키를 저장한다?
  - 키 값은 중복될 수 없기 때문에 `id+영화no`의 조합으로 만들어야 한다.

- 쿠키는 여러개 동시에 저장할 수 없음
  - 하나 생성+전송, 생성+전송
  - 쿠키저장시간은 기본 30분임
  
- 매개변수에 response 추가하기
  - HttpServletResponse에는 ip 주소 받는 등의 처리가 다 된 상태의 값이 담겨 있다. (웹서버 담당)
  
- detail화면으로 이동하는 클래스 작성


### [mapper]

- movieDetailData를 재사용

### [DAO]


### [Model]

- movie/total.do 요청 들어왔을 때 


### total.jsp
- 최근 본 영화 출력하기

```jsp
   <c:forEach var="vo" items="${list }">
      <div class="col-md-2">
	    <div class="thumbnail">
	      <a href="#">
	        <img src="${vo.poster }" alt="Lights" style="width:100%">
	        <div class="caption">
	          <p>${vo.title }</p>
	        </div>
	      </a>
       </div>
     </div>
    </c:forEach>
```


