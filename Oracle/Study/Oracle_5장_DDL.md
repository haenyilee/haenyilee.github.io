---
sort: 9
---

# DDL

## SQL의 종류


<html>
<table>
<tr>
<th>DML</th>
<th>DDL</th>
<th>DCL</th>
<th>TCL</th>
</tr>
<tr>
<th>데이터 조작</th>
<th>테이블 선언</th>
<th>데이터 제어</th>
<th>트랜잭션 제어</th>
</tr>
<tr>
<td>
SELECT : 데이터 검색<br>
INSERT : 데이터 추가<br>
UPDATE : 데이터 수정<br>
DELETE : 데이터 삭제<br>
</td>
<td>
CREATE : 테이블 제작<br>
DROP : table삭제<br>
ALTER : 수정, 삭제, 추가<br>
TRUNCATE : data삭제<br>
RENAME : 테이블명 변경<br>
</td>
<td>
GRANT : 권한부여<br>
REVOKE : 권한제어<br>
</td>
<td>
COMMIT<br>
ROLLBACK<br>
</td>
</tr>
</table>
</html>


## CREATE
- TABLE, VIEW , SEQUENCE , INDEX , PL/SQL
- PL/SQL => PROCEDURE , FUNCTION , TRIGGER
- COMMIT을 날릴 필요없음 (Auto Commit)


### 1. CAST : 복사
- 기존에 존재하는 테이블을 복사 

```
CREATE TABLE table_name
AS SELECT ~
```

### 2. 새로운 테이블 제작 (테이블 저장하는 공간)
- 컬럼명 뒤 제약조건을 '컬럼레벨'이라고 함
  - 테이블과 동시에 생성됨
  - ```NOT NULL``` , ```DEFAULT``` 는 무조건 컬럼레벨에 붙여야함
- 나중에 첨부된 제약조건은 '테이블레벨'이라고 함
  - 테이블 생성 후 나중에 생성됨
  - 수정이 용이해서 테이블레벨을 권장함

```
CREATE TABLE table_name(
컬럼명 데이터형 [제약조건 (여러개 사용이 가능)],
컬럼명 데이터형 [제약조건 (여러개 사용이 가능)],
컬럼명 데이터형 [제약조건 (여러개 사용이 가능)]
[제약조건...],
[제약조건...],
[제약조건...]
);
```






## DROP : 삭제, table 자체를 삭제
- 복구 불가하기 때문에 항상 백업해두어야함

```
DROP TABLE 삭제할 테이블명;
```

## ALTER : 수정, 삭제, 추가 (컬럼 관련된 것들만)
- 컬럼 추가

```
ALTER TABLE table_name ADD 컬럼명 데이터형;
```

- 컬럼 삭제

```
ALTER TABLE DROP COLUMN 컬럼명;
```

- 컬럼 이름 변경
  - 컬럼명에 작은따옴표``` ' ' ``` 불필요

```
ALTER TABLE table_name RENAME COLUMN 이전컬럼명 TO 변경컬럼명;
```

- 컬럼 데이터 변경

```
ALTER TABLE MODIFY COLUMN 컬럼명 데이터형;
```


## TRUNCATE : 데이터만 삭제 ,  복구불가

```
TRUNCATE TABLE table_name;
``` 

## RENAME : 테이블 이름 변경

```
RENAME 이전 테이블명 TO 변경할 테이블명;
```

## read only : 읽기 전용으로 변경 
- SELECT만 이용 가능

```
ALTER TABLE table_name read only
```



## 서브쿼리
SELECT ename,sal FROM emp WHERE sal<(SELECT AVG(sal) FROM emp);

## 테이블 제작 방식
#### 1. 테이블 복사
- 복사

```
CREATE TABLE myDept 
AS SELECT * FROM dept;
```

- 특정 조건 성립할때만 복사

```
CREATE TABLE myEmpDept 
AS SELECT empno,ename,job,hiredate,sal,emp.deptno,dname,loc 
FROM emp, dept
WHERE emp.deptno=dept.deptno;
```

- 컬럼명만 복사해오려면? => 값을 가져오는 조건이 false가 되면 됨
  - WHERE 뒤에 false가 나오는 어떤 조건을 줘도 상관 없음
    - ORDER BY처럼 첫 번째 칼럼과 두 번째 칼럼이 같을때라고 쓴 조건이 아님

```
CREATE TABLE new_emp2(no,name,hiredate)
AS SELECT no,name,hiredate 
FROM new_emp
WHERE 1=2;
```


- WHERE절 뒤에 false가 나오도록하면 내용은 복사하지 않고 


