# [이것이 취업을 위한 코딩 테스트다 with Python] 2. 구현

## 2. 구현(Implementation)
- 행렬 <br>
![image](https://user-images.githubusercontent.com/66978721/102961725-6460b580-4528-11eb-9569-4713447e051a.png)

- 방향 벡터 <br>
![image](https://user-images.githubusercontent.com/66978721/102961941-04b6da00-4529-11eb-90a2-b12fbc730c28.png)


## 2.1 시각

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
package Implementation;
import java.util.*;

public class Clock {

    // 특정한 시각 안에 '3'이 포함되어 있는지의 여부
    public static boolean check(int h, int m, int s) {
        if (h % 10 == 3 || m / 10 == 3 || m % 10 == 3 || s / 10 == 3 || s % 10 == 3)
            return true;
        return false;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // H를 입력받기 
        int h = sc.nextInt();
        int cnt = 0;

        for (int i = 0; i <= h; i++) {
            for (int j = 0; j < 60; j++) {
                for (int k = 0; k < 60; k++) {
                    // 매 시각 안에 '3'이 포함되어 있다면 카운트 증가
                    if (check(i, j, k)) cnt++;
                }
            }
        }

        System.out.println(cnt);
    }

}
```

## 2.2 상하좌우

### 문제

![image](https://user-images.githubusercontent.com/66978721/102964274-9117cb80-452e-11eb-9131-e57e89ae719d.png)
![image](https://user-images.githubusercontent.com/66978721/102964375-cb816880-452e-11eb-97a4-78a921ca66d3.png)
![image](https://user-images.githubusercontent.com/66978721/102964397-de943880-452e-11eb-902b-aae4a20f8ecc.png)

### 힌트
- 일련의 명령에 따라서 충실히 구현하면 되는 문제이다. (시뮬레이션 유형)

### 답안
- 파이썬

```python
# n 입력받기
n = int(input())
x, y = 1, 1
plans = input().split()

# L, R, U, D에 따른 이동 방향
dx = [0, 0, -1, 1]
dy = [-1, 1, 0, 0]
moves_types = ['L', 'R', 'U', 'D']

# 이동 계획을 하나씩 확인하기
for plan in plans:
  # 이동 후 좌표 구하기
  for i in range(len(move_types)):
    for i in range(len(move_types)):
      if plan == move_types[i]:
        nx = x + dx[i]
        ny = y + dy[i]

  # 공간을 벗어나는 경우 무시
  if nx < 1 or ny < 1 or nx > n or ny >n:
    continue
  # 이동 수행
    x, y = nx, ny
    
print(x,y)
```

- 자바

  - nx, ny를 -1로 설정한 이유? L , R , U , D 이외의 문자가 들어왔을 때 대처하기 위해서???

```java
package Implementation;

import java.util.Scanner;

public class Direction {
	public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // N을 입력받기
        int n = sc.nextInt();
        sc.nextLine(); // 버퍼 비우기
        String[] plans = sc.nextLine().split(" ");
        int x = 1, y = 1;

        // L, R, U, D에 따른 이동 방향 
        int[] dx = {0, 0, -1, 1};
        int[] dy = {-1, 1, 0, 0};
        char[] moveTypes = {'L', 'R', 'U', 'D'};

        // 이동 계획을 하나씩 확인
        for (int i = 0; i < plans.length; i++) {
            char plan = plans[i].charAt(0);
            // 이동 후 좌표 구하기 
            int nx = -1, ny = -1;
            for (int j = 0; j < 4; j++) {
                if (plan == moveTypes[j]) {
                    nx = x + dx[j];
                    ny = y + dy[j];
                }
            }
            // 공간을 벗어나는 경우 무시 
            if (nx < 1 || ny < 1 || nx > n || ny > n) continue;
            // 이동 수행 
            x = nx;
            y = ny;
        }

        System.out.println(x + " " + y);
    }

}
```

### 기타








## 2.2 상하좌우

### 문제

### 힌트




### 답안
- 파이썬

```python
```

- 자바

```java
```

### 기타
 


