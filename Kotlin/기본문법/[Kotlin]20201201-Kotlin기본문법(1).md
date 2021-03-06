# 20201201-Kotlin기본문법(1)

## 코틀린
- 자바 코딩으로 변환된다. 
  - 자바 기반이기 때문이다.
- 스크립트 형식의 언어이다.

### 자바와 다른점?
- `;`을 사용하지 않는다. (코틀린, 파이썬)


## 코틀린 기본 문법

1. 변수
2. 연산자
3. 제어문
4. 클래스
5. 객체지향 프로그램
6. 인터페이스
7. 컬렉션
8. 안드로이드에 적용

## 변수

### 변수의 종류
- var (변수) : 변경이 가능한 변수
- val (상수) : final

### 데이터형
#### 숫자형
- Double : 64bit (실수형) , `var a:Double`
- Float : 32bit (실수형) , `var a:Float`
- Long :  64bit (정수형) , `var a:Long`
- Int : 32bit (정수형) , `var a:Int`
- Short : 16bit (정수형) , `var a:Short`
- Byte : 8bit (정수형) , `var a:Byte`

#### 문자형
- String : 문자열 , `var a:String="";`

  - 문자열 결합(여러줄에 걸쳐서 쓸때), `"""`을 사용한다.  

  ```kotlin
  var s="""HELLO Kotlin
          Kotlin !!
          """
  ```

  - 데이터가 여러개일 경우에 배열 ( `Array<Int>` )을 사용한다.
    - 배열은 0번부터 시작한다.

  ```kotlin
  val number:Array<Int>=arrayOf(1,2,3,4,5)
  number[3]=100 
  ```

- Char : 문자 1개 , `var a:Char='A';`


### 변수 선언

```kotlin
var a= 10;
a=20;
val b =10;
b=20; // 오류 발생 , 자동 지정 변수
var a="aaa" => a => String
var a:Int=10
```


## 연산자

### 산술연산자 
- 종류 : `+` , `-` , `*` , `/` , `%`

### 비교연산자
- 종류 : `==` , `!=` , `<` , `>` , `<=` , `>=`

### 논리연산자
- 종류 : `&&` , `||` , `!`



### 삼항연산자
- 종류 : `?:`


## 제어문

### 조건문
- 종류 : `if` , `if~else` , `if~else if ~~`

- 단일 조건문 : `if`

- 형식 : 

```kt
if(조건문)
  실행문장 => 조건이 true일 경우에만 실행
```


- 선택 조건문 : `if~else` 


- 예)

```kt
var a:Int=10
var b:Int=20
var max:Int=0
if(a<b) {
  max=b
}
else
  max=a

// 약식처리
var max:Int=if(a<b) b else a
```

- 다중 조건문 : `if~else if ~ else if ~ else`



### 선택문
- 종류 : `switch~case`(x) , `when` (O)


- 예)

```kt
var a:Int=1
when(a)
{
  1-> 처리문장
  2-> 처리문장
  3-> 처리문장
  4-> 처리문장
  else-> 처리문장
}
var num:Int=10
var number:String=when(num%2) // :String안쓰면 number는 자동으로 String으로 설정됨
{
  0->"짝수"
  else-> "홀수"
}
```

### 반복문
- 종류 : `for` , `while`

#### for
- `for(i in 1..10)` : i는 1부터 10까지 루프
- `for(i in 1..10 step 2)` : 2개씩 증가
- `for(i in 10 downTo 1 step 2)` : 역순으로 출력

- 예)

```kt
for(i in 1..10) // i는 1부터 10까지 루프
for(i in 1..10 step 2) // 2개씩 증가
for(i in 10 downTo 1 step 2) // 10부터 1까지 2씪 건너 뛰기``
```

#### while
- 형식 : 

```kt
var x=1
while(조건문)
{
  실행문장 print(x)
  증가식 i++ (i--);
}
```

- `do~while`

```kt
var x=1
do{
  처리문장
  i++
}while(조건문)
{
  실행문장 print(x)
  증가식 i++ (i--)
}
```


### 반복제어문

- `break` , `continue`


## 클래스
- `new`를 선언하지 않는다.

- 선언

```kt
class Member{
}
var mem=Member()
```

