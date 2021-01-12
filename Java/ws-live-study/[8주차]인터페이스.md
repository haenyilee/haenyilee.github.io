# [8주차]인터페이스

## 목차
- 인터페이스 정의하는 방법
- 인터페이스 구현하는 방법
- 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
- 인터페이스 상속
- 인터페이스의 기본 메소드 (Default Method), 자바 8
- 인터페이스의 static 메소드, 자바 8
- 인터페이스의 private 메소드, 자바 9


## 인터페이스란?
- 일종의 추상클래스이다.
- 추상클래스보다 추상화 정도가 높다.
- 추상클래스가 미완성 설계도라면 인터페이스는 기본 설계도이다. 

### 추상클래스와 인터페이스의 차이점
- 사용의도의 차이점이 있다. 

```note
- 추상 클래스는 IS-A : "~이다"
- 인터페이스는 HAS-A : "~을 할 수 있는"
```

![image](https://user-images.githubusercontent.com/66978721/104103569-24e6e700-52e6-11eb-9cb0-79417b822fe9.png)

- 인간은 생명체이다, 동물은 생명체이다 : 인간과 동물은 생명체라는 추상클래스를 상속한다.
- 인간인 케빈은 말할 수 있고, 수영할 수 있는 프로그래머이다 : 말하고, 프로그래머이고, 수영할 수 있는 기능은 인터페이스로 구현한다.

## 인터페이스 정의하는 방법
- class 대신 `interface` 키워드를 통해 define한다. 


```java
public interface Centered {
    void setCenter(double x, double y);
    double getCenterX();
    double getCenterY();
}
```

- 접근제어자로 public 또는 default를 사용할 수 있다.
  - public 을 선언하면 다른 패키지에서도 접근 가능하다.
  - 접근지시어가 없으면 같은 패키지 내에서만 접근가능하다. (default)


- 모든 멤버변수는 `public static final` 이여야 하며, 이를 생략할 수 있다.
  - 생략 시 컴파일러가 자동추가


- 모든 메서드는 `public abstract` 이어야 하며, 이를 생략할 수 있다.
  - 단, JDK1.8부터 static 메서드와 default 메서드는 추가를 허용한다.
  - 생략 시 컴파일러가 자동추가


- interface상수는 static 상수에 접근하는 것과 유사하게 한 클래스 안에서 접근할 수 있다.

```java
public interface MyInterface {

    public String hello = "Hello";

    public void sayHello();
}

System.out.println(MyInterface.hello);
```


## 인터페이스 구현하는 방법(implementing)
- 인터페이스도 추상클래스와 같이 그 자체만으로는 인스턴스를 생성할 수 없다.
- 자신에게 정의된 추상메서드의 몸통을 만들어주는 클래스를 작성해야 하는데 이를 **구현**이라고 한다.

- 인터페이스는 `implements` 키워드를 통해 구현한다.

- 만약 클래스가 한 개 이상의 interface를 implement하면, 반드시 인터페이스의 모든 메소드가 구현되어야 한다.

```java
interface Centered {
    public void setCenter(double x, double y);
    public double getCenterX();
    public double getCenterY();
}

public class CenteredRectangle implements Centered{
    @Override
    public void setCenter(double x, double y) {
        
    }

    @Override
    public double getCenterX() {
        return 0;
    }

    @Override
    public double getCenterY() {
        return 0;
    }
}
```

```warning
- public interface와 public class 가 동시에 선언되면 오류가 난다
- 왜?
  - 한 파일에 여러 클래스를 선언할 경우, public 클래스는 하나만 있어야 하기 때문이다.
```

- 구현하는 interface 중 일부만 구현한다면 class 앞에 abstract을 붙여서 추상클래스로 선언해야 한다.

```java
public abstract class AbstractRectangularShape implements RectangularShape { 
  // The position and size of the shape 
  protected double x, y, w, h; 
  
  // Default implementations of some of the interface methods 
  public void setSize(double width, double height) { 
    w = width; h = height; 
  } 
  public void setPosition(double x, double y) { 
    this.x = x; this.y = y; 
  } 
  public void translate (double dx, double dy) { 
    x += dx; y += dy; } 
  } 
```

```tip
- 인터페이스는 어떤 기능 또는 행위를 하는데 필요한 메서드를 제공하기 때문에 '~able'로 끝나는 이름이 많다.
```

## 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
- 인터페이스는 인터페이스를 구현한 클래스의 조상이라고 할 수 있다. 
- 그러므로, 해당 인터페이스 타입의 참조변수로 이를 구현 클래스의 인터페이스를 참조할 수 있고 해당 인터페이스 타입으로 형변환도 가능하다.

```java
// Fightable 인터페이스를 Fighter가 구현한 경우
Fightable f = (Fightable) new Fighter();

Fightable f = new Fighter();
```

- 인터페이스는 메서드의 매개변수 타임으로 사용될 수도 있다. 
  - 메서드 호출 시, 해당 인터페이스를 구현한 클래스의 인스턴스를 매개변수로 제공해야 한다는 의미이다.

- 메서드의 리턴타입으로 인터페이스의 타입을 지정하는 것도 가능하다. 
  - 리턴타입이 인터페이스라는 것은 메서드가 해당 인터페이스를 구현한 클래스의 인스턴스를 반환한다는 것을 의미한다. 

## 인터페이스 상속(extending)
- 인터페이스는 **인터페이스로부터만** 상속가능하다.

- 자손 인터페이스는 조상 인터페이스에 정의된 멤버를 모두 상속받는다.

- 클래스와 달리 여러 개의 인터페이스로부터 상속을 받는 **다중 상속**이 가능하다.
  - 하지만 다중상속을 사용하는 경우는 거의 없다.
  

- `extends` 키워드 뒤에 인터페이스명을 작성해 주면 된다.
  - 메서드가 해당 인터페이스를 구현한 클래스의 인스턴스를 반환한다.


```java
// 단일 상속
public interface Positionable extends Centered { 
  public setUpperRightCorner(double x, double y); 
  public double getUpperRightX(); 
  public double getUpperRightY(); 
} 

// 다중 상속
// Transformable는 정의된 멤버가 없지만, 조상 인터페이스로부터 상속받은 추상메서드를 멤버로 가지고 있다. 
public interface Transformable extends Scalable, Translatable, Rotatable {}
```

- 상속(extending)과 구현(implementing)을 동시에 할 수 있다. 

## 인터페이스의 기본 메소드 (Default Method), 자바 8
- JDK 1.8부터 변경된 내용이다. 
- 원래는 interface에 Default 메서드를 추가할 수 없었다. 

- 인터페이스에서 메서드를 추가한다는 것은 추상 메서드를 추가한다는 것이고, 이는 보통 큰 일이 아니다.
- 이는 이 인터페이스를 구현한 기존의 모든 클래스들이 새로 추가된 메서드를 구현해야하기 때문이다. 
- 이러한 인터페이스 변경 문제에 대처하기 위해 JDK 설계자들은 디폴트 메서드를 고안했다. 
- 디폴트 메서드는 추상메서드가 아니기 때문에 인터페이스에 새로 추가되어도 해당 인터페이스를 구현한 클래스를 변경할 필요가 없다. 

- 디폴트 메서드는 앞에 `default`키워드를 붙이며, 추상메서드와 달리 일반 메서드처럼 `{}` (몸통)이 있어야 한다. 

- 디폴트 메서드의 접근제어자는 `public`이며, 생략가능하다. 

```java
// 디폴트 메서드를 구현한 인터페이스
interface MyInterface {
  // 추상 메서드
  void method();
  
  // 디폴트 메서드
  default void newMethod(){}
}
```

- 디폴트 메서드를 추가했을 때 기존 메서드와 이름이 중복되어 충동될 수 있다. 
- 이때는 필요한 쪽의 메서드와 같은 내용으로 오버라이딩 하거나, 새로 정의해버리면 된다. : `JoinMember.super.afterJoin();`

```java
interface JoinMember {
  public void preJoin(){
    System.out.println("반갑습니다");
  }
  public void afterJoin(){
    System.out.println("잘부탁드립니다");
  }
}

interface GroupMember {
}

public class HelloJoinMember implements JoinMember, JoinGroup {
  @Override
  public void preJoin(){
    System.out.println("new preJoin");
  }
  
  @Override
  public void afterJoin(){
    JoinMember.super.afterJoin();
  }
}
```



## 인터페이스의 static 메소드, 자바 8
- JDK 1.8부터 변경된 내용이다. 
- 원래는 interface에 static 메서드를 추가할 수 없었다. 

- static메서드의 접근 제어자는 항상 public이며 생략할 수 있다. 

- static메소드는 오버라이딩을 통한 재정의가 불가능하다.

- static메소드는 참조변수로 구현할 수 없고, 반드시 클래스 명으로 메소드를 호출해야 한다. : `클래스명.static메소드();`

## 인터페이스의 private 메소드, 자바 9
- 단지 특정 기능을 처리하는 내부method 일뿐인데도 외부에 공개되는 public method로 만들었어야 하는 불편함을 해소하기 위해 사용되었다.

- interface를 구현하는 다른 interface 또는 class가 해당 method에 액세스하거나 상속 할수 있는것을 원하지 않을 때 private 메서드를 사용한다.

- Java9 Private Interface Method는 interface에 private method / private static method라는 새로운 기능을 제공하여 문제를 해결한다. 

- 이제 중복 코드를 피하고 interface에 대한 캡슐화를 유지 할 수 있다.

```java
public interface IMyInterface{
    private void method1(String arg){
        // do something
    }
    private static void method2(Integer arg){
        // do something
    }
}
```

## 함수형 인터페이스, 자바 8
- 함수형 인터페이스는 한 개 이상의 추상 메소드를 가지고 있는 인터페이스를 말한다. 
- Single Abstrct Method(SAM)라고 불리기도 한다. 
- 함수형 인터페이스를 사용하는 이유는 자바의 람다식이 함수형 인터페이스로만 접근이 되기 때문이다,.

```java
public interface FunctionalInterface {
     public abstract void doSomething(String text);
}

FunctionalInterface func = text -> System.out.println(text);
func.doSomething("do something");
// 실행 결과
// do something
```

- 자바에서는 함수형 인터페이스를 매번 정의하기 불편하므로 `java.util.function` 패키지에 많은 라이브러리를 제공한다. 
- 자바에서 기본적으로 제공하는 함수형 인터페이스는 아래와 같다.
  - Runnable
  - Supplier
  - Consumer
  - Function<T, R>
  - Predicate


## 강한 결합과 느슨한 결합

## 출처
- [JAVA IN A NUTSHELL](https://docstore.mik.ua/orelly/java-ent/jnut/ch03_07.htm)
- [jenkov.com - Java Interfaces](http://tutorials.jenkov.com/java/interfaces.html)
- [나만 모르고 있던 - Java 9 (Java9 빠르게 훑어 보기)](https://www.popit.kr/%EB%82%98%EB%A7%8C-%EB%AA%A8%EB%A5%B4%EA%B3%A0-%EC%9E%88%EB%8D%98-java9-%EB%B9%A0%EB%A5%B4%EA%B2%8C-%EB%B3%B4%EA%B8%B0/)
- [함수형 인터페이스](https://codechacha.com/ko/java8-functional-interface/)
- [느슨한 결합과 인터페이스](https://taesan94.tistory.com/85)
