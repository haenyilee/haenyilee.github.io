# 20201207-Kotlin기본문법(2)

```kt
import java.util.*

// 배열
var color = arrayOf("red","blue","yellow","black","green")

print("색상을 선택하세요(0~4)")
var no:Int= readLine()!!.toInt()

// switch~case
when(no)
{
    0-> println("선택된 색상:${color[no]}")
    1-> println("선택된 색상:${color[no]}")
    2-> println("선택된 색상:${color[no]}")
    3-> println("선택된 색상:${color[no]}")
    4-> println("선택된 색상:${color[no]}")
    else-> println("선택된 색상이 없습니다.")
}


val scan:Scanner= Scanner(System.`in`)
/*자바문법 형식
Scanner scan = new Scanner(System.`in`)*/
print("정수입력:")
var num:Int=scan.nextInt()
println("num=$num")

var r:Int=((Math.random()*100)+1).toInt()

var kor:Int =(Math.random()*101).toInt()
var eng:Int =(Math.random()*101).toInt()
var math:Int =(Math.random()*101).toInt()

/*
    $변수명
    ${변수명+2}
    $배열 => ${score[0]}
*/

println("국어점수:$kor")
println("영어점수:$eng")
println("수학점수:$math")
println("총점:${kor+eng+math}")
println("평균:${kor+eng+math/3.0}")

when((kor+eng+math)/30)
{
    10-> println("A")
    9-> println("A")
    8-> println("B")
    7-> println("C")
    6-> println("D")
    else-> println("F")
}

/*
    반복문
    for: 횟수가 지정이 된
    while: 횟수가 지정이 되지 않은
    do~while:
 */

/*
    while 형식 :

    초기값
    while(조건식)
    {
        반복문장수행
        증감식
    }
*/

/*
    do ~ while 형식 : 반드시 한번은 수행한다.

    초기값
    do
    {
        반복수행
        증감식
    }
    while(조건식)
*/

/*
    for문의 형식(**자바와 다름**)

    1) 순차적
    for(i in 1..10)

    2) 역순
    for(i in 10 downTo 1)

    3) 2이상 증가
    for(i in 1..10 step 2)
*/

var i:Int=1
do{
    println("i=$i \t")
    i++
}while (i<=10)
println()
i=1
while (i<=10)
{
    print("i=$i")
    i+=2
}
println()
for(i in 1..10)
{
    print("i=$i")
}
println()
for(i in 1..10 step 2)
{
    print("i=$i")
}
for(i in 10 downTo 1 step 2)
{
    print("i=$i")
}

// break , continue
for(i in 1..10)
{
    if(i==5)
        break
    println("i=$i")
}

println()
for(i in 1..10)
{
    if(i==5)
        continue
    println("i=$i")
}
```

### 함수(메소드)

```tip
**면접**
- 메소드 : 클래스 종속 , 클래스 안에서 작동하는 기능
- 함수 : 독립적인 기능
```

- 함수 : 한 개의 기능을 수행하기 위해서 명령문을 모아서 관리하는 모듈
  - 명령문 : 변수 , 연산자 , 제어문

- 형식 : `fun 함수명(매개변수,매개변수...):데이터형`
  - 마지막 데이터형은  return형의 데이터형이다.

#### 매개변수 O , 리턴형 O 함수

```kt
fun max(i:Int,j:Int):Int
{
  var max:Int=0;
  if(i<j)
    max=j
  else
    max=i
  return max
}

var res:Int=max(10,9)
println("최대값:$res")
```

#### 매개변수 O , 리턴형 X 인 함수

```kt
fun isName(name:String)
{
  println("name=$name")
}
isName("홍길동")
```

#### 매개변수 X , 리턴형 O 인 함수

```KT
fun getRand():Int
{
  var rand:Int=(Math.random()*101)!!.toInt() // !! : 함수 형변환할 때 사용
  return rand
}

var r:Int=getRand()
println("r=$r")
```

#### 매개변수 X , 리턴형 X 인 함수

```kt
fun gugudan()
{
  println("구구단")
  for(i in 1..9)
  {
    for(j in 2..9)
    {
      print($j * $i = ${i*j} \t")
    }
    println()
  }
}
gugudan()
```



