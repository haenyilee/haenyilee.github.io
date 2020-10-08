# 내부 클래스

## 멤버클래스

## 익명의 클래스
- 형식
```java
class A
{
}
class B
{
}

class C extends A,B // error!
{
}

class C extends A
{
    B b = new B(){
        // 오버라이딩이 가능하다    
    }
}
```

## 내부클래스

```java
class A
{
    class B
    {
    }
}
B b = new B(); // 사용할 수 없다.
A.B.C 

class C
{
    B b = new B(); // 사용 불가
    A.B b;
}
```
