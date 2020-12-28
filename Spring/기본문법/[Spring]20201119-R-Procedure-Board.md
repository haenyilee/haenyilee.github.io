# 스프링 지능형웹 (1) R그래프, 프로시저 게시판

## R
- X64클릭하기
- install.packages("rJava")
  - 코리아- 서울
- install.packages("Rserve")
- install.packages("KoNLP")
  - 실패
  
- 

```r
install.packages("multilinguer")
library(multilinguer)
install_jdk()
install.packages(c('stringr', 'hash', 'tau', 'Sejong', 'RSQLite', 'devtools'), type = "binary")
install.packages("remotes")
remotes::install_github('haven-jeon/KoNLP', upgrade = "never", INSTALL_opts=c("--no-multiarch"))
library(KoNLP)
```

> library("rJava")
> library("Rserve")
> library("KoNLP")

- [문서\R\win-library\4.0\Rserve\libs\x64]에 있는 Rserve 파일을 [Program Files\R\R-4.03\bin] 에 붙여넣기

- 서버 연결
- c:\Program Files\R\R-4.0.3\bin>R.exe CMD Rserve.exe

- data<-read.csv("c:/upload/emp.csv",header=T,sep=",")
barplot(data$sal,names=data$ename,col
## Spring

### pom.xml  , web.xml 셋팅

### list.jsp
- 리얼패스 받기 : `<%=application.getRealPath("/") %>` 작성 후 서버 구동

### EmpMapper
- empListData : emp테이블 데이터 조회

### EmpDAO
- empListData 연결접속 후 실행하기

### EmpController.java
- emp/list.do 요청이 들어오면 emp/list 반환하기

- FileWriter 클래스 : 문자 기반 스트림으로 텍스트 데이터를 파일에 저장할 때 사용. 문자 단위로 저장하므로 텍스트만 저장 가능

  - 생성 방법 : `FileWriter fw = new FileWriter("파일경로");`
  - `FileWriter fw = new FileWriter("파일경로", true);` : 지정된 파일이 이미 있을 경우, 기존 파일 내용 끝에 데이터를 추가할 경우 두번 째 매개값에 true를 준다.
  - `fw.write(csv)` :  파일안에 문자열 쓰기

- csv : 쉼표로 구분되어 있는 엑셀 파일

### list.jsp

```jsp
<div class="row">
			<img src="emp.png" width=100%>
</div>
```

### RManager

- `header=T` : `"empno,ename,sal\n";` 가 존재한다


### EmpController


## 프로시저와 함수만으로 게시판 만들기

### DB 테이블 제작

- 게시판 테이블

```oracle
CREATE TABLE project_board(
    no NUMBER,
    name VARCHAR2(34) CONSTRAINT pb_name_nn NOT NULL,
    subject VARCHAR2(1000) CONSTRAINT pb_sub_nn NOT NULL,
    content CLOB CONSTRAINT pb_cont_nn NOT NULL,
    pwd VARCHAR2(10) CONSTRAINT pb_pwd_nn NOT NULL,
    regdate DATE DEFAULT SYSDATE,
    hit NUMBER DEFAULT 0,
    CONSTRAINT pb_no_pk PRIMARY KEY(no)
);
```

- 총 페이지수

```ORACLE
CREATE OR REPLACE FUNCTION projectBoardTotalPage RETURN NUMBER
IS
    pTotal NUMBER;
BEGIN
    SELECT CEIL(COUNT(*)/10.0) INTO pTotal
    FROM project_board;
    
    RETURN pTotal;
END;
```

- 새글 삽입 프로시저

```SQL
CREATE OR REPLACE PROCEDURE projectBoardInsert(
    pName project_board.name%TYPE,
    pSubject project_board.subject%TYPE,
    pContent project_board.content%TYPE,
    pPwd project_board.pwd%TYPE
)
IS
BEGIN
    INSERT INTO project_board(no,name,subject,content,pwd)
    VALUES((SELECT NVL(MAX(no)+1,1) FROM project_board),
        pName,pSubject,pContent,pPwd);
    COMMIT;
END;
```

- 총페이지 수, 목록데이터 프로시저
	- pResult : 목록데이터
	- pTotal : 총 페이지 수
	- OUT SYS_REFCURSOR
		- OUT
		- REF

```SQL
CREATE OR REPLACE PROCEDURE projectBoardList(
    pStart NUMBER,
    pEnd NUMBER,
    pResult OUT SYS_REFCURSOR
)
IS
BEGIN
    OPEN pResult FOR
    SELECT no,subject,name,regdate,hit,num
    FROM (SELECT no,subject,name,regdate,hit,rownum as num
    FROM (SELECT no,subject,name,regdate,hit
    FROM project_board ORDER BY no DESC))
    WHERE num BETWEEN pStart AND pEnd;
END;
/
```



- 조회수 증가, 상세보기 프로시저

```oracle
CREATE OR REPLACE PROCEDURE projectBoardDetailData(
    pNo project_board.no%TYPE,
    pResult OUT SYS_REFCURSOR
)
IS
BEGIN
    -- 조회수 증가
    UPDATE project_board SET
    hit=hit+1
    WHERE no=pNo;
    COMMIT;
    -- 상세보기
    OPEN pResult FOR
    SELECT no,name,subject,content,regdate,hit
    FROM project_board
    WHERE no=pNo;
END;
/
```

- 수정하기 프로시저

	- 수정할 글 상세 데이터 받아오기

```oracle
CREATE OR REPLACE PROCEDURE projectBoardUpdateData(
    pNo project_board.no%TYPE,
    pResult OUT SYS_REFCURSOR
)
IS
BEGIN
    OPEN pResult FOR
        SELECT no,name,subject,content
        FROM project_board
        WHERE no=pNo;
END;
/
```

	- 실제 수정하기

```oracle
CREATE OR REPLACE PROCEDURE projectBoardUpdate(
    pNo project_board.no%TYPE,
    pName project_board.name%TYPE,
    pSubject project_board.name%TYPE,
    pContent project_board.content%TYPE,
    pPwd project_board.pwd%TYPE,
    pResult OUT project_board.name%TYPE
)
IS
    vPwd project_board.pwd%TYPE;
BEGIN
    SELECT pwd INTO vPwd
    FROM project_board
    WHERE no=pNo;
    
    IF(vPwd=pPwd) THEN
    pResult:='true';
    UPDATE project_board SET
    name=pName,subject=pSubject,content=pContent
    WHERE no=pNo;
    COMMIT;
    ELSE
    pResult:='false';
    END IF;
END;
/
```

- 삭제하기 프로시저

```oracle
CREATE OR REPLACE PROCEDURE projectBoardDelete(
    pNo project_board.no%TYPE,
    pPwd project_board.pwd%TYPE,
    pResult OUT project_board.name%TYPE
)
IS
    vPwd Project_board.pwd%TYPE;
BEGIN
    SELECT pwd INTO vPwd
    FROM project_board
    WHERE no=pNo;
    
    IF(vPwd=pPwd) THEN
        pResult:='true';
        DELETE FROM project_board
        WHERE no=pNo;
    ELSE
        pResult:='false';
    END IF;
END;
/
```


- 찾아보기 프로시저


### DBConnection
- db 연결 정보 셋팅?
- CallableStatement : 

### DBAspect
- 공통 모듈 제작
	- 연결
	- 해제
	- 예외처리
	- return값

### BoardDAO
- 메모리 할당

- 프로시저 호출 , 실행 : CallableStatement , prepareCall
- 함수 , 일반 쿼리문장 호출 , 실행 : PreparedStatement , prepareStatement
	- FUNCTION은 SELECT 문장 뒤에 붙아서 사용하기 때문에 단독 호출하지 않는다.
		- 대표적인 FUNCTION이 CEIL이다.
	
- dbConn.getConn()이 오라클 연결 정보를 가지고 있다. 
- 결과값 받는 법 : cs.registerOutParameter(6, OracleTypes.VARCHAR);
	- VARCHAR2가 아님


- 커서의 값을 받을 때는 Object로 받고 형변환 시켜줘야 한다.
	- 커서라는 데이터형이 오라클에만 존재하기 때문이다.


- rs.next() : 다음행으로 실행위치를 이동한다. 이후 한 레코드(row)를 가리킨다.
	- 다음행이 있으면 true, 없으면 false를 리턴한다.




### BoardController
- @Controller : forward, sendirect만 사용 가능하기 때문에 스크립트를 사용할 수 없다.
	- 스크립트를 보내고 싶다면 @RestController를 사용해야 한다.
	- 다만, 파일 이동인데 @RestController을 사용하면 문자열로만 인식되어서 화면 이동이 안된다.

### application-context.xml


### detail.jsp
- 분석 결과를 출력함

### restcontroller는 크롬에서만 실행됨
- ajax로 하면 익스플로어에서도 실행됨


## 댓글 게시판

### db 테이블 및 프로시져 제작
- 댓글 수정하기 프로시저

```ORACLE
CREATE OR REPLACE PROCEDURE replyUpdate(
    pNo project_reply.no%TYPE,
    pMsg project_reply.msg%TYPE
)
IS
BEGIN
    UPDATE project_reply SET
    msg=pMsg
    WHERE no=pNo;
    COMMIT;
END;
/
```

- 댓글 삭제 프로시저

```ORACLE
CREATE OR REPLACE PROCEDURE replyDelete(
    pNo project_reply.no%TYPE
)
IS
BEGIN
    DELETE FROM project_reply
    WHERE no=pNo;
    COMMIT;
END;
/
```

- 댓글 삽입 프로시저

```oracle
CREATE OR REPLACE PROCEDURE replyInsert(
    pType project_reply.type%TYPE,
    pCno project_reply.cno%TYPE,
    pId project_reply.id%TYPE,
    pName project_reply.name%TYPE,
    pMsg project_reply.msg%TYPE
)
IS
    vNo project_reply.no%TYPE;
BEGIN
    SELECT NVL(MAX(no)+1,1) INTO vNo
    FROM project_reply;
    INSERT INTO project_reply(no,type,cno,id,name,msg)
    VALUES(vNo,pType,pCno,pId,pName,pMsg);
    COMMIT;
END;
/
```


- 댓글 테이블

```oracle
CREATE TABLE project_reply(
    no NUMBER,
    type NUMBER,
    cno NUMBER,
    id VARCHAR2(20) CONSTRAINT pr_id_nn NOT NULL,
    name VARCHAR2(34) CONSTRAINT pr_name_nn NOT NULL,
    msg CLOB CONSTRAINT pr_msg_nn NOT NULL,
    regdate DATE DEFAULT SYSDATE
);
```


- 댓글 목록 프로시저

```oracle
CREATE OR REPLACE PROCEDURE replyListData(
    pType project_reply.type%TYPE,
    pCno project_reply.cno%TYPE,
    pStart NUMBER,
    pEnd NUMBER,
    pResult OUT SYS_REFCURSOR
)
IS
BEGIN
    OPEN pResult FOR
    SELECT no,type,cno,id,name,msg,TO_CHAR(regdate,'YYYY-MM-DD HH24:MI:SS'),num
    FROM (SELECT no,type,cno,id,name,msg,regdate,rownum as num
    FROM (SELECT no,type,cno,id,name,msg,regdate
    FROM project_reply WHERE type=pType AND cno=pCno ORDER BY no DESC))
    WHERE num BETWEEN  pStart AND pEnd;
END;
/
```

### DBAspect
- reply로 시작하는 DAO클래스에 AOP 적용시켜야 함
	- `||execution(* com.sist.board.dao.BoardDAO.reply*(..))`을 추가하기


### detail.jsp

- 댓글 수정하는 화면은 특정 댓글을 수정했을 때만 떠야 한다.
	- `display:none`으로 설정해두고 수정하기 버튼 눌렀을 때만 나타나도록 설정하면 된다.
	- 수정하기 버튼은 `<span>`으로 변경하고 `data-no="${rvo.no}`로 설정한다.
	- 제이쿼리 : 댓글 수정 버튼 눌렸을 떄는 show / 버튼 안 눌렸을떄는 hide

## 로그인 처리

### BoardController

- board_login_ok : 세션 객체에 로그인 성공시 로그인 정보를 담아둬야함

- board_detailData : 
