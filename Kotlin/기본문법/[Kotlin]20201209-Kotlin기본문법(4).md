# 20201208-Kotlin기본문법(4) : 클래스


## 클래스 (1)

### 1. 클래스의 구성요소
  
- 멤버변수 : 멤버변수 설정
- 멤버함수 : 함수(멤버메서드) 설정
- 생성자 : 멤버번수 초기화 => 생성자 , init{}
- 접근지정어 설정

```tip
**자바 생성자**
- 생성자는 new 연산자를 통해서 인스턴스를 생성할 때 반드시 호출이 되고 제일 먼저 실행되는 일종의 메소드(하지만 메소드와는 다르다.)이다. 
- 생성자는 인스턴스 변수(필드 값 등)를 초기화 시키는 역할을 한다. 
- 출처 : [자바 생성자](http://blog.naver.com/PostView.nhn?blogId=heartflow89&logNo=220955879645)
```
  


### 2. 클래스의 형식

```kotlin
class A{
  멤버변수
  함수
  생성자
}
```


- (예시) 코틀린의 getter/setter 클래스

```kotlin
class Sawon{
    // 멤버변수 => 생성자(멤버변수 초기화) , init()
    private var sabun:Int=0
    private var name:String=""
    // ?=null의 뜻 : null값을 허용한다.
    private var dept:String=""
    private var job:String=""
    // 클래스 블록 안에서 사용이 가능
    fun setSabun(sabun:Int)
    {
        this.sabun=sabun
    }
    fun getSabun():Int
    {
        return this.sabun
    }
    fun setName(name: String)
    {
        this.name=name
    }
    fun getName():String
    {
        return this.name
    }
    fun setDept(dept: String)
    {
        this.dept=dept
    }
    fun getDept():String
    {
        return dept
    }
    fun setJob(job: String)
    {
        this.job=job
    }
    fun getJob():String
    {
        return job
    }
}

val sa:Sawon=Sawon()
sa.setSabun(100)
sa.setName("홍길동")
sa.setDept("영업부")
sa.setJob("대리")

// 출력
println("사번:${sa.getSabun()}")
println("이름:${sa.getName()}")
println("부서:${sa.getDept()}")
println("직위:${sa.getJob()}")
```

### 3. 생성자
- 멤버변수의 초기화를 담당
- 멤버변수 초기화 방법

  - 클래스 영역에서 초기화
   
  ```kotlin
  class A
  {
    여기서 멤버변수 선언
  }
  ```
  
  - 클래스 이름 뒤에
    - 선언과 초기화를 한번에!
  
  ```kotlin
  class A(멤버변수)
  {
  }
  ```
  
  - constructor 키워드 사용
    - 생성자 영역에서 초기화

  ```kotlin
  class A()
  {
    constructor()
    {
    }
  }
  ```

- 생성자 예시 (1) : 

```kotlin
class Person3(var name:String,var sex:String)
{
    // var name:String,var sex:String => 멤버변수
    fun myPrintln()
    {
        println("name:$name")
        println("sex:$sex")
    }
}
val p=Person3("홍길동","남자")
p.myPrintln()
p.name="심청이"
p.sex="여자"
println("이름:${p.name}")
println("성별:${p.sex}")
```


- 생성자 예시 (2) : 

```kotlin
class Member{
    var no:Int=0
    var name:String=""
    var sex:String=""
    // no:Int,name:String => 매개변수, 지역변수
    constructor(no:Int,name:String){
        this.no=no
        this.name=name
    }
    // 오버로딩
    constructor(sex:String)
    {
        this.sex=sex
    }
}

// 선언
val mem:Member= Member(1,"홍길동")
println("번호:${mem.no}")
println("이름:${mem.name}")

val mem2:Member= Member("남자")
println("성별:${mem2.sex}")
```

### 4. 초기화

```kotlin
class Member2{
    var name:String="홍길동"
    // 생성자
    constructor()
    {
        name="심청이"
    }
    // 초기화 블록
    init {
        name="이순신"
    }
}

// 명시적 초기화 => init()
val mem4=Member2()
println("이름:${mem4.name}")
```


