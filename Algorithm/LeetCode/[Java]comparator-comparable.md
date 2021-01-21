# [Java] Comparator VS Comparable


## 1. Comparable
- 기본적으로 적용되는 정렬 기준이 되는 메서드를 정의하는 인터페이스이다. 

- Comparable 인터페이스를 구현하고 있는 클래스들은 같은 타입의 인스턴스끼지 비교할 수 있는 클래스들이다. 

- Comparable은 java.lang 패키지에 있다. 

- 오름차순으로 정렬되도록 구현되어 있다. 

```java
public interface Comparable {
  public int compareTo(Object o);
}
```

- Comparable 인터페이스의 compareTo 메서드는 비교하는 두 객체가 같은면 0, 비교하는 값보다 작으면 음수, 크면 양수를 반환한다.

- Comparable을 구현하는 클래스들(Integer, String, Data, File)은 기본적으로 오름차순으로 정렬되어 있지만, 
내림차순으로 정렬하고 싶은 경우, 반대로 뺄셈하면 된다. 

```java
public int compareTo(Integer anotherInteger) {
  int thisVal = this.value;
  int anotherVal = anotherInteger.value;
  
  return thisVal - anotherVal; 
  // 삼항연산자를 쓰면 뺄셈보다 성능이 2~3% 더 빠르다.
  // return (thisVal < anotherVal ? -1 : (thisVal == anotherVal ? 0 : 1 ));
  
}
```


- 예시) x좌표가 증가하는 순, x좌표가 같으면 y좌표가 감소하는 순으로 정렬하라.

```java
package me.haeni.string;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Point implements Comparable<Point>{

    private int x;
    private int y;

    public Point(int x,int y)
    {
        this.x = x;
        this.y = y;
    }
    @Override
    public int compareTo(Point o) {
        if(this.x > o.x) {
            return 1; // x에 대해서는 오름차순
        }
        else if(this.x == o.x) {
            if(this.y < o.y) { // y에 대해서는 내림차순
                return 1;
            }
        }
        return -1;
    }


    public static void main(String[] args) {
        List<Point> pointList = new ArrayList<>();

        Point first = new Point(9,6);
        Point second = new Point(3,4);
        Point third = new Point(6,5);

        pointList.add(first);
        pointList.add(second);
        pointList.add(third);

        Collections.sort(pointList);

        for(Point i:pointList)
        {
            System.out.println(i.x);
        }
    }
}
```

## 2. Comparator

- 기본 정렬 기준과 다르게 정렬하고 싶을 때 사용하는 인터페이스이다. 
- java.util.Comparator
- 주로 익명 클래스로 사용된다.
- 오름차순을 내림차순으로 정렬할 때 많이 사용된다. 

- 예시) 내림차순으로 정렬하기

```java
package me.haeni.string;

import java.util.Arrays;
import java.util.Comparator;

public class DescendingSort {
    public static void main(String[] args) {
        String[] strArr = {"cat", "Dog", "Rabbit", "tiger"};

        Arrays.sort(strArr, new Descending());
        System.out.println(Arrays.toString(strArr));
    }

}
class Descending implements Comparator {
    @Override
    public int compare(Object o1, Object o2) {
        if( o1 instanceof Comparable && o2 instanceof Comparable) {
            Comparable c1 = (Comparable) o1;
            Comparable c2 = (Comparable) o2;
            return c1.compareTo(c2) * -1; // 역순 정렬을 위해 기본 정렬 기준에 -1을 곱한다.
        }
        // Comparable한 대상이 아니면 -1 반환
        return -1;
    }
}
```

- Comparator interface를 implements한 후, compare() 메서드를 오버라이드한 클래스를 작성한다. 

- compare() 메서드 작성방법
  - return값이 음수 또는 0이면 자리가 그대로 유지되며, 양수인 경우에는 두 객체의 자리가 변경된다. 
  - 첫번째 < 두번째 : return 음수
  - 첫번째 = 두번째 : return 0
  - 첫번째 > 두번째 : return 양수
  - Integer.compare(x,y) : 오름차순
  - Integer.compare(y,x) : 내림차순
 
- 사용방법
  - Arrays.sort(정렬대상, 정렬기준)
  - Collections.sort(정렬대상, 정렬기준)
  - 정렬기준으로 Comparator interface를 받을 수 있다. 

```java
// 정렬기준은 가변 : Comparator c 대신 정렬 기준만 바꿔주면 된다.
static void sort(Object[] objArr, Comparator c) {

  // 정렬 방법은 불변
  for(int i =0 ; i<objArr.length-1; i++) {
  
    for(int j =0 ; j<objArr.length-1-i ; j++) {
        Object tmp = null;
        
        if(c.compare(objArr[j], objArr[j+1]>0) {
          tmp = objArr[j];
          objArr[j] = objArr[j+1];
          objArr[j+1] = tmp;
        }
        
    }
    
  }
}
```


## 출처
- [[Java] Comparable와 Comparator의 차이와 사용법](https://gmlwjd9405.github.io/2018/09/06/java-comparable-and-comparator.html)
- [자바의 정석 3판](https://github.com/castello/javajungsuk3/blob/master/source/ch11/ComparatorEx.java) 
