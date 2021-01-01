---
sort: 12
---


# [Java]지네릭스, 열거형, 애너테이션




## 제너릭스 타입 
> 전체의 데이터형을 변경하는 것

- <T,E,V,K> : 임시변수 , 타입 임시 데이터형

```java
class Box<T>      // 임시로 데이터형을 T라고 지정
{
  T t;
  public void setT(T t)
  {
     this.t=t;
  }
  public T getT()
  {
     return t;
  }
}
Box<String> box = new Box<String>()                   // 마지막에 데이터형을 String으로 결정
ArrayList<Integer> list = new ArrayList<Integer>();   // int는 wrapper클래스로 사용해야함을 유의!
```



