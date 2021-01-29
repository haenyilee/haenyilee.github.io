# [Java] Set

## Set이란?

- 중복되는 값들은 제거되고 고유한 값들만 저장되는 Container이다.

- 순서대로 저장되어 있지 않다.

```tip
**List**
- List는 중복되는 값 저장이 가능하다.
- List에 있는 값들은 순서대로 저장되어 있다.
```

- 수학에서의 집합과 같다.

- Collection 인터페이스를 상속받을 뿐 Set 고유 인터페이스는 없다.

![image](https://user-images.githubusercontent.com/66978721/106254370-fa8ca780-625b-11eb-8cd6-03a7fc5fdd0d.png)


## Set Methods

### add() : 값을 넣기

- `boolean add(E e)`

### remove() : 제거하기

- `boolean remove(Object o)`

### size() : 갯수 세기

- `int size()`


### isEmpty() : 

- `boolean isEmpty()`

### contains()

- `boolean contains(Object o)`


### toArray() : 배열로 바꾸기

- `Object[] toArray()`

- `<T> T[] toArray(T[] a)`

### containAll() : 부분집합

- `boolean containsAll(Collection<?> c)`

- `A.containAll(B)` : B는 A의 부분집합인가?

- B가 A의 부분집합이면 true를 반환하고, 아니면 false를 반환한다.


### addAll() : 합집합

- `A.addAll(B)` : B 전체를 A에 add한다.

- `boolean addAll(Collection<? extends E> c)` : 값이 바뀌면 true를, 바뀌지 않으면 false를 반환한다.

- A와 B의 합집합을 만들 때 사용된다.

```java
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class Ex_Iterator {
    public static void main(String[] args) {
        // SET A 생성
        Set<Integer> A = new HashSet<>();
        A.add(4);
        A.add(3);
        A.add(1);

        // SET B 생성
        Set<Integer> B = new HashSet<>();
        B.add(100);
        B.add(3);
        B.add(-25);

        // A와 B 합집합 만들기
        A.addAll(B);

        // A 출력하기
        Iterator it = A.iterator();

        while (it.hasNext()) {
            System.out.println(it.next()); // 결과 : 1 3 4 100 -25
        }
    }
}
```

### retainAll() : 교집합

- `A.retainAll(B)` : A와 B의 교집합 값만을 A에 남기겠다.

- `boolean retainAll(Collection<?> c)` : 값이 바뀌면 true를, 바뀌지 않으면 false를 반환한다.

```java
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class Ex_Iterator {
    public static void main(String[] args) {
        // SET A 생성
        Set<Integer> A = new HashSet<>();
        A.add(4);
        A.add(3);
        A.add(1);
        A.add(3);

        // SET B 생성
        Set<Integer> B = new HashSet<>();
        B.add(100);
        B.add(3);
        B.add(-25);
        B.add(30);

        // A와 B 교집합 만들기
        System.out.println(A.retainAll(B)); // true

        // A 출력하기
        Iterator it = A.iterator();

        while (it.hasNext()) {
            System.out.println(it.next());  // 결과 : 1 3
        }
    }
}
```

### removeAll() : 차집합

- `A.removeAll(B)` : A집합에서 B를 뺀 차집합

- `boolean removeAll(Collection<?> c)` : 값이 바뀌면 true를, 바뀌지 않으면 false를 반환한다.

```JAVA
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class Ex_Iterator {
    public static void main(String[] args) {
        // SET A 생성
        Set<Integer> A = new HashSet<>();
        A.add(4);
        A.add(3);
        A.add(1);
        A.add(3);

        // SET B 생성
        Set<Integer> B = new HashSet<>();
        B.add(100);
        B.add(3);
        B.add(-25);
        B.add(30);

        // A = (A-B) 차집합 만들기
        System.out.println(A.removeAll(B)); // true

        // A 출력하기
        Iterator it = A.iterator();

        while (it.hasNext()) {
            System.out.println(it.next());  // 결과 : 1 4
        }
    }
}
```

