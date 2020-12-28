---
sort: 3
---

# RETURN

- 실행 중인 메서드를 종료하고 호출한 곳으로 되돌아간다.
- 메서드가 작업을 마쳤을때 ```return;```써줘야함
  - 메서드 반환타입이 ```void```일 경우 ```return;```생략가능
  - 반환타입이 void가 아니면 생략 불가임

# 반환값
- 메서드 반환타입이 ```void```가 아닐 경우 return해주는 값을 **반환값**이라고 함
- 타입은 메서드 타입과 일치해야함
  - 메서드 타입은 클래스타입과 일치해야함
  - char,byte,short등은 int로 자동 형변환됨

- (예제) 반환값 : ```return result;```에서 result가 반환값임
```java
int add(int x,int y)
{
    int result = x+y;
    return result; 
}
```
