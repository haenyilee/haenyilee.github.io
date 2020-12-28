# AOP (3)트랜젝션, Session, Cookie, 커맨드 객체 검증

## 기초셋팅
1. 자바 버전 변경
2. pom.xml 설정
3. web.xml 설정
4. WEB-INF폴더 내에 config 폴더 생성해서 application-context.xml 파일 생성하기
- aop
- beans
- context
- mvc
- p
- tx
5. com.sist.dao 패키지 내에 BoradVO, BoardDAO , BoradMapper 파일 생성



## DB테이블 제작

```tip
- 트랜잭션 기반 AOP가 대부분임
```

- 답변형 게시판 테이블

```sql
CREATE TABLE spring_reply(
    no NUMBER PRIMARY KEY,
    name VARCHAR2(34) NOT NULL,
    subject VARCHAR2(1000) NOT NULL,
    content CLOB NOT NULL,
    pwd VARCHAR2(10) NOT NULL,
    regdate DATE DEFAULT SYSDATE,
    hit NUMBER DEFAULT 0,
    gi NUMBER,
    gs NUMBER DEFAULT 0,
    gt NUMBER DEFAULT 0,
    root NUMBER DEFAULT 0,
    dept NUMBER DEFAULT 0
);
```

## 답변형 게시판 만들기

### BoardVO.java

```
이름      널?       유형             
------- -------- -------------- 
NO      NOT NULL NUMBER         
NAME    NOT NULL VARCHAR2(34)   
SUBJECT NOT NULL VARCHAR2(1000) 
CONTENT NOT NULL CLOB           
PWD     NOT NULL VARCHAR2(10)   
REGDATE          DATE           
HIT              NUMBER         
GI               NUMBER         
GS               NUMBER         
GT               NUMBER         
ROOT             NUMBER         
DEPT             NUMBER  
```

### BoardMapper.java


### BoardDAO.java
- boardListData
- boardInsert



## 로그인 만들기


### 테이블 제작
- board_count

```
CREATE TABLE board_count(
    name VARCHAR2(34) NOT NULL,
    hit NUMBER 
);
```

- board_trigger

```
CREATE OR REPLACE TRIGGER board_trigger
AFTER INSERT ON spring_board
FOR EACH ROW
DECLARE
    v_cnt NUMBER;
BEGIN
    SELECT COUNT(*) INTO v_cnt
    FROM board_count
    WHERE name=:NEW.name;
    
    IF(v_cnt=0) THEN
        INSERT INTO board_count VALUES(:NEW.name,1);
        COMMIT;
    ELSE
        UPDATE board_count SET
        hit=hit+1
        WHERE name=:NEW.name;
        COMMIT;
    END IF;
END;
/
```




### MemberMapper

### BoardVO
- 커맨드 객체 검증

- @NotNull을 붙이면 null값이 들어갔을 때 유효성 검증해줌 
  - import javax.validation.constraints.NotNull;
  
- @Size(min=4,max=8)은 4~8글자 사이로 들어가게 해줌


### BoardController
- public String board_insert_ok(@ModelAttribute("vo") BoardVO vo)
  - @ModelAttribute("vo")는 생략 가능

### application-context.xml
<!-- AOP autoproxy제어 -->
   <aop:aspectj-autoproxy/>
   <!-- 트랜젝션 매니저 -->
   <tx:jta-transaction-manager/>

### login.jsp
- `$('#id').focus();` : focus를 통해 입력 가능하게 만듬
    - trim을 주지 않으면 스페이스를 쳤을 때 값이 넘어가게 됨
- `<input type="text">` 태그로 만들어진 텍스트박스가 있는데 사용자가 이 텍스트박스에 어떤 자료를 입력 하기 위해 클릭하고 커서가 깜빡이는 상황이 되면 포커스를 가진 것입니다.
- 반대로 위 텍스트박스에서의 작업을 마치고 다른 텍스트박스에 작업을 하기 위해 사용자가 옮겨 간다면 위 텍스트박스는 포커스를 잃은 것입니다.

### MemberController.java
- Session, Cookie 활용
- id, pwd를 받아서 세션에 저장한 뒤 넘겨준다.

### login_ok.jsp
- id,pwd 없을 경우 각각 어떻게 처리할지 코딩한다.

### list.jsp
- 로그인 상태가 아니면, 새글작성, 삭제 등의 기능을 사용하지 못하게 제한함


## 상세보기


### BoardMapper
- boardHitIncrement
- boardDetailData

### BoardDAO
- boardDetailData

### BoardController
- `@GetMapping` : a태그로 값이 넘어오기 때문에 get방식이다.
- `Model model` : 해당 JSP로 데이터를 전송하는 역할을 담당한다.
- DB연동한 뒤(BoardMapper-BoardDAO), `board/detail.jsp`로 이동해야 한다.

### detail.jsp


### application-context.xml
- 트랜젝션 설정


```xml
<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager"
		p:dataSource-ref="ds"
/>
```

## 답변달기

### BoardMapper
- 쿼리문장 4개가 한 번에 실행되어야 한다. => 이런 경우 트랜젝션의 대상이 된다. 
    - gi : group id
    - gs : group step
    - gt : group tab
    - root : 원글의 글번호?언
- 오라클의 단점 : 같은 번호가 insert되면 늦게 들어가는 것이 sort됨


```java
@Select("SELECT gi,gs,gt FROM spring_reply "
        + "WHERE no=#{no}")
public BoardVO boardParentData(int no);

@Update("UPDATE spring_reply SET "
        + "gs=gs+1 "
        + "WHERE gi=#{gi} AND gs>#{gs}")
public void boardGsIncrement(BoardVO vo);

@SelectKey(keyProperty="no",resultType=int.class,before=true, 
        statement="SELECT NVL(MAX(no)+1,1) as no FROM spring_reply")
@Insert("INSERT INTO spring_reply(no,name,subject,content,pwd,gi,gs,gt,root) "
        + "VALUES(#{no},#{name},#{subject},#{content},#{pwd},"
        + "#{gi},#{gs},#{gt},#{root})")
public void boardReplyInsert(BoardVO vo);

@Update("UPDATE spring_reply SET "
        + "depth=depth+1 "
        + "WHERE no=#{no}")
public void boardDepthIncrement(int no);
```

### BoardDAO
- 트랜젝션 : `@Transactional(propagation=Propagation.REQUIRED,rollbackFor=Exception.class)`
    - setautocommit이 false로 변경되고, 예외처리는 rollback , 둘 다 정상수행되면 commit이 자동 수행됨

```java
@Transactional(propagation=Propagation.REQUIRED,rollbackFor=Exception.class)
	public void boardReplyInsert(int root,BoardVO vo)
	{
		// conn.setAutoCommit(false) ==> @Before
		BoardVO pvo=mapper.boardParentData(root);
		mapper.boardGsIncrement(pvo);
		vo.setGi(pvo.getGi());
		vo.setGs(pvo.getGs()+1);
		vo.setGt(pvo.getGt()+1);
		mapper.boardReplyInsert(vo);
		mapper.boardDepthIncrement(root);
		// conn.commit() ==> @Around
		// catch() ==> conn.rollback() ==> @AfterReturning
	}
```


### BoardController
- board_reply

```java
@GetMapping("board/reply.do")
	public String board_reply(int no,Model model)
	{
		model.addAttribute("no",no);
		return "board/reply";
	}
```

### detail.jsp
- `<a href="../board/reply.do?no=${vo.no }" class="btn btn-xs btn-danger">답변</a>`


### BoardController


### list.jsp
- getGt해야 함
