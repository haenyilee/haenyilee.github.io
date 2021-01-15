# [Java] String,StringBuffer,StringBuffer

## String
- 연산이나 concat할 때 새로운 String객체를 만든다.(new)

- String의 길이를 구할 때 `length()`를 사용한다. 

```java
String s = "abcdefghi";

System.out.println(s.length()); // length : 10
System.out.println(newStr.charAt(newStr.length()-1)); // 마지막글자 = i
```

## StringBuffer
- StringBuffer : 멀티 쓰레드 환경에서는 synchronized(동기화)

## StringBuilder
- StringBuilder : ansynchronized 싱글 쓰레드 환경

### `append()`
- 문자열 데이터 끝에 문자의 형태로 추가

### `insert()`
- 첫번째 인자로 삽입될 위치 (0이 맨 앞을 의미 문자가 삽입될 위치), 두번째 인자 삽입될 문자

- 2 자리에 삽입된다면, 2자리 있던 것이 뒤로 밀린다.

### 예시 : 8F3Z-2e-9-wfj을 8F-3Z2E-9WFJ로 바꾸기

```java
String S = "8F3Z-2e-9-wfj";

String newStr = S.replace("-",""); // - 없애기
newStr = newStr.toUpperCase();     // 대문자로 바꾸기

StringBuilder sb = new StringBuilder(); // StringBuilder 생성

// append 예시
for(int i = 0; i < newStr.length(); i++) {
    sb.append(newStr.charAt(i)); // StringBuilder에 추가하기
}

int len = sb.toString().length();

// insert예시
for(int i = K; i < len ; i = i + K) {
    sb.insert(len - i,'-');
}
```
