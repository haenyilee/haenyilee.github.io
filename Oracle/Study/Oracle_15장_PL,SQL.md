---
sort: 16
---

# PL,SQL

## PL/SQL이란
- Procedural Language
- PROCEDURE, FUNCTION, PACKAGE , TRIGGER를 제작할 때 사용하는 언어이다.
  - PROCEDURE : 리턴형이 없는 함수 
    - 리턴형이 없다는 특징이 자바스크립트 언어와 같음
    - 캐시 메모리에 저장되어 속도가 빠르다.
    - 트랜젝션 제어할 때 사용된다. 
  - FUNCTION : 리턴형이 있는 함수
    - 함수 : 독립(C언어)
    - 메소드 : 클래스 종속
  - PACKAGE : 관련된 PROCEDURE , FUNCTION 을 모아서 둔 곳
  - TRIGGER : 이미 지정된 이벤트 발생시에 자동 처리
- 사용자 정의 언어이다.
- 반복구간이 많을 때 재사용하여 코드를 단축하기 위함이 목적이다.
- 코드를 통해 PL/SQL 내용이 드러나지 않기 때문에 정보 보안에 유리하다. 



## PL/SQL BLOCK 기본 구성

- 형식

```ORACLE
DECLARE
-- 선언부(변수선언)
BEGIN
-- 구현부
-- 예외처리 (생략 가능)
END;
/
```

- 사용시 유의사항
  - SELECT에서 실행된 결과값을 받을 때, INTO를 사용한다.
  - 변수에 값 설정 할 때, `:=`를 사용한다.

## PL/SQL에서 변수의 의미와 사용법

### 변수
- 지역변수
- 매개변수 : SUBSTR('',1,2) MAX(컬럼명)

### 변수 사용법
- `스칼라변수` : 단순 변수(NUMBER,VARCHAR2,CLOB,DATE)
  - id VARCHAR2(10)
  
```ORACLE
SET SERVEROUTPUT ON;
DECLARE
    vempno NUMBER(4);
    vename VARCHAR2(20);
    vjob VARCHAR2(20);
    vhiredate DATE;
    vsal NUMBER(7,2);
BEGIN
    SELECT empno,ename,job,hiredate,sal INTO vempno,vename,vjob,vhiredate,vsal
    FROM emp
    WHERE empno=7788;
    -- 출력
    DBMS_OUTPUT.PUT_LINE('======결과======');
    DBMS_OUTPUT.PUT_LINE('사번:'||vempno);
    DBMS_OUTPUT.PUT_LINE('이름:'||vename);
    DBMS_OUTPUT.PUT_LINE('직위:'||vjob);
    DBMS_OUTPUT.PUT_LINE('입사일:'||vhiredate);
    DBMS_OUTPUT.PUT_LINE('급여:'||vsal);
END;
```  
  
 
- `%TYPE` : 참조변수 
  - emp.ename%TYPE => ename이 가지고 있는 데이터형을 가지고 온다.
  - 다른 테이블이 가지고 있는 데이터 원형을 가지고올 때 사용한다.

```oracle   
SET SERVEROUTPUT ON;
DECLARE
    vempno NUMBER(4);
    vename VARCHAR2(20);
    vjob VARCHAR2(20);
    vhiredate DATE;
    vsal NUMBER(7,2);
BEGIN
    SELECT empno,ename,job,hiredate,sal INTO vempno,vename,vjob,vhiredate,vsal
    FROM emp
    WHERE empno=7788;
    -- 출력
    DBMS_OUTPUT.PUT_LINE('======결과======');
    DBMS_OUTPUT.PUT_LINE('사번:'||vempno);
    DBMS_OUTPUT.PUT_LINE('이름:'||vename);
    DBMS_OUTPUT.PUT_LINE('직위:'||vjob);
    DBMS_OUTPUT.PUT_LINE('입사일:'||vhiredate);
    DBMS_OUTPUT.PUT_LINE('급여:'||vsal);
END;
```

- `%ROWTYPE` : 
  - emp%ROWTYPE => emp테이블이 가지고 있는 모든 컬럼의 데이터형을 읽어 온다.

