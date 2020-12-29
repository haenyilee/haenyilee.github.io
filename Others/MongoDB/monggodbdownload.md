# monggodb 다운로드

## nodejs
1) 다운로드 : 15.2.0 
2) cmd : 
cd c\:Program Files
cd nodejs
npm -g install create-react-app

## monggodb- try free- community server
1) complete 다운로드

## nodejs
1) 다운로드 : 15.2.0 
2) cmd : 
cd c\:Program Files
cd nodejs
npm -g install create-react-app

## monggodb- try free- community server
1) complete 다운로드


## webstorm
- 경로에서 programfiles 지우기
- npm start
- 서버 구동 : MongoDB-server-4.4-bin-mongod --dbpath c:\data



## 몽고디비 편집

- RUN SQL COMMAND LINE 비슷한 것 키는 법 : c:\Program Files\MongoDB\Server\4.4\bin>mongo
- 서버 구동 : MongoDB-server-4.4-bin-mongod --dbpath c:\data
  - MongoDB 원격으로 접속하기  : mongod --bing_ip 211.238.142.181 -dbpath c:\data
  - MongoDB localhost로 접속하기  : mongod --dbpath c:\data


- 데이터 추가 : 
  - db.member.insert({no:1,name:'hong',sex:'man'})
  - db.member.insert({no:2,name:'shim',sex:'woman'})

- 데이터 불러오기, 데이터 찾기 : 
  - db.member.find() , 
  - db.member.find({no:1})

