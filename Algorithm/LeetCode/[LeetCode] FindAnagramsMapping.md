# [LeetCode] Find Anagrams Mapping

## 문제

![image](https://user-images.githubusercontent.com/66978721/109428852-358d2100-7a3c-11eb-8c5d-df9925870499.png)

## 해결

- Anagrams이란 순서가 바뀐 단어를 뜻한다. 
<br>![image](https://user-images.githubusercontent.com/66978721/109428892-72591800-7a3c-11eb-8aa6-adca60948b3b.png)

- Map의 Key와 Value를 활용해서 푸는 간단한 문제이다. 

## 코드


```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class T14_FindAnagramsMapping {
    public static void main(String[] args) {
        int[] A = {11, 27, 45, 31, 50};
        int[] B = {50, 11, 31, 45, 27};

        T14_FindAnagramsMapping fam = new T14_FindAnagramsMapping();
        int[] result = fam.AnagramsMapping(A, B);
        System.out.println(Arrays.toString(result));

    }
    public int[] AnagramsMapping(int[] A, int[] B) {
        // 자료를 담을 그릇
        int[] result = new int[A.length];
        Map<Integer, Integer> map = new HashMap<>();

        // 배열 B의 값과 index를 Map에 담기
        for(int i = 0; i<A.length; i++) {
            map.put(B[i], i);
        }

        // 배열 A와 매핑해서 결과 도출
        for(int i=0; i<A.length; i++) {
            result[i] = map.get(A[i]);
        }

        return result;
    }
}
```

## 기억할만한 점

- 일차원 배열을 출력할 때는 `Arrays.toString(배열이름)`을 사용하면 된다. 

- `toString(배열이름)`을 사용하기 위해서는 `import java.util.Arrays;`를 해야한다. 

- `toString(배열이름)`을 사용하지 않으면 주소값이 출력된다.

```java
System.out.println(Arrays.toString(result)); // 결과 : [1, 4, 3, 2, 0]

System.out.println(result); // 결과 : [I@1b6d3586
```

- 참고 : https://haenyilee.github.io/Algorithm/LeetCode/%5BJava%5D%20Arrays.html

