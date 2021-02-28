# [LeetCode]Find All Anagrams in a String

## 문제

- [find-all-anagrams-in-a-string 문제 링크](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

- 아나그램이 시작되는 배열의 시작 index 값들을 구하여라.



## 풀이

### 1. Brute Force 방식 : O(N²)
- 배열 방에 특정위치를 유니크하게(아스키코드값) 셋팅해서 일일히 일치 여부를 비교하여 구하는 방법이다. 


### 2. sliding Window방식 : O(N)
- HashMap을 활용해서 아나그램 기준값을 저장해두고, Start Point와 End Point를 이동시켜서 비교해 푸는 방식이다. 
