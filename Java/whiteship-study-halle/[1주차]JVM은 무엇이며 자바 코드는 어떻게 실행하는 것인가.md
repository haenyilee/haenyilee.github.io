# [1주차]JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가

## 목차
- JVM이란 무엇인가
- 컴파일 하는 방법
- 실행하는 방법
- 바이트코드란 무엇인가
- JIT 컴파일러란 무엇이며 어떻게 동작하는지
- JVM 구성 요소
- JDK와 JRE의 차이

## JVM이란 무엇인가
- Java Virtual Machine(JVM)은 자바를 실행하기 위한 가상 머신이다.

- Java Byte Code를 OS에 맞게 해석 해주는 역할을 한다.
- Byte Code 는 기계어가 아니기 때문에 OS에서 바로 실행되지 않는다. 
- 이때 JVM은 OS가 ByteCode를 이해할 수 있도록 해석해준다.
- 자바 프로그램과는 달리 자바 가상 머신(JVM)은 운영체제에 종속적이므로, 각 운영체제에 맞는 자바 가상 머신을 설치해야 한다.
- Java 파일 하나만 만들면 어느 OS든 각자의 OS에 맞는 JVM 위에서 실행 할 수 있게 되는 것이다. 
![image](https://user-images.githubusercontent.com/66978721/103441306-4a457680-4c90-11eb-8e7c-741718c9c874.png)
  
- 자바로 작성된 어플리케이션은 모두 이 가상머신(JVM)위에서만 실행되기 때문에 자바 어플리케이션이 실행되기 위해서는 반드시 JVM이 필요하다.
- JVM은 크게 Class Loader, Runtime Data Areas, Excution Engine 3가지로 구성되어 있다.


## 바이트코드란 무엇인가

- 바이트코드(Bytecode, portable code, p-code)는 특정 하드웨어가 아닌 가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법이다.
- 자바 바이트 코드의 확장자는 `.class`이다.
- 자바 바이트 코드는 자바 가상 머신만 설치되어 있으면, 어떤 운영체제에서라도 실행될 수 있다.

- 바이너리 코드란 CPU가 이해할 수 있는 언어이다. 


## 컴파일 하는 방법

![image](https://user-images.githubusercontent.com/66978721/103441668-dbb5e800-4c92-11eb-82e6-582d266f6c87.png)

### 컴파일이란?
- 컴퓨터에게 일을 시키기 위해서 사람의 말을 컴퓨터가 알아듣게 기계어로 번역하는 것이다.
- 프로그래밍 언어로 작성한 코드를 기계가 알아듣게 다른 언어로 옮기는 작업을 컴파일이라고 한다. 
- 이러한 작업을 하는 프로그램은 compiler라고 한다. 
- Java compiler는 `.java` 파일을 `.class` 라는 Java byte code로 변환 시켜 준다.
- 자바 컴파일러는 자바를 설치하면 javac.exe라는 실행 파일 형태로 설치된 

### cmd에서 자바 컴파일 하는 방법

1. 소스 코드의 디렉토리 경로에 접속한다 : `cd 파일경로`
2. javac와 함께 파일명.java을 입력한다 : `javac 파일명.java`
3. java와 함께 파일명을 입력한다 : `java 파일명`
  - 패키지가 존재할 때에는 `java 패키지.패키지.패키지.파일명` 으로 실행해야 한다. 



## JVM 구성 요소
- 자바로 작성된 프로그램은 다음과 같은 순서로 실행된다. 
![image](https://user-images.githubusercontent.com/66978721/103441805-b2e22280-4c93-11eb-9965-21a0f96c84e2.png)


### 클래스 로더(class loader)
- 동적으로 클래스를 로딩해주는 역할을 하는 것이 바로 클래스 로더(class loader)이다.
- 클래스 로더가 classpath라는 환경 변수에 등록된 디렉토리에 있는 모든 클래스들을 먼저 JVM에 로딩한다. 
- JVM에 로딩된 클래스만이 JVM에서 객체로 사용할 수 있다. 
- 클래스 로딩은 클래스를 로딩하는 시점 또는 실행 중간에도 할 수 있다.
- 자바에서는 클래스 로딩 매커니즘을 보다 빠르게 동작하고 쉽게 확장할 수 있도록 하기 위해 클래스 로더 간 계층구조로 되어 있다.
  - **Bootstrap Class Loader**
    - JVM이 실행될 때 맨 처음 실행되는 클래스 로더로 $JAVA_HOME/jre/lib에 있는 JVM 실행에 필요한 가장 기본적인 라이브러리(rt.jar 등)를 로딩한다. 
    - 다른 클래스 로더와 달리 자바가 아닌 네이티브로 구현되어 있다.
  - **Extensions Class Loader**
    - Bootsrap Loading 후 기본적으로 로딩되는 클래스로 $JAVA_HOME/jre/lib/ext에 있는 클래스들이 로딩된다. 
    - 이 클래들은 별도로 classpath에 잡혀 있지 않아도 로딩된다.
  - **System Class Loading**
    - 다음으로 CLASS PATH에 정의되거나 JVM option에서 -cp, -classpath에 지정된 클래들이 로딩된다.
  - **User-Defined Class Loader** : 사용자가 직접 생성해서 사용하는 클래스 로더다.



### 자바 인터프리터(interpreter)
- 자바 컴파일러에 의해 변환된 자바 바이트 코드를 읽고 해석하는 역할을 한다.

### JIT 컴파일러(Just-In-Time compiler)
- JIT 컴파일러(Just-In-Time compiler)란 프로그램이 실행 중인 런타임에 실제 기계어로 변환해 주는 컴파일러이다.

### 가비지 컬렉터(garbage collector)
- 자바 가상 머신은 가비지 컬렉터(garbage collector)를 이용하여 더는 사용하지 않는 메모리를 자동으로 회수해 준다.
- 따라서 개발자가 따로 메모리를 관리하지 않아도 되므로, 더욱 손쉽게 프로그래밍을 할 수 있도록 도와준다.



## JIT 컴파일러란 무엇이며 어떻게 동작하는지
- JIT 컴파일러(Just-In-Time compiler)란 프로그램이 실행 중인 런타임에 실제 기계어로 변환해 주는 컴파일러를 의미한다.

- 동적 번역(dynamic translation)이라고도 불리는 이 기법은 프로그램의 실행 속도를 향상시키기 위해 개발되었다.
  - 일반 애플리케이션은 OS만 거치고 하드웨어로 전달된다. 
  - 반면 Java애플리케이션은 OS 사이에 JVM이라는 가상머신을 한 번 더 거치기 때문에 속도가 느리다는 단점이 있었다. 
  - 또 하드웨어에 맞게 완전히 컴파일된 상태가 아니고 실행 시에 해석되기 때문에 속도가 느렸다.
  - 이러한 단점을 해결하기 위해 바이트코드를 하드웨어의 기계어로 바로 변환해주는 JIT 컴파일러가 필요하게 된 것이다.

- 즉, JIT 컴파일러는 자바 컴파일러가 생성한 자바 바이트 코드를 런타임에 바로 기계어로 변환하는 데 사용합니다.

## JDK와 JRE의 차이

## 참고
- [](https://medium.com/@lazysoul/jvm-%EC%9D%B4%EB%9E%80-c142b01571f2)
- [](https://saml2l.tistory.com/13)
- [](https://opentutorials.org/module/516/5559)
- [](http://www.tcpschool.com/java/java_intro_programming)
- 자바의 정석
- [](https://futurists.tistory.com/43)
