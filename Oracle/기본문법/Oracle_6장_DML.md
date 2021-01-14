---
sort: 6
---

# [Oracle] DML

- 자바에서 DML은 AutoCommit

## INSERT : 새로운 데이터 입력하기
### INSERT 유형
1. [전체 추가](#전체-추가)
2. [특정 칼럼만 추가](#원하는-데이터만-추가)
3. [다른 테이블 내용 전체 추가](#다른-테이블에-있는-데이터-전체를-추가)
4. [여러 테이블에 전체 저장](#같은-데이터를-여러-테이블에-저장)

### 전체 추가
```
INSERT INTO table_name  
VALUES(values1,values2,....)
```
- 문자, 날짜는 ```' '``` 작성 필수
- 데이터베이스 순서대로 입력
- 모든 칼럼에 값 넣을때는 테이블 이름 뒤에 컬럼명 생략 가능

### 원하는 데이터만 추가
```
INSERT INTO table_name (column1,column2...) 
VALUES(values1,values2,....)
```
- values에 값을 입력하지 않으면 NULL값이 자동으로 들어간다.

### 다른 테이블에 있는 데이터 전체를 추가
```
INSERT INTO table_name 
SELECT * FROM table_name
```

### 같은 데이터를 여러 테이블에 저장
```
INSERT ALL
WHEN [조건] THEN
INTO table_name VALUES(column1,..)
WHEN [조건] THEN
INTO table_name VALUES(column1,..)
WHEN [조건] THEN
INTO table_name VALUES(column1,..)
SELECT column1,..
FROM table_name; 
```
- 모든 컬럼을 가져오려면 VALUES 뒤에 컬럼명 생략가능


### INSERT연습
#### 1. 테이블 카피
- 데이터는 COPY X, 테이블 형식만 COPY하기 => 어떤 문장이던 WHERE 조건에 false가 나오는 조건을 주면됨
```
-- 실행
CREATE TABLE dept_test
AS SELECT * FROM dept
WHERE 1=2;
```
```
-- 결과
SQL> CREATE TABLE dept_test
  2  AS SELECT * FROM dept
  3  WHERE 1=2;

Table created.

SQL> SELECT * FROM dept_test;

no rows selected

SQL> DESC dept_test
 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 DEPTNO                                             NUMBER(2)
 DNAME                                              VARCHAR2(14)
 LOC                                                VARCHAR2(13)
```
#### 2. 데이터 전체 추가
- 50, 영업부, 서울 데이터 추가하기
- 60, 기획부, 부산 데이터 추가하기
```
-- 실행
INSERT INTO dept_test VALUES(50,'영업부','서울');
INSERT INTO dept_test VALUES(60, '기획부','부산');
COMMIT;
```
```
-- 결과
SQL> INSERT INTO dept_test VALUES(50,'영업부','서울');
1 row created.

SQL> INSERT INTO dept_test VALUES(60, '기획부','부산');
1 row created.

SQL> COMMIT;
Commit complete.

SQL> SELECT * FROM dept_test;

    DEPTNO DNAME                        LOC
---------- ---------------------------- --------------------------
        50 영업부                       서울
        60 기획부                       부산
```

#### 3. 부분적으로 추가하기
- 부서번호랑, 부서이름만 있는 데이터 추가하기

```
-- 실행
INSERT INTO dept_test (deptno,dname) VALUES(70,'총무부');
```
```
-- 결과
SQL> INSERT INTO dept_test (deptno,dname) VALUES(70,'총무부');
1 row created.

SQL> COMMIT;
Commit complete.

SQL> SELECT * FROM dept_test;

    DEPTNO DNAME                        LOC
---------- ---------------------------- --------------------------
        50 영업부                       서울
        60 기획부                       부산
        70 총무부
```

#### 4. 부분적으로 추가하기
- DEPT테이블의 데이터 가져오기
```
-- 실행
INSERT INTO dept_test SELECT * FROM dept;
```
```
-- 결과
SQL> INSERT INTO dept_test SELECT * FROM dept;
4 rows created.

SQL> COMMIT;
Commit complete.

SQL> SELECT * FROM dept_test;

    DEPTNO DNAME                        LOC
---------- ---------------------------- --------------------------
        50 영업부                       서울
        60 기획부                       부산
        70 총무부
        10 ACCOUNTING                   NEW YORK
        20 RESEARCH                     DALLAS
        30 SALES                        CHICAGO
        40 OPERATIONS                   BOSTON

7 rows selected.
```

#### 5. 테이블 두 개 만들어서 dept테이블 데이터 전체 집어넣기
- 테이블 만들기(데이터없이 형태만)
```
CREATE TABLE dept_test1
AS SELECT * FROM dept WHERE 1=2;
CREATE TABLE dept_test2
AS SELECT * FROM dept WHERE 1=2;
/* 커밋 날리지 않아도 자동저장됨 */
```
- 테이블 두 군데(teset1,test2)에 dept테이블 동시에 집어넣기
```
-- 실행
INSERT ALL 
INTO dept_test1 VALUES(deptno,dname,loc)
INTO dept_test2 VALUES(deptno,dname,loc)
SELECT * FROM dept;
```


#### 5. 부서별로 나눠서 emp 테이블 만들기
- 부서별로 나눠서 형식만 있는 표 만들기
```
-- 실행
CREATE TABLE emp10
AS SELECT * FROM emp WHERE 1=2;
CREATE TABLE emp20
AS SELECT * FROM emp WHERE 1=2;
CREATE TABLE emp30
AS SELECT * FROM emp WHERE 1=2;
```
- 나눠서 모든 칼럼의 값 집어넣기
```
INSERT ALL 
WHEN deptno=10 
THEN INTO emp10 VALUES(empno,ename,job,mgr,hiredate,sal,comm,deptno)
WHEN deptno=20 
THEN INTO emp20 VALUES(empno,ename,job,mgr,hiredate,sal,comm,deptno)
WHEN deptno=30 
THEN INTO emp30 VALUES(empno,ename,job,mgr,hiredate,sal,comm,deptno) 
SELECT * FROM emp;
```


- 나눠서 일부 칼럼의 값 집어넣기
```
-- 실행
CREATE TABLE emp100
AS SELECT * FROM emp WHERE 1=2;
CREATE TABLE emp200
AS SELECT * FROM emp WHERE 1=2;
CREATE TABLE emp300
AS SELECT * FROM emp WHERE 1=2;
```
```
INSERT ALL 
WHEN deptno=10 
THEN INTO emp100(empno,ename,job,hiredate,deptno)
WHEN deptno=20 
THEN INTO emp200(empno,ename,job,hiredate,deptno)
WHEN deptno=300
THEN INTO emp30(empno,ename,job,hiredate,deptno) 
SELECT empno,ename,job,hiredate,deptno FROM emp;
```


#### 6. 입사월별로 나눠서 emp 테이블 4개 만들기
- 테이블 만들기 
```
CREATE TABLE emp1 AS SELECT * FROM emp WHERE 1=2;
CREATE TABLE emp2 AS SELECT * FROM emp WHERE 1=2;
CREATE TABLE emp3 AS SELECT * FROM emp WHERE 1=2;
CREATE TABLE emp4 AS SELECT * FROM emp WHERE 1=2;
```

- 월별로 분리해서 값 집어넣기
```
INSERT ALL 
WHEN TO_CHAR(hiredate,'MM') BETWEEN 01 AND 03 THEN INTO emp1
WHEN TO_CHAR(hiredate,'MM') BETWEEN 04 AND 06 THEN INTO emp2
WHEN TO_CHAR(hiredate,'MM') BETWEEN 07 AND 09 THEN INTO emp3
WHEN TO_CHAR(hiredate,'MM') BETWEEN 10 AND 12 THEN INTO emp4 
SELECT * FROM emp;
```




## UPDATE
- 데이터수정
- 형식 : 
```
UPDATE table_name SET
컬럼명=변경할값, 컬럼명=변경할 값....
------------------------------------- 뒤에 조건 없으면 전체 데이터가 변경됨
[WHERE 조건]
```

#### 예시
- 표 COPY하기
```
CREATE TABLE emp_update(empno,ename,job,sal) 
AS SELECT empno,ename,job,sal FROM emp;
```
- 데이터 내용 일괄 변경하기
```
UPDATE emp_update SET
sal=3500;
```
- 데이터 내용 조건부로 변경하기
```
UPDATE emp_update SET
sal=3500 
WHERE empno=7788;
```

- job이 매니저인 사람 급여를 3200으로 바꾸기
```
UPDATE emp_update SET 
sal=3200 
WHERE job='MANAGER';
```

- job이 매니저인 사람 급여를 3200으로, 직업을 CLERK으로 바꾸기
```
UPDATE emp_update SET 
sal=3300, job='CLERK'
WHERE job='MANAGER';
```


## DELETE
- 데이터 삭제
```
DELETE FROM table_name
------------------------- 뒤에 조건 없으면 전체 데이터가 삭제됨
[WHERE 조건]
```
```
DELETE FROM emp_update 
WHERE ename='SCOTT' AND ; 
```
- 연습문제
#### 이름에 A자가 들어가는 사람 지우기
  - LIKE, INSTR 등 사용하기 [제어함수 참고](https://github.com/haenyilee/Oracle_Basic/wiki/Oracle_2%EC%9E%A5_%EB%8B%A8%EC%9D%BC%ED%96%89%ED%95%A8%EC%88%98#%EC%A0%9C%EC%96%B4%ED%95%A8%EC%88%98)
  - UPPER , LOWER

```
DELETE FROM emp_update 
WHERE ename LIKE '%A%';
```


#### 게시판 데이터 만들기
- 테이블만들기
```
CREATE TABLE freeboard(
    no NUMBER,
    name VARCHAR2(34),
    subject VARCHAR2(2000),
    content CLOB,
    pwd VARCHAR2(10),
    regdate DATE DEFAULT SYSDATE,
    hit NUMBER DEFAULT 0
);
```

- 값 집어넣기 (**디폴트 값 지정된 부분에는 값 채우지 않도록 설정해야함**)
```
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(1,'홍길동','자유게시판만들기','CURD~~','1233');
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(2,'홍길동','자유게시판만들기','CURD~~','1233');
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(3,'홍길동','자유게시판만들기','CURD~~','1233');
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(4,'홍길동','자유게시판만들기','CURD~~','1233');
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(5,'홍길동','자유게시판만들기','CURD~~','1233');
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(6,'홍길동','자유게시판만들기','CURD~~','1233');
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(7,'홍길동','자유게시판만들기','CURD~~','1233');
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(8,'홍길동','자유게시판만들기','CURD~~','1233');
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(9,'홍길동','자유게시판만들기','CURD~~','1233');
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(10,'홍길동','자유게시판만들기','CURD~~','1233');
INSERT INTO freeboard(no,name,subject,content,pwd) 
VALUES(11,'홍길동','자유게시판만들기','CURD~~','1233');
COMMIT;
```

```
-- 오류주의 (**디폴트 값 넣었다고해서 컬럼 명시 안하고 VALUES에 디폴트 뺴고 INSERT하면 오류남**)
INSERT INTO freeboard 
VALUES(1,'홍길동','자유게시판만들기','CURD~~','1233');
ERROR at line 1:ORA-00947: not enough values
```



## MERGE
- [MERGE문](https://thebook.io/006696/part01/ch03/04/)

- MERGE문은 조건을 비교해서 테이블에 해당 조건에 맞는 데이터가 없으면 INSERT, 있으면 UPDATE를 수행하는 문장이다. 

- 특정 조건에 따라 어떤 때는 INSERT를, 또 다른 경우에는 UPDATE문을 수행해야 할 때, 과거에는 해당 조건을 처리하는 로직을 별도로 작성해야 했지만, 
MERGE문이 나온 덕분에 이제 한 문장으로 처리할 수 있게 되었다.

```
MERGE INTO [스키마.]테이블명
    USING (update나 insert될 데이터 원천)
         ON (update될 조건)
WHEN MATCHED THEN
       SET 컬럼1 = 값1, 컬럼2 = 값2, ...
WHERE update 조건
       DELETE WHERE update_delete 조건
WHEN NOT MATCHED THEN
       INSERT (컬럼1, 컬럼2, ...) VALUES (값1, 값2,...)
       WHERE insert 조건;
```