### 5. 클래스에서 꼭 숙지해야 할 것
- 객체 생성 방법
- 상속 => 오버라이딩
- 클래스의 종류
  - 추상클래스
  - 인터페이스
  - inner클래스


## 클래스 (2)

### 1. 멤버변수

#### 1.1 클래스 안에서 생성하는 방법

- 방법 (1) : 

```kotlin
class A
{
  var name:String=""
}
```

- 방법 (2) : 

```kotlin
class A{var name:String=""}
```

- 멤버 변수 vs 매개변수
  - 멤버 변수 : `class Sawon(var sabun:Int,var name:String)`
  
  ```kotlin
  class Sawon(var sabun:Int,var name:String)
  {
      init{
          println("사번:$sabun")
          println("이름:$name")
      }
  }

  val sa:Sawon= Sawon(100,"홍길동")
  ```
  
  - 매개변수(생성자) : `class Sawon2(sabun:Int,var name:String)`
  
  ```kotlin
  class Sawon2(sabun:Int, name:String)
  {
      var sabun:Int=0
      var name:String=""
      init{
          this.sabun=sabun
          this.name=name
      }
      fun printData(){
          println("사번:$sabun")
          println("이름:$name")
      }
  }

  val sa:Sawon=Sawon(100,"홍길동")
  ```  

- 멤버변수

  - 방법 (1) : 

  ```kotlin
  class A(var age:Int)
  ```

  - 방법(2) : 

  ```kotlin
  class A
  {
    var age:Int
  }
  ```

- 일반 매개변수

  - 방법 (1) : 

  ```kotlin
  class A(age:Int)
  ```

  - 방법(2) : 

  ```kotlin
  class A
  {
    constructor(age:Int)
  }
  ```


### 2. 멤버변수 초기화
#### 2.1 명시적 초기화 
- 일반 데이터를....

- 방법 : 

```kotlin
class A
{
  var name:String="홍길동"
}
```


#### 2.2 생성자 이용해서 초기화하는 방법

- 방법 (1) : 

```kotlin
class A
{
  var name:String=""
  constructor()
  {
    name="심청이"
  }
}
```

- 방법 (2) : 사용자가 보내준 데이터를 초기화할 때 사용함 

```kotlin
class A
{
  var name:String=""
  constructor(name:String)
  {
    this.name=name
  }
}
```


#### 2.3 초기화 블록 이용해서 초기화하는 방법
- 자동 인식함
  
- 외부에서 데이터 설정할 때 주로 사용한다.
  - 예를 들어 데이터베이스 드라이버 등록할 때 사용한다.
  - 클래스 내에서는 구현(연산처리,제어문,파일읽기,웹서버접근)이 불가능하다
  - 이럴 때 클래스 내에 `init{}`을 주고 구현해야 한다. 
  - 자바에서는 init없이 `{}`만 사용한다.
  
  ```java
  class A
  {
    1. 변수 선언
    int a;
    // ==================================== 코틀린에서는 init{}
    {
      구현
    }
    A()
    {
      구현
    }
    // ==================================== 
    2. 메소드 선언 : 초기값 변경할 때 사용
  }
  ```
  

- 방법 : 

```kotlin
class A
{
  var name:String=""
  init
  {
    name="심청이"
  }
}
```

### 3. 멤버함수

#### 3.1 return 형이 존재하는 경우

```kotlin

```


#### 3.2 return 형이 존재하지 않는 경우


```kotlin

```


#### 3.3 상속을 받아서 함수를 재정의할 때 
- 오버라이딩이 가능한 함수 (재정의 가능)
- 자바에서는 `extends` , 코틀린에서는 `open` + `:A()`를 사용한다.


```kotlin
// 상속 내리는 클래스
open class A
{
    // 재정의가 가능한 함수
    open fun run()
    {
    }
    // 재정의가 불가능한 함수
    fun ride()
    {
    }
}

// 상속 받은 클래스
class B:A()
{
    // 재정의하는 함수
    override fun run()
    {
    }
}
```