#### 2. 사용자 정의
- [오라클에서 제공하는 데이터형](https://github.com/haenyilee/Oracle_Basic/wiki/Oracle_0%EC%9E%A5_%EA%B0%9C%EC%9A%94-%EB%B0%8F-%ED%8A%B9%EC%A7%95#%EC%98%A4%EB%9D%BC%ED%81%B4%EC%9D%98-%EB%8D%B0%EC%9D%B4%ED%84%B0%ED%98%95)
  - 문자 : CHAR , VARCHAR2 , CLOB
  - 숫자 : NUMBER
  - 날짜 : DATE , TIMESTAMP
  - 기타 : BLOB , BFILE (동영상 , 사진)

- 테이블 형식 : ```컬럼명 데이터형``` ```name VARCHAR(34)```
  - VO잡을때와 비슷하지만 데이터형이 뒤에 붙는다는 것이 차이점임
  - VARCHAR2는 가변형이기 때문에 글자수보다 더 작게 범위를 잡지 않도록 주의!
    - 이름이 16바이트인데 VARCHAR2(10) 처럼 사용하면 테이블 생성 안됨
  - 글자수가 고정된 경우는 CHAR사용

- (EX) emp테이블의 생성 

```
CREATE TABLE emp(
사번 정수     => empno NUMBER(4)    -- 1~9999
이름 문자     => ename VARCHAR2(34) -- 한글 : 17자까지...* 2byte
직위 문자     => job VARCHAR2(10)
입사일 날짜형  => hiredate DATE
급여 실수     => sal NUMBER(7,2)    -- 실수 , 정수 모두 가능
부서번호 정수 => deptno NUMBER(2)
)
```

#### 2. 테이블, 컬럼명 제작
- 테이블명은 반드시 문자로 시작
- 숫자는 사용이 가능하지만, 시작문자가 숫자면 안된다.
- 한글도 사용이 가능하지만, 한글이 깨질 수 있어서 비권장함(알파벳 권장)
- 대소문자 구분을 하지 않는다. 
  - BUT 테이블명이 오라클에 저장될 때는 대문자로 저장된다.

```
// 소문자로 써서 못찾아옴
SELECT column_name,data_type 
FROM user_tab_columns 
WHERE table_name='emp';
```

```
// 대문자로 써야찾을 수 있음
SELECT column_name,data_type 
FROM user_tab_columns 
WHERE table_name='EMP';
```

- 테이블,컬럼명은 저장길이가 30byte이다. 한글은 15글자까지 설정 가능
- 테이블은 중복해서 만들 수 없다
  - 저장되는 폴더가 다르면 같은 이름을 쓸 수도 있다.
- 키워드는 사용할 수 없다.
- 테이블명과 컬럼명이 동일할 수도 있다.

```
-- 게시판 테이블 만들기
CREATE TABLE board(
    no NUMBER,
    name VARCHAR2(34),
    email VARCHAR2(100)
    subject VARCHAR2(4000),
    content CLOB,
    pwd VARCHAR2(10),
    regdate DATE,
    hit NUMBER
);
```

```
// 바뀔 수 있는 부분들은 정수로 받기
CREATE TABLE movie(
    mno NUMBER,
    title_ko VARCHAR2(100),
    title_en VARCHAR2(100),
    score NUMBER(3,2),
    genre VARCHAR2(30),
    regdate VARCHAR2(50),
    time NUMBER,
    grade VARCHAR2(50),
    director VARCHAR2(50),
    actor VARCHAR2(100),
    reserve NUMBER(4),
    showUser NUMBER,
    story CLOB,
    poster VARCHAR2(4000),
    address VARCHAR2(200)
);
```

```
-- 네이버 영화 테이블 만들기
CREATE TABLE naver_Movie(
mno NUMBER,
title_ko VARCHAR2(100),
title_en VARCHAR2(100),

score_general NUMBER(3,2),
score_expert NUMBER(3,2),
score_net NUMBER(3,2),

genre VARCHAR2(50),
nation VARCHAR2(50),
runtime NUMBER,
regdate DATE,
director VARCHAR2(50),
actor VARCHAR2(20),
grade VARCHAR2(100),
rank VARCHAR2(100),
total NUMBER
);
```

```
CREATE TABLE test(
// 여기 공백 들어가면 오류
no NUMBER,
name VARCHAR2(10)
// 여기 공백 들어가면 오류
);
```



## ```ALTER``` 수정

#### 필요없는 컬럼 삭제
```ALTER TABLE movie DROP COLUMN address;```

#### 필요한 컬럼 추가
```ALTER TABLE movie ADD cno NUMBER;```

#### 컬럼 데이터 크기 수정
``` ALTER TABLE movie MODIFY genre VARCHAR2(100);```

#### 컬럼이름 변경
```ALTER TABLE movie RENAME COLUMN showUser TO su;```

#### (*) 테이블 이름 변경하기
```RENAME movie TO daum_movie;```

```ALTER TABLE 테이블명 CHANGE 기존컬럼명 변경할컬럼명 컬럼타입;```

## 삭제
#### ```DELETE``` 
- 복구 가능, 데이터 내용만 삭제(칼럼유지)
```
CREATE TABLE emp_test AS SELECT * FROM emp;
-- SELECT * FROM emp_test;
DELETE FROM emp_test;
-- SELECT * FROM emp_test;
ROLLBACK;
-- SELECT * FROM emp_test;
```

#### ```TRUNCATE``` 
- 복구 불가, 데이터 내용만 삭제(칼럼유지)

```
TRUNCATE TABLE emp_test;

SQL> SELECT * FROM emp_test;
no rows selected

SQL> DESC emp_test
 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 EMPNO                                              NUMBER(4)
 ENAME                                              VARCHAR2(10)
 JOB                                                VARCHAR2(9)
```

#### ```DROP``` 
- 완전 삭제, 복구(ROLLBACK) 불가

```
DROP TABLE movie;
DROP TABLE board;
```

```
CREATE TABLE member(
no NUMBER,
id VARCHAR(20),
name VARCHAR2(34),
addr VARCHAR2(100),
tel VARCHAR2(20)
);

INSERT INTO member VALUES(1,'hong','홍길동','서울','010-0000-0000');
COMMIT;
SELECT * FROM member;

ALTER TABLE member read only;

// read only이기때문에 SELECT만 가능!
INSERT INTO member VALUES(1,'hong','홍길동','서울','010-0000-0000');
COMMIT;
SELECT * FROM member;
```

- ```tab``` : 사용할 수 있는 테이블 확인
```SELECT * FROM tab;```

- 업데이트 & 줄수와 블럭수 세기

```
ANALYZE TABLE emp COMPUTE STATISTICS;
SELECT num_rows, blocks FROM user_tables WHERE table_name='EMP';
```
