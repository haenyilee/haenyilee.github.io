---
sort: 2
---

# MVC구조로 게시판 만들기 (3) 상세보기


## applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>
	<bean id="list.do" class="com.sist.model.ListModel"/>
	<bean id="detail.do" class="com.sist.model.DetailModel"/>
</beans>
```



## [mapper] : board-mapper.xml

```tip
- mybatis의 태그 하나 당, sql문장 1개만 실행이 가능하다.
- 단, DAO에서 구현한 메소드는 여러개의 쿼리 문장을 동시에 읽어서 처리할 수 있다.
```

- **조회수 증가시키기**

```xml
  <update id="hitIncrement" parameterType="int">
  	UPDATE freeboard SET
  	hit=hit+1
  	WHERE no=#{no}
  </update>  
```

- **게시물 1개의 데이터 읽어 오기**

```xml
  <select id="boardDetailData" resultType="BoardVO" parameterType="int">
  	SELECT no,subject,name,regdate,hit,content
  	FROM freeboard
  	WHERE no=#{no}
  </select>
```

## [DAO] : BoardDAO.java
- sql문장 실행시켜서 결과값 받기

```java
	public static BoardVO boardDetailData(int no)
	{
		SqlSession session = ssf.openSession();
		session.update("hitIncrement", no);
		session.commit();
		BoardVO vo= session.selectOne("boardDetailData",no);
		session.close();
		return vo;
	}
```

## [Model] : DetailModel.java

```note
- URI : detail.do?no=1
- 값 받는 순서 : request.setParameter("no",1); => request.getParameter("no") => 1
```

- 사용자가 보낸 값을 request에 넘겨주는 역할은 톰캣이 담당한다.
- service(HttpServletRequest request)에서 request는 매개변수이다 보니, 화면이 바뀔때마다 service가 새로 호출되어 request가 초기화된다. 

- **실행순서**
- 1. 사용자가 보내준 게시물 번호를 받는다.

```java
String no=request.getParameter("no");
```

- 2. board-mapper, BoardDAO를 통해서 게시물 한 개의 데이터를 읽어온다.(BoardVO)

```java
BoardVO vo=BoardDAO.boardDetailData(Integer.parseInt(no));
```

- 3. 읽어온 BoardVO값을 jsp로 전송한다.

```java
request.setAttribute("vo", vo);
return "board/detail.jsp";
```

## [Controller] : Controller.java
- 코딩할 내용은 없지만, Model에서 넘겨진 값이 이 Controller를 통해서 값을 받아 view로 전송된다.


## [View] : detail.jsp

- 화면 흐름 제어할 때, `.do`로 받아줘야 Controller를 거쳐서 request를 전달할 수 있다. 
