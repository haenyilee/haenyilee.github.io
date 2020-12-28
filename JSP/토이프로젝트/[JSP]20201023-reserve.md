---
sort:
---

---
### 참고
- 코드 : ()[]
---

# 예매하기

## 테이블 제작

```oracle
-- 20201023 : 영화 예매 테이블 만들기에 사용되는 테이블

CREATE TABLE movie_info
    (SELECT * FROM daum_movie WHERE title LIKE '%2020%');
    
SELECT * FROM movie_info;

ALTER TABLE movie_info ADD theater_no VARCHAR2(200);

CREATE TABLE theater_info(
    tno NUMBER PRIMARY KEY,
    tname VARCHAR2(100) NOT NULL,
    tloc VARCHAR2(100) NOT NULL
);

INSERT INTO theater_info VALUES(1,'CGV','목동');
INSERT INTO theater_info VALUES(2,'CGV','동서울');
INSERT INTO theater_info VALUES(3,'CGV','강남');
INSERT INTO theater_info VALUES(4,'CGV','일산');
INSERT INTO theater_info VALUES(5,'CGV','부천');
INSERT INTO theater_info VALUES(6,'CGV','목동');

INSERT INTO theater_info VALUES(7,'메가박스','신촌');
INSERT INTO theater_info VALUES(8,'메가박스','이대');
INSERT INTO theater_info VALUES(9,'메가박스','신사');
INSERT INTO theater_info VALUES(10,'메가박스','동대문');

INSERT INTO theater_info VALUES(11,'롯데시네마','신촌');
INSERT INTO theater_info VALUES(12,'롯데시네마','이대');
INSERT INTO theater_info VALUES(13,'롯데시네마','신사');
INSERT INTO theater_info VALUES(14,'롯데시네마','동대문');

INSERT INTO theater_info VALUES(14,'롯데시네마','동대문');
```