```oracle
SET SERVEROUTPUT ON;
DECLARE
    vemp emp%ROWTYPE;
BEGIN
    SELECT * INTO vemp
    FROM emp
    WHERE empno=7788;
    -- 출력
    DBMS_OUTPUT.PUT_LINE('======결과======');
    DBMS_OUTPUT.PUT_LINE('사번:'||vemp.empno);
    DBMS_OUTPUT.PUT_LINE('이름:'||vemp.ename);
    DBMS_OUTPUT.PUT_LINE('직위:'||vemp.job);
    DBMS_OUTPUT.PUT_LINE('입사일:'||vemp.hiredate);
    DBMS_OUTPUT.PUT_LINE('급여:'||vemp.sal);
END;
```


- `RECORD` : VO와 같다. 여러개의 테이블 컬럼을 받아서 처리한다. 복합변수

- `CURSOR` : ArrayList와 같다. 여러개의 ROW처리가 가능하다.
  - 스칼라변수 , %TYPE , %ROWTYPE , RECORD 는 한 개의 ROW만 처리 가능하다.

```TIP
**출력함수**
- DBMS_OUTPUT.PUT_LINE() : System.out.println()
- DBMS_OUTPUT.PUT() : System.out.print()
```



## PL/SQL Cursor(커서)

### Cursor란?
- 여러개의 Row(Record)데이터를 저장하는 공간이다.
  - 처리하고 자바에서 받을 때, ResultSet으로 받는다. 

```oracle
CURSOR emp_cur IS
  SELECT * FROM emp;
```

### Cursor 사용 방법

- 1) 커서 등록

```oracle
Cursor cur_name IS
  SELECT * FROM emp
```

- 2) open
- 3) fetch : 데이터 가져오기
- 4) close

### Cursor 실습

- 예) 

```oracle
SET SERVEROUTPUT ON;
DECLARE
    vemp emp%ROWTYPE;
    CURSOR cur IS
        SELECT * FROM emp;
BEGIN
    OPEN cur;
    DBMS_OUTPUT.PUT_LINE('========결과=========');
    LOOP
        FETCH cur INTO vemp;
        EXIT WHEN cur%NOTFOUND; -- 데이터가 없는 경우 
        DBMS_OUTPUT.PUT_LINE('사번:'||vemp.empno);
        DBMS_OUTPUT.PUT_LINE('이름:'||vemp.ename);
        DBMS_OUTPUT.PUT_LINE('직위:'||vemp.job);
        DBMS_OUTPUT.PUT_LINE('입사일:'||vemp.hiredate);
        DBMS_OUTPUT.PUT_LINE('급여:'||vemp.sal);
        DBMS_OUTPUT.PUT_LINE('=====================');
    END LOOP;
    CLOSE cur;
END;
```

- 예) for문 사용하기

```oracle
SET SERVEROUTPUT ON;
DECLARE
    vdept dept%ROWTYPE;
    CURSOR cur IS
        SELECT * FROM dept;
    
BEGIN
    FOR vdept IN cur LOOP
        DBMS_OUTPUT.PUT_LINE(vdept.deptno||' '||vdept.dname||' '||vdept.loc);
    END LOOP;
        
END;
/
```

## PL/SQL에서의 연산자
- 산술연산자 : + , - , * , / , MOD(숫자,나머지) , % (x)
  - %는 사용할 수 없다.
- 비교연산자 : = , != , <> , ^= , < , > , <= , >=
- 논리연산자 : OR , AND , NOT , !(x)
  - 부정을 표현할 때 `!`를 사용하면 안되고, NOT을 붙여야 한다.
- BETWEEN AND : 기간, 범위
- IN
- LIKE
- 제어문 : IF , IF ~ ELSE , FOR



## PL/SQL 제어문

### 조건문
- 단일 조건문 : `IF ~ ELSE`
  - 비교 연산자(= , != , < , > , <= , >=) , 논리 연산자(NOT , OR , AND)
  - 변수에 값 대입할때는 `:=`를 사용한다.

