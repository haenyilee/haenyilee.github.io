# [LeetCode] K-Closest

## 문제
[K-Closest](https://leetcode.com/problems/k-closest-points-to-origin/)

- 원점으로부터의 가까운 거리순으로 정렬했을 때, 가까운 순서대로 K개를 뽑아 배열로 리턴하는 문제이다.


## 해결방법

- 힙정렬 : 최대 k 만큼 떨어진 요소들을 정렬할 때 삽입 정렬보다 더욱 개선된 결과를 얻어낼 수 있다.

- 순차적으로 정렬한 것을 PriorityQueue를 이용해 담는다. 

- 원점으로부터의 거리가 가장 가까운 것부터 오름차순으로 정렬하는 방법 : `Comparator<정렬 요소 타입>`, `Comparable<정렬 요소 타입>`

- 두 점의 좌표 구하기 : 제곱 = `Math.pow(2,4)=16` , 루트(제곱근) = `Math.sqrt(25)=5`

- K번째로 뺀 좌표까지 도출한다.

## 코드

```java
{% raw %}
package PriorityQueue;

import java.util.*;

public class KClosest {
    public int[][] kClosest(int[][] points, int k) {
        PriorityQueue<Point> queue = new PriorityQueue<>();

        for(int i=0; i<points.length; i++)
            queue.offer(new Point(points[i][0], points[i][1]));

        int[][] result = new int[k][2];

        for(int i=0; i<k; i++) {
            Point p = queue.poll();
            result[i][0] = p.x;
            result[i][1] = p.y;
        }

        return result;
    }

    public static void main(String[] args) {
        KClosest kc = new KClosest();
        int[][] points = new int[][]{{-1,3},{-5,2}};
        int k = 2;
        System.out.println(Arrays.deepToString(kc.kClosest(points,k)));
    }
}
class Point implements Comparable<Point> {
    public int x;
    public int y;
    public int distance;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
        this.distance = x*x + y*y;
    }
    @Override
    public int compareTo(Point other) {
        return Integer.compare(distance,other.distance);
    }
}
{% endraw %}
```
