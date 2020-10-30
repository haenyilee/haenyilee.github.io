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
- POOLED를 사용하는 이유 : 