```
IF(조건문) THEN
  처리
END IF;
```

- 예)

```oracle
DECLARE
-- 선언부(변수선언)
    vename emp.ename%TYPE;
    vjob emp.job%TYPE;
    vdname dept.dname%TYPE;
    vdeptno emp.deptno%TYPE;
BEGIN
-- 구현부
-- 변수에 값 대입 => :=
    SELECT deptno,ename,job INTO vdeptno,vename,vjob
    FROM emp
    WHERE ename='KING';
    
    IF(vdeptno=10) THEN
        vdname:='개발부';
    END IF;
    IF(vdeptno=20) THEN
        vdname:='영업부';
    END IF;
    IF(vdeptno=30) THEN
        vdname:='총무부';
    END IF;
    -- 결과값 출력
    DBMS_OUTPUT.PUT_LINE('=======결과값=======');
    DBMS_OUTPUT.PUT_LINE('이름:'||vename);
    DBMS_OUTPUT.PUT_LINE('부서:'||vdname);
-- 예외처리
END;
/
```

- 예)

```ORACLE
DECLARE
-- 선언부(변수선언)
    vename emp.ename%TYPE;
    vjob emp.job%TYPE;
    vdname dept.dname%TYPE;
    vdeptno emp.deptno%TYPE;
BEGIN
-- 구현부
-- 변수에 값 대입 => :=
    SELECT deptno,ename,job INTO vdeptno,vename,vjob
    FROM emp
    WHERE ename='KING';
    
    IF(vdeptno=10) THEN
        vdname:='개발부';
    ELSIF(vdeptno=20) THEN
        vdname:='영업부';
    ELSIF(vdeptno=30) THEN
        vdname:='총무부';
    END IF;
    -- 결과값 출력
    DBMS_OUTPUT.PUT_LINE('=======결과값=======');
    DBMS_OUTPUT.PUT_LINE('이름:'||vename);
    DBMS_OUTPUT.PUT_LINE('부서:'||vdname);
-- 예외처리
END;
/
```

- 선택 조건문 : `IF ~ ELSE`
  - 연산처리 => NULL값일 때, 값이 NULL이다.
  
```
IF(조건문) THEN
  처리(조건문이 TRUE) => SQL문장
ELSE
  처리 => SQL문장
END IF;
```

- 다중 조건문 : `IF ELSIF ~ ELSIF ~ ELSE`

```
IF(조건문) THEN
  처리
ELSEIF(조건문) THEN
  처리 => SQL문장
ELSEIF(조건문) THEN
  처리 => SQL문장
ELSEIF(조건문) THEN
  처리 => SQL문장
ELSE
  처리
END IF;
```

```ORACLE
DECLARE
-- 선언부(변수선언)
    vename emp.ename%TYPE;
    vcomm emp.comm%TYPE;
    vsal emp.sal%TYPE;
BEGIN
-- 구현부
-- 변수에 값 대입 => :=
    SELECT ename,comm,sal INTO vename,vcomm,vsal
    FROM emp
    WHERE ename='MARTIN';
    -- 연산처리 => NULL값일 때, 값이 NULL이다.
    IF(vcomm>0) THEN
        dbms_output.put_line(vename||'님의 성과급은 '||vcomm||' 입니다');
    ELSE
        dbms_output.put_line(vename||'님의 성과급은 없습니다');
    END IF;
-- 예외처리
END;
/
```


- 선택문 : `CASE ~ WHEN ~ THEN`

```
CASE(조건)
  WHEN 조건 THEN 결과
  WHEN 조건 THEN 결과
  WHEN 조건 THEN 결과
ELSE
  처리
END;
```

