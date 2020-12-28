




- 서버 가동

```
C:\Users\SIST>cd "c:\Program Files"

c:\Program Files>cd mongodb

c:\Program Files\MongoDB>cd server

c:\Program Files\MongoDB\Server>cd 4.4

c:\Program Files\MongoDB\Server\4.4>cd bin

c:\Program Files\MongoDB\Server\4.4\bin>mongod --dbpath c:\data
```


- 데이터 추가 및 찾기

```
C:\Users\SIST>cd "c:\Program Files"

c:\Program Files>cd mongodb

c:\Program Files\MongoDB>cd server

c:\Program Files\MongoDB\Server>cd 4.4

c:\Program Files\MongoDB\Server\4.4>cd bin

c:\Program Files\MongoDB\Server\4.4\bin>mongo
MongoDB shell version v4.4.1
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("a038256f-3cb7-4477-aefb-eef0f54129c5") }
MongoDB server version: 4.4.1
---
The server generated these startup warnings when booting:
        2020-11-18T16:13:19.105+09:00: ***** SERVER RESTARTED *****
        2020-11-18T16:13:28.960+09:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
---
---
        Enable MongoDB's free cloud-based monitoring service, which will then receive and display
        metrics about your deployment (disk utilization, CPU, operation statistics, etc).

        The monitoring data will be available on a MongoDB website with a unique URL accessible to you
        and anyone you share the URL with. MongoDB may use this information to make product
        improvements and to suggest MongoDB products and deployment options to you.

        To enable free monitoring, run the following command: db.enableFreeMonitoring()
        To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---
> use mydb
switched to db mydb
> db.member.insert({no:1,name:'hong'})
WriteResult({ "nInserted" : 1 })
> db.member.find()
{ "_id" : ObjectId("5fb24b40068886f098636dd0"), "no" : 1, "name" : "hong", "sex" : "man" }
{ "_id" : ObjectId("5fb24b7f068886f098636dd1"), "no" : 2, "name" : "shim", "sex" : "woman" }
{ "_id" : ObjectId("5fb363213d9e42d05566a3dc"), "no" : 3, "name" : "park", "sex" : "man" }
{ "_id" : ObjectId("5fbde8fd2193366596efe092"), "no" : 1, "name" : "hong" }
> dn.member.find({no:1})
uncaught exception: ReferenceError: dn is not defined :
@(shell):1:1
```


### BoardVO

### BoardDAO
- LIKE함수가 없기 때문에 데이터 찾기가 까다로운 편이다.
- skip : 페이지 바꿀 때, 페이지 1이면 skip은 0이 되어서 0 이전을 자름
- `DBCursor cursor=dbc.find().skip(skip).limit(rowSize).sort(new BasicDBObject("no",-1));`
  - skip
    - 페이지가 2라면, skip 이 10이니까 앞에 10개를 건너 뛴다.
  - sort

  - BasicDBObject
    - -1 : DESC
    - 1 : ASC
 
 #### 상세보기
 - `BasicDBObject obj = (BasicDBObject) dbc.findOne(where);` : dbc.findOne : 오라클에서는 `SELECT * FROM board WHERE no=1;` 이다.
 - `BasicDBObject up=new BasicDBObject("$set",new BasicDBObject("hit",hit+1));`
