# [7주차]패키지


## 1. package 키워드

### 1.1 package란?
- 자바의 package는 자바의 클래스들을 모아 넣는 자바의 디렉토리이다. 
- 비슷한 종류의 클래스나 인터페이스를 하나의 집단으로 묶은 것이다. 
- 따라서 하나의 프로젝트는 여러 개의 package로 이루어져 있다. 
- JDK에서는 개발자를 위해 제공하는 기본 package들이 존재한다. 

### 1.2 package를 사용하는 이유는?
- 클래스명의 고유성을 보장하기 위함이다. 
- 다른 패키지에 속해있다면 같은 클래스명을 생성하여도 충돌을 피할 수 있다. 

### 1.3 package 명명 규칙

```
최상위 도메인/나라코드.회사명.프로젝트명
```

- 패키지명에 대문자는 사용하지 않는 편이다. 
- 패키지 이름으로 소스가 들어가는 디렉토리가 자동으로 만들어진다. 


### 1.4 package의 선언

```
package 패키지명;
```

- 패키지 선언문은 소스파일에서 주석과 공백을 제외한 첫 번째 문장이어야 한다. 
- 하나의 소스파일에 단 한 번만 선언된다. 
- 패키지를 지정하지 않으면 자동적으로 이름없는 패키지에 속하게 된다. 
- `-d` 옵션을 추가하여 컴파일 한다. 
  - 옵션 뒤에는 루트 디렉토리의 경로를 적어준다.

```
C:\jdk1.8\work>javac -d . Test.java
                       == =========
              디렉토리경로 소스파일명 
```


### 1.5 빌트-인 패키지(Built-in Package)

- java.lang
  - import문으로 호출하지 않아도 기본적으로 로딩되는 패키지이다. 
  - 자바의 기본 클래스와 인터페이스를 제공한다. 
  - java.lang.Object클래스는 JDK에서 제공하는 모든 패키지의 최상위 클래스이다.

- java.util
  - 날짜, 수학, 자료 구조 등의 유용한 기능들을 모아놓은 패키지이다. 

- java.io
  - 데이터 저장, 호출 등의 입출력 기능을 모아둔 패키지이다. 
  - 자바에서는 입출력을 위해 Stream을 사용하는데 실제 입출력장치와 프로그램 사이의 연결을 도와주는 역할을 담당한다.

- java.net
  - 자바에서 네트워킹 프로그래밍을 할 수 있게 해주는 패키지이다. 



## 2. import 키워드
- import 키워드를 사용하면 같은 다른 패키지 내의 public 클래스를 가져와 사용할 수 있게 된다. 
- `import java.util.*` 처럼 `*`을 사용하게 되면 패키지 내의 전체 클래스를 사용할 수 있다. 
- `import java.util.Random` 처럼 특정 클래스를 지정하면 `Random` 클래스만 사용할 수 있다.

```warning
- 만약 서로다른 패키지가 같은 이름을 가진 클래스를 가지고 있다면 이는 컴파일시 에러로 이어진다. 
- 컴파일러가 무슨 클래스를 로드해야될지 모르기 때문이다.
- 이러한 컴파일 에러를 피하려면 특정 클래스의 주소를 import하는 방식을 사용해야한다. 
```

### 2.1 static import

- static import는 일반적인 import와는 다르게 메소드나 변수를 패캐지, 클래스명없이 접근가능하게 해준다.
- 예를 들어, 아래와 같이 `Math.PI` 대신 `PI`만 작성할 수 있게 된다.

```java
import static java.lang.Math.*;

public class StaticImportCase {
    public static void main(String[] args) {
        Double pi = Math.PI;
        Double pi2 = PI;
    }
}
```

- 테스트 프레임워크인 JUnit을 사용하다보면 static import를 자주 사용하게 된다. 

### 2.2 FQCN (Fully Qualified Class Name)
- FQCN은 클래스가 속한 패키지명을 모두 포함한 이름을 말한다.
- `java.lang.String s = new java.lang.String();` 처럼 쓰인 방법이 FQCN이다. 
- `String s = new String();`으로 쓰인 방식은 Alias Name 이라고 한다. 

## 3. 클래스패스

### 3.1 클래스 패스란?
- 클래스(`.class` 파일)를 찾기 위한 경로이다. 
- JVM이 프로그램을 실행할 때, 클래스 파일을 찾는 데 기준이 되는 파일 경로를 뜻한다. 

### 3.2 java runtime과 classpath
- jre(java runtime)가 `.class`파일을 실행할 때 파일을 찾기 위해 사용하는 것이 classpath에 지정된 경로이다. 
- jre(java runtime)는 이 classpath에 지정된 경로를 모두 검색해서 특정 클래스에 대한 코드가 포함된 `.class` 파일을 찾는다. 
- 찾으려는 클래스 코드가 포함된 `.class` 파일을 찾으면 첫 번째로 찾은 파일을 사용한다.

```note
- `.java` 파일 = 소스코드
- `.class` 파일 = 바이트 코드 (바이너리 형태)
```

## 4. CLASSPATH 환경변수


### 4.1 환경변수란?
- 환경변수란 프로세스가 컴퓨터에서 동작하는 방식에 영향을 미치는 동적인 값들이다. 
- 환경변수 중 path라는 변수는 운영체제가 어떤 프로세스를 실행 시킬 때 그 경로를 찾는데 이용된다.
- 자주 쓰는 프로그램의 경로를 Path에 등록해두면 그 경로에 존재하는 프로그램을 어떠한 장소에서든 실행시킬 수 있도록 한다. 

```tip
- 환경변수 확인하는 명령어  : `set`
- 예시 : ![image](https://user-images.githubusercontent.com/66978721/103438325-86b7a900-4c75-11eb-99f9-91eb24b125a5.png)
```

