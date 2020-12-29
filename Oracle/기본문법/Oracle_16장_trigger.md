---
sort: 17
---

# Trigger

## 트리거란?
- 데이터베이스에 미리 정해 놓은 조건에 만족하면 자동으로 이벤트를 처리하도록 하는 기능
- 오라클 내에서 처리되기 때문에 자바에서는 처리 코드를 작성하지 않는다. 
  - 트리거가 많이 쓰이면 웹 개발자들은 작동원리를 알 수 없다는 단점이 있다. 
- 트리거에서는 DML만 사용 가능하다. (INSERT , UPDATE , DELETE)


```tip
- TRIGGER 는 AUTOCOMMIT(O)
- FUCNTION , PROCEDURE는 AUTOCOMMIT(X)
```

## 트리거의 형식

### 트리거 생성

```ORACLE
CREATE [OR REPLACE] TRIGGER tri_name
BEFORE|AFTER (INSERT , UPDATE , DELETE) ON table_name
BEGIN
  TRIGGER 처리 (=> 다른 테이블 처리)
END;
/
```

### 트리거 삭제

```oracle
DROP TRIGGER trigger_name
```

### 트리거 수정
- 수정과 동시에 생성

```oracle
ALTER TRIGGER trigger_name
```

## 재고관리로 보는 트리거 예시

### 테이블 제작

- 상품

```ORACLE
CREATE TABLE 상품(
  품번 NUMBER,
  항목명 VARCHAR2(100),
  단가 NUMBER
);
INSERT INTO 상품 VALUES(100,'새우',1500);
INSERT INTO 상품 VALUES(200,'감자',600);
INSERT INTO 상품 VALUES(300,'고구마',5000);
INSERT INTO 상품 VALUES(400,'호박',3500);
INSERT INTO 상품 VALUES(500,'오징어',2200);
```


- 입고

```ORACLE
CREATE TABLE 입고(
  품번 NUMBER,
  수량 NUMBER,
  금액 NUMBER
);
INSERT INTO 입고 VALUES(100,2,1500);
```

- 출고

```ORACLE
CREATE TABLE 출고(
  품번 NUMBER,
  수량 NUMBER,
  금액 NUMBER
);
```

- 재고

```ORACLE
CREATE TABLE 재고(
  품번 NUMBER,
  수량 NUMBER,
  금액 NUMBER
);
INSERT INTO 재고 VALUES(100,2,3000);
```

### 입고 트리거 제작
- 있던 상품이면 수량과 금액만 바꾸고(UPDATE) , 없는 상품이면 전부 INSERT시키기
- 새로 입력된 값을 표기할 때는 `:NEW.컬럼명`으로 표기한다.

```ORACLE
CREATE OR REPLACE TRIGGER 입고_trigger
AFTER INSERT ON 입고
FOR EACH ROW
DECLARE
    v_cnt NUMBER;
BEGIN
    SELECT COUNT(*) INTO v_cnt
    FROM 재고
    WHERE 품번=:NEW.품번;
    -- :NEW.품번 : 입고에서 INSERT되어 들어온 값이 NEW임
    -- 품번 : 재고 테이블에 존재하던 품번
    IF(v_cnt=0) THEN -- 새로운 상품이 들어왔다면?
        INSERT INTO 재고 VALUES(:NEW.품번,:NEW.수량,:NEW.금액*:NEW.수량);
    ELSE -- 재고에 있는 상품이 들어왔다면?
        UPDATE 재고 SET
        수량=수량+:NEW.수량,
        금액=금액+(:NEW.금액*:NEW.수량)
        WHERE 품번=:NEW.품번;
    END IF;
END;
/
```

- 입고시켜 보기

```ORACLE
SELECT * FROM 재고;
INSERT INTO 입고 VALUES(100,3,1500);
COMMIT;
SELECT * FROM 재고;
```

### 출고 트리거 제작
- 수량이 0이라면 DELETE시켜야 한다.

```ORACLE
CREATE OR REPLACE TRIGGER 출고_Trigger
AFTER INSERT ON 출고 
FOR EACH ROW 
DECLARE 
    v_cnt NUMBER;
    /*
        재고 100 5 7500
        출고 INSERT INTO 출고 VALUES(100,3,1500)
        재고 100 2 3000
        출고 INSERT INTO 출고 VALUES(100,2,1500)
        재고 100 0 0 => DELETE
    */
BEGIN
    SELECT 수량-:NEW.수량 INTO v_cnt
    FROM 재고 
    WHERE 품번=:NEW.품번;
    IF(v_cnt=0) THEN
        DELETE FROM 재고
        WHERE 품번=:NEW.품번;
    ELSE
        UPDATE 재고 SET
        수량=수량-:NEW.수량,
        금액=금액-(:NEW.수량*:NEW.금액)
        WHERE 품번=:NEW.품번;
    END IF;
END;
/
```

- 출고시켜 보기

```ORACLE
SELECT * FROM 재고;
INSERT INTO 출고 VALUES(200,1,1000);
SELECT * FROM 재고;
```


```tip
- 존재하는 테이블 확인하기 : `SELECT * FROM tab;`
```

## 영화 댓글로 보는 트리거 예시

- 댓글(트리거용) 테이블 제작

```oracle
```

- 댓글 작성 시, hit수 증가시키는 trigger 제작

```oracle
CREATE OR REPLACE TRIGGER movie_trigger
AFTER INSERT ON trigger_reply
FOR EACH ROW
BEGIN
    UPDATE daum_movie SET 
    hit=hit+1
    WHERE no=:NEW.mno;
END;
/
```
