# [String&Array] MeetingRooms

## Comparator vs Comparator
- [[Java] 객체 정렬하기 1부 - Comparable vs Comparator](https://www.daleseo.com/java-comparable-comparator/)
- [[자바의 정석 - 기초편] ch11-30~33 Comparator와 Comparable](https://www.youtube.com/watch?v=EW3Mub24wYg)
- [[JAVA]Array.sort 와 Collections.sort 의 차이](https://devlog-wjdrbs96.tistory.com/68)

## 1. Array.sort

### 1.1 Array.sort (오름차순)

```
Array.sort(정렬대상(배열),정렬기준) 
```

- 정렬이 되는 기준은 `오름차순 : 숫자 > 대문자 > 소문자 > 한글` 이다.
  - 정렬 기준이 없으면 기본적으로 사전순서에 맞게 문자와 숫자를 정렬한다.
  - 대문자보다 소문자를 먼저 정렬한다.


```java
String[] strArr = {"cat", "Dog", "lion", "tiger"};

// 기본정렬 : 대소문자 구분
Array.sort(strArr); // {"Dog", "cat", "lion", "tiger"}
```
  
- 대소문자를 구분하지 않고 정렬하려면 `String.CASE_INSENSITIVE_ORDER`을 사용한다.
  - String.CASE_INSENSITIVE_ORDER : Comparator 인터페이스를 구현하는 내부 클래스 


```java
String[] strArr = {"cat", "Dog", "lion", "tiger"};

// 대소문자 구분 x
Array.sort(strArr, String.CASE_INSENSITIVE_ORDER); // {"cat", "Dog", "lion", "tiger"}
```

### 1.2 Array.sort (내림차순)

```
Array.sort(정렬대상, Collections.reverseOrder());
```

- `Collections.reverseOrder()`은 파이썬에서의 `reverse` 내장함수와 같다.
- 오름차순과 마찬가지로, `한글 > 소문자 > 대문자 > 숫자` 정렬이 된다. 



## 2. Collections.sort 


### 2.1 기본정렬

- 오름차순 : `Collection.sort(정렬대상);`
  - 순서 : 한글 > 소문자 > 대문자 > 숫자
  
- 내림차순 : `Collection.reverse(정렬대상);`
  - 순서 : 숫자 > 대문자 > 소문자 > 한글



```note
- Comparator와 Comparable은 정렬 기준을 제공하는 메서드를 정의한 인터페이스이다. 
  - Comparator : 기본 정렬기준 외에 다른 
  - Comparable : 기본 정렬 기준을 구현하는데 사용됨
```

### 2.2 Comparable 


#### compareTo

```java
public final class Integer extens Number implements Comparable {
  ...
   public int compareTo(Integer anotherInteger) {
   
      // 자기자신(this)와 anotherInteger을 비교함
      int v1 = this.value;
      int v2 = anotherInterger.value;
      
      // v1과 v2가 같으면 0, 오른쪽 값이 크면 -1, 오른쪽 같이 작으면 1을 반환
      // 3항연산자 2번 사용
      return (v1 < v2 ? -1 : (v1==v2? 0 : 1));
   }
   ...
}
```




### 2.3 Comparator

#### compare

```java
// 역순으로 정렬하기
Arrays.sort(strArr, new Descendion()); 

// 정렬 기준 정의하기
class Descending implements Comparator {
  public int compare(Object o1, Object o2) {
    if(o1 instanceof Comparable && o2 instanceof Comparable) {
      Comparable c1 = (Comparable)o1;
      Comparable c2 = (Comparable)o2;
      return c1.compareTo(c2) * -1 // 기본 정렬방식의 역으로 변경하기 위해 -1을 곱함
    }
    return -1;
  }
}
```

