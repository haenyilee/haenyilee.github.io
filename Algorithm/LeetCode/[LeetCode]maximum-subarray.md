# [LeetCode] Maximum Subarray

## 문제

- [Maximum Subarray 문제 링크](https://leetcode.com/problems/maximum-subarray/)

- 숫자로된 배열이 주어졌을 때, 가장 큰 합계를 가지는 연속적인 부분 배열의 합계를 반환해라

  - 최소 한 개 이상의 숫자를 포함하는 부분 집합이어야 한다.

  - 배열은 연속적이어야 한다. (`contiguous` : 연속적인)



## 해결

- 최대부분합을 구하는 문제이다.

- 단순 이중 for문으로 풀게되면 시간복잡도는 `O(N³)` O(N^3^) `O(N^3^)` O(N<sup>3</sup>) `O(N<sup>3</sup>)`이다.

- [Brute Force]()방식으로 풀면 가능한 모든 부분 배열의 합을 구하고 이에 대한 최대값을 구하기 때문에 시간복잡도가 `O(N²)`이다.


- [카데인 알고리즘]()을 이용하면 시간복잡도 `O(N)`으로 문제를 해결할 수 있다.