```oracle
DECLARE
-- 선언부(변수선언)
    vempno emp.empno%TYPE;
    vename emp.ename%TYPE;
    vdeptno emp.deptno%TYPE;
    vdname dept.dname%TYPE;
BEGIN
-- 구현부
-- 변수에 값 대입 => :=
    SELECT empno,ename,deptno INTO vempno,vename,vdeptno
    FROM emp
    WHERE ename='SMITH';
    vdname:=CASE vdeptno
        WHEN 10 THEN '개발부'
        WHEN 20 THEN '총무부'
        WHEN 30 THEN '자재부'
        WHEN 40 THEN '신입'
        END;
    DBMS_OUTPUT.PUT_LINE('======결과======');
    DBMS_OUTPUT.PUT_LINE('이름:'||vename);
    DBMS_OUTPUT.PUT_LINE('사번:'||vempno);
    DBMS_OUTPUT.PUT_LINE('부서:'||vdname);
END;
/
```


### 반복문
- `WHILE`

```
WHILE 조건 LOOP
  처리
END LOOP;
```

- 예) while : 1~10까지 출력하기

```ORACLE
DECLARE
    i NUMBER:=1; -- 초기값
BEGIN
    WHILE i<=10 LOOP
        DBMS_OUTPUT.PUT_LINE(i);
        --증가식
        i:=i+1;
        END LOOP;
END;
```

- 예) while : 짝수만 출력하기

```ORACLE
DECLARE
    i NUMBER:=1; -- 초기값
BEGIN
    WHILE i<=10 LOOP
        IF(MOD(i,2)=0) THEN
        DBMS_OUTPUT.PUT_LINE(i);
        END IF;
        --증가식
        i:=i+1;
        END LOOP;
END;
/
```



- `basic (loop)` : do~while문과 같다.

```
LOOP
  출력
  증가식
  EXIT WHEN 조건 => 종료조건
END LOOP;
```

- 예) basic loop : 1~9까지 출력하기

```oracle
DECLARE
    i NUMBER:=1; -- 초기값
BEGIN
    LOOP         -- {
        DBMS_OUTPUT.PUT_LINE(i);
        i:=i+1;
        EXIT WHEN i>=10;
    END LOOP;    -- }
END;
/
```

- 예) basic loop : 짝수만 출력하기

```ORACLE
DECLARE
    i NUMBER:=1; -- 초기값
BEGIN
    LOOP         -- {
        IF(MOD(i,2)=0) THEN
        DBMS_OUTPUT.PUT_LINE(i);
        END IF;
        i:=i+1;
        EXIT WHEN i>=10;
    END LOOP;    -- }
END;
/
```

- `FOR`
  - 가장 많이 쓰인다.
  - 선언(DECLARE)이 필요없다.


```
-- 형식(1) : 코틀린 형식과 동일하다.
FOR 변수 IN low..high LOOP
  출력
END LOOP;
```

```
-- 형식(2)
FOR i IN [REVERSE] 1..9 LOOP
  처리
END LOOP;
```

- 예) for : 1~10까지 출력

 ```oracle
BEGIN
    FOR i IN 1..10 LOOP
        DBMS_OUTPUT.PUT_LINE (i);
    END LOOP;
END;
/
```

- 예) for : 정수를 입력받아서 해당 구구단을 출력하는 프로그램을 작성하시오.

```ORACLE
ACCEPT pno PROMPT '몇단?'
DECLARE
    vno NUMBER:=&pno;
BEGIN 
    FOR i IN 1..9 LOOP
        DBMS_OUTPUT.PUT_LINE(vno||'*'||i||'='||vno*i);
    END LOOP;
END;
```


## PROCEDURE


### PROCEDURE 형식

- 생성 형식 : ALTER가 없다.

```ORACLE
CREATE [OR REPLACE] PROCEDURE pro_name(
  매개변수,
  매개변수,
  매개변수....
    1) 스칼라 변수 , 2) %TYPE
)
IS (AS)
  변수 선언 (지역변수)
BEGIN
  SQL 구현 => 제어문 || 연산자 || SQL
END;
/
```


- 매개변수
  - `IN` : SQL 실행 시 필요한 데이터 (WHERE ,INSERT ,UPDATE ,DELETE)
  - `OUT` : SQL 문장을 실행하고 결과값을 받아올 때 사용한다. (SELECT)
  
- 삭제 형식

```oracle
DROP PROCEDURE pro_name;
```

- 호출 형식
  - SELECT는 EXECUTE로 호출한다.
  - INSERT , UPDATE , DELETE는 CALL로 호출한다.


