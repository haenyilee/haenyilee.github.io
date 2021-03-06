# [LeetCode]Find All Anagrams in a String

## 문제

- [find-all-anagrams-in-a-string 문제 링크](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

- 아나그램이 시작되는 배열의 시작 index 값들을 구하여라.




## 1. 배열 방에 특정위치를 셋팅해서 구한방법

### 1.1 풀이
- 배열 방에 특정위치를 아스키코드값을 이용해서 유니크하게 셋팅해서 구하는 방법이다.

- **아스키코드**
<br>![image](https://user-images.githubusercontent.com/66978721/110206429-e4a87d00-7ec0-11eb-9de6-4a28fdc5633a.png)
    - a는 이진법(decimal)으로 97이다.
    - (a – a) = (97 – 97) = 0
    - (b – a) = (98 – 97) = 1
    - (z – a) = (122 – 97) = 25 


- 따라서 `String P = "cba"`를 아스키코드값으로 만든 알파벳 소문자 방(`int[] Parr`)에 저장하면 아래와 같다.
![image](https://user-images.githubusercontent.com/66978721/110206490-4cf75e80-7ec1-11eb-87c9-db6ac01c0ddd.png)

- **풀이과정**
    - 소문자 알파벳을 아스키코드를 활용해 유니크한 배열로 만든다.
    - `String S, P`를 유니크한 알파벳 배열에 담는다.
    - `String S = "cbaebabacd"`를 P의 길이만큼 Anagram으로 만든 뒤 아스키 코드로 만든 방에 담는다.
    - `S.length()-P.length()+1`까지 순회하면서 Anagram이 P와 일치하는지 확인한다.
    - 이때, `Sarr`과 `Parr`은 배열의 크기만큼 for문을 돌려 각 방의 값이 일치하는지 비교한다.

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

### 1.3 그림으로 이해하는 풀이
![image](https://user-images.githubusercontent.com/66978721/110206739-f428c580-7ec2-11eb-8d31-09b7a6121b94.png)
![image](https://user-images.githubusercontent.com/66978721/110206776-1cb0bf80-7ec3-11eb-8b83-dbb0746ea01f.png)
![image](https://user-images.githubusercontent.com/66978721/110206783-2803eb00-7ec3-11eb-9ddd-ce91beb90685.png)

### 1.4 유의사항
- Sarr는 순회할때마다 새로운 Anagram을 담아야 하기 때문에 매번 초기화되야 한다. 따라서 비교할 for문 안에서 초기화해준다.
- 혹은 Parr과 함께 초기화한 후 `Arrays.fill(Sarr, 0)`을 활용해서 for문이 돌기 전에 전체값을 0으로 채워주면 된다.

- **아스키코드의 합으로 비교하면 안되는 이유는?**
![image](https://user-images.githubusercontent.com/66978721/110206810-5681c600-7ec3-11eb-9c4d-254206ae2e7f.png)


## 2. sliding Window방식 : O(N)

### 2.1 풀이
- 1번의 방식과 유사하지만, 이중 for문이 아닌 start와 end를 활용해 앞의 문자는 빼고 뒤의 문자는 더해주는 방식으로 anagram을 찾기 때문에 좀 더 효율적이다.
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

### 2.3 그림으로 이해하는 코드

```java
int[] Sarr = new int[26];
int[] Parr = new int[26];

// 초기화
for(int i = 0; i<P.length();i++) {
    Sarr[S.charAt(i) - 'a']++; // 0~2자리값으로 초기화
    Parr[P.charAt(i) - 'a']++; // 전체 값 배열화
}
```

- Sarr 초기화
![image](https://user-images.githubusercontent.com/66978721/110209789-a5cef300-7ed1-11eb-9bd3-6d85a8f10024.png)


- Parr 초기화

![image](https://user-images.githubusercontent.com/66978721/110209782-9b145e00-7ed1-11eb-96f4-2457e98f89b8.png)



- Sliding Window : Start = 0, End = 3

![image](https://user-images.githubusercontent.com/66978721/110209832-f8101400-7ed1-11eb-89c4-e0a4975733aa.png)


- Sliding Window : Start = 1, End = 4
![image](https://user-images.githubusercontent.com/66978721/110209859-11b15b80-7ed2-11eb-8582-bacf65abc1f4.png)


- Sliding Window : Start = 2, End = 5
![image](https://user-images.githubusercontent.com/66978721/110209868-20980e00-7ed2-11eb-9571-b6c0a9b649ad.png)


- Sliding Window : Start = 3, End = 6
![image](https://user-images.githubusercontent.com/66978721/110209876-2f7ec080-7ed2-11eb-9f76-5dbf3fa42514.png)

- Sliding Window : Start = 4, End = 7
![image](https://user-images.githubusercontent.com/66978721/110209881-3ad1ec00-7ed2-11eb-9361-dc48271f9bc6.png)


- 같은 방식으로 `Start = 5, End = 8` , `Start = 6, End = 9`까지 돌고 `if (end == S.length()) break;` 조건으로 인해 while문을 빠져나온다.


### 2.4 유의사항

- 연산자 우선순위에 유의해야 한다.
- `A = 1, B = 1, C = 1`일 때, `{(A++) + (B++)} + (C++)`의 결과는 `3`이다.
- 그래서 `Sarr[S.charAt(start++) - 'a']--` 에서도 `start`의 값은 _연산 후에 증가_ 한다.
- 참고 : 
![image](https://user-images.githubusercontent.com/66978721/110209930-8c7a7680-7ed2-11eb-8eb8-5e7159246bb9.png)


- while문의 break point를 설정하지 않으면 에러가 발생한다.


- **`배열1.equals(배열2)`와 `Arrays.equals(배열1, 배열2)`의 차이점은?** 
![image](https://user-images.githubusercontent.com/66978721/110207150-ed9b4d80-7ec4-11eb-9371-a2b5b821acfa.png)


## 참고
- [bcp0109님 풀이](https://bcp0109.tistory.com/entry/LeetCode-Medium-Find-All-Anagrams-in-a-String-Java)
