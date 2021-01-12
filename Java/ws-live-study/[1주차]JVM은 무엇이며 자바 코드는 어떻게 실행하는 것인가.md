# [1주차]JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가

## 0. 목차
- JVM이란 무엇인가
- 컴파일 하는 방법
- 실행하는 방법
- 바이트코드란 무엇인가
- JIT 컴파일러란 무엇이며 어떻게 동작하는지
- JVM 구성 요소
- JDK와 JRE의 차이

## 1. JVM이란 무엇인가
![image](https://user-images.githubusercontent.com/66978721/103453087-82dd6280-4d19-11eb-88b9-d1cf9b024358.png)

- Java Virtual Machine(JVM)은 자바를 실행하기 위한 가상 머신이다.

- Java Byte Code를 OS에 맞게 해석 해주는 역할을 한다.
- Byte Code 는 기계어가 아니기 때문에 OS에서 바로 실행되지 않는다. 
- 이때 JVM은 OS가 ByteCode를 이해할 수 있도록 해석해준다.
- 자바 프로그램과는 달리 자바 가상 머신(JVM)은 운영체제에 종속적이므로, 각 운영체제에 맞는 자바 가상 머신을 설치해야 한다.
- Java 파일 하나만 만들면 어느 OS든 각자의 OS에 맞는 JVM 위에서 실행 할 수 있게 되는 것이다. 
![image](https://user-images.githubusercontent.com/66978721/103441306-4a457680-4c90-11eb-8e7c-741718c9c874.png)
  
- 자바로 작성된 어플리케이션은 모두 이 가상머신(JVM)위에서만 실행되기 때문에 자바 어플리케이션이 실행되기 위해서는 반드시 JVM이 필요하다.
- JVM은 크게 Class Loader, Runtime Data Areas, Execution Engine 3가지로 구성되어 있다.



## 2. 컴파일 하는 방법
![image](https://user-images.githubusercontent.com/66978721/103451794-7b16c180-4d0b-11eb-914a-787a3e138f89.png)

### 컴파일이란?
- 컴퓨터에게 일을 시키기 위해서 사람의 말을 컴퓨터가 알아듣게 기계어로 번역하는 것이다.
- 프로그래밍 언어로 작성한 코드를 기계가 알아듣게 다른 언어로 옮기는 작업을 컴파일이라고 한다. 
- 이러한 작업을 하는 프로그램은 compiler라고 한다. 
- Java compiler는 `.java` 파일을 `.class` 라는 Java byte code로 변환 시켜 준다.
- 자바 컴파일러는 자바를 설치하면 javac.exe라는 실행 파일 형태로 설치된다.

### cmd에서 자바 컴파일 하는 방법

1. 소스 코드의 디렉토리 경로에 접속한다 : `cd 파일경로`
2. **javac와 함께 파일명.java을 입력한다** : `javac 파일명.java`




## 3. 실행하는 방법

### cmd에서 자바 실행 하는 방법
- java와 함께 `.class` 확장자의 파일명을 입력한다 : `java 파일명`
  - 패키지가 존재할 때에는 `java 패키지1.패키지2.패키지3.파일명` 으로 실행해야 한다. 


### 실행 과정
- 자바로 작성된 프로그램은 다음과 같은 순서로 실행된다. 
![image](https://user-images.githubusercontent.com/66978721/103441805-b2e22280-4c93-11eb-9965-21a0f96c84e2.png)

- JVM의 구성요소인 클래스로더가 `.class` 파일을 메모리상의 JVM으로 가져온다.
- 내부적으로는 classLoader -> Byte Code Verifier (바이트코드 변조 확인) -> Execution Engine에서 실행되는 구조다.
- Execution Engine에서 클래스파일(바이트코드로 구성)을 기계어로 변경해서 명령어 단위로 실행한다.

- 다만 명령어 단위 실행은 2가지 방식으로 동작한다. (Interpreter 방식 / JIT(Just In Time compiler) 방식)



## 4. 바이트코드란 무엇인가

- 바이트코드(Bytecode, portable code, p-code)
- 바이트코드는 특정 하드웨어가 아닌 가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법이다.

- 자바 바이트 코드의 확장자는 `.class`이다.
- 자바 바이트 코드는 각 OS에 맞는 자바 가상 머신(JVM)만 설치되어 있으면, 어떤 운영체제에서라도 실행될 수 있다.

- 즉 바이트 코드는 JVM이 이해하는 언어이며, 네이티브어는 CPU 이해하는 언어이다.



## 5. JIT 컴파일러란 무엇이며 어떻게 동작하는지

### JIT 컴파일러란?
- JIT 컴파일러는 자바 컴파일러가 생성한 자바 바이트 코드를 런타임에 바로 기계어로 변환하는 컴파일러이다.

- 즉, 바이트코드를 컴퓨터 프로세서(CPU)로 직접 보낼 수 있는 명령어로 바꾸는 프로그램이다. 

### JIT가 필요해진 이유
- 동적 번역(dynamic translation)이라고도 불리는 이 기법은 프로그램의 실행 속도를 향상시키기 위해 개발되었다.

- 일반 애플리케이션은 OS만 거치고 하드웨어로 전달된다. 
- 반면 Java애플리케이션은 OS 사이에 JVM이라는 가상머신을 한 번 더 거치기 때문에 속도가 느리다는 단점이 있었다. 

- 더불어 기존 클래스파일(바이트코드)를 실행하는 방법은 Interpreter 방식이 기본이다.
- Interpreter 방식은 하드웨어에 맞게 완전히 컴파일된 상태가 아니고 실행 시에 명령어를 하나씩 해석해서 처리하기 때문에 속도가 느렸다.

- 이러한 느린 속도라는 단점을 해결하기 위해 바이트코드를 하드웨어의 기계어로 바로 변환해주는 JIT 컴파일러가 필요하게 된 것이다.
  
```tip
- **컴파일러**
  - 고급언어로 쓰여진 프로그램이 컴퓨터가 이해할 수 있는 기계어로 바꿔준다. 
  - 번역과 실행 과정을 거쳐야 하기에 번역과정이 번거롭고 시간이 오래 걸린다. 
  - 반면 한번 번역하면 이후엔 다시 번역하지 않으므로 실행속도 빠르다.

- **인터프리터**
  - 한 단계씩 기계어로 해석하여 실행하는 언어처리 프로그램이다. 
  - 줄 단위로 번역, 실행되기 때문에 시분할 시스템에 유용하며 원시 프로그램의 변화에 대한 반응이 빠르다.
  - 한 단계씩 테스트와 수정을 하면서 진행시켜 나가기 때문에 실행 시간이 길어 속도가 늦다.
```

### JIT 컴파일러란 어떻게 동작하는가

- JIT 컴파일러는 런타임 시 `.class` 파일(바이트코드)를 네이티브 기계어로 한 번에 컴파일 후 사용한다.
- 전체 컴파일 후 캐싱 -> 이후 변경된 부분만 컴파일하고 나머지는 캐시에서 가져다가 바로 실행하는 방식으로 작동한다.
- 네이티브 코드는 캐시에 보관되기 때문에 한 번 컴파일된 코드는 빠르게 실행할 수 있다.


### JIT 컴파일러 vs Interpreter
- 캐시에 바로 꺼내서 사용하고 변경 부분만 컴파일 하기 때문에 코드 수행속도가 Interpreter 방식에 비해서 빠르다.
- 하지만 한 번만 실행되는 코드라면 JIT 보다 Interpreter 방식이 유리하다.
- 따라서 JVM은 해당 메소드가 얼마나 자주 수행되는지 체크하고 일정 정도를 넘을 때 컴파일을 수행한다.

```note
= 평소에는 Interpreter 방식으로 실행 
= 적절한 시점에 바이트 코드 전체를 컴파일 하여 네이티브 코드로 변경
= 더 이상 Interpreting 하지 않고 네이티브 코드로 직접 실행
```


## 6. JVM 구성 요소
- JVM은 크게 Class Loader, Runtime Data Areas, Execution Engine 3가지로 구성되어 있다.

![image](https://user-images.githubusercontent.com/66978721/103453232-38f57c00-4d1b-11eb-8cae-de7daedec272.png)

### Class Loader

- 자바 컴파일러에 의해 생성된 `.class` 파일들을 엮어서 JVM이 운영체제로부터 할당받은 메모리영역인 Runtime Data Area로 적재하는 역할을 Class Loader가 한다. 
- 즉, 동적으로 클래스를 로딩해주는 역할을 하는 것이 바로 클래스 로더(class loader)이다.
- 클래스 로딩은 클래스를 로딩하는 시점 또는 실행 중간에도 할 수 있다.

- 자바에서는 클래스 로딩 매커니즘을 보다 빠르게 동작하고 쉽게 확장할 수 있도록 하기 위해 클래스 로더 간 계층구조로 되어 있다.
  - **Bootstrap Class Loader**
    - JVM이 실행될 때 맨 처음 실행되는 클래스 로더이다.
    - $JAVA_HOME/jre/lib에 있는 JVM 실행에 필요한 가장 기본적인 라이브러리(rt.jar 등)를 로딩한다. 
    - 다른 클래스 로더와 달리 자바가 아닌 네이티브로 구현되어 있다.
  - **Extensions Class Loader**
    - Bootsrap Loading 후 기본적으로 로딩되는 클래스이다.
    - $JAVA_HOME/jre/lib/ext에 있는 클래스들이 로딩된다. 
    - 이 클래들은 별도로 classpath에 잡혀 있지 않아도 로딩된다.
  - **System Class Loading**
    - 다음으로 CLASS PATH에 정의되거나 JVM option에서 -cp, -classpath에 지정된 클래들이 로딩된다.
  - **User-Defined Class Loader** 
    - 사용자가 직접 생성해서 사용하는 클래스 로더다.


### Execution Engine
- ByteCode를 실행 가능 하도록 해석한다.
- Class Loader에 의해 메모리에 적재된 클래스(바이트 코드)들을 기계어로 변경해 명령어 단위로 실행하는 역할을 한다.
- 명령어를 하나 하나 실행하는 인터프리터(Interpreter)방식이 있고 
- JIT(Just-In-Time) 컴파일러를 이용하는 방식이 있다.
  - JIT 컴파일러는 적절한 시간에 전체 바이트 코드를 네이티브 코드로 변경해서 Execution Engine이 네이티브로 컴파일된 코드를 실행하는 것으로 성능을 높이는 방식이다.


### Runtime Data Areas
![image](https://user-images.githubusercontent.com/66978721/103453293-c638d080-4d1b-11eb-8965-8f24cfd570a2.png)


- **Method area (메소드 영역)**
  - 객체 생성 후에 메소드를 실행하게 되면 해당 클래스 코드에 대한 정보를 Method Area에 저장 하게 된다. 
  - 저장되는 내역은 아래와 같다.
    - 클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보같은 필드 정보
    
    - 메소드의 이름, 리턴 타입, 파라미터, 접근 제어자 정보같은 메소드 정보
    
    - Type정보(Interface인지 class인지)
    
    - Constant Pool(상수 풀 : 문자 상수, 타입, 필드, 객체 참조가 저장됨)
    
    - static 변수, final class 변수

 

- **Heap area (힙 영역)**

  - new 키워드로 생성된 객체와 배열이 생성되는 영역이다.

  - 메소드 영역에 로드된 클래스만 생성이 가능하다.
  
  - Garbage Collector가 참조되지 않는 메모리를 확인하고 제거하는 영역이다.
 

- **Stack area (스택 영역)**

  - 지역 변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값등이 생성되는 영역이다.

  - `int a = 10;` 이라는 소스를 작성했다면?
    - 정수값이 할당될 수 있는 메모리공간을 a라고 잡아두고 
    - 그 메모리 영역에 값이 10이 들어간다.
    - 즉, 스택 메모리에 이름을 a라고 붙여주고 값이 10인 메모리 공간을 만든다.

  - 클래스 `Person p = new Person();` 이라는 소스를 작성했다면?
    - Person p는 스택 영역에 생성되고 
    - new로 생성된 Person 클래스의 인스턴스는 힙 영역에 생성된다.
    - 그리고 스택영역에 생성된 p의 값으로 힙 영역의 주소값을 가지고 있다. 
    - 즉, 스택 영역에 생성된 p가 힙 영역에 생성된 객체를 가리키고(참조하고) 있는 것입니다.

  - 메소드를 호출할 때마다 개별적으로 스택이 생성된다.

 

- **PC Register (PC 레지스터)**

  - Thread(쓰레드)가 생성될 때마다 생성되는 영역이다.
  - Program Counter 즉, 현재 쓰레드가 실행되는 부분의 Java Virtual Machine Instruction의 주소와 명령을 저장하고 있는 영역이다. (*CPU의 레지스터와 다름)

  - 이것을 이용해서 쓰레드를 돌아가면서 수행할 수 있게 한다.

 

- **Native method stack**

  - 자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역이다.

  - 보통 C/C++등의 코드를 수행하기 위한 스택이다. (JNI)


## 7. JDK와 JRE의 차이
![image](https://user-images.githubusercontent.com/66978721/103454099-cb9a1900-4d23-11eb-9c43-bf3f8469d27f.png)

### JDK(Java Development kit)란?
- 자바 개발 도구이다.
- 자바 프로그래밍을 할 때 필요한 컴파일러 등이 포함되어 있다. 
- JDK를 설치하면 JRE도 함께 설치되어 있다.
  
### JRE(Java Runtime Enviroment)
- 컴파일된 자바 프로그램을 실행시킬 수 있는 자바환경이다. 




## 참고
- [링크1](https://medium.com/@lazysoul/jvm-%EC%9D%B4%EB%9E%80-c142b01571f2)
- [링크2](https://saml2l.tistory.com/13)
- [링크3](https://opentutorials.org/module/516/5559)
- [링크4](http://www.tcpschool.com/java/java_intro_programming)
- [링크5](https://futurists.tistory.com/43)
- [링크6](https://devspark.tistory.com/entry/JIT-vs-Interpreter)
- [링크7](https://smujihoon.tistory.com/147)
- [링크8]( https://joeylee.tistory.com/30)
- 자바의 정석
