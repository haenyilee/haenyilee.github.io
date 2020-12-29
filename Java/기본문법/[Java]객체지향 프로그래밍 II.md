---
sort: 7
---

# [Java]객체지향 프로그래밍 II : 추상클래스와 인터페이스

## 클래스의 종류
- 일반클래스 : POJO / public class A
    - 재사용(확장된 클래스) 
    - 데이터형으로만 사용함 : 필요한 데이터를 모아서 관리(캡슐화)
    - 데이터 은닉화(private) 메소드를 통해 접근함(getter/setter)
    - 액션형 : 기능(메소드)만 가지고 있음
    - 변수 + 메소드 = 혼합형으로 사용 가능함
    - 형식


      ```java
      public class className {
          멤버변수 , 공유변수
          생성자 
          메소드
      }
      ```


        - 상속 : ``public class A extends B``
        - 포함 :


          ```Java
          public class A {
              B b=new B();
          }
          ```



            
- 내부클래스 : 클래스 안에 클래스가 들어가 있는 형식
    - 멤버클래스 : 두 개 이상의 클래스에서 공유하는 데이터가 있을 때 사용 , static으로 대체 가능하기도 함
        - 네트워크 프로그램 , 쓰레드 프로그램 , 빅데이터에서 많이 사용함
        - 애플리케이션 제작에 자주 쓰임
        - 형식


            ```java
            class A
            {
                class B
                {
                }
            }
            ```

        - 등장배경 
            ```java
            class A
            {
                o , x , q
            }
            class B
            {
                A a=new A();
                a.o , a.x , a.q
            }
            B b=new B();     
            B b1=new B();   // error : b1과 b가 모두 a라는 저장공간에 다른 값을 저장하기 때문에!
            ```



    - 지역클래스
        - 형식


            ```java
            class A
            {
                public void display()
                {
                    class B
                }
            }
            ```


    - 익명클래스 : 상속 없이 오버라이딩 사용이 가능한 클래스(속도가 더빠름)
        - 형식


            ```java
            class A
            {
                B b = new B()
                {
                    public void display(); __// 여기서 내용 변경__
                }
            }

            class B
            {
                public void display()
                {
                }
            }
            ```


- 종단클래스 ==> 확장불가

- 추상클래스 : 미완성된 클래스(설계용/클래스관리용 ==> 서로 다른 여러개의 클래스를 모아서 관리)
    - 형식 : ``public abstract class A ``
    - 단일상속만 가능함
    - 메모리 할당이 불가능함
    - 생성 시에는 반드시 상속을 받은 클래스를 통해서 메모리를 저장해야 함
    - 상속을 받은 클래스는 반드시 구현이 안된 메소드를 구현해서 사용해야 함
    - 여러개의 클래스가 있을 때, 공통으로 적용되는 메소드가 있는 경우 추상클래스를 제작한다.
    - 구분 : 메모리할당 시, '클래스 선언 = 생성자'이면 추상클래스
        Ex. A a = new A() :  일반클래스
            A a = new B() : 추상클래스
    - 메소드 구현이 애매한 경우(경우의 수가 많을 때) 추상클래스를 주로 사용한다.
    - 기본틀이 제작되어 있기 때문에 구현이 쉽다.
    - 요구사항 분석할 때, 주로 사용함

- 인터페이스(추상클래스의 일종 ==> 서로 다른 여러개의 클래스를 모아서 관리)
    - 형식 : ``public interface class A``
    - 다중상속
    - 웹 프로그램에서 주로 사용함
    - 상수형 변수 => public final static 데이터형=값; => 생략 시 자동으로 추가됨
    - protected, private 접근제어자 사용불가(무조건 public abstract)
    - 메소드 : 구현이 안된 메소드만 사용 가능함
    - ***__BUT!__*** JDK 1.8부터는 default를 통해 구현된 메소드 작성이 가능함
    - 상위클래스 : 상속을 내려야 사용이 가능하기 때문에
    - 클래스부터 상속을 받을 수가 없다!
    - [인터페이스 > 인터페이스] 상속 가능 : extends
    - [인터페이스 > 클래스] 상속 가능 : implements
    - 결합성(다른 클래스에 미치는 영향)이 낮은 프로그램을 만들 경우 주로 사용함
    - 스프링은 인터페이스 기반 프로그램임


### 추상클래스와 인터페이스

|-|추상클래스|인터페이스|
|-|---------|---------|
|1|단일상속|다중상속|
|2|구현이 안된 메소드,구현이 된 메소드|구현이 안된 메소드|

- 형식 비교
    - 추상클래스 :

    ```java
    public abstract class A             // className
    {
        public abstract void display(); // 멤버변수 , 공유변수
        public void aaa()               // 구현이 안된 메소드 , 구현이 된 메소드
        {
        }
    }
    ```


    - 인터페이스 : 


    ```java
    public interface B    // interfaceName
    {
        void display(); // _public abstract void display();_가 생략되어 있음
        void aaa();     // _public abstract void aaa();_가 생략되어 있음
    }
    ```



