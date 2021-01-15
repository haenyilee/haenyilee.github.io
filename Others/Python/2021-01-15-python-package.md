# [Pyhton] 파이썬 패키지

## 1. 패키지란?
- 패키지는 모듈을 모아 놓은 단위이다.

- 패키지의 묶음을 라이브러리라고 한다.

- 파이썬 배포 패키지들을 설치하거나 업그레이드할 때는 가상 환경(Virtual Environment)라는 격리된 실행 환경을 사용한다.

- 서로 다른 프로젝트에서 요구하는 패키지 목록이나 버전이 다르기 때문에 다른 파이썬 응용 프로그램의 동작에 영향을 주지 않기 위함이다.

- 파이참에서 New Project로 열 때, venv 폴더가 없다면 가상 환경이 설정되지 않은 것이다. 

```tip
**파이참 가상환경 만들어주기(Windows)**
- File → Settings → Project: jungle > Project Interpreter로 접근
- 톱니바퀴 → Add... 버튼 클릭
- virtual enviorment - New enviorement 선택
- Location을 `현재폴더/venv` 로 설정
```

## 2. 패키지 설치하기

- File → Settings → Project: jungle > Project Interpreter로 접근

- `+` 버튼을 누르기

- 필요한 패키지 검색해서 install package 하기

- pip와 setiptools는 다른 패키지 설치를 돕는 기본 패키지이다.

## 3. requests 패키지 연습해보기

- json 형식으로 읽기

```python
# 라이브러리 설치
import requests

# URL주소 내용 읽어오기
r = requests.get('http://openapi.seoul.go.kr:8088/6d4d776b466c656533356a4b4b5872/json/RealtimeCityAir/1/99')

# json 형식으로 받기
rjson = r.json()

# json 출력하기
print(rjson)

# 중구의 NO2 값 출력하기
print(rjson['RealtimeCityAir']['row'][0]['NO2'])
```

