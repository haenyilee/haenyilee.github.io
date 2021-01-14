---
sort: 9
---

# [Oracle] VIEW

## VIEW

- 보여만 주는 상태
- 테이블을 새롭게 생성하는 것이 아니라 기존의 테이블에서 필요한 데이터를 모아서 관리하는 것
- 필수 요건 : 테이블
- 메모리에 저장되는 과정이 아니고 가상으로 저장된다.

### VIEW의 장점
- 가상으로 저장되기 때문에 보안이 좋다. (해킹불가)
- SQL문장을 단순화시킬 수 있다는 장점이 있다.

### VIEW의 특징
- ALTER를 사용할 수 없다.
- 삭제를 할 때는 반드시 ```DROP VIEW view명칭``` 을 사용해야한다.
- 저장이나 수정이 가능한데 View에서 수정이나 삭제가 되는 것이 아니라 참조테이블에서 변경된다.
- READ ONLY이기 때문에 DML 사용이 불가하다.
- SELECT만 사용해서 테이블만 볼 수 있게 만든다.
- 같은 이름의 View는 사용할 수 없다.



### VIEW의 형식
```sql
CREATE [OR REPLACE] VIEW view명칭
AS SELECT ~
```
- AS 밑의 문장이 가상데이터에 저장되는 것이 View임


### VIEW 종류

#### 1. 단순뷰 
- 한 개의 테이블을 참조하는 것

#### 2. 복합뷰
- 여러개의 테이블을 참조하는 것
- JOIN 이나 SubQuery가 들어가면 복합뷰임

#### 3. 인라인뷰 (TOP-N)
- FROM SELECT~ 로 시작하는 뷰들



## VIEW 실습예제
#### 단순뷰 만들기
```sql
CREATE VIEW dept_view
AS
SELECT * FROM dept;
```

#### 단순뷰에 INSERT하기
```sql
INSERT INTO dept_view VALUES(50,'영업부','서울');
```

- View에 데이터 추가하면 참조하고 있는 테이블에 데이터가 추가되는 것이기 때문에 주의해야함

#### READ ONLY 단순뷰
- READ ONLY옵션을 추가하면 데이터 수정/추가 불가하다.
```sql
CREATE VIEW dept_view
AS
SELECT * FROM dept WITH READ ONLY;
```

```SQL 
INSERT INTO dept_view VALUES(50,'영업부','서울');
*
ERROR at line 1:
ORA-42399: cannot perform a DML operation on a read-only view
```


#### 복합뷰 만들기
```SQL
CREATE VIEW emp_dept_view
AS 
SELECT empno,ename,job,hiredate,sal,dname,loc
FROM emp,dept
WHERE emp.deptno=dept.deptno;
```

#### 같은 이름의 View를 수정 OR 생성할 때
- ALTER는 사용 불가함
- ```OR REPLACE```를 사용하면 ```DROP TABLE``` 과정이 불필요해짐
- 주로 칼럼 추가할 때 주로 사용됨

```SQL
CREATE OR REPLACE VIEW emp_dept_view
AS 
SELECT empno,ename,job,hiredate,comm,sal,dname,loc
FROM emp,dept
WHERE emp.deptno=dept.deptno;
```

#### 원본 테이블에 없는 컬럼 추가해서 View 생성하기
- 학생 Table 생성
```SQL
CREATE TABLE student(
hakbun NUMBER,
name VARCHAR2(34) CONSTRAINT st_name_nn NOT NULL,
kor NUMBER(3),
eng NUMBER(3),
math NUMBER(3),
CONSTRAINT st_hakbun_pk PRIMARY KEY(hakbun)
);
```
- 데이터 넣기

```sql
INSERT INTO student VALUES(1,'홍길동',80,90,75);
INSERT INTO student VALUES(2,'심청쓰',60,40,88);
INSERT INTO student VALUES(3,'이도령',75,95,85);
INSERT INTO student VALUES(4,'을지문덕',45,55,65);
INSERT INTO student VALUES(5,'이순신',100,80,95);
```

- 원본 테이블에 없는 컬럼 추가해서 View 생성하기
```sql
CREATE OR REPLACE VIEW student_view(hakbun,name,kor,eng,math,total,avg,rank)
AS
SELECT hakbun,name,kor,eng,math,kor+eng+math,ROUND((kor+eng+math)/3.0,2),
RANK() OVER(ORDER BY (kor+eng+math) DESC) 
FROM student;
```



### rownum
- rownum : 자동지정번호 (오라클에서 지정하는 컬럼명)
- 중간에 수정이나 추가되면 자동으로 1번부터 다시 매겨짐

#### rownum 사용하기
```sql
SELECT empno,ename,rownum
FROM emp;
```

#### rownum으로 5개씩 자르기
```sql
SELECT empno,ename,rownum
FROM emp
WHERE rownum<=5;
```

#### 급여가 높은 사람 상위 5명만 출력하기 (_인라인뷰_ )
- 비교연산자 활용하기
```sql
SELECT ename,sal,rownum
FROM (SELECT empno,ename,sal FROM emp ORDER BY sal DESC)
WHERE rownum<=5;
```


- BETWEEN 연산자 활용하기
```sql
SELECT ename,sal,rownum
FROM (SELECT empno,ename,sal FROM emp ORDER BY sal DESC)
WHERE rownum BETWEEN 1 AND 5;
```

#### 급여 높은 순으로 중위권 5명만 출력하기 (_인라인뷰_ )
- 오류
```sql
-- 오류 : **중간을 자를 순 없음**
SELECT ename,sal,rownum
FROM (SELECT empno,ename,sal FROM emp ORDER BY sal DESC)
WHERE rownum BETWEEN 6 AND 8;
```

- 정상
```sql
SELECT empno,ename,sal,num
FROM (SELECT empno,ename,sal,rownum as num  /* as num : Ailas num */
FROM (SELECT empno,ename,sal 
FROM emp ORDER BY sal DESC))
WHERE num BETWEEN 6 AND 10;
```
