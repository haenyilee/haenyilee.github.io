---
sort: 2
---

# 2. MVC구조로 게시판 만들기 (2) 목록 

### mapper.xml

```xml
<select id="boardListData" resultType="BoardVO" parameterType="hashmap">
		SELECT no,subject,name,regdate,hit,num
		FROM (no,subject,name,regdate,hit, rownum as num
		FROM (no,subject,name,regdate,hit
		FROM freeboard4 ORDER BY no DESC))
		WHERE num BETWEEN #{start} AND #{end}
</select>
	
<select id="boardTotalPage" resultType="int">
		SELECT CEIL(COUNT(*)/10.0) FROM freeboard4
</select>
```


### DAO
- 목록 출력에 필요한 데이터 받기 

```java
	public static List<BoardVO> boardListData(Map map)
	{
		List<BoardVO> list = new ArrayList<BoardVO>();
		SqlSession session= ssf.openSession();
		list=session.selectList("boardListData",map);
		session.close();
		return list;
	}
```

- 페이지 나눌때 필요한 총 페이지수 데이터 받기

```java
	public static int boardTotalPage()
	{
		int total=0;
		SqlSession session= ssf.openSession();
		total=session.selectOne("boardTotalPage");
		session.close();
		return total;
	}
```

### ListModel.java

- 사용자가 요청한 페이지값 받기

```java
String page=request.getParameter("page");
```

- 현재 페이지 curpage에 담기

```java
		if(page==null)
			page="1";
		int curpage=Integer.parseInt(page);
```

- 페이지 분할 범위, 페이지 첫 데이터 번호, 페이지 마지막 데이터 번호 map으로 담기

```java
		Map map = new HashMap();
		int rowSize=10;
		int start=rowSize*(curpage-1)+1;
		int end = rowSize*curpage;
		
		map.put("start", start);
		map.put("end", end);
```

- 게시판 목록을 가지고 온다.

```java
List<BoardVO> list=BoardDAO.boardListData(map);
```

- 총 페이지 수 데이터 받기

```java
int totalpage=BoardDAO.boardTotalPage();
```

- request에 담아서 JSP로 전송한다.

```java
		request.setAttribute("list", list);
		request.setAttribute("curpage", curpage);
		request.setAttribute("totalpage", totalpage);
```

- return값에 전송한 JSP 페이지 작성하기
  - 여기서 바로 jsp로 전송되는 것이 아니라, Controller에서 request를 받아서 jsp로 전송된다.

```java
return "board/list.jsp";
```



### Controller

### applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>
	<bean id="list.do" class="com.sist.model.ListModel"/>
</beans>
```


### View


