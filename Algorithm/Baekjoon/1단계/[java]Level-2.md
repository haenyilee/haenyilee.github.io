# [java]Level-2

## 윤년

- 윤년의 조건 : 윤년은 연도가 4의 배수이면서, 100의 배수가 아닐 때 또는 400의 배수일 때이다.

- 코드

```java
if(year%4==0 && (year%100!=0 || year%400==0))
            System.out.println("윤년이다.");
        else
            System.out.println("윤년이 아니다");
```
