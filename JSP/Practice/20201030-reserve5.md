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
- insert를 제외하고는 한  mapper문장에 한 쿼리 이상 들어갈 수 없다.


### [DAO]

```JAVA
	/* 새글 작성 */
	public static void boardInsert(BoardVO vo)
	{
		// 연결
		SqlSession session=ssf.openSession(true);
		session.insert("boardInsert",vo);
		// 반환
		session.close();
	}
```

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
<!-- 총페이지 구하기 -->
	<select id="boardTotalPage" resultType="int">
		SELECT CEIL(COUNT(*)/10.0) FROM movie_board
	</select>
```

### [DAO]

```JAVA
	/* 목록 출력 */
	public static List<BoardVO> baordListData(Map map)
	{
		// 연결
		SqlSession session=ssf.openSession();
		List<BoardVO> list=session.selectList("boardListData",map);
		// 반환
		session.close();
		return list;
	}
	/* 총 페이지 구하기 */
	public static int boardTotalPage()
	{
		SqlSession session=ssf.openSession();
		int total=session.selectOne("boardTotalPage");
		session.close();
		return total;
	}
```	

- 연결 : SqlSession
- 마이바티스는 DBCP를 사용하기 때문에 꼭 사용이 종료된 뒤에는 반환해줘야 한다. [DBCP와 JDBC란?](https://aljjabaegi.tistory.com/402)
- Connection Pool를 사용하는 이유 :

```note
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

출처 : <https://wwhite103.tistory.com/35>
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


### [Model]
- 목록에 출력할 데이터와 총페이지수를 동시에 실행시켜야 한다.

### list.jsp
- 날짜 비교해서 new 붙이기!


## 상세 보기

### [mapper]

```oracle
<!-- 조회수 증가 -->
	<update id="hitIncrement" parameterType="int">
		UPDATE movie_board SET
		hit=hit+1
		WHERE no=#{no}
	</update>

<!-- 내용보기 -->
	<select id="boardDetailData" parameterType="int" resultType="BoardVO">
		SELECT * FROM movie_board
		WHERE no=#{no}
	</select>
```

### [DAO]

### [Model]

```java
/* 상세보기 */
public static BoardVO boardDetailData(int no)
{
	SqlSession session=ssf.openSession();
	/* 조회수 증가 */
	session.update("hitIncrement",no);
	session.commit();
	/* 상세 데이터 읽기 */
	BoardVO vo= session.selectOne("boardDetailData",no);
	session.close();
	return vo;
}
```

### list.jsp
- 상세보기로 넘어가는 링크 만들기

### detail.jsp


## 댓글

### detail.jsp
- `style="white-space: pre-wrap"`: 자동으로 다음줄 정렬
- 

### boardModel

### [mapper]
- 11개 중에 디폴트 없는 컬럼만 지정하고 insert하면 디폴트값은 자동으로 값이 채워진다.
- selectKey는 한 번만 적용된다. 이외의 기능은 서브쿼리로 구현해야 한다.


```xml
<!-- 댓글 올리기 -->	
<insert id="replyInsert" parameterType="com.sist.vo.ReplyVO">
	<!-- 자동증가 번호 -->
	<selectKey keyProperty="no" resultType="int" order="BEFORE">
		SELECT NVL(MAX(no)+1,1) as no FROM movie_reply
	</selectKey>
	INSERT INTO movie_reply(no,bno,id,name) VALUES(
	#{no},
	#{bno},
	#{name},
	#{msg},
	(SELECT NVL(MAX(group_id)+1,1) FROM movie_reply)
)
</insert>
```

### boardModel : detail.do
