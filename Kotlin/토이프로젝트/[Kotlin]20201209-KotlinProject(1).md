# 20201209-KotlinProject(1)

## 1. MainActivity.kt

- `AppCompatActivity`
  - 자바에서는 JFrame을 상속받아 사용했던 것과 같다.
- `setContentView()`
  - 어떤 화면을 보여줄지 설정함
  - activity_main 위치 : res > activity_main.xml
  
  
  
## 2. 한글 셋팅

- [Editor - File Encodings] : utf-8로 변경
- [Editor - General - Auto Import] : Ask로 변경

## 3. Gradle
- pom.xml과 같은 기능임
- build.gradle(:app)
  - `implementation "org.jetbrains.anko:anko:$anko_version"`
  - `id 'kotlin-android-extensions'`
- build.gradle(프로젝트명) : `ext.kotlin_version = "1.3.72"` - synk now 클릭
- MainActivity에서 `import kotlinx.android.synthetic.main.activity_main.*`

## 4. res > activity_main.xml
- 화면 출력

