---
sort: 11
---

# 제약조건

## 테이블 생성에 필요한 요소 
## 1. 테이터형
  - 문자 : 자바에서 String 으로 매칭한다.
    - CHAR (1~2000byte) - 고정바이트이기 때문에 글자수가 동일할 때 주로 설정한다
    - VARCHAR2 (1~4000byte) - 가변바이트, 일반적으로 사용하는 문자를 저장한다.
    - CLOB (4G) - 가변바이트 , 줄거니라 내용설명 등 긴 내용의 데이터를 저장한다.

  - 숫자
    - NUMEBR(정수값) : 실제 정수 
    - NUMBER(정수값,정수값) : 정수 , 실수 저장
    - NUMBER : 14자리까지 저장한다.
  
  - 날짜
    - DATE : 시스템의 시간
    - TIMESTAMP : DATE 확장형(1/100만 초까지 저장 가능)

  - 기타 : 4G까지 저장 가능
    - BLOB : 동영상 , 그림 , 사진을 바이너리(2진법)으로 저장
    - BFILE : 동영상 , 그림 , 사진을 파일(File)형태로 저장

## 2. 제약조건
- 오라클은 정형화된 데이터만 저장하기 때문에 사이트에 필요한 데이터만 저장할 수 있도록 하는 것이 제약조건이다.



### ```NOT NULL```
- 데이터에 NULL값을 허용하지 않는다.
- NULL값을 허용하지 않기 떄문에 반드시 입력값을 추가해야한다.
- 컬럼레벨 (컬럼뒤)에 작성해야 한다.
  - 컬럼레벨 : 컬럼과 동시에 제약조건 생성

```
-- 형식(1)
컬럼명 데이터형 NOT NULL
```

```
-- 형식(2) : 권장사항 => 명칭을 통해 제약조건에 대한 수정과 삭제가 용이하다
컬럼명 데이터형 CONSTRAINT 제약조건명 NOT NULL
```

```
-- 형식 (3) : 표 제작 후에 추가하기
ALTER TABLE 테이블명 MODIFY 컬럼명 CONSTRAINT 제약조건명 NOT NULL
```

```
-- 예시
name VARCHAR2(34) NOT NULL
```

### ```UNIQUE```
- 유일값 , 중복이 없는 값 (NULL 허용)
- 후보키 (이메일 , 전화번호 , 주민번호)
- 컬럼을 다 생성한 후 테이블 마지막에 첨부하는 것을 권장한다.

```
-- 형식 (1)
컬럼명 데이터형 UNIQUE
```
```
-- 형식 (2)
컬럼명 데이터형 CONSTRAINT 제약조건명(=테이블명_컬럼명_uk) UNIQUE(컬럼명)
```
```
-- 형식 (3)
컬럼명 데이터형, 
CONSTRAINT 제약조건명(=테이블명_컬럼명_uk) UNIQUE(컬럼명)
```

```
-- 형식 (4) : 테이블 제작 후 제약조건 설정
ALTER TABLE 테이블명 ADD 컬럼명 CONSTRAINT 제약조건명(=테이블명_컬럼명_uk) UNIQUE(컬럼명)
```

```
email VARCHAR2(200) UNIQUE
```

### ```PRIMARY KEY``` 
- NOT NULL + UNIQUE = NULL값 허용X + 중복 허용X
- 일반적으로 숫자에 사용 : 게시물번호 , 영화번호 => MAX+1, SEQUENCE(자동증가번호)
- 예외적으로 ID가 대표적인 PRIMARY KEY 
- 권장사항 : 모든 테이블은 PRIMARY KEY를 한 개 이상 가지고 있다.
  - 이상 현상 방지(무결성)
  - 수정 OR 삭제 시, => 원치 않는 데이터가 적용됨
- 컬럼을 다 생성한 후 테이블 마지막에 첨부하는 것을 권장한다.

```
-- 형식 (1)
컬럼명 데이터형 PRIMARY KEY
```

```
-- 형식 (2)
컬럼명 데이터형 CONSTRAINT 제약조건명(=테이블명_컬럼명_pk) PRIMARY KEY(컬럼명)
```

```
-- 형식 (3) : 테이블 제작 후 제약조건 설정
ALTER TABLE 테이블명 ADD 컬럼명 CONSTRAINT 제약조건명(=테이블명_컬럼명__pk) PRIMARY KEY(컬럼명)
```


