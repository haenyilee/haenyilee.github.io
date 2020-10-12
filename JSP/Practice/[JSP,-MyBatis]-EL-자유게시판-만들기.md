---
sort: 2
---

# EL태그로 자유게시판 만들기

# 1. 게시판 목록

## VO.java 
- DB Attribute를 vo로 제작한다.
- vo에 없는 값들은 hashmap으로 받아서 출력할 수 있다.

## mapper.xml 
- SQL쿼리 문장 작성하기

## DAO,java 
1. 데이터베이스 연결
2. 게시판 목록 데이터 받아오기

## manager.java
- 그동안 JSP ```<% %>```에 작성했던 Java코딩을 manager로 옮기기
1. Page 받아오기
2. Page 나누기
3. 결과값 데이터 읽기
4. jsp로 결과값 보내주기
- request에 값을 넣어주면 request가 공유되어서 jsp에서 결과값 출력이 가능해진다.

## list.jsp



(중간 날림..)



# 업데이트
> 글 수정하는 기능

## detail.jsp
1. 수정하기 버튼을 누르면 update.jsp로 이동하도록 a태그를 수정한다.
2. detail.jsp에서 사용자가 클릭한 게시물 번호를 update.jsp로 넘겨준다.

## DAO.java
- 수정할 데이터 읽기
  - boardDetailData mapper를 활용하면 값을 받을 수 있다.

```java
public static BoardVO boardUpdateData(int no)
{
    BoardVO vo= new BoardVO();
    sqlSession session=null;
    try{
        session =ssf.openSession();
        vo=session.selectOne("boardDetailData",no);
    } catch
    {
        e.printStackTracee();
    } finally
    {
        if(session!=null)
            session.close();
    }
    return vo;
}
```

## BoardManager.java
- DAO로부터 출력되는 값을 request에 담아주기

```java
public void boardUpdateData(HttpServletRequest request)
{
    String no= request.getParameter("no");
    BoardVO vo= BoardDAO.boardUpdateData
}
```

## update.jsp

- DAO로부터 데이터 받기

```jsp
<%@ import="com.sist.manager.*"%>
```

```jsp
<jsp:useBean id="mgr" class="com.sist.manager.BoardManager"></jsp:useBean>
```

- BoardManager가 request에 결과값을 담아서 보내줌

```jsp
<%
   mgr.boardUpdateData(
%>
```

- `<input>`태그의 value값 출력하기

```jsp
<input
```

- `<textarea>`태그에도 내용 출력하기

## update_ok.jsp
- [manager.java] import하기
- mgr객체 메모리 할당하기

```jsp
<jsp:useBean id="mgr" class="com.sist.manager.BoardManager"/>
```

- ```BoardManager mgr=new BoardManager()```의 자바 코딩을 JSP태그로 바꾼 것이다.
- BoardManager로 request로 전송하면 처리해서 결과값을 JSP로 전송해서 요청한 결과의 출력만 담당하는 것이 JSP(View)의 역할이다. 이 부분을 프리젠테이션 로직이라고 한다. 
- Java로 처리하는 것은 Model이며, 비지니스로직이라고 한다. Manager,DAO,VO 등이 모두 객체 모델이다. 
- View와 Model을 연결하는 객체가 바로 Controller이다. 

## BoardManager.java
- 실제 수정하기
  - 한글로 변환하기
  - 사용자가 보내준 데이터 받기 
  - 받은 데이터 BoardVO에 묶어서 BoardDAO로 전송하면 오라클에서 수정하면 됨

```java
public void boardUpdate(HttpServletRequest request)
{
    try
    {
        request.setCharacterEncoding("utf-8");
        String no = request.getParameter("no");
        String name = request.getParameter("name");
        String subject = request.getParameter("subject");
        String content = request.getParameter("content");
        String pwd = request.getParameter("pwd");
        
        BoardVO vo=new BoardVO();
        vo.setNo();

    }
}
```

## mapper.xml
- 비밀번호 가져오기

```xml
<select id="boardGetPassword" resultType="string" parameterType="int">
    SELECT pwd FROM freeboard
    WHERE no=#{no}
</select>
```

- 비밀번호 가져오기

```xml
<update id="boardUpdate" parameterType="BoardVO">
    UPDATE freeboard SET
    name=#{name},
    subject=#{subject}
    content=
    WHERE no=#{no}
</update>
```

## BoardDAO.java
- 비밀번호 확인해서 수정한 결과 처리하기
```

(중간 날림...)


# Delete

## delete.jsp
- 화면 출력 디자인 잡기
- 





- 기타..
- ```${param.no}```는 ```request.getParameter("no")``와 같은 코딩이다.
- ```String no=${param.no}```는 ```String no=out.println()```과 같은 코딩이므로 사용이 불가하다
- EL은 출력할때만 사용할 수 있다.

