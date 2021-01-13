# minimal-mistakes테마로 gitblog만들기

## thema fork하기

- 원하는 테마의 github repository로 접속한다.
![image](https://user-images.githubusercontent.com/77392444/104453556-f455c480-55e7-11eb-9c9f-49b5057533f1.png)

- repository명칭을 (본인의 git계정 이름).github.io로 변경해준다.
	- settings 클릭
![image](https://user-images.githubusercontent.com/77392444/104453821-59a9b580-55e8-11eb-9042-05e333344ac7.png)
	- repository 명 변경
![image](https://user-images.githubusercontent.com/77392444/104453957-8b228100-55e8-11eb-995d-8447f46eab21.png)

- setting로 다시 돌아가서 github page가 만들어졌는지 확인한다.
![image](https://user-images.githubusercontent.com/77392444/104454263-f3716280-55e8-11eb-852e-bac477fb4700.png)
	- 만일, 빨간색 글씨 부분이 나오지 않는다면 branch가 master로 되어있지 않은지 확인한다.
	- 해당 문구가 나오면 `https://(본인계정명).github.io` 주소를 클릭한다.

- 아래와 같이 해당 주소로 접근했을 때, 아래 페이지가 나오면 github blog가 만들어진 것이다. 
![image](https://user-images.githubusercontent.com/77392444/104454565-65e24280-55e9-11eb-8aec-d37889cadc3e.png)

- 이제 깃허브 블로그를 본인 취향에 맞게 꾸미고, 카테고리를 나누는 작업만 남았다. 


## 메인페이지 설정 변경하기
- _config.yml은 블로그의 url, 테마 등 메인페이지에 나오는 내역을 설정하는 파일이다. 

- repository의 [code] 탭으로 이동해서 _config.yml 파일을 클릭한다. 

- 현재 페이지와 코드 부분을 비교하면서 수정하고 싶은 부분을 수정한다
	- 앞에 # 이 붙으면 주석처리 되기 때문에, 설정하고 싶은 내용은 주석처리를 해제해 준다(단축키 : Ctrl + /)

![image](https://user-images.githubusercontent.com/77392444/104456530-279a5280-55ec-11eb-8caa-06c3125802f0.png)

- 블로그에 접속해보면 `_config.yml`에 설정한대로 변경되어 있는 것을 확인할 수 있다. 
	- Github Pages는 build되기까지 시간이 소요되기 때문에 5분 정도 기다려주면 변경되어 있다. 
	- 만일 오류가 발생한다면 본인의 github 계정 메일로 build가 실패했다는 내역의 메일이 와 있다. 
	- 오류가 났다면 혹시 _config.yml에서 `"`를 빠뜨리지 않았는지 다시 점검해보길 바란다. 
![image](https://user-images.githubusercontent.com/77392444/104456780-83fd7200-55ec-11eb-9191-6698065e5206.png)

## Navigation  변경하기

- 이제 블로그 우측 상단의 nav바 내용을 변경해보자
![image](https://user-images.githubusercontent.com/77392444/104457285-2fa6c200-55ed-11eb-9794-b006a4a9f4e8.png)


- repository의 `_data` 폴더에 접근한다. 
![image](https://user-images.githubusercontent.com/77392444/104457384-582ebc00-55ed-11eb-8c06-4ae8aa66cfd9.png)

- navigation.yml 파일에 접근한다. 
![image](https://user-images.githubusercontent.com/77392444/104457517-83b1a680-55ed-11eb-81e9-a91fb4fec81f.png)

- 상단 버튼을 클릭해 내용을 수정한다.
![image](https://user-images.githubusercontent.com/77392444/104457632-a5129280-55ed-11eb-95de-7660503dbb23.png)

- 원하는 내역으로 수정한다.

![image](https://user-images.githubusercontent.com/77392444/104457877-f28eff80-55ed-11eb-8aba-bae9e80b39ed.png)

- 블로그로 접속해보면 navigation 부분이 수정된 것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/77392444/104457996-16524580-55ee-11eb-93bf-202c01e4fd8c.png)

- 아직 페이지를 연결해주지 않았기 때문에, 404오류가 뜬다.

- 페이지와 연결시켜주기 위해서 _pages 폴더의 about.md, category-archive.md 파일을 수정해야 한다.
	- 근데 _pages 폴더가 보이지 않는다.
	- _pages 폴더를 만들어주기 위해 로컬 레파지토리에서 폴더를 생성해 주기로 한다. 

	```note
	- 나는 [C:\Git]디렉토리에 로컬 레파지토리를 생성해주기로 했다.
	- git bash 명령어
		- git init
		- git clone https://github.com/hennylee/hennylee.github.io.git
	- 만약 해당 방법을 모른다면, github repository clone하는 방법을 구글링해보자
	```

	- _pages폴더와 _post폴더를 생성해준뒤 원격 레파지토리에 반영한다. 

	```note
	- github 내의 원격  repository로 변경내역 반영하는 명령어
		- git add . 
		- git commit -m "_pages, _post Directory Add" 
		- git push origin master 
	```
	
	- github에 접속해보면 해당 디렉토리가 생겨난 것을 확인할 수 있다. 
	