- 상속 예시 : 

  - 클래스 선언

  ```kotlin
  pen class Ani{
      open fun run(){
          println("달린다")
      }
      open fun eat(){
          println("먹는다")
      }
      fun sleep(){
          println("잔다")
      }
  }
  class Horse:Ani()
  {
      override fun run() {
          println("말이 달린다")
      }

      override fun eat() {
          println("말이 먹는다")
      }
  }
  class Pig:Ani()
  {

  }
  class Dog:Ani()
  {

  }
  ```

  - 클래스 출력 (1)

  ```kotlin
  val h:Horse= Horse()
  h.eat() // 말이 먹는다
  h.run() // 말이 달린다
  h.sleep() // 잔다
  val p:Pig= Pig()
  p.eat()
  p.run()
  val d:Dog= Dog()
  d.eat()
  d.run()
  ```

  - 클래스 출력 (2)
    - 상속을 받으면 클래스 객체를 상위클래스로 제어할 수 있다.
    - 그래서 상속의 이점이 여러개의 클래스를 모아서 관리할 수 있다는 것이다.

  ```kotlin
  var a:Ani=Horse()
  a.run()
  a.eat()

  a=Pig()
  a.run()
  a.eat()

  a=Dog()
  a.run()
  a.eat()
  ```


### 4. 클래스의 종류

#### 4.1 추상 클래스
- 추상화는 공통적인 것들을 모아둔다는 것이다.
- 추상 클래스는 단일 상속만 가능하다.
- 구현된 함수를 가져올 수도 있고, 구현이 안된 함수를 가지고 올 수도 있다.

- 예시 (1)

```kotlin
// 추상클래스
abstract class Graph
{
  abstract fun draw()
  abstract fun color(col:String)
}
// 추상클래스 구현
class Line:Graph(){
    override fun draw() {
        TODO("Not yet implemented")
    }

    override fun color(col: String) {
        TODO("Not yet implemented")
    }
}
```

- 예시 (2)

```kotlin
abstract class Graph
{
    abstract fun draw()
    abstract fun color(col:String)
    open fun display(){
        println("도화지에 그린다")
    }
}
class Line:Graph(){
    override fun draw() {
        println("사각형")
    }

    override fun color(col: String) {
        println("$col")
    }

    override fun display() {
        println("컴퓨터에 그린다")
    }
}
var g:Graph=Line()
g.color("파란색")
g.draw()
g.display()

/* 결과 : 
  파란색
  사각형
  컴퓨터에 그린다
*/
```


#### 4.2 인터페이스
- 추상클래스의 일종이다.
- 다중 구현이 가능하다.
- 구현이 안된 함수 , 구현된 함수를 상속 받는 것이 가능하다.
- 서로 다른 클래스를 연결해서 사용 가능하다
  - 자바에선 `implements`로 상속 받아서 사용한다.
- 독립적으로 사용이 가능하다
- 여러개의 클래스를 모아서 관리한다.
- 표준화가 가능하다.
- **단점** : 인터페이스에 구현안된 함수를 추가하면, 이를 상속받은 클래스에서 다 오류가 발생하게 된다. 
  - 그래서 빈 공백으로 구현된 클래스를 추가해야 한다.
- 인터페이스 상속은 `()`하면 안된다. 
  - `class A:I` (O)
  - `class A:I()` (X)
  - 메모리 할당할 수 없는 애들이라 생성자를 가질 필요가 없기 때문이다.

```tip
**[면접]인터페이스 사용해보았나요?**
- 모든 메소드가 구현없이 선언만 했죠 (X)
- 구현된 메소드와, 구현안된 메소드를 모두 인터페이스로 사용했습니다.(O)
  - JDK 1.8이상에서는 구현된 메소드가 가능하다 
```

#### 4.3 inner클래스
- 잘 사용되지 않음