### 매개변수

- IN : 내부에서만 사용하는 변수 , 값을 넣어주기만 하는 것 (INSERT , DELETE)
- OUT : Call By Reference , 결과 값을 받는 변수 (SELECT)
- INOUT : 내부사용 , 값을 받는 변수 
- 생략하면 default가 `IN 변수`임
- 일반 function은 결과값이 1개이기 때문에 사용할 수 없다.

- 예) 

```oracle
CREATE PROCEDURE empInsert(
  name IN VARCHAR2(20),
  addr IN VARCHAR2(100),
  tel IN VARCHAR2(20),
  result OUT VARCHAR2(100)
)
```

- 예) p는 c언어의 포인터와 유사한 out변수 , k는 in변수와 비슷하다

```
int* p;
void disp(int* p,int k)
(
  *p=100;
)
disp(p,10); ==> p=100
```


### PL/SQL 만들어보기

- 테이블 제작

```oracle
CREATE TABLE pl_student(
    hakbun NUMBER PRIMARY KEY,
    name VARCHAR2(34) NOT NULL,
    kor NUMBER,eng NUMBER, math NUMBER
);
```

- 값 삽입

```oracle
INSERT INTO pl_student VALUES(1,'홍길동',90,90,100);
INSERT INTO pl_student VALUES(2,'박문수',85,80,75);
COMMIT;
```

- 추가함수 작성

```oracle
CREATE OR REPLACE PROCEDURE studentInsert(
    pName pl_student.name%TYPE,
    pKor pl_student.kor%TYPE,
    pEng pl_student.eng%TYPE,
    pMath pl_student.math%TYPE
)
IS
BEGIN
    INSERT INTO pl_student VALUES(
        (SELECT NVL(MAX(hakbun)+1,1) FROM pl_student),
        pName,pKor,pEng,pMath
    );
    COMMIT;
END;
/
```

- 추가함수 호출하여 값 삽입

```oracle
CALL studentInsert('심청이',80,90,76);
```

- 데이터 하나만 상세 읽기 함수 작성

```oracle
CREATE OR REPLACE PROCEDURE studentDetailData(
    pNo NUMBER,
    pName OUT VARCHAR2,
    pKor OUT NUMBER,
    pEng OUT NUMBER,
    pMath OUT NUMBER
)
IS 
BEGIN
    SELECT name,kor,eng,math INTO pName,pKor,pEng,pMath
    FROM pl_student
    WHERE hakbun=pNo;
END;
/
```

- 데이터 하나만 상세 읽기 함수 실행

```oracle
VARIABLE pName VARCHAR2;
VARIABLE pKor NUMBER;
VARIABLE pEng NUMBER;
VARIABLE pMath NUMBER;
EXECUTE studentDetailData(1,:pName,:pKor,:pEng,:pMath);
PRINT pName;
PRINT pKor;
PRINT pEng;
PRINT pMath;
```

- 삭제하기 함수 제작

```oracle
CREATE OR REPLACE PROCEDURE studentDelete(
    pNo pl_student.hakbun%TYPE
)
IS 
BEGIN
    DELETE FROM pl_student
    WHERE hakbun=pNo;
    COMMIT;
END;
/
```

- Update하기
  - 값만 넣어주는 것은 IN 변수
  - 값을 가져오는 것은 OUT 변수
  
```ORACLE
CREATE OR REPLACE PROCEDURE studentUpdate(
    pNo NUMBER,
    pName VARCHAR2,
    pKor NUMBER,
    pEng NUMBER,
    pMath NUMBER
)
IS
    -- 변수 필요없음
BEGIN
    UPDATE pl_student SET
    name=pName,kor=pKor,eng=pEng,math=pMath
    WHERE hakbun=pNo;
    COMMIT;
END;
/
```

- totalPage 함수
  - SYS_REFCURSOR : 자바에서 받아서 처리할 수 있게 하기 위해서

```oracle
CREATE OR REPLACE PROCEDURE studentListData(
    pResult OUT SYS_REFCURSOR
)
IS
BEGIN
    OPEN pResult FOR
        SELECT * FROM pl_student;
END;
/
```

