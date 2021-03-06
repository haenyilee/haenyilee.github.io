# [LeetCode]Find All Anagrams in a String

## 문제

- [find-all-anagrams-in-a-string 문제 링크](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

- 아나그램이 시작되는 배열의 시작 index 값들을 구하여라.




## 1. 배열 방에 특정위치를 유니크하게(아스키코드값) 셋팅해서 구한방법

### 1.1 풀이

### 1.2 코드

```java
// 문제 링크 : https://leetcode.com/problems/find-all-anagrams-in-a-string/
// Brute Force 방식

import java.util.ArrayList;
import java.util.List;

class T15_1_FindAllAnagramsString {
    public static void main(String[] args) {
        String S = "cbaebabacd";
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

        // Sarr[0]~ Sarr[2]을 ++해준 뒤, Sarr과 Parr과 비교
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

    // Sarr와 Parr과 비교 : O(26)
    private boolean check(int[] Parr, int[] Sarr) {
        for(int i=0; i<Parr.length; i++) {
            if(Parr[i] != Sarr[i]) {
                return false;
        }
        return true;
    }
}
```


### 1.3 유의사항
- Sarr는 매번 초기화되야 하기 때문에 비교할 for문 안에서 초기화해준다.
- 또는 `Arrays.fill(Sarr, 0);`을 활용해서 for문이 돌기 전에 전체값을 0으로 채워주면 된다.

## 2. sliding Window방식 : O(N)

### 2.1 풀이
- 앞의 문자는 빼고 뒤의 문자는 더해주는 방식으로 anagram을 찾는다.
- 배열을 비교할 때, `Arrays.equals(arr1, arr2)` 인터페이스를 사용한다.

### 2.2 코드
```java
// 문제 링크 : https://leetcode.com/problems/find-all-anagrams-in-a-string/
// Brute Force 방식

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class T15_FindAllAnagramsString {
    public static void main(String[] args) {
        String S = "cbaebabacd";
        String P = "abc";

        T15_FindAllAnagramsString fas1 = new T15_FindAllAnagramsString();
        System.out.println(fas1.findAnagrams(S,P));
    }

    // Solution
    public List<Integer> findAnagrams(String S, String P) {
        List<Integer> result = new ArrayList<>();

        int[] Sarr = new int[26];
        int[] Parr = new int[26];

        // 초기화
        for(int i = 0; i<P.length();i++) {
            Sarr[S.charAt(i) - 'a']++; // 0~2자리값으로 초기화
            Parr[P.charAt(i) - 'a']++; // 전체 값 배열화
        }

        // Sliding Window
        int start = 0, end = P.length();

        while(true) {
        
            // 배열 비교
            if(Arrays.equals(Sarr,Parr)) {
                result2.add(start);
            }
            
            // Break Point
            if (end == S.length()) break;

            // start의 글자는 빼고, end의 글자는 추가하기
            Sarr[S.charAt(start++) - 'a']--;
            Sarr[S.charAt(end++) - 'a']++;
        }
        return result;
    }
}
```

### 2.3 유의사항

- 연산자 우선순위에 유의해야 한다.
- `A = 1, B = 1, C = 1`일 때, `{(A++) + (B++)} + (C++)`의 결과는 `3`이다.
- 그래서 `Sarr[S.charAt(start++) - 'a']--` 에서도 start의 값은 
연산 후{:.text-red} 
에 증가한다.

## 참고

- [bcp0109님 풀이](https://bcp0109.tistory.com/entry/LeetCode-Medium-Find-All-Anagrams-in-a-String-Java)
