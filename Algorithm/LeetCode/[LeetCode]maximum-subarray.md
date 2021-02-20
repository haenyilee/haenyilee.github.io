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
![image](https://user-images.githubusercontent.com/66978721/108597409-40a4d900-73cc-11eb-9d0a-f66ee31c09fc.png)
- 더 간단하게 풀기위해서 동적 계획법을 활용한 [카데인 알고리즘](https://medium.com/@vdongbin/kadanes-algorithm-%EC%B9%B4%EB%8D%B0%EC%9D%B8-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-acbc8c279f29)을 이용하면 시간복잡도 `O(N)`으로 문제를 해결할 수 있다.

- 동적 계획법은 재귀호출이 아닌 작은 문제의 값을 어딘가에 저장해두고 큰 문제를 해결할 때, 재사용하는 방법이라고 볼 수 있다.

- 작은 문제의 solution들은 겹칠 수 있다.
 
```
// 문제 : 1부터 n까지의 합을 구하기
sum(n) = (1 + 2 + 3 + ... + n)
       = (1 + ... + n-1) + n

// 분할 정복법(재귀 호출)
sum(n) = sum(n-1) + n 
sum(1) = 1

// 동적계획법 (값을 저장해두고 재사용) 
S[n] = S[n-1] + n 
S[1] = 1
```


## 코드

![그림1](https://user-images.githubusercontent.com/66978721/108597523-d7719580-73cc-11eb-9ebf-2af5595a604f.png)
![그림2](https://user-images.githubusercontent.com/66978721/108597524-d7719580-73cc-11eb-8e1d-1b634afa7f55.png)
![그림3](https://user-images.githubusercontent.com/66978721/108597525-d80a2c00-73cc-11eb-86d6-5eed756cba9e.png)
![그림4](https://user-images.githubusercontent.com/66978721/108597526-d80a2c00-73cc-11eb-9e14-e2b8c2d93c6a.png)
![그림5](https://user-images.githubusercontent.com/66978721/108597527-d8a2c280-73cc-11eb-8afc-160aa0fdf2fa.png)
![그림6](https://user-images.githubusercontent.com/66978721/108597528-d8a2c280-73cc-11eb-9720-8e97d6b198f5.png)
![그림7](https://user-images.githubusercontent.com/66978721/108597531-d93b5900-73cc-11eb-9db2-b652c932c82e.png)
![그림8](https://user-images.githubusercontent.com/66978721/108597533-d93b5900-73cc-11eb-8e57-d26cbc7b26ee.png)
![그림9](https://user-images.githubusercontent.com/66978721/108597521-d6406880-73cc-11eb-89d3-c4a27bd9e215.png)


```java
// 문제 : https://leetcode.com/problems/maximum-subarray/

public class T13_MaximumSubarray {
    public static void main(String[] args){
        int[] nums = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
        T13_MaximumSubarray t13 = new T13_MaximumSubarray();
        int result = t13.maxSubArray(nums);
        System.out.println(result);
    }
    public int maxSubArray(int[] nums) {
        int newSum = nums[0];
        int max = nums[0];

        for(int i=1; i<nums.length; i++) {
            newSum = Math.max(nums[i],nums[i]+newSum);
            max = Math.max(newSum, max);
        }
        return max;
    }
}
```


## 참고

- [Kadane’s Algorithm (카데인 알고리즘)](https://medium.com/@vdongbin/kadanes-algorithm-%EC%B9%B4%EB%8D%B0%EC%9D%B8-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-acbc8c279f29)

- [알고리즘 - 동적계획법 소개](https://www.youtube.com/watch?v=-G8kDiMAPf8)

- [알고리즘 동적계획법 - 예시문제 - 최대구간합 3+4](https://www.youtube.com/watch?v=8EBr8aPdL9g)

- [정말 쉽게 풀어보는 코딩 테스트 top 기본 문제 (with 자바)](https://www.inflearn.com/course/%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%9E%90%EB%B0%94/lecture/22827?tab=note&speed=1.25)
