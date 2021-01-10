# [java] Arrays

## 1. Arrays 
- Arrays클래스에는 배열을 다루기 편리한 메서드(static)가 정의되어 있다.

### 1.1 toString(), equals()  - 1차원 배열의 출력과 비교

- 출력 : `toString()`

```java
int[] arr = {2, 1, 4, 6, 7, 3, 5};

System.out.println(Arrays.toString(arr));
// 출력결과  : [2, 1, 4, 6, 7, 3, 5]
```

- 비교 : `equals()`

```java
int[] arr = {2, 1, 4, 6, 7, 3, 5};
int[] arr2 = {2, 1, 4, 6, 7, 3, 5};

System.out.println(Arrays.equals(arr, arr2));
// 출력결과  : true
```

### 1.2 deepToString(), deepEquals() - 다차원 배열의 출력과 비교

- 출력 : `deepToString()`

```java
int[][] arr2D = { {11,12}, {21,22} };
System.out.println(Arrays.deepToString(arr2D));
// 출력결과  : [[11, 12], [21, 22]]
```

- 비교 : `deepEquals()`

```java
String[][] str2D = new String[][]{{"AAA","BBB","CCC"}, {"DDD","EEE","FFF"}};
String[][] str2D2 = new String[][]{{"AAA","BBB","CCC"}, {"DDD","EEE","FFF"}};

System.out.println(Arrays.deepEquals(str2D,str2D2)); 
// 출력결과 : true
```

### 1.3 copyOf() , copyOfRange() - 배열의 복사
- 배열의 생성과 복사를 함께 해줌

- `copyOf()` : 끝범위만 지정 가능 

```java
int[] arr = {0, 1, 2, 3, 4};

int[] arr2 = Arrays.copyOf(arr, arr.length); // {0, 1, 2, 3, 4}
int[] arr3 = Arrays.copyOf(arr, 3); // {0, 1, 2}
```

- `copyOfRange()` : 시작, 끝 범위 모두 지정 가능

```java
int[] arr = {0, 1, 2, 3, 4};

int[] arr2 = Arrays.copyOfRange(arr, 0, arr.length); // {0, 1, 2, 3, 4}
int[] arr3 = Arrays.copyOfRange(arr, 3 , 4); // {3, 4}
```

### 1.4 fill(), setAll() - 배열 채우기
- `fill()` : 배열의 모든 요소를 지정된 값으로 채운다 

- `setAll()` : 배열을 채운데 사용할 함수형 인터페이스를 매개변수로 받는다. 

```java
int[] arr = new int[5];

Arrays.fill(arr, 9); // arr = [9, 9, 9, 9, 9]
Arrays.fill(arr, () -> (int)(Math.random() * 5) + 1); // arr = [0 ,2, 1, 4, 4]
```

### 1.5 sort() - 배열의 정렬

- `sort()` : 배열을 정렬

- 정렬 기준이 없으면 기본적으로 Comparable 구현에 의해 정렬된다.

  - Comparable의 오름차순 정렬이 되는 기준은 `숫자 > 대문자 > 소문자 > 한글` 이다.


```java
String[] strArr = {"cat", "Dog", "lion", "tiger"};

// 기본정렬 : 대소문자 구분
Array.sort(strArr); // {"Dog", "cat", "lion", "tiger"}
```
  
- 대소문자를 구분하지 않고 정렬하려면 `String.CASE_INSENSITIVE_ORDER`을 사용한다.

  - `String.CASE_INSENSITIVE_ORDER` : Comparator 인터페이스를 구현하는 내부 클래스 


```java
String[] strArr = {"cat", "Dog", "lion", "tiger"};

// 대소문자 구분 x
Array.sort(strArr, String.CASE_INSENSITIVE_ORDER); // {"cat", "Dog", "lion", "tiger"}
```


- 내림차순 정렬은 `Collections.reverseOrder()`을 사용한다.

  - `Collections.reverseOrder()`은 파이썬에서의 `reverse` 내장함수와 같다.

  - 오름차순과 반대로, `한글 > 소문자 > 대문자 > 숫자` 정렬이 된다. 


### 1.6 binarySearch() - 배열의 정렬과 검색

- `binarySearch()` : 배열이 저장된 index번호를 찾아서 반환
  - 배열이 정렬되어 있어야 올바른 결과를 얻는다.
  - 중복값이 존재한다면 어떤 것의 위치가 반환될 지 알 수 없다.
  - 이진 검색은 배열의 검색할 범위를 반복적으로 절반씩 줄여가면서 검색하기 때문에 검색속도가 상당히 빠르다.
  
```java
int[] arr = {3, 2, 0, 1, 4};
int idx = Arrays.binarySearch(arr, 2} // 오류발생! : 정렬이 되어 있지 않기 때문

Arrays.sort(arr);
System.out.println(Arrays.toString(arr)); // [0, 1, 2, 3, 4]
idx = Array.binarySearch(arr, 2); // idx = 2
```

### 1.7 asList(Object...a) - 배열을 List로 변환
- 배열을 List에 담아서 반환한다.
- 매개변수 타입이 가변인수라서 배열 생성 없이, 저장할 요소들만 나열하는 것도 가능하다.

```java
List list = Arrays.asList(new Integer[]{1, 2, 3, 4, 5}); // list = [1, 2, 3, 4, 5]
List list2 = Arrays.asList(1, 2, 3, 4, 5); // list2 = [1, 2, 3, 4, 5]
```

- asList가 반환한 List는 추가/삭제가 불가능하기 때문에 new를 통해 List 객체를 먼저 생성 후 asList를 반환결과를 대입하면 된다.

```java
List list = new ArrayList(Arrays.asList(1, 2, 3, 4, 5)); // list = [1, 2, 3, 4, 5]
// 수정, 삭제 가능함
```


## 출처
- 자바의 정석 3판 : 11장, 624page ~ 627page
