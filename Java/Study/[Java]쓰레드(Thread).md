---
sort: 13
---


# [Java]쓰레드(Thread)

## 프로세스와 쓰레드
- 프로세스
  - 실행 중인 프로그램
  - 자원(메모리, CPU)와 쓰레드로 구성됨
  - PID : 프로세스 아이디
- 쓰레드 : 프로세스 내에서 실제 작업을 수행하는 것

- 쓰레드의 장점 
  - CPU를 효율적으로 사용하기 때문에 속도가 빨라짐
    - 실행중인 프로그램 => 프로세스(쓰레드가 1개 이상 존재)

```note
- 하나의 새로운 프로세스를 생성하는 것보다
- 하나의 새로운 쓰레드를 생성하는 것이 더 적은 비용이 든다
```


## 멀티쓰레드
- 클라이언트 요청이 들어올때마다 각자 1개의 쓰레드를 배정시켜줘서 동시에 다른 역할 수행 가능


## 멀티쓰레드의 장점
- 자원을 보다 효율적으로 사용
- 사용자에 대한 응답성이 향상 
  - 쓰레드마다 따로따로 대답해줄 수 있어진다고 생각하면 됨
- 작업이 분리된다(소스코드가 간결화됨)



## 멀티쓰레드의 단점
- 동기화 문제 발생
- deadlock(교착상태) 발생 : 두 쓰레드가 자원을 점유한 상태에서 상대편이 점유한 자원을 사용하려고 기다리느라 진행이 멈춰있는 것
- 싱글쓰레드로 작업했을 때보다 시간이 더 걸릴 수 있다.
  - 작업을 전환할 때 context switching 시간이 소요되기 때문이다.
- 블락킹 구간이 있을 때에는 멀티쓰레드가 정체구간 없이 동시에 작업을 진행하기 때문에 더 빨리 작업을 마칠 수 있다.

## 쓰레드 구현 방법
1. Runnable 인터페이스
- 인터페이스를 구현하면 다른 클래스를 상속받을 수 있기 때문에 더욱 유연하다.

```java
// 구현
class A implements Runnable

// 생성
Runnable r = new A();
Thread t2 = new Thread(r);

// 실행
t2.start();
```

2. Thread 클래스
-  자바는 단일 상속만 허용하기 때문에 다른 클래스는 상속받을 수 없다.

```java
// 구현
class A extends Thread

// 생성
A t1 = new A();

// 실행
t1.start();
```

## 쓰레드 실행과정
> 쓰레드의 동작을 정의하고 run()을 구현하는 과정  

> 쓰레드는 반드시 한 개의 동작만 수행함

1. 생성 : ```new Thread()```  
2. 실행대기 : ```start()``` 쓰레드에서 사용하는 데이터 수집단계  
3. 동작 : ```run()``` 클라이언트가 접속을 했을 경우, 각자 통신이 가능하게 쓰레드랑 연결시켜줌
4. 기다리기
  - (1) 대기상태인 쓰레드를 실행대기 상태로 만들기 : ```interrupt()```    
  - (2) 일시정지 : ```sleep()``` , ```wait()``` , ```join()```  
    - `join()` : main쓰레드가 다른 작업이 끝날때까지 기다린다.
    
## 쓰레드의 호출
- ```start()```를 시행하면 자동으로 ```run()```이 호출됨
- 실행할때마다 속도가 달라짐
- 각 쓰레드마다 한 번씩 시행됨
    - 시분할 : 한번씩 번갈아서 수행됨
    
## 쓰레드 생성과 가상머신
- 쓰레드가 생성되면 가상머신이 자동으로 결정해주는 요소가 있음  
  - 가상머신(JVM)이 순위를 결정해줌  
    - getPriority(),setPriority()
    - 1: MIN_PRIORITY , 10 : MAX_PRIORITY , 5 : NORM_PRIORITY
    
  - 가상머신(JVM)이 각 쓰레드마다 이름을 부여해줌  
      - 자바프로그램은 MAX_PRIORITY(main)과 MIN_PRIORITY(gc)를 가지고 있기 때문에 멀티 쓰레드를 지원함
      - getName(), setName()
      
- ```.setName("")```과 ```.setPriority()```를 통해 직접 설정해줄수도 있음

  ```note
  - 사용자 정의 쓰레드 (5)
    
  ```



## 사용자 쓰레드와 데몬쓰레드
- 사용자 쓰레드 : main 쓰레드
  - 쓰레드 그룹을 지정하지 않고 생성한 쓰레드는 main 쓰레드 그룹에 속한다.
- 데몬 쓰레드 : 보조 쓰레드
  - 가비지 컬렉터, 자동저장, 화면 자동갱신 등에 사용된다.
  - 무한루프 `while(true)`로 작성한다.

## 서버와 쓰레드
- 서버는 접속만 담당하고, 클라이언트는 실제 통신을 담당한다.
- 이때 서버는 클라이언트마다 따로 통신하기 때문에 쓰레드를 이용한다.

![image](https://user-images.githubusercontent.com/66978721/103266810-c3746d80-49f3-11eb-8e9f-6fec97870248.png)
