---
sort: 2
---

# 접근제한

- 클래스를 선언할 때 필드는 일반적으로 private접근 제한을 한다.
- 읽기 전용 필드가 있을 수 있다(Getter의 필요성)
- 외부에서 엉뚱한 값으로 변경할 수 없도록 한다.(Setter의 필요성)

## Getter
- private 필드의 값을 리턴하는 역할을 한다.
  - 필요한 경우 필드의 값을 가공해서 리턴한다.
- ```getFieldName()``` 또는 ```isFieldName()``` 메소드를 말한다.
  - 필드 타입이 boolean인 경우에만 ```isFieldName()```을 사용한다.
- (예문)
```java
class Car{
  private double speed;
  public double getSpeed(){return speed;} // {}안에서 가공 가능
}
```

## Setter
- 외부에서 주어진 값을 필드값으로 수정한다.
  - 필요한 경우 외부의 값을 유효성 검사한다(올바른 값이 주어졌는지 검사)
- ```setFieldName(타입 변수)``` 메소드를 말한다.
  - 매개 변수 타입은 필드의 타입과 동일하다.
- (예문)
```java
class Car{
  private double speed;
  public void setSpeed(double speed) { this.speed=speed; }
}
```
