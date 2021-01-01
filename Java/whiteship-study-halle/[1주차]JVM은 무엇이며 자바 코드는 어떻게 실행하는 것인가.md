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
- Java 파일 하나만 만들면 어느 OS든 각자의 OS에 맞는 JVM 위에서 실행 할 수 있게 되는 것이다. 

![image](https://user-images.githubusercontent.com/66978721/103441306-4a457680-4c90-11eb-8e7c-741718c9c874.png)
  
- 자바로 작성된 어플리케이션은 모두 이 가상머신(JVM)위에서만 실행되기 때문에 자바 어플리케이션이 실행되기 위해서는 반드시 JVM이 필요하다.








- JVM은 크게 Class Loader, Runtime Data Areas, Excution Engine 3가지로 구성되어 있다.

- Java compiler는 `.java` 파일을 `.class` 라는 Java byte code로 변환 시켜 준다.







## 컴파일 하는 방법

### 컴파일이란?
- 컴퓨터에게 일을 시키기 위해서 사람의 말을 컴퓨터가 알아듣게 기계어로 번역하는 것이다.
- 프로그래밍 언어로 작성한 코드를 기계가 알아듣게 다른 언어로 옮기는 작업을 컴파일이라고 한다. 
- 이러한 작업을 하는 프로그램은 compiler라고 한다. 


## 실행하는 방법


## 바이트코드란 무엇인가


## JIT 컴파일러란 무엇이며 어떻게 동작하는지


## JVM 구성 요소


## JDK와 JRE의 차이

## 참고
- [](https://medium.com/@lazysoul/jvm-%EC%9D%B4%EB%9E%80-c142b01571f2)
- [](https://saml2l.tistory.com/13)
