# [Java] 점의 좌표 구현하기


## 좌표 입력
### List 

```java
List<List<Integer>> list = new ArrayList<List<Integer>>();

// 좌표 입력
List<Integer> point1 = new ArrayList<>();
List<Integer> point2 = new ArrayList<>();
point1.add(1); point1.add(-2);
point2.add(3); point2.add(5);
```


### Array

```java
{% raw %}
// 선언과 초기화 동시에
int[][] points = new int[][]{{3,-2},{1,1}};
{% endraw %}
```

### List<List<Integer>>를 int[][]로 변환하기


```java
public static int[][] convert(List<List<Integer>> list) {
  // list크기만큼 이중 배열 생성
  int[][] array = new int[list.size()][];
  
  // i번쨰 배열은 list의 i번째 요소 사이즈
  for (int i = 0; i < array.length; i++) {
     array[i] = new int[list.get(i).size()];
  }
  ???
  for(int i=0; i<list.size(); i++){
    for (int j = 0; j < list.get(i).size(); j++) {
      array[i][j] = list.get(i).get(j);
    }
  }
return array;
}
```