### 4.2 사용자변수와 시스템변수

- 사용자 변수란?
  - OS내의 사용자 별로 다르게 설정가능한 환경변수이다.
  - 해당 사용자의 계정으로 컴퓨터에 로그인 시에만 적용되는 변수이다.
  
- 시스템 변수란?
  - 시스템 전체에 모두 적용되는 환경변수이다.

### 4.3 CLASSPATH란?
- classpath는 자바에서 컴파일하기 위해 `*.class`가 모여있는 곳을 가르키는 곳이다.
- 자바 컴파일러나 클래스 파일 로더 등을 아무 위치에서나 사용하고 싶다면 Path에 이 프로그램들이 있는 경로를 지정해줘야 한다. 
- `;`를 구분자로 하여 여러 개의 경로를 클래스패스에 지정할 수 있다.

## 5. -classpath 옵션
- 환경 변수를 시스템에 설정하지 않고도 클래스 패스를 지정하는 것이 가능하다.
- 이는 javac, java를 이용해서 실행할 때 –classpath 라는 옵션을 사용하는 것이다.
- 윈도우의 경우 아래의 형태로 일단 사용이 가능하다.

```
java –classpath “C:\Program Files\Java\jre1.6.0_05\lib”;. HelloWorld
```

- 주의점은 경로상에 빈칸이 존재하는 경우에는 경로를 반드시 “”를 이용해서 묶어줘야 한다. 
- 또한 만약 자바의 경로를 환경변수 상에 지정한 경우라면 아래와 같은 형태의 표현도 가능하다.

```
java –classpath %JAVAPATH%\bin;. HelloWorld
```

- 이 경우 마찬가지로 자바의 경로상에 빈칸이 존재하는 경우라면 환경변수 설정된 디렉토리를 “”로 묶어줘야 한다.


## 6. 접근지시자

### 6.1 Modifiers
![image](https://user-images.githubusercontent.com/66978721/103439617-c6848d80-4c81-11eb-9e0b-d88b7c702470.png){: width=”50px“ height=”10px“}

- 제어자란 클래스, 변수 또는 메서드의 선언부에 함께 사용되어 부가적인 의미를 부여하는 것이다. 
- 주로 클래스나 멤버변수, 메서드에 주로 사용된다. 

### 6.2 Access Modifier
![image](https://user-images.githubusercontent.com/66978721/103439614-bcfb2580-4c81-11eb-8c83-84e3ea5d471a.png){: width=”100px“ height=”30px“}

- 자바에서는 정보은닉을 위해 접근 제어가라는 기능을 제공하고 있다. 
- 접근 지시자를 사용하면 클래스 외부에서 직접적인 접근을 허용하지 않는 멤버를 설정하여 정보 은닉을 구체화할 수 있다. 

```tip
- 객체 지향에서 정보은닉(data hiding)이란 사용자가 굳이 알 필요 없는 정보는 사용자로부터 숨겨야 한다는 개념이다.
- 그렇게 함으로써 사용자는 언제나 최소한의 정보만으로 프로그램을 손쉽게 사용할 수 있게 된다. 
```

- 접근제어자는 2개 이상 사용할 수 없다.

#### 6.2.1 private
![image](https://user-images.githubusercontent.com/66978721/103440491-7c9fa580-4c89-11eb-8f4e-aaf163ee4322.png)

- private를 사용하여 선언된 클래스 멤버는 외부에 공개되지 않으며, 외부에서 직접 접근할 수 없다. 
- private 멤버는 해당 멤버를 선언한 클래스에서만 접근할 수 있다. 


#### 6.2.2 public
![image](https://user-images.githubusercontent.com/66978721/103440540-def8a600-4c89-11eb-9b1f-3e7ecaaf9b52.png)

- 외부로 공개되며 어디에서나 직접 접근할 수 있게 된다.
- private 멤버와 프로그램 사이의 인터페이스 역할을 수행한다. 


#### 6.2.3 default
![image](https://user-images.githubusercontent.com/66978721/103440572-1ebf8d80-4c8a-11eb-877a-100f9672eff4.png)

- 접근제어의 기본값으로, 접근제어자가 명시되지 않으면 자동적으로 default 접근 제어를 가지게 된다. 
- 같은 클래스, 같은 패키지에 속하는 멤버에서만 접근할 수 있다. 



#### 6.2.4 protected
![image](https://user-images.githubusercontent.com/66978721/103440606-562e3a00-4c8a-11eb-8eda-5cae9092d5f6.png)

- 부모 클래스에 대해서는 public 멤버처럼 취급되며, 외부에서는 private 멤버처럼 취급된다. 

- **클래스의 protected 멤버에 접근할 수 있는 영역**
1. 이 멤버를 선언한 클래스의 멤버
2. 이 멤버를 선언한 클래스가 속한 패키지의 멤버
3. 이 멤버를 선언한 클래스를 상속받은 자식 클래스(child class)의 멤버


## 참고

- [https://m.blog.naver.com/PostView.nhn?blogId=k_builder&logNo=40164572729&proxyReferer=https:%2F%2Fwww.google.com%2F](https://m.blog.naver.com/PostView.nhn?blogId=k_builder&logNo=40164572729&proxyReferer=https:%2F%2Fwww.google.com%2F)
- [https://www.cpltutorials.com/Modifiers_in_Java/](https://www.cpltutorials.com/Modifiers_in_Java/)
- [http://www.tcpschool.com/java/java_modifier_accessModifier](http://www.tcpschool.com/java/java_modifier_accessModifier)
- [https://dreamzelkova.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%9D%98-%EA%B8%B0%EC%B4%88-FQCN-JAR](https://dreamzelkova.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%9D%98-%EA%B8%B0%EC%B4%88-FQCN-JAR)
