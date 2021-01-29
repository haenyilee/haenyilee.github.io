# [Java] Iterator

## Iterator

![image](https://user-images.githubusercontent.com/66978721/106254370-fa8ca780-625b-11eb-8cd6-03a7fc5fdd0d.png)

- Collection 인터페이스가 상속받았기 때문에 Set, List에서 모두 사용할 수 있다.

- 컬렉션에 저장된 요소를 읽어오는 방법을 표준화한 인터페이스이다.


## Iterator 메소드

```java
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class Ex_Iterator {
    public static void main(String[] args) {
        Set<Integer> set = new HashSet<>();
        set.add(4);
        set.add(3);
        set.add(1);

        Iterator it = set.iterator();

        while (it.hasNext()) {
            System.out.println(it.next());
        }
    }
}
```

### Iterator 

`Iterator it = set.iterator();`

- Iterator라는 데이터 타입을 가지고 있는 객체(인스턴스)를 만든다.

- `Set<Integer> set = new HashSet<>()`와 같은 형태의 Iterator를 반환한다. 

-  나는 Collection의 임시저장소 느낌으로 이해하였다.


### hasNext

`while (it.hasNext())`

- `boolean hasNext()`

- 만약 iteration이 요소를 더 가지고 있다면 true를 반환한다. 

- 더 가지고 있는 요소가 없다면 false를 반환한다.

### next

`System.out.println(it.next());`

- `E next()`

- iteration의 다음 요소를 반환하고 반환된 값을 iterator에서 삭제한다.

- 만약 반환할 요소가 없다면 `NoSuchElementException`예외를 던진다.
