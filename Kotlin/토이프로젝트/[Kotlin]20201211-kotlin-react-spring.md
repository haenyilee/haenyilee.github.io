## 코틀린
- `data class MovieVO(var no:Int,..)`


## spring

### db구축
#### interface MovieMapper
- 쿼리문장 작성

#### MovieDAO
- 메모리할당
- mapper 구현 요청

### json으로 넘겨주기

#### WebAppController
- `@RestController`
- DAO객체 얻어오기
- `@RequestMapping`
`
- 1. 리액트 => axios("url주소") => json 파싱되어서 들어옴
- 2. 코틀린 => HttpUrlConnection("url주소") => gson라이브러리로 파싱해서 들어가야함

- 한개에 대한 정보(row)가 12개 필요하다면 배열을 활용해 [{}이 12개]를 묶어서 주고받아야함

## react

#### package

```
"axios": "^0.21.0",
"react-router": "^5.2.0",
"react-router-dom": "^5.2.0"
```


## kotlin

#### build.gradle(module)


#### build.gradle(app)

'org.jsoup:jsoup:1.11.2'

'com.google.code.gson:gson:2.8.5'

'com.android.support:recyclerview-v7:28.0.0'

'com.github.bumptech.glide:glide:4.9.0'
