---
sort: 1
---

# GROUPBY

## 1. 개와 고양이

```warning
- 조건 꼼꼼히 읽기!
- 정렬 조건을 안봐서 틀림
```

```sql
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) as count 
FROM ANIMAL_INS 
GROUP BY ANIMAL_TYPE 
ORDER BY ANIMAL_TYPE;
```

## 2. 동명 동물 수 찾기

```warning
- GROUP BY의 조건은 WHERE보다는 HAVING을 사용해야 한다!
- HAVING절 뒤에 COUNT를 사용할 때는 COUNT(*)를 사용하면 안됨
```

```sql
SELECT NAME, COUNT(*)
FROM ANIMAL_INS
GROUP BY NAME
HAVING COUNT(NAME)>1
ORDER BY NAME
```

## 3. 입양시각 (1)

```warning
- DATETIME형식 변환 시에는 TO_CHAR 사용하기
```

```sql
SELECT to_char(datetime, 'hh24'), count(datetime) count
FROM animal_outs
GROUP BY to_char(datetime, 'hh24')
HAVING to_char(datetime, 'hh24')>=9 
AND to_char(datetime, 'hh24')<20
ORDER BY to_char(datetime, 'hh24')
```

## 

```warning
```

```sql
```
