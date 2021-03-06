# 20201208-Kotlin기본문법 (3) : 함수

# 절차적 언어
## 함수
- 함수?
  - 목적 : 명령문 여러개를 모아서 한 번에 처리, 재사용, 반복 제어
  - 절차 : 변수선언, 연산처리, 제어문 처리
- 리턴형이 있다면, 받아서 메모리에 저장한다.
- 변수가 있다면 변수를 통해서 데이터를 받는다.


### 함수의 구성

```kotlin
// 선언부
fun 함수명(매개변수:데이터형)
// 구현부
{
  /* 
      구현에 필요한 데이터를 받기 (매개변수)
      처리 후, 
        - 결과값 보낼 때 => 리턴형이 존재
        - 결과값을 출력 => 리턴형이 존재하지 않음
  */
  
}
```

### 함수의 형식

#### 매개변수 O , Return X : void
- 호출방식 : `func_name("홍길동","남자")`
- 형식 : 

```kotlin
// 선언부
fun func_name(매개변수_name:데이터형,sex:String)
// 구현부
{
  // 구현에 필요한 데이터를 받기 (매개변수)
  
}
```

- 예시 : 

```kotlin
// 함수
fun func_name(name:String)
{
    println("사용자가 보내준 이름=$name")
}
// 호출
func_name("홍길동")
```

#### 매개변수 X , Return O
- 호출방식 : `var name:String=func_name()`
- 형식 : 

```kotlin
fun func_name():String
{
  return 문자열
}
```

- 예시 : 

```kotlin
fun func_name3():Int
{
    return (Math.random()*100)!!.toInt()+1
    // 실수를 정수형으로 변환
}
// 호출
var rand:Int=func_name3()
println("난수값:$rand")
```

#### 매개변수 O , Return O
- 호출방식 : `var name:String=func_name("홍길동")`
- 형식 : 

```kotlin
fun func_name(name:String):String
{
  return 문자열
}
```

- 예시 : 

```kotlin
un func_name2(name:String,sex:String):String
{
    return "name=$name , sex=$sex"
}

var info:String=func_name2("심청이","여자")
println(info)
```

#### 매개변수 X , Return X
- 호출방식 : `func_name()`
- 형식 : 

```kotlin
fun func_name()
{
}
```

- 예시 : 

```kotlin
fun func_name4()
{
    println("함수호출")
}
// 호출
func_name4()
```

### main함수

- 코틀린

```kotlin
fun main(args:Array<String>)
{

}
```

- 자바

```java
public static void main(String[] args)
{

}
```

### 구구단 함수 만들기
- 코틀린에서는 클래스 객체를 생성할 때 new를 사용하지 않는다.
- run으로 시작

- 코드 : 

```kotlin
// 자바 패기지 : 자바의 모든 클래스 사용이 가능
import java.util.*

// 구구단 함수
fun gugudan(dan:Int)
{
    for(i in 1..9)
    {
        println("$dan * $i = ${dan*i}")
    }
}

// 입력한 값 받기
val scan=Scanner(System.`in`)

print("단을 입력:")
var dan:Int=scan.nextInt()

// 호출
gugudan(dan)
```


### 함수 예시

```kotlin
// 함수
fun add(i:Int,j:Int):Int
{
    return i+j
}

// 리턴을 생략할 수도 있다.
fun add1(i:Int,j:Int)=i+j

var k:Int=add1(10,20)
print("k=$k")

fun add2(i:Int,j:Int)=i+j
var k1:Int=add2(100,200)
println("k1=$k1")

fun getInfo(name:String,sex:String):String
{
    return "이름:$name,성별:$sex"
}

var info:String=getInfo("홍길동","남자")
println("개인정보:$info")

fun getInfo2(name:String,sex:String)="이름:$name,성별:$sex"
info=getInfo2("심청쓰","여자")

var getInfo3={name:String,sex:String->"이름:$name,성별:$sex"}
println(getInfo3("dddl","Dd"))
```



## 배열

```kotlin
// 배열
var arr= arrayOf(0,0,0)

// 자바 => String[] arr = new String[3]
for(i in 0..2)
{
    arr[i]=(Math.random()*100)!!.toInt()+1
}

// 배열 출력
println("arr[0]=${arr[0]}")
println("arr[1]=${arr[1]}")
println("arr[2]=${arr[2]}")

// 자동지정 변수
var arr1= arrayOf("홍","1","1.55")

// for-each로 출력하기
for(name in arr1)
{
    println("name=$name")
}
```

## 컬렉션
- 컬렉션 : listOf:list , mutableListOf:list , mapOf:map , setOf:set

```tip
**배열보다 컬렉션이 좋은 이유**
- 자동으로 인덱스 번호를 변경해준다.
- 인덱스를 지정할 수 있다.
```


### List
- 순서를 가지고 있다(index)
- 중복 데이터 저장이 가능하다


#### listOf
- 변경하지 않는 데이터가 있는 경우
- 웹서버를 통해서 데이터를 가지고 오는 경우
- 읽기 전용

```kotlin
val names:List<String> = listOf("심","길","호")

// 출력
for(name in names)
{
    println("name:$name")
}

// 읽기 전용이기 떄문에 데이터를 변경할 수 없다
// names[0] = "박문수"
```

#### mutableListOf
- 데이터가 변경될 경우
- 입력창을 통해서 데이터를 가지고 오는 경우
- 읽기 , 쓰기 전용

- 예시 : 

```kotlin
// mutableListOf : 데이터 변경 가능
val names2 = mutableListOf("홍길동","이순신","심청이")
names2[0] = "강감찬"
names2[1] = "을지문덕"
names2[2]="춘향이"

// 출력
for(i in names2)
{
    println("name=$i")
}
```