### 생성자

```kt
// 방법1
class Member(var name:String){
}

// 방법2
class Member(var name:String){
  constructor(name:String){
  }
}
```

### 멤버변수

- 멤버변수를 선언하는 방법 : 생성자 뒤에 선언하기

```kt
// 방법1
class Member(var no:Int,var name:String,var sex:String)
{
}
var mem=Member(1,"홍길동","남자")
mem.no
mem.name
mem.sex

// 방법2
class Member{
  var no=1 // public(자바)
  private var a=2
  protected var b=3
  internal var c=4 // default(자바)
}
```

- 상속

```kt
open class A{}
class B:A(){
}
```


## 실습

### 실습 (1)

```kt
// 변수
var a:Int=10
    print(a)
var b:String="Hello Kotlin"
    println(b)
// 상수
val c:Int=100
    println(c)

// 연산
var i:Int=10
var j:Int=20
    println(i+j)
    println(i-j)
    println(i*j)
    println(i/j)
    println(i%j)

// 정수/정수
println(5%2)

// 여러줄 문자가 있는 경우에 처리
var str:String="""
        Hello Kotlin
        hello kotiln
        GOOD BYE
            """
println(str)

// Boolean
var d:Boolean=true
    println(d)

var e:Boolean=10>12
    println(e)

var e1:Boolean=10<12
    println(e1)

/*
    논리연산자
 */
var e2:Boolean=(10<12) && (5>7)
    println(e2)

// 단항연산자
var a1:Int=10
var b1:Int=++a
    println("a1=$a,b1=$b1")

var a2:Int=10
var b2:Int=a2++
    println("a2=$a2,b2=$b2")

// 부정연산자(!)
var a3:Boolean=true
var b3:Boolean=!a3
    println("a3=$a3,b3=$b3")

// 부정, 논리, 비교 연산자 = 조건문에 이용
```


### 실습 (2)

```kt
// 조건문
var a4:Int=20
if(a4%2==0)
    println("$a4 는(은) 짝수다")

var a:Int=10

// Unit : 양수만 저장
var b: Int =100

if(a%2==0)
    println("$a 는 짝수다")

var b2:Char='A'
if(b2>='A' && b2<='Z')
    println("$b2 는 대문자이다.")
else
    println("$b2 는 소문자이다.")

var score:Int=85
if(score>=90)
    println("A")
else if(score>=80)
    println("B")
else if(score>=70)
    println("C")
else
    println("F")

// 선택문 : When => 메뉴에 자주 쓰임
var c:Int =3
when(c)
{
    1-> println("HOME")
    2-> println("회원가입")
    3-> println("게시판")
    4-> println("마이페이지")
    else-> println("HOME")
}

// 반복문
/*
    for(i in lo..hi)
 */
for(i in 1..10)
    println("i=$i")

for(i in 1..10 step 3)
    println("i=$i")

for(i in 10 downTo 1)
    println("i=$i")

var c1:Int=10
var d:Int=20
var max:Int=if(c1<d) d else c1
    println(max)

var no:Int=1
var res:String=when(no%2){
    0->"짝수"
    else->"홀수"
}
println(res)

// 배열
val numbers= arrayOf(1,2,3,4,5)
for(num in numbers)
    println(num)

var num:Int=1
fun isNumber(num:Int)=when(num%2){
    0->"짝수"
    else->"홀수"
}
println(isNumber(num))
var sum:Int=0
for(i in 1..10)
{
    sum+=i
}
println("1~10까지의 합:$sum")

// while
/*
    초기값
    while(조건문)
    {
        반복문장
        증감식
    }


    초기값
    do
    {
    }while(조건문)
    {
    }
 */

var a2:Int=1
while(a2<10)
{
    println("a2=$a2")
    a2++
}

var a3:Int=1
do{
    println("a3=$a3")
    a3++
}while (a3<10)

// break,continue : 반복제어문
for(i in 1..10)
{
    if(i==5)
        break
    println("i=$i")
}

for(i in 1..10)
{
    if(i==5)
        continue
    println("i=$i")
}

// 이중 for문
for(i in 1..9)
{
    for(j in 2..9)
    {
        print("$i * $j = ${i*j} |")
    }
    println()
}
```
