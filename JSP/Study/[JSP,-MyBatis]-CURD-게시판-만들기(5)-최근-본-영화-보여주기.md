# 쿠키를 활용한 최근 본 영화 출력하기

#### 쿠키 생성단계
1. 쿠키 생성 단계
2. 쿠키 저장 단계
3. 쿠키 전송 단계
    - 클라이언트로 전송할 때 response를 사용하는데, response는 클라이언트 요청을 한 개밖에 수행하지 못한다.
```
<response의 용도>
(1) 쿠키 전송
(2) HTML 전송
(3) 헤더 전송
```


## home.jsp에서 사용자가 클릭한 포스터 번호 쿠키로 넘기기
```
<a href="../movie/cookie.jsp?&no=<%=vo.getNo()%>">
```


## cookie.jsp 생성해서 쿠키값 모으기
- 사용자가 클릭한 no값 받기
```jsp
<%
 	String no=request.getParameter("no");
 	MovieVO vo= MovieDAO.movieDetailData(Integer.parseInt(no));
 %>   
```

- 쿠키 저장하기
  - 쿠키의 값으론 받을 수 있는 것은 String 뿐이다.
```jsp
<%
Cookie cookie = new Cookie("m"+no,vo.getPoster());
%>
```

- cookiesetpath??
```jsp
<%
cookie.setPath("/");
%>
```


- 쿠키 유지 기간 정하기
```jsp
<%
cookie.setMaxAge(60*60*24);
%>
```

- 클라이언트의 컴퓨터로 저장된 쿠키값 전송하기
  - 현재 로컬호스트가 서버로 설정되어 있음
```jsp
<%
response.addCookie(cookie);
%>
```

- 쿠키저장됐으면 detail.jsp로 넘기기
  - 한 jsp파일 안에서 응답을 두 번 받을 수 없다.
    - 그래서 cookie.jsp에서 쿠키를 먼저 전송받고,
    - detail.jsp로 넘겨서 상세보기 화면을 다시 전송 받는다. 
```jsp
<%
response.sendRedirect("../main/main.jsp?mode=8&no="+no);
%>
```


## home.jsp에서 최신에 본 영화 정보 쿠키를 활용해서 보여주기
(1) Java
- 쿠키 읽기
```jsp
<%
List<String> cList=new ArrayList<String>();
Cookie[] cookies=request.getCookies();
%>
```

- 쿠키 값 가져오기
  - 쿠키 값을 여러개 저장되어 있기 때문에 배열로 받아야 한다.
```jsp
<%
for(int i=0;i<cookies.length;i++)
   	{
   		if(cookies[i].getName().startsWith("m"))
   		{
   			cList.add(cookies[i].getValue());
   		}
   	}
%>
```

(2) HTML
- 쿠키값이 없으면 "방문 기록이 없습니다" 출력하기
```jsp
        <%
   		if(cList==null || cList.size()<1)
   		{
   	%>
   			<font color=red><h1>방문한 기록이 없습니다.</h1></font>
   	<%		
   		}
```
- 쿠키 값이 있으면 영화 포스터 출력하기
```jsp
                else
   		{
   			for(String s:cList)
   			{
   	%>
   				<div class="col-md-2">
				    <div class="thumbnail">
				        <img src="<%=s %>" alt="Lights" style="width:100%">
				      </a>
				    </div>
				</div>
   	
   	<%
   			}
   		}
   	%>
```


# 쿠키 삭제하기
## home.jsp에서 쿠키 삭제 버튼 만들기
```jsp
<h3>최근 방문한 영화
&nbsp;<a href="../movie/delete.jsp" class="btn btn-sm btn-primary">쿠키 삭제하기</a>
</h3>
```

## delete.jsp 생성
- 쿠키 값 받기
```jsp
<%
	Cookie[] cookies=request.getCookies();
%>
```

- 쿠키 값 삭제하기
```jsp
<%
        for(Cookie c:cookies)
	{
		if(c.getName().startsWith("m"))
		{
			c.setPath("/");
			c.setMaxAge(0);
			response.addCookie(c);
		}
		
	}
%>
```



- main.jsp로 전송하고 화면 전환하기
```jsp
<%
	response.sendRedirect("../main/main.jsp");
%>
```
