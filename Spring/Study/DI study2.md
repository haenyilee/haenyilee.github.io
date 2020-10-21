---
sort: 3
---

# DI (2)

## interface
- 같은 인터페이스를 두 클래스가 받으면 오류가 난다. 
- 이 것을 제어하는 방법이 '컬리 파이어'이다. 

```java
interface I
class A implements I
class B implements I

@Autowired
I i =new A();
I i =new B();
```

- 라이브러리는 작동되는 과정을 볼 수 없기 때문에 오류를 잡기 힘들다.
  - output창을 보면서 혼자 오류볼 수 있는 힘을 키워야 한다. 
  