```
id VARCHAR2(20) PRIMARY KEY
```

### ```FOREIGN KEY```
- 외래키 , 참조키
- 참조하는 다른 테이블의 컬럼값을 벗어나면 안됨
- 게시판 , 댓글 
- 정규화를 통해 테이블을 여러 개 만들 때 JOIN 기준이 되는 컬럼
- 어떤 값을 참조할지 ```REFERENCE```를 꼭 함께 작성해야 함
- 컬럼을 다 생성한 후 테이블 마지막에 첨부하는 것을 권장한다.

```
컬럼명 데이터형 CONSTRAINT 제약조건명 FOREIGN KEY(컬럼명)
REFERENCES 참조할 테이블명(칼럼명)
```



```
deptno NUBER(2) FOREIGN KEY
REFERENCES dept(deptno)
```

### ```CHECK```
- 지정된 데이터만 첨부
- 입력시에 라디오버튼 , 콤보박스를 사용할 때 활용한다.
- 부서명, 직위에 활용
- 컬럼을 다 생성한 후 테이블 마지막에 첨부하는 것을 권장한다.

```
-- 형식 (1)-1
컬럼명 데이터형 CHECK(컬럼명 IN(10,20,30))
```

```
-- 형식 (1)-2
컬럼명 데이터형 CHECK(컬럼명>0)
```

```
-- 형식 (2) : 권장사항(제약조건에 이름을 붙여야 수정,삭제에 용이함)
컬럼명 데이터형 CONSTRAINT 제약조건명 CHECK(컬럼명>0)
```





```
sex VARCHAR2(4) CHECK(sex IN('남자' ,'여자'));
```

### ```DEFAULT```
- 저장값이 없는 경우에 자동으로 설정된 값을 첨부

- 컬럼레벨 (컬럼뒤)에 작성해야 한다.
  - 컬럼레벨 : 컬럼과 동시에 제약조건 생성

```
regdate DATE DEFAULT SYSDATE
```


### 제약조건 연습
#### 1. 이름 (제약조건) 부여 : 테이블명_컬럼명_제약조건의 약자
  - PK : PRIMARY KEY
  - NN : NOT  NULL
  - CK : CHECK
  - FK : FOREIGN KEY
  - UK : UNIQUE
```
CREATE TABLE dept_test(
    deptno NUMBER(2),
    dname VARCHAR2(20),
    loc VARCHAR2(20),
    CONSTRAINT dept_deptno_pk PRIMARY KEY(deptno),
    CONSTRAINT dept_dname_ck CHECK(dname IN('ACCOUNTING','RESEARCH','OPERATIONS','SALES')),
    CONSTRAINT dept_loc_ck CHECK(loc IN('NEW YORK','CHICAGO','BOSTON','DALLAS'))
);
```

#### 2. 제약조건 제작 순서 : NOT NULL => DEFAULT => PRIMARY => FOREIGN => CHECK
```
CREATE TABLE emp_test(    
    empno NUMBER(4),
    ename VARCHAR2(34) CONSTRAINT emp_enmae_nn NOT NULL,
    job VARCHAR2(20),
    mgr NUMBER(4),
    hiredate DATE DEFAULT SYSDATE,
    sal NUMBER(7,2) CONSTRAINT emp_sal_nn NOT NULL,
    comm  NUMBER(7,2),
    deptno NUMBER(2),
    CONSTRAINT emp_empno_pk PRIMARY KEY(empno),
    CONSTRAINT emp_mgr_fk FOREIGN KEY(mgr)
    REFERENCES emp_test(empno),
    CONSTRAINT emp_deptno_fk FOREIGN KEY(deptno)
    REFERENCES dept_test(deptno),
    CONSTRAINT emp_job_ck CHECK(job IN('CLERK','SALESMAN','PRESIDENT','MANAGER','ANALYST'))
);
```

#### 3. freeboard

- 조건
|freeboard|||| | | | ||
|---|-|----- |----|---- |---- |----  |-----  |----|
|컬럼명|no|name  |email|  subject  |content  |pwd   |regdate   |hit|
|PK/FK/NN|PK | NN   |UK   |NN   |NNN  |NN  |DEFAULT SYSDATE|DEFAULT 0|
|데이터형|NUMBER|VAR(34)|VAR(200)|VAR(4000)|CLOB|VAR(10)|DATE|NUMBER|

