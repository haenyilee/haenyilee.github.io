---
sort: 
---

# [Oracle] ROLL UP / GROUP BY /CUBE

### ```GROUP BY``` : 단순히 모아서 출력
```
SQL> SELECT deptno,sal,COUNT(*) FROM emp GROUP BY deptno,sal ORDER BY deptno,sal;

    DEPTNO        SAL   COUNT(*)
---------- ---------- ----------
        10       1300          1
        10       2450          1
        10       5000          1
        10                     1
        20        800          1
        20       1100          1
        20       2975          1
        20       3000          2
        30        950          1
        30       1250          2
        30       1500          1
        30       1600          1
        30       2850          1

13 rows selected.
```

### ```ROLLUP``` : 모은 뒤 모은 것끼리 합계를 구하고, 그 합계들의 합계를 구함

```
SQL> SELECT deptno,sal,COUNT(*) FROM emp GROUP BY ROLLUP(deptno,sal) ORDER BY deptno,sal;

    DEPTNO        SAL   COUNT(*)
---------- ---------- ----------
        10       1300          1
        10       2450          1
        10       5000          1
        10                     1
        10                     4
        20        800          1
        20       1100          1
        20       2975          1
        20       3000          2
        20                     5
        30        950          1
        30       1250          2
        30       1500          1
        30       1600          1
        30       2850          1
        30                     6
                              15

17 rows selected.
```

```
SQL> SELECT deptno,sal FROM emp GROUP BY ROLLUP(deptno,sal) ORDER BY deptno,sal;

    DEPTNO        SAL
---------- ----------
        10       1300
        10       2450
        10       5000
        10
        10
        20        800
        20       1100
        20       2975
        20       3000
        20
        30        950
        30       1250
        30       1500
        30       1600
        30       2850
        30


17 rows selected.
```

```
SQL> SELECT deptno,sal FROM emp GROUP BY ROLLUP(sal,deptno) ORDER BY deptno,sal;

    DEPTNO        SAL
---------- ----------
        10       1300
        10       2450
        10       5000
        10
        20        800
        20       1100
        20       2975
        20       3000
        30        950
        30       1250
        30       1500
        30       1600
        30       2850
                  800
                  950
                 1100
                 1250
                 1300
                 1500
                 1600
                 2450
                 2850
                 2975
                 3000
                 5000



27 rows selected.
```


```
SQL> SELECT sal,deptno,COUNT(*) FROM emp GROUP BY ROLLUP(sal,deptno) ORDER BY sal,deptno;

       SAL     DEPTNO   COUNT(*)
---------- ---------- ----------
       800         20          1
       800                     1
       950         30          1
       950                     1
      1100         20          1
      1100                     1
      1250         30          2
      1250                     2
      1300         10          1
      1300                     1
      1500         30          1
      1500                     1
      1600         30          1
      1600                     1
      2450         10          1
      2450                     1
      2850         30          1
      2850                     1
      2975         20          1
      2975                     1
      3000         20          2
      3000                     2
      5000         10          1
      5000                     1
                   10          1
                               1
                              15

27 rows selected.
```
