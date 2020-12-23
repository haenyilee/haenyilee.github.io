# [이것이 취업을 위한 코딩 테스트다 with Python] 구현

## 구현(Implementation)
![image](https://user-images.githubusercontent.com/66978721/102961725-6460b580-4528-11eb-9569-4713447e051a.png)

- 방향 벡터
![image](https://user-images.githubusercontent.com/66978721/102961941-04b6da00-4529-11eb-90a2-b12fbc730c28.png)


## 1.1 시각

### 문제
![image](https://user-images.githubusercontent.com/66978721/102960533-7db43280-4525-11eb-8151-c3f2dc28da52.png)
![image](https://user-images.githubusercontent.com/66978721/102960655-bfdd7400-4525-11eb-8eb1-7978d79de2b6.png)

### 힌트
- 하루는 86,400초이다. : `24 * 60 * 60 = 86,400`
- 시각을 1초씩 증가시키면서 3이 포함되어 있는지 확인하면 되는 '완전탐색'유형이다.

### 코드 

- 파이썬

```python
# 시간 입력받기
h = int(input())

count = 0
for i in range(h +1):
  for j in range(60):
    for k in range(60):
      # 매 시각 안에 '3'이 포함되어 있다면 카운트 증가
      if '3' in str(i) + str(j) + str(k):
        count += 1
        
print(count)
```


- 자바

```java
public class Main(){
  public static void main(String[] args){
    Scanner sc = new Scanner
  
  }
}
```
