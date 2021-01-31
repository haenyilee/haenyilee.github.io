# [LeetCode] longest-substring-with-at-most-two-distinct-characters

## 문제

- [LongestSubMostTwoDist 문제 링크](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)

- Given a string s, find the length of the longest substring t  that contains at most 2 distinct characters.

- Example 1:

```
Input: "eceba"
Output: 3
Explanation: t is "ece" which its length is 3.
```

- Example 2:

```
Input: "ccaabbb"
Output: 5
Explanation: t is "aabbb" which its length is 5.
```

- 최대 2개(중복 허용)의 character를 가진 가능한 가장 긴 문자열로 substring해서 그 길이를 구하라.

## 해결

- 두 개의 Pointer(start, end)를 이동시키면서 distinct한 2개의 연속 구간 갯수를 구한다.

- Pointer를 이동시키면서 각 문자가 몇 개인지 `Map`을 활용해서 저장한다.
  - Map에는 문자(Character)와 문자의 갯수(Integer)가 쌍으로 저장된다. 

- Map 키의 값이 1이면 문자의 갯수(Counter)를 증가시킨다.

- 만약 distinct한 문자가 3개 이상 나오면, Pointer를 이동시킨다.

- 연속 구간 갯수를 `int maxLength`에 저장한 뒤 `Math.max`를 활용해서 최대값을 구한다.

## 코드

```java
// 문제 링크 : https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/

import java.util.HashMap;
import java.util.Map;

public class T12_LongestSubMostTwoDist {
    public static void main(String[] args) {
//        String s = "eceba";
        String s = "ccaabbb";
        T12_LongestSubMostTwoDist t12 = new T12_LongestSubMostTwoDist();
        int result = t12.lengthOfLongestSubMostTwoDist(s);
        System.out.println(result);
    }
    public int lengthOfLongestSubMostTwoDist(String s) {
        // 1
        int start = 0, end = 0;
        int length = 0; // 구간 길이
        int counter = 0; // 구간 내 문자의 갯수
        Map<Character,Integer> map = new HashMap<>(); // 구간 내 저장된 문자

        // 2
        while(end < s.length()) {
            char endChar = s.charAt(end); // end포인터가 가르키는 문자

            // start~end구간 내 문자 현황을 Map에 저장
            map.put(endChar,map.getOrDefault(endChar,0)+1);

            // map에 새로운 문자가 저장되면 구간내 문자의 갯수 증가시키기
            if(map.get(endChar)==1) counter++;

            // end 포인터 이동시키기
            end++;

            // 3 : 구간 내 문자가 2개 초과하는 경우 start 포인터 조정
            while(counter > 2) {
                char startChar = s.charAt(start);
                map.put(startChar,map.get(startChar)-1); // map에서 문자 하나 빼기
                if(map.get(startChar)==0) counter--;
                start++;
            }
            // 최대 구간 길이
            length = Math.max(length,end-start);
        }
        return length;
    }
}
```
