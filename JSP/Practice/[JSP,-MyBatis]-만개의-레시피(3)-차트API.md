---
sort: 2
---

# 만개의 레시피 - 차트 API

## google visualization
- Chart-gallery : https://developers.google.com/chart/interactive/docs/gallery
- 3d pie chart : https://developers.google.com/chart/interactive/docs/gallery/piechart#making-a-3d-pie-chart


## chef.jsp
- 파이차트 스크립트 작성
  - css스타일 밑에 붙여넣기
```
<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
    <script type="text/javascript">
      google.charts.load("current", {packages:["corechart"]});
      google.charts.setOnLoadCallback(drawChart);
      function drawChart() {
        var data = google.visualization.arrayToDataTable([
          ['Task', 'Hours per Day'],
          ['Work',     11],
          ['Eat',      2],
          ['Commute',  2],
          ['Watch TV', 2],
          ['Sleep',    7]
        ]);

        var options = {
          title: 'My Daily Activities',
          is3D: true,
        };

        var chart = new google.visualization.PieChart(document.getElementById('piechart_3d'));
        chart.draw(data, options);
      }
    </script>
```
- 파이차트 모양 출력
```
<div id="piechart_3d" style="width: 900px; height: 500px;"></div>
```

## mapper.xml
- 10명만 출력하기

## VO.java
- recipeCount 변수 추가

## DAO.java
- sql결과값 받기
- for문 돌려서 값 넣기?

## Model.java
- chefListData request에 결과값 담기
```java
List<ChefVO> cList=RecipeDAO.chefRecipeCount();
request.setAttribute("cList", cList);
```
