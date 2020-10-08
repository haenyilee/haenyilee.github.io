# 1. 데이터 수집
# 2. 테이블 제작
- 컬럼명
- 데이터형
- 제약조건
- 테이블 만드는 형식
```
CREATE TABLE table_name
(
    컬럼명 데이터형 [제약조건(NOT NULL | DEFAULT)],
    컬럼명 데이터형 [제약조건(NOT NULL | DEFAULT)],
    컬럼명 데이터형 [제약조건(NOT NULL | DEFAULT)],
    [제약조건(CHECK | FOREIGN | PRIMARY) => 여러 컬럼을 동시 처리 가능],
    [제약조건],
);
```
#### daum_movie : 영화 데이터
```
CREATE TABLE daum_movie(
    no NUMBER,
    cateno NUMBER,
    title VARCHAR2(200) CONSTRAINT dm_title_nn NOT NULL,
    poster VARCHAR2(300) CONSTRAINT dm_poster_nn NOT NULL,
    regdate VARCHAR2(200),
    genre VARCHAR2(100) CONSTRAINT dm_genre_nn NOT NULL,
    grade VARCHAR2(100) CONSTRAINT dm_grade_nn NOT NULL,
    actor VARCHAR2(100),
    score VARCHAR2(20),
    director VARCHAR2(100) CONSTRAINT dm_director_nn NOT NULL,
    strory CLOB,
    key VARCHAR2(50),
    CONSTRAINT dm_no_pk PRIMARY KEY(no)
);
```


#### daum_news : 뉴스 데이터
```
CREATE TABLE daum_news(
    title VARCHAR2(1000) CONSTRAINT dn_title_nn NOT NULL,
    poster VARCHAR2(1000) CONSTRAINT dn_poster_nn NOT NULL,
    link VARCHAR2(1000) CONSTRAINT dn_link_nn NOT NULL,
    content CLOB CONSTRAINT dn_content_nn NOT NULL,
    author VARCHAR2(1000) CONSTRAINT dn_author_nn NOT NULL
);
```


#### member : 회원정보
```
CREATE TABLE member(
    id VARCHAR2(20),
    pwd VARCHAR2(10) CONSTRAINT member_pwd_nn NOT NULL,
    CONSTRAINT member_id_pk PRIMARY KEY(id)
);

INSERT INTO member VALUES('hong','1234');
INSERT INTO member VALUES('shim','1234');
INSERT INTO member VALUES('kang','1234');
COMMIT;
```

#### daum_reply : 댓글 데이터
- MSG : 댓글
- ID : 멤버 테이블에서 참조

```
CREATE TABLE daum_reply(
    no NUMBER,
    mno NUMBER,
    id VARCHAR2(20) CONSTRAINT dr_id_nn NOT NULL,
    msg CLOB CONSTRAINT dr_msg_nn NOT NULL,
    regdate DATE DEFAULT SYSDATE,
    CONSTRAINT dr_no_pk PRIMARY KEY(no),
    CONSTRAINT dr_mno_fk FOREIGN KEY(mno)
    REFERENCES daum_movie(no)
);

INSERT INTO daum_reply VALUES(1,1,'hong','댓글 올리기',SYSDATE);
INSERT INTO daum_reply VALUES(2,1,'shim','댓글 올리기',SYSDATE);
COMMIT;
```


#### ChefVO
```
CREATE TABLE chef(
    poster VARCHAR2(260) CONSTRAINT chef_poster_nm NOT NULL,
    chef VARCHAR2(100) CONSTRAINT chef_chef_nm NOT NULL,
    mem_cont1 VARCHAR2(20),
    mem_cont3 VARCHAR2(20),
    mem_cont7 VARCHAR2(20),
    mem_cont2 VARCHAR2(20)
);
```


# 3. 수집한 데이터 테이블에 삽입
- 


# 4. 웹에서 출력(Servlet)
- 서버에서 클라이언트에게 데이터를 전송하는 과정은 다음과 같다.  

![](https://kouzie.github.io/assets/jsp/image16.png)  
 

- 클라이언트의 요청이 일어나면 jsp에서 만들어진 서블릿 객체가 out.print()메서드를 통해 버퍼에 출력할 값들을 저장한다.  

![](https://kouzie.github.io/assets/jsp/image15.png)

- doGet함수는 톰캣이 호출함 
  - 출력되는 내용은 html코드만 메모리에 저장됨
  - 메모리의 html이 웹에서 출력되는 구조임

- 아파치 웹서버 깔고 그 안에 톰캣을 깔아야....
  - 아파치 안깔고 톰캣만 깔면 테스트용 웹서버?