- 평균 구하기

```oracle
CREATE OR REPLACE FUNCTION studentAvg(
    pNo pl_student.hakbun%TYPE
)RETURN NUMBER
IS
    pAvg NUMBER;
BEGIN
    SELECT (kor+eng+math)/3 INTO pAvg
    FROM pl_student
    WHERE hakbun=pNo;
    
    RETURN pAvg;
END;
/
```

- 총점 구하기 



```oracle
CREATE OR REPLACE FUNCTION studentTotal(
    pNo pl_student.hakbun%TYPE
)RETURN NUMBER
IS
    pTotal NUMBER;
BEGIN
    SELECT (kor+eng+math) INTO pTotal
    FROM pl_student
    WHERE hakbun=pNo;
    
    RETURN pTotal;
END;
/
```

- 각 과목별 점수, 총점, 평균 확인하기

```oracle
SELECT name,kor,math,eng,studentAvg(hakbun),studentTotal(hakbun)
FROM pl_student;
```

## FUNCTION 
- Function은 리턴값이 있는 함수이다. 


```tip
- PROCEDURE : 리턴값이 없는 함수가 필요할 때 사용함
- FUNCTION : 리턴값이 있는 함수가 필요할 때 사용함
```

### FUNCTION 형식

  
```oracle
CREATE [OR REPLACE] FUNCTION func_name(
  매개변수 (OUT 변수 존재X)
  ...
) RETURN 데이터형 (결과값의 데이터형)
IS
  변수선언(지역변수)
BEGIN
  SQL 구현 => 쿼리문장
  RETURN 값 (결과값 보내주기)
END;
/
```

- 매개변수 
  - OUT변수가 존재하지 않는다.

```tip
**IN변수와 OUT변수**
- IN변수 : Call by value(값에 의한 호출)
- OUT변수 : Call by reference(참조에 의한 호출)
```
  
  
- RETURN  
  - IS 앞에 선언한 RETURN 데이터형과 실제 RETURN되는 결과값의 데이터형이 동일해야 한다.

### FUNCTION 실습해보기
- 예 1-1) JOIN을 통해 값 출력하기

```oracle
SELECT ename,job,dname,loc
FROM emp,dept
WHERE emp.deptno=dept.deptno;
```

- 예 1-2) SubQuery을 통해 값 출력하기

```oracle
SELECT ename,job,(SELECT dname FROM dept d WHERE d.deptno=e.deptno) as dname,
(SELECT loc FROM dept d WHERE d.deptno=e.deptno) as loc
FROM emp e;
```

- 예 1-3) Function을 통해 값 출력하기
  - 사용 : `SELECT ename,job,getDname(deptno) FROM emp;`
  
```oracle
CREATE OR REPLACE FUNCTION getDname(
    pDeptno emp.deptno%TYPE
) RETURN VARCHAR
IS
    vdname dept.dname%TYPE;
BEGIN
    SELECT dname INTO vdname
    FROM dept
    WHERE deptno=pDeptno;
    RETURN vdname;
END;
/
```

- 예 2) Function을 통해 loc값 출력하기
  - 값 조회하기 : `SELECT ename,job,getLoc(deptno) FROM emp;`
  
```oracle
CREATE OR REPLACE FUNCTION getLoc(
    pDeptno emp.deptno%TYPE
) RETURN VARCHAR
IS 
    vdname dept.dname%TYPE;
BEGIN
    SELECT loc INTO vdname
    FROM dept
    WHERE deptno=pDeptno;
    RETURN vdname;
END;
/
```

 


## PL/SQL의 런타임 구조
## PL/SQL 블록 작성 시 기본 규칙과 권장사항
## PL/SQL 문 내에서의 SQL 문장 사용하기
## PL/SQL에서의 렉시칼
## PL/SQL에서의 블록 구문 작성 지침
## 중첩된 PL/SQL 블록 작성하기
## ORACLE EXCEPTION(예외 처리)
## ORACLE SUBPROGRAM
