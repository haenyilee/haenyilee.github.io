# [LeetCode] Maximum Subarray

## 문제

- [Maximum Subarray 문제 링크](https://leetcode.com/problems/maximum-subarray/)

- 숫자로된 배열이 주어졌을 때, 가장 큰 합계를 가지는 연속적인 부분 배열의 합계를 반환해라

  - 최소 한 개 이상의 숫자를 포함하는 부분 집합이어야 한다.

  - 배열은 연속적이어야 한다. (`contiguous` : 연속적인)



## 해결

- 최대부분합을 구하는 문제이다.


### 브루트 포스(Brute Force)

![image](https://user-images.githubusercontent.com/66978721/107371853-17608f00-6b28-11eb-8da6-fdd14c595915.png)

- 단순 이중 for문으로 모든 경우를 탐색하는 방식이다.

- 가능한 모든 부분 배열의 합을 구하고 이에 대한 최대값을 구하기 때문에 시간복잡도는 `O(N³)`이다.

- 이때 prefixSum을 먼저 계산해두고 풀면 시간복잡도가 `O(N²)`이다.

### 분할 정복(Divide and Conquer)

![image](https://user-images.githubusercontent.com/66978721/107372767-1f6cfe80-6b29-11eb-8e78-b3be4a7a8ed3.png)

- Middel 구간 합을 구할 때는 for loop가 돌고, Left와 Right를 구할 때는 배열의 갯수만큼 재귀적으로 반복하기 때문에 시간복잡도는 `O(nlog₂n)` 이다. 

### 동적계획법(Dynamic Programming)
![image](https://user-images.githubusercontent.com/66978721/107373325-c5206d80-6b29-11eb-967d-341781b58fb5.png)

- 더 간단하게 풀기위해서 동적 계획법을 활용한 [카데인 알고리즘](https://www.youtube.com/watch?v=8EBr8aPdL9g)을 이용하면 시간복잡도 `O(N)`으로 문제를 해결할 수 있다.


## 참고

- [Kadane’s Algorithm (카데인 알고리즘)](https://medium.com/@vdongbin/kadanes-algorithm-%EC%B9%B4%EB%8D%B0%EC%9D%B8-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-acbc8c279f29)

- [알고리즘 동적계획법 - 예시문제 - 최대구간합 3+4](https://www.youtube.com/watch?v=8EBr8aPdL9g)

- [정말 쉽게 풀어보는 코딩 테스트 top 기본 문제 (with 자바)](https://www.inflearn.com/course/%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%9E%90%EB%B0%94/lecture/22827?tab=note&speed=1.25)