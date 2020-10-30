---
sort:
---

# 댓글형 게시판 만들기


## DB제작 및 초기 셋팅

```
CREATE TABLE movie_board(
  no NUMBER,
  name VARCHAR2(34) CONSTRAINT movieb_name_nn NOT NULL,
  subject VARCHAR2(1000) CONSTRAINT movieb_sub_nn NOT NULL,
  content CLOB CONSTRAINT movieb_cont_nn NOT NULL,
  pwd VARCHAR2(10) CONSTRAINT movieb_pwd_nn NOT NULL,
  regdate DATE DEFAULT SYSDATE,
  hit NUMBER DEFAULT 0,
  CONSTRAINT movieb_no_pk PRIMARY KEY(no)
);

CREATE TABEL movie_reply(
  no NUMBER,
  bno NUMBER,
  id VARCHAR2(20) CONSTRAINT movier_id_nn NOT NULL,
  name VARCHAR2(34) CONSTRAINT mvoier_name_nn NOT NULL,
  msg CLOB CONSTRAINT movier_msg_nn NOT NULL,
  regdate DATE DEFAULT SYSDATE,
  group_id NUMBER,
  group_step NUMBER DEFAULT 0,
  group_tab NUMBER DEFAULT 0,
  root NUMBER DEFAULT 0,
  depth NUMBER DEFAULT 0,
  CONSTRAINT movier_no_pk PRIMARY KEY(no),
  CONSTRAINT movier_bno_fk FOREIGN KEY(bno)
  REFERENCES movie_board(no)
);
```

### [VO]
- 테이블에 맞게 BoardVO, ReplyVO 제작


### [mapper] , [Config.xml]
- Config.xml에 mapper와 VO alias 등록

## 새글 작성
### [mapper]

```xml 
<!-- 새글 작성 -->
	<insert id="boardInsert" parameterType="BoardVO">
		<selectKey keyProperty="no" resultType="int" order="BEFORE">
			SELECT NVL(MAX(no)+1,1) as no FROM movie_board
		</selectKey>
		INSERT INTO movie_board VALUES(
			#{no},
			#{name},
			#{subject},
			#{content},
			#{pwd},
			SYSDATE,
			0
		)
	</insert>
```

- selectkey 태그에서 keyproperty에는 컬럼명이 들어가야 한다.


### [DAO]

### [View]

### [Model]

## 목록 출력
### [mapper]

```xml
<!-- 목록 출력 -->
	<select id="boardListData" resultType="BoardVO" parameterType="hashmap">
		SELECT no,subject,name, TO_CHAR(regdate,'YYYY-MM-DD') as dbday,hit,num
		FROM (SELECT no,subject,name, regdate,hit,rownum as mum
		FROM (SELECT no,subject,name, regdate,hit 
		FROM movie_board ORDER BY no DESC))
		WHERE num BETWEEN #{start} AND #{end}
	</select>
```

### [DAO]
- 연결 : SqlSession
- 마이바티스는 DBCP를 사용하기 때문에 꼭 사용이 종료된 뒤에는 반환해줘야 한다. (https://aljjabaegi.tistory.com/402)[DBCP와 JDBC란?]
- Connection Pool를 사용하는 이유 :

```
<PRE>
- 시스템의 성능향상 
- Connection Pool : 미리 커넥션을 생성 Client 접속시 이용가능 하도록 한 후 사용 후 회수 하도록 하는 형식

javax.sql.DataSource
DataSource.getConnection()

Java Programming에서 Database로 Connection을 맺는 일은 매우 느리며 자원을 많이 소모하는 작업이다.
그러므로 불특정 다수의 사용자들이 동시에 Database의 Connection을 요구한다면 최악의 경우 server가 down되기도 한다.
이것을 해결하기 위해 Connection Pool을 이용한다.

Connection Pool은 Database와의 연결을 효율적으로 관리하는 역할을 한다.
사전에 일정량의 Connection 객체를 만들어 공유된 장소에 모아둔다.
시간이 걸리는 Connection 객체 생성을 사전에 해 둠으로써 속도향상을 기대할 수 있다.
Java Programming에서 사용이 끝난 Connection객체를 다시 공유된 장소에 넣어둔다.

- 애플리케이션 서버가 시동될때 일정 수의 커넥션을 미리 생성한다.
- 애플리케이션 요청에 따라 생성된 커넥션 객체를 전달한다.
- 일정 수 이상의 커넥션이 사용되면 새로운 커넥션을 만든다.
- 사용하지 않는 커넥션은 종료하고 최소한의 커넥션을 유지한다.

예전에 이 같은 Connection Pool을 직접 개발하곤 햇으나 최근에는 JDBC Driver내에 자체적으로 내장되어 있는 경우가 많다.

출처 : (https://wwhite103.tistory.com/35)[W.C.]
<PRE>
```

- DML(INSERT , UPDATE, DELETE)은 커밋을 날려야 한다. 

```note
**SQL언어의 종류**
- DQL : SELECT
- DML : INSERT , UPDATE, DELETE
- DDL : CREATE , DROP , ALTER , RENAME
- DCL : GRANT , REVOKE
- TCL : COMMIT , ROLLBACK
```



