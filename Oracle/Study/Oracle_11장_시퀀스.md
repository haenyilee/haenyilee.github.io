### 목차
1. SEQUENCE(시퀀스)
2. SYNONYM(시노님 - 동의어)


# 시퀀스
- 자동 증가번호에 쓰임
- MAX+1 대신 사용 가능한 것
- 무조건 증가만 함 
  - 중간에 삭제해도 대체되지 않고 마지막값에 이어서 증가됨
- PRIMARY KEY에 자주 쓰임 => 중복이 없는 값을 만들기 때문에 자동 증가번호 사용

- 테이블과 시퀀스는 별도임

- 이름 : table명_column명_seq

- 삭제 : DROP SEQUENCE 시퀀스명;
```
DROP SEQUENCE seq_no;
```
- 형식
```
CREATE SEQUENCE seq_no(시퀀스명)
옵션 /* 시작번호 , 증가값 , CACHE , CYCLE , MAX , MIN */
```

- 예시
```
CREATE SEQUENCE seq_no
START WITH 1
INCREMENT BY 1
NOCACHE
NOCYCLE;
```



## START WITH
- 시작번호

## INCREMENT BY
- 증가값, 증가폭

## NOCYCLE | CYCLE
- CYCLE
- 1~10까지 사용 => 1로 돌아옴
- PRIMARY KEY에 자주 쓰임
- 무조건 증가만 함 
- 1 2 3 4 5 => 4번 삭제 => 1 2 3 5 6 ...



- NOCYCLE


## CACHE | NOCACHE
- 미리 20정도를 저장 => NOCACHE

## MAX_VALUE , MIN_VALUE
- 사용 빈도가 거의 없다.

## seq.nextval
- 다음 값을 가지고 온다

```
SELECT seq_no.nextval FROM DUAL;
```

## seq.currval 
- 현재 값을 가지고 온다

```
SQL> SELECT seq_no.currval FROM DUAL;
```

#### 시퀀스 예제

- 테이블 추가하기(제약조건)
```sql
CREATE TABLE jsp_board(
no NUMBER,
name VARCHAR2(34) CONSTRAINT jb_name_nn NOT NULL,
subject VARCHAR2(1000) CONSTRAINT jb_subject_nn NOT NULL,
content CLOB CONSTRAINT jb_content_nn NOT NULL,
pwd VARCHAR2(10) CONSTRAINT jb_pwd_nn NOT NULL,
regdate DATE DEFAULT SYSDATE,
hit NUMBER DEFAULT 0,
CONSTRAINT jb_no_pk PRIMARY KEY(no)
);
```

- 시퀀스 추가하기
```sql
CREATE SEQUENCE jb_no_seq
START WITH 1
INCREMENT BY 1
NOCYCLE 
NOCACHE;
```

- no에 시퀀스 적용해서 데이터값 집어넣기
```sql
INSERT INTO jsp_board(no,name,subject,content,pwd)
VALUES(jb_no_seq.nextval,'홍길동','JSP처음수업','사용법(JavaScript,CSS)','1234');
INSERT INTO jsp_board(no,name,subject,content,pwd)
VALUES(jb_no_seq.nextval,'홍길동','JSP처음수업','사용법(JavaScript,CSS)','1234');
INSERT INTO jsp_board(no,name,subject,content,pwd)
VALUES(jb_no_seq.nextval,'홍길동','JSP처음수업','사용법(JavaScript,CSS)','1234');
INSERT INTO jsp_board(no,name,subject,content,pwd)
VALUES(jb_no_seq.nextval,'홍길동','JSP처음수업','사용법(JavaScript,CSS)','1234');
INSERT INTO jsp_board(no,name,subject,content,pwd)
VALUES(jb_no_seq.nextval,'홍길동','JSP처음수업','사용법(JavaScript,CSS)','1234');
INSERT INTO jsp_board(no,name,subject,content,pwd)
VALUES(jb_no_seq.nextval,'홍길동','JSP처음수업','사용법(JavaScript,CSS)','1234');
INSERT INTO jsp_board(no,name,subject,content,pwd)
VALUES(jb_no_seq.nextval,'홍길동','JSP처음수업','사용법(JavaScript,CSS)','1234');
INSERT INTO jsp_board(no,name,subject,content,pwd)
VALUES(jb_no_seq.nextval,'홍길동','JSP처음수업','사용법(JavaScript,CSS)','1234');
COMMIT;
```

