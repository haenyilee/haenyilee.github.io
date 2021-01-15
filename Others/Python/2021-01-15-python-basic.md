# [Python] 파이썬 기초 문법 포인트

6개월동안 자바 배우면서 흐릿해진 파이썬의 기억을 상기하고자 정리합니다...★

## 1. 자료형

### 1.1 bool

- True, False가 있다. 

- true, false처럼 소문자로 사용하면 안된다. 

- 논리 연산자를 이용할 수 있다. 

```python
a = 4 > 2 # True
not a     # False
```

### 1.2 list

```python
a_list = []
a_list.append(1)     # 리스트에 값을 넣는다
a_list.append([2,3]) # 리스트에 [2,3]이라는 리스트를 다시 넣는다

a_list         # [1,[2,3]]
len(a_list)    # 2 리스트의 길이
a_list[0]      # 1
a_list[1]      # [2,3]
a_list[1][0]   # 2
```

- 리스트 간의 덧셈과 자연수 곱셈이 정의되어 있다. 

```python
a = [3, 3, 1]
b = [5, 2]

a + b  # 리스트 연결 : [3, 3, 1, 5, 2]
a * 2  # 리스트 n번 반복 : [3, 3, 1, 3, 3, 1]
```

### 1.3 dictionary

```python
a_dict = {}
a_dict = {'name':'bob','age':21}
a_dict['height'] = 178

# a_dict의 값은? {'name':'bob','age':21, 'height':178}
# a_dict['name']의 값은? 'bob'
# a_dict['age']의 값은? 21
# a_dict['height']의 값은? 178
```

### 1.4 list와 dict 

```python
people = [{'name':'bob','age':20},{'name':'carry','age':38}]

# people[0]['name']의 값은? 'bob'
# people[1]['name']의 값은? 'carry'

person = {'name':'john','age':7}
people.append(person)

# people의 값은? [{'name':'bob','age':20},{'name':'carry','age':38},{'name':'john','age':7}]
# people[2]['name']의 값은? 'john'
```

## 2. 함수

### 2.1 함수의 정의

```python
def f(x):
    return 2*x+3   # 중괄호 대신에 들여쓰기로 각 블록의 범위를 표시한다.

f(2)  # 7
```

### 2.2 함수의 응용

```python
def sum_all(a,b,c):
    return a+b+c

def mul(a,b):
    return a*b

result = sum_all(1,2,3) + mul(10,10)

# result라는 변수의 값은? 6 + 100 = 106
```

## 3. 조건문

- if (~ elif) ~ else 형식으로 사용한다. 

```python
def is_adult(age):
	  if age > 20:
		    print('성인입니다') 
	  elif age >= 13:
		    print('청소년이에요')  
    else:
        print('어린이네요!')

is_adult(30)              # 성인입니다
```

## 4. 반복문

- 파이썬에서 반복문은 리스트의 요소들을 하나씩 꺼내쓰는 형태이다. 

```python
fruits = ['사과','배','감','귤']

for fruit in fruits:
    print(fruit)

# 사과
# 배
# 감
# 귤
```

## 5. 연습문제

### 5.1 리스트에 담긴 과일의 갯수 세기

```python
fruits = ['사과', '배', '배', '감', '수박', '귤', '딸기', '사과', '배', '수박']


def cnt(target):
    result = 0;
    result = len(target)
    return print(result)


cnt(fruits)
```

### 5.2 사람의 나이 출력하기

```warning
- str과 int는 + 로 연결할 수 없다.

`
print(person['name']+person['age'])
TypeError: can only concatenate str (not "int") to str
`

```


```python
people = [{'name': 'bob', 'age': 20},
          {'name': 'carry', 'age': 38},
          {'name': 'john', 'age': 7},
          {'name': 'smith', 'age': 17},
          {'name': 'ben', 'age': 27}]

# 각 사람의 이름과 나이 출력해보기
for person in people:
    print(person['name'], person['age'])

# 사람의 나이를 구하기
def count(name):
    for person in people:
        if name == person['name']:
            return person['age']

# bob의 나이 출력하기        
print(count('bob'))
```