- 데이터 지우기 : removeAt(index) , removeAll()

```kotlin
names2.removeAt(0)
println(names2)
```

- 데이터 추가하기 : add(데이터) , add(index,데이터)

```kotlin
names2.add("심청이")
println(names2)
names2.add(1,"이순")
println(names2)
```

- 데이터 수정하기 : set(index,데이터)

```kotlin
names2.set(2,"감강")
println(names2)
```




#### List 사용 방법
- 자바의 ArrayList와 같다
- `val names:List<String> = listOf("","","")`
  - 데이터형 선언을 하지 않아도 된다
    - `val names = listOf("심","길","호")`
  - 등호 사이에 띄어쓰기가 없으면 오류가 난다.
    - `val names:List<String>=listOf("","","")` (x)

### Map
- '키'와 '값'이 쌍으로 저장되어 있다.
- 키는 중복할 수 없다.
- 값은 중복이 가능하다.
- 맵 형식 : JSON , Properties , request , response , session , cookie
- 선언 형식 : `키 to 값`

- 읽기 전용 : 

```kotlin
// 선언 및 출력
val muMap = mutableMapOf("한국" to "서울",
    "미국" to "워싱턴","일본" to "도쿄")
for((key,value) in muMap)
{
    println("국가=$key , 수도=$value")
}
```

- 수정 : 

```kotlin
// 변경
muMap["한국"]="서울특별시"
for((key,value) in muMap)
{
    println("국가=$key , 수도=$value")
}
```

- 추가 : 

```kotlin
// 추가
muMap["중국"]="북경"
for((key,value) in muMap)
{
    println("국가=$key , 수도=$value")
}
```

- 삭제 : 

```kotlin
// 삭제
muMap.remove("일본")
for((key,value) in muMap)
{
    println("국가=$key , 수도=$value")
}
```

### Set
- 순서가 없다
- 순서가 없기 때문에 중복된 데이터를 사용할 수 없다.

#### setOf 
- 변경이 불가하다
- 예시 : 

```kotlin
val names3= setOf("홍","박","순")
println(names3)
```

#### mutableSetOf
- 변경이 가능하다
- 예시(수정,추가,찾기) : 

```kotlin
val names4= mutableSetOf<String>("홍","신","박")

// 추가
names4.add("심청이")
println(names4)

// 삭제
names4.remove("홍")
println(names4)

// 찾기
println(names4.contains("박"))
```

## 코틀린의 기본문법 정리

### 변수 설정
- var : 값을 변경할 수 있다 (변수)
- val : 값을 변경할 수 있다 (상수)
- 자동 지정 변수 : 데이터 형을 알아서 설정해주는 변수
- 사용자 지정 변수 : 사용자가 직접 데이터형을 설정하는 변수
  - `var 변수:데이터형`
  - 코틀린에서 지원하는 데이터형
    - Byte , Short , Int , Long , Float , Double , Char , String , Boolean
- null값을 지정하는 방법 : `var c:Char?=null` 
  - `?null` : null을 허용
  
### 여러개의 데이터를 저장하는 법
- 배열 : `var names=arrayOf("","","")`
- 클래스
- 컬렉션 
  - `var names=listOf("",""...)` , `var names=mutableListOf("","",...)`
  - `var names=mapOf("키 to 값","키 to 값"...)` , `var names=mutableMapOf("키 to 값","키 to 값"...)`

- 함수 (코틀린 라이브러리 , 자바 라이브러리 => import)

```kotlin
fun func_name(매개변수):리턴형
{
  리턴내용
}
```

### 내장함수

- 형변환 : `toInt()` , `toDouble()` , `toString()` , `Integer.parseInt("10")`

### 확장함수

```kotlin
fun Int.isEven()=this%2==0
var a=5
var b=8
println(a.isEven())
println(b.isEven())
```

# 객체지향 프로그램

```tip
**객체지향 프로그램의 3대 특성**
- 목적 : 재사용 , 확장성 (유지보수에 용이)
- 캡슐화 : 데이터 감추고 변수를 이용 , 데이터 보호
- 상속 , 포함
- 다형성(확장성) : 오버로딩 , 오버라이딩
```

## 코틀린 객체지향 3대 특성


- 코틀린의 접근지정어
  - 디폴트값이 public임
  - 자바에서의 default는 internal이다.

||자신클래스|같은패키지|같은패키지(상속)|모든 클래스|
|----|----------|---------|--------------|-----------|
|public(생략)|O|O|O|O|
|protected|O|O|O||
|internal|O|O|||
|private|O(은닉화)||||


```tip
**자바의 접근지정어**
  - default(같은 패키지 내에서 접근 가능)
  - private(자신의 클래스 내에서 접근 가능)
  - protected(상속받은 경우 접근 가능)
  - public(모든 클래스에서 접근 가능)
```


- 상속 , 포함 
  - 상속(is-a) : 
  
  ```kotlin
  open class A
  class B:A()
  ```
  
  - 포함(has-a) : 
  
  ```kotlin
  class A
  class B
  {
    val a:A=A() // 객체생성할 때 new를 사용하지 않는다.
  }
  ```
  
- 다형성(오버로딩, 오버라이딩) : 확장성
  - 오버로딩 : 
    - 같은 클래스 안에서 함수명이 동일
    - 매개변수가 다르다 : 객수, 데이터형이 다르다
    - 리턴형은 관계없다.
  
  ```kotlin
  open class A{
    open fun func_name(){
    }
  }
  ```
  
  - 오버로딩 예시 : 
  
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
  ```
  
  - 오버라이딩 : 
  
  ```kotlin
  class B:A()
  {
    override fun func_name(){}
  }
  ```
  