## 추상클래스

### 1. 추상클래스란?
- 미완성된 글래스
- new 를 이용해서 메모리 할당 불가능(인스턴스를 생성할 수 없다)
- 사용 방법 : 항상 상속을 내려서 하위 클래스에서 구현한 다음 사용
- 기본틀을 제시하는 역할을 함
    예)  
        1. 게시판을 만들어라  
        2. 게시판에 [글쓰기 , 내용보기 , 수정하기 , 삭제하기 , 찾기] 기능을 만들어라 __=> 추상 클래스의 역할__

    예) 영화관 웹사이트 : CGV, 메가박스, 다음영화, 네이버영화  
        ==============  
           영화목록
             예매
         영화 상세보기
            이벤트
           극장설명        

- 구현하는 내용이 프로그램마다 다르기 때문에 틀만 제시함

### 2. 추상클래스의 장점
- 구현이 안된 메소드, 구현이 된 메소드 둘 다 사용 가능


## 인터페이스

### 1. 인터페이스란?
- 추상클래스의 일종(추상클래스의 단점을 보완)
- 미완성된 클래스
- 자신이 메모리 할당을 할 수 없다(구현한 클래스를 통해서 메모리 할당)
    - new 를 이용해서 메모리 할당 불가능(인스턴스를 생성할 수 없다)
- 추상 클래스(단일상속) vs 인터페이스(다중상속)
- 모든 메소드가 abstract : 선언만 가능함
    - JDK 1.8부터는 default 메소드를 이용해서 메소드 구현이 가능함
*** 인터페이스에 선언된 모든 메소드는 public임

### 1.1. 인터페이스의 변수


|추상클래스|인터페이스|
|---------|---------|
|멤버변수|상수형변수|

    - 인터페이스에 선언된 모든 변수는 public이다.


|사용불가변수|사용가능변수|
|---------|---------|
|private|public|
|protected||
|default||


### 1.2. 인터페이스의 형식


```java
interface A
{
    //변수
    int a=10; =====> JVM (public static final int a=10;이 생략됨)

    // 메소드
    void aaa(); =====> JVM (public abstract void aaa();이 생략됨)
    void bbb();
}
```


### 1.3. 인터페이스의 구현


```java
interface A
{
    (public abstract)void aaa();
}

    class B implements A
{
    **public** void aaa(); // public 안쓰면, default로 축소되기 때문에 error!
    {
    }
}
```

- (참고) 오버라이딩의 특징
    - 상속관계
    - 메소드명이 동일
    - 매개변수 동일
    - 리턴형이 동일
    - 접근지정어 => 확대 가능, 축소 불가능

- 상속과정 : 관련이 없는 여러개의 클래스를 연결하는 방법
1) 인터페이스 > 인터페이스
    - extends (확장해서 사용)
2) 인터페이스 > 클래스
    - implements (구현 안된 것을 구현해서 사용)
3) 클래스 > 인터페이스
    - error! 상속 불가
- 인터페이스는 모든 메소드가 구현이 안된다.
    
    - 예)

    - 단일 상속
    ```java
    interface A
        K L
    interface B extends A
        P => K L
    interface B implements A
        P
        K
        L
    ```


    - 다중상속(1)
    ```java
    interface A
    interface B
    class C implements A,B
    ```


    - 다중상속(2)
    ```java
    interface A
    class B
    class C extends B implements A
    ```


### 1.4. 자바에서 지원하는 인터페이스
- Window(javax.swing.*)



|리스너|메소드|
|--------------|-------------------------|
|ActionListener|버튼, 메뉴, text에서 엔터|
||actionPerformed()|
|MouseListener|마우스관련,JTable,JTree,JLabel|
||mouseClicked()|
||mouseReleased()|
||mousePressed()|
||mouseEntered()|
||mouseExited()|
|MouseMotionListener|마우스의 움직임|
||mouseMoved()|
||mouseDragged()|
|KeyListener|키보드|
||keyPressed()|
||keyReleased()|
||keyTyped()|
|FocusListener||
||focusLost()|
||focusGained()|
|ItemListener||
||ComboBox,JList|
||itemStateChanged()|




- 데이터베이스 연결
    - Connection
    - Statement
    - ResultSet


### 1.5. 인터페이스의 역할
- 여러 클래스를 묶어서 인터페이스로 관리 : 서로 다른 클래스들에게 관계를 맺어준다.
- Spring이 인터페이스 기반 프로그램임




### 2. 인터페이스의 단점
- 기능 추가하면 모든 클래스에서 error 발생
- 구현이 안된 메소드는 반드시 구현해서 사용해야 함 
   => 필요없는 기능이 있어도 전부 구현해야 사용 가능함 
   => 필요없는 기능은 {}만 작성해두면 됨

