# [이것이 취업을 위한 코딩 테스트다 with Python] 그리디 알고리즘
## 1. 그리디란?
- 탐욕
- 현재 상황에서 가장 좋아보이는 것을 선택하는 것

## 1.1 거스름돈

### 문제 
![image](https://user-images.githubusercontent.com/66978721/102842760-0dd17980-444b-11eb-86b5-a13e5add63fb.png)


### 답안

- 파이썬

```python
n = 1260
count = 0

# 큰 단위 화폐부터 차례대로 확인하기
array = [500,100,50,10]

for coin in array:
  count += n // coin # 해당 화폐로 거슬러 줄 수 있는 동전의 갯수 세기
  n %= coin

print(count)  
```

- 자바

```java
public class change {
	 public static void main(String[] args) {
		    int n = 1260;
		    int cnt = 0;
		    int[] coinTypes = {500, 100, 50, 10};
		    
		    for(int i = 0; i < coinTypes.length; i++) { // length()가 아님
		      int coin = coinTypes[i];
		      cnt += n / coin;
		      n %= coin;
		    }
		    
		    System.out.println(cnt);
		  }
}
```

### 시간복잡도 
- 시간 복잡도는 화폐의 종류에 비례한다.
- 시간 복잡도는 거슬러줘야 하는 금액과는 무관하다.
- 화폐의 종류가 K라고 할 때, 소스코드의 시간복잡도는 O(K)이다.

### 유의사항
- 파이썬에서 몫 구하기 : `//`
- 자바에서 몫 구하기 : `/`
- 자바, 파이썬에서 나머지 구하기 : `%`

- length()와 length의 차이
  - length는 상수이고 length()는 메소드 입니다. 
  - 배열에서 사용 가능한 length는 최초 배열이 생성 될 때 길이가 결정 되는 상수 이고 
  - String의 length() 메소드는 호출 될 때 (가변적) 문자의 길이를 결정하는 변수가 됩니다.

```tip
- length()와 size()의 차이
  - length() : 배열의 전체 크기
  - size() : 리스트에 들어있는 원소의 갯수
```

## 1.2 1이 될때까지 

### 문제
![image](https://user-images.githubusercontent.com/66978721/102842760-0dd17980-444b-11eb-86b5-a13e5add63fb.png)


### 힌트
- 가능한 많이 나눌때 -1보다 더 효율적인 해법이 된다.
- n을 최대한 k의 배수로 만들어주는 것이 중요하다

### 답안

- 파이썬

```python
# N , K 공백을 기준으로 구분하여 입력 받기
n, k = map(int, input().split())

result = 0

while True:
  # n이 k로 나누어 떨어지는 수가 될때까지 1씩 빼기
  target = (n // k) * k 
  result += (n - target)
  n = target
  
  # n이 k보다 작을 때 (더 이상 나눌 수 없을 때) 반복문 탈출
  if n < k:
    break
  # k로 나누기
  result += 1
  n //= k

# 마지막으로 남은 수에 대하여 1씩 빼기
result += (n-1)
print(result)  
```

- 자바

```java
package greedy;
import java.util.*;

public class minusToOne {
	
	public static void main(String[] args) {
		
	    Scanner sc = new Scanner(System.in);
	    
	    // n,k를 공백을 기준으로 구분하여 입력 받기
	    int n = sc.nextInt();
	    int k = sc.nextInt();
	    int result = 0;
	    
	    while (true) {
	      // n 이 k로 나누어 떨어지는 수가 될 때까지 1씩 
	      int target = (n / k) * k;
	      result += (n - target);
	      // n이 k보다 작을 때 반복문 탈출
	      if(n<k) break;
	      result += 1;
	      n /= k;
	    }
	    // 마지막으로 남은 수에 대하여 1씩 빼기
	    result += (n-1);
	    System.out.println(result);
	    
	  }
	
}
```


## 1.3 곱하기 혹은 더하기

### 문제
![image](https://user-images.githubusercontent.com/66978721/102848194-8e967280-4457-11eb-9f9a-0f783e72ec64.png)

### 힌트

- 0 혹은 1은 더하기를 하는 것이 효율적이다.
- 2 이상의 수는 곱하는 것이 효율적이다.

### 답안

- 파이썬 

```python
data = input()

# 첫 번째 문자를 숫자로 변경하여 대입
result = int(data[0])

for i in range(1, len(data)):
  # 두 수 중에서 하나라도 0 혹은 1인 경우, 더하기 수행
  num = int(data[i])
  if(num <=1 or result <=1:
    result += num
  else:
    result *= num

print(result)
```

- 자바 
  - 첫번째 자리의 숫자를 구하는대서 막힘
  - charAt으로 구하면 문자를 숫자로 인식해서 이상한 숫자를 도출함


```java
package greedy;

import java.util.Scanner;

public class mulyiplyDivision {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int input = sc.nextInt();
		
		System.out.println(String.valueOf(input).charAt(0));
		
	}
}
```

