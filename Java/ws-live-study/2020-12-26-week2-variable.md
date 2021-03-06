# [2주차]자바 데이터 타입, 변수 그리고 배열

## 목차
- 프리미티브 타입 종류와 값의 범위 그리고 기본 값
- 프리미티브 타입과 레퍼런스 타입
- 리터럴
- 변수 선언 및 초기화하는 방법
- 변수의 스코프와 라이프타임
- 타입 변환, 캐스팅 그리고 타입 프로모션
- 1차 및 2차 배열 선언하기
- 타입 추론, var

## 프리미티브 타입 종류와 값의 범위 그리고 기본 값
- primitive type = 원시 타입 = 기본형 타입

![image](https://user-images.githubusercontent.com/66978721/104088173-327d7c00-52a8-11eb-9d47-19e4b96f4964.png)

```tip
- A String in Java is actually a non-primitive data type, because it refers to an object.

![image](https://user-images.githubusercontent.com/66978721/104088108-cdc22180-52a7-11eb-86de-6765e1ad92f4.png)

![image](https://user-images.githubusercontent.com/66978721/104088143-f8ac7580-52a7-11eb-8a7a-0c7456ca3b97.png)
```

## 프리미티브 타입과 레퍼런스 타입

### 레퍼런스 타입
- Reference Type = 참조 타입

- 참조한다는 것은 자바에선 __실제 값이 저장되어 있는 곳의 위치를 저장 한 값__ 즉, 주소값을 뜻한다. 

- Reference Type은 JVM의 Runtime Data Area 영역 중에서 runtime stack 영역과 garbage conllection heap 영역에 저장된다.


### primitive 타입과 Reference 타입의 차이점
- **Primitive Type** : 메모리 공간에 직접 데이터를 담는다.

![image](https://user-images.githubusercontent.com/66978721/104087826-ed584a80-52a5-11eb-9d36-5aaf381c95bf.png)

![image](https://user-images.githubusercontent.com/66978721/104087813-d4e83000-52a5-11eb-8c28-9314ac199aa7.png)


- **Reference Type** : 다른 곳을 참조하는 주소값을 담는다.

![image](https://user-images.githubusercontent.com/66978721/104087971-bf273a80-52a6-11eb-8b99-f614ac875b2d.png)

![image](https://user-images.githubusercontent.com/66978721/104087997-e7af3480-52a6-11eb-9fa7-1bb3fb222198.png)

![image](https://user-images.githubusercontent.com/66978721/104088036-42e12700-52a7-11eb-865e-c6b9a4af2427.png)
 



## 리터럴
## 변수 선언 및 초기화하는 방법
## 변수의 스코프와 라이프타임
## 타입 변환, 캐스팅 그리고 타입 프로모션
## 1차 및 2차 배열 선언하기
## 타입 추론, var


## 출처
- [@nimkoes님 2주차 과제](https://blog.naver.com/hsm622/222144931396)
- [Primitive and Reference (Object) Types in Memory (Java Tutorial)](https://www.youtube.com/watch?v=LTnp79Ke8FI)
