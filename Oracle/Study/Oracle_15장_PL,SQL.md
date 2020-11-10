---
sort: 16
---

# PL,SQL

## PL/SQL이란
- PROCEDURE, FUNCTION, PACKAGE , TRIGGER를 제작할 때 사용하는 언어이다.
- 사용자 정의로 사용한다.
- 재사용하기 위함이 목적이다.




## PL/SQL 기본 구조
- PROCEDURE : 리턴형이 없는 자바스크립트 언어와 같음
- FUNCTION : 리턴형이 있는 함수
  - 함수 : 독립(C언어)
  - 메소드 : 클래스 종속
- PACKAGE : 관련된 PROCEDURE , FUNCTION 을 모아서 둔 곳
- TRIGGER : 이미 지정된 이벤트 발생시에 자동 처리


## PL/SQL BLOCK 기본 구성

- SELECT에서 실행된 결과값을 받을 때, INTO를 사용한다.
- 변수에 값 설정 할 때, `:=`를 사용한다.

```
-- 형식
선언부
  변수 선언
구현부
  SQL문장
예외처리부 (생략이 가능)
```

```ORACLE
DECLARE
-- 선언부(변수선언)
BEGIN
-- 구현부
-- 예외처리
END;
/
```

- 변수선언
- 제어문
- SQL 구현방식
- 예외처리


## PL/SQL에서 변수의 의미와 사용법
### 변수
- 지역변수
- 매개변수 : SUBSTR('',1,2) MAX(컬럼명)
### 변수 사용법
- 스칼라변수 : 일반 변수(NUMBER,VARCHAR2,CLOB,DATE)
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
END;SET SERVEROUTPUT ON;
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
  
 
- %TYPE : 원형 (다른 테이블의 원형)
  - emp.ename%TYPE => ename이 가지고 있는 데이터형을 가지고 온다.

```oracle   
SET SERVEROUTPUT ON;
DECLARE
    vempno emp.empno%TYPE;
    vename emp.ename%TYPE;
    vjob emp.job%TYPE;
    vhiredate emp.hiredate%TYPE;
    vsal emp.sal%TYPE;
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
END;SET SERVEROUTPUT ON;
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

- %ROWTYPE
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


- RECORD : VO와 같다. 여러개의 테이블 컬럼을 받아서 처리한다. 

- CURSOR : ArrayList와 같다. 여러개의 ROW처리가 가능하다.
  - 스칼라변수 , %TYPE , %ROWTYPE , RECORD 는 한 개의 ROW만 처리 가능하다.

```TIP
**출력함수**
- DBMS_OUTPUT.PUT_LINE() : System.out.println()
- DBMS_OUTPUT.PUT() : System.out.print()
```

## PL/SQL 제어문 사용법

### 조건문
- 단일 조건문
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

- 선택 조건문
  - 연산처리 => NULL값일 때, 값이 NULL이다.
  
```
IF(조건문) THEN
  처리(조건문이 TRUE) => SQL문장
ELSE
  처리 => SQL문장
END IF;
```

- 다중 조건문

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


- 선택문

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
- while

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



- basic (loop) : do~while문과 같다.

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

- **for**
  - 가장 많이 쓰인다.
  - 선언(DECLARE)이 필요없다.


```
-- 형식(1) : 코틀린 형식과 동일하다.
FOR counter IN 1..9 LOOP
  출력
END LOOP;
```

```
-- 형식(2)
FOR i IN[REVERSE] 1..9 LOOP
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




## PL/SQL에서의 연산자 사용하기
- 비교연산자 : = , != , < , > , <= , >=
- 논리연산자 : OR , AND , NOT
- 제어문 : IF , IF ~ ELSE , FOR


## PL/SQL Cursor(커서)
- 여러개의 데이터를 저장하는 공간

```
CURSOR emp_cur IS
  SELECT * FROM emp;
```

## PL/SQL의 런타임 구조
## PL/SQL 블록 작성 시 기본 규칙과 권장사항
## PL/SQL 문 내에서의 SQL 문장 사용하기
## PL/SQL에서의 렉시칼
## PL/SQL에서의 블록 구문 작성 지침
## 중첩된 PL/SQL 블록 작성하기
## ORACLE EXCEPTION(예외 처리)
## ORACLE SUBPROGRAM
