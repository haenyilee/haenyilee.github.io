# [LeetCode]Find All Anagrams in a String

## 문제

- [find-all-anagrams-in-a-string 문제 링크](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

- 아나그램이 시작되는 배열의 시작 index 값들을 구하여라.



## 풀이

### 1. Brute Force 방식 : O(N²)
- 배열 방에 특정위치를 유니크하게(아스키코드값) 셋팅해서 일일히 일치 여부를 비교하여 구하는 방법이다. 

```java
// 문제 링크 : https://leetcode.com/problems/find-all-anagrams-in-a-string/
// Brute Force 방식

import java.util.ArrayList;
import java.util.List;

class T15_1_FindAllAnagramsString {
    public static void main(String[] args) {
        String S = "cbaekabacd";
        String P = "abc";

        /*String S = "abab";
        String P = "ab";*/

        T15_1_FindAllAnagramsString fas1 = new T15_1_FindAllAnagramsString();
        System.out.println(fas1.solve1(S,P));
    }

    public List<Integer> solve1(String S, String P) {
        // 자료구조 설정
        List<Integer> result = new ArrayList<>();

        int[] Parr = new int[26];

        // 기준이 되는 P를 Parr로 구현
        for(int i=0; i<P.length(); i++) {
            Parr[P.charAt(i) - 'a']++; // abc -> [1,1,1,0,0,....]
        }

        // S의 0번째부터 3개씩 잘라서 Sarr에 담은 뒤 Parr과 비교
        for(int i=0; i<S.length()-P.length()+1; i++) {
            int[] Sarr = new int[26];
            for(int j=0; j<P.length(); j++) {
                Sarr[S.charAt(i+j) - 'a']++;
            }
            if(check(Parr,Sarr)) {
                result.add(i);
            }
        }
        return result;
    }

    // Sarr와 Parr과 비교
    private boolean check(int[] Parr, int[] Sarr) {
        for(int i=0; i<Parr.length; i++) {
            if(Parr[i] != Sarr[i]) {
                return false;
            }
        }
        return true;
    }
}
```


## 기억할만한 점

- Sarr는 매번 초기화되야 하기 때문에 비교할 for문 안에서 초기화해준다.

### 2. sliding Window방식 : O(N)
- HashMap을 활용해서 비교 기준이 되는 아나그램 값과 갯수를 저장해두고, Start Point와 End Point를 이동시켜서 비교해 푸는 방식이다. 


```java

```