- 조건(제약조건)에 맞게 테이블 제작
```
DROP TABLE freeboard;

CREATE TABLE freeboard(
no NUMBER,
name VARCHAR2(34) CONSTRAINT fb_name_nn NOT NULL,
email VARCHAR2(200),
subject VARCHAR2(4000) CONSTRAINT fb_subject_nn NOT NULL,
content CLOB CONSTRAINT fb_content_nn NOT NULL,
pwd VARCHAR2(10) CONSTRAINT fb_pwd_nn NOT NULL,
regdate DATE DEFAULT SYSDATE,
hit NUMBER DEFAULT 0,
CONSTRAINT fb_no_pk PRIMARY KEY(no),
CONSTRAINT fb_email_uk UNIQUE(email)
);
```


- 제작된 테이블에 데이터 삽입

```
INSERT INTO freeboard(no,name,email,subject,content,pwd)
VALUES(1,'홍길동','hong1@sist.co.kr','오라클 제약조건','제약조건=>게시판','1234');
INSERT INTO freeboard(no,name,email,subject,content,pwd)
VALUES(2,'홍길동','hong2@sist.co.kr','오라클 제약조건','제약조건=>게시판','1234');
INSERT INTO freeboard(no,name,email,subject,content,pwd)
VALUES(3,'홍길동','hong3@sist.co.kr','오라클 제약조건','제약조건=>게시판','1234');
INSERT INTO freeboard(no,name,email,subject,content,pwd)
VALUES(4,'홍길동','hong4@sist.co.kr','오라클 제약조건','제약조건=>게시판','1234');
INSERT INTO freeboard(no,name,email,subject,content,pwd)
VALUES(5,'홍길동','hong5@sist.co.kr','오라클 제약조건','제약조건=>게시판','1234');
```

- 오류 => 매개변수가 8개 넣겠다고 했는데 6개만 넣었으니까 오류
```
INSERT INTO freeboard VALUES(6,'홍길동','hong6@sist.co.kr','자유 게시판','게시판 내용 설정','1234');

ERROR at line 1:
ORA-00947: not enough values
```


#### 4. notnull 제약조건


```
CREATE TABLE notnull_test(
no NUMBER,
name VARCHAR2(100) CONSTRAINT nt_name_nn NOT NULL,
CONSTRAINT nt_no_pk PRIMARY KEY(no)
);

INSERT INTO notnull_test VALUES(1,'홍길동');
-- INSERT INTO notnull_test VALUES(1,null);
INSERT INTO notnull_test VALUES(2,'심청이');
```

- 데이터가 있는 상태에서 NOT NULL인 컬럼을 추가하면 오류발생
```
-- 컬럼추가(오류)
ALTER TABLE notnull_test ADD addr VARCHAR2(100) NOT NULL;

ERROR at line 1:
ORA-01758: table must be empty to add mandatory (NOT NULL) column
```

- 값을 넣고 싶다면 디폴트값을 지정해야함
```
-- 컬럼추가(디폴트값 지정)
ALTER TABLE notnull_test ADD addr VARCHAR2(100) DEFAULT '서울' NOT NULL;
```
- UNIQUE로 지정해도 됨 (null값이 가능하기 때문에)
```
ALTER TABLE notnull_test ADD tel VARCHAR2(100) UNIQUE;
```

- CHECK도 null값 허용함
```
ALTER TABLE notnull_test ADD sex VARCHAR2(10) CHECK(sex IN('남자','여자'));
```




#### 5. DDL 테이블 생성 연습
- DDL 테이블 생성 연습(1)
```
CREATE TABLE 제품(
    제품번호 VARCHAR2(12),
    제품명 VARCHAR2(100),
    제품단가 NUMBER CONSTRAINT 제품_제품단가_nn NOT NULL,
    CONSTRAINT 제품_제품번호_pk PRIMARY KEY(제품번호),
    CONSTRAINT 제품_제품명_uk UNIQUE(제품명),
    CONSTRAINT 제품_제품단가_ck CHECK(제품단가>0)
);
```


