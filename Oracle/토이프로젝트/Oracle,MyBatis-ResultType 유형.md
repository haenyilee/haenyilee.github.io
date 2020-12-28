# Oracle,MyBatis-ResultType 유형

## 결과값이 1개 일때

```oracle
SELECT COUNT(*) FROM naver_shopping;
```

![image](https://user-images.githubusercontent.com/66978721/102735514-04320e00-4386-11eb-93d4-ebeab9748ba6.png)

- 결과값의 데이터형으로 받는다
  - Number => int
  - VARCHAR2 => String
  - DATE => Date 

## 결과값이 1개의 row일 때

```oracle
SELECT * FROM naver_shopping WHERE no=3;
```

![image](https://user-images.githubusercontent.com/66978721/102735822-bec21080-4386-11eb-8456-9d4e2910819b.png)

- VO로 받는다
  - EX) `ShoppingVO` <br>
  ![image](https://user-images.githubusercontent.com/66978721/102735898-eadd9180-4386-11eb-8f82-1eb03e73005b.png)


## 결과값이 여러개의 ROW일 때

```oracle
SELECT * FROM naver_shopping;
```

![image](https://user-images.githubusercontent.com/66978721/102735970-19f40300-4387-11eb-8948-20a38c12e9b3.png)

- ArrayList로 받는다.
  - EX) `List<ShoppingVO>`