- DDL 테이블 생성 연습(2)
```
CREATE TABLE 전표상세(
    전표번호 VARCHAR2(12),
    제품번호 VARCHAR2(12),
    수량 NUMBER CONSTRAINT 전표상세_수량_nn NOT NULL,
    단가 NUMBER CONSTRAINT 전표상세_단가_nn NOT NULL,
    금액 NUMBER CONSTRAINT 전표상세_금액_nn NOT NULL,
    CONSTRAINT 전표상세_전표번호_pk PRIMARY KEY(전표번호),
    CONSTRAINT 전표상세_제품번호_fk FOREIGN KEY(제품번호)
    REFERENCES 제품(제품번호),
    CONSTRAINT 전표상세_금액_ck CHECK(금액>0)
);
```

- DDL 테이블 생성 연습(3)
```
CREATE TABLE 판매전표(
    전표번호 VARCHAR2(12),
    판매일자 DATE CONSTRAINT 판매전표_판매일자_nn NOT NULL,
    고객명 VARCHAR2(34) CONSTRAINT 판매전표_고객명_nn NOT NULL,
    총액 NUMBER,
    CONSTRAINT 판매전표_전표번호_pk PRIMARY KEY(전표번호),
    CONSTRAINT 판매전표_전표번호_fk FOREIGN KEY(전표번호)
    REFERENCES 전표상세(전표번호),
    CONSTRAINT 판매전표_총액_ck CHECK(총액>0)
);
```


#### 6. DDL 테이블 삭제 순서
- 삭제할때도 참조받는 것 순서대로 삭제해야함
  - 표 만드는 순서 : 제품 > 전표상세 > 판매전표
  - 삭제하는 순서 : 판매전표 > 전표상세 > 제품

```
DROP TABLE 판매전표;
DROP TABLE 전표상세;
DROP TABLE 제품;
```

#### 6. 제약조건 확인
- 연습(1) : 제약조건 사용된 컬럼명 확인하기
```
SELECT owner,constraint_name,table_name,constraint_type,column_name 
FROM user_cons_columns 
WHERE table_name='전표상세';
```

- 연습(2) : 제약조건 타입 확인하기
```
SELECT owner,constraint_name,constraint_type,status 
FROM user_constraints 
WHERE table_name='전표상세';
```
- C : CHECK , NOT NULL
- P : PRIMARY KEY
- R : FOREIGN KEY



#### 7. 표 생성 후 제약조건 설정하기

- DDL 테이블 생성 후 제약조건 걸기 연습(1)
  - MODIFY : NOT NULL
  - ADD : NOT NULL외 기타
```
CREATE TABLE 제품(
    제품번호 VARCHAR2(12),
    제품명 VARCHAR2(100),
    제품단가 NUMBER
);
```
```
ALTER TABLE 제품 MODIFY (제품단가 CONSTRAINT 제품_단가_nn NOT NULL);
ALTER TABLE 제품 ADD CONSTRAINT 제품_번호_pk PRIMARY KEY(제품번호);
ALTER TABLE 제품 ADD CONSTRAINT 제품_제품명_uk UNIQUE(제품명);
ALTER TABLE 제품 ADD CONSTRAINT 제품_단가_ck CHECK(제품단가>0);
```


- DDL 테이블 생성 후 제약조건 걸기 연습(2)
```
CREATE TABLE 전표상세(
    전표번호 VARCHAR2(12),
    제품번호 VARCHAR2(12),
    수량 NUMBER,
    단가 NUMBER,
    금액 NUMBER
);
```
```
ALTER TABLE 전표상세 ADD CONSTRAINT 전표_제품번호_fk FOREIGN KEY (제품번호) 
REFERENCES 제품(제품번호);
```

- DDL 테이블 생성 후 제약조건 걸기 연습(3)
```
CREATE TABLE 판매전표(
    전표번호 VARCHAR2(12),
    판매일자 DATE,
    고객명 VARCHAR2(34),
    총액 NUMBER
);
```






## 3. 같은 데이터베이스(XE)에서는 테이블은 유일값이다.

## 4. 테이블명 제작
- 시작은 문자(영문 , 한글)로 시작한다
- 숫자를 사용이 가능하다. (앞에 사용할 수 없다)
- 테이블명은 30byte(한글 15글자)까지 사용이 가능하다.
- 영문 사용 시에 테이블명은 **대문자**로 저장된다.
- 특수문자는 단어가 2개 이상일 때 주로 ```_```로 연결됨
- 키워드는 사용할 수 없다. (SELECT , FROM...)

