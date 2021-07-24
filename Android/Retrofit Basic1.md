# Retrofit2
<!--Table of Contents-->
- Retrofit2 란?
- Retrofit2 장점
- Retrofit2 사용방법

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- Retrofit2 란 무엇인가요 ?

<!--Contents-->

---
## Retrofit2 란?
Retrofit2는 Square사에서 만든 REST API 통신에 특화된 라이브러리로, Retrofit이라는 라이브러리를 출시한 뒤 기존 라이브러리의 문제점을 개선해 현재의 Retrofit2가 만들어졌다. Retrofit2는 Retrofit의 업그레이드 된 버전이기 때문에 일반적으로 1과 2를 구분짓지 않고 Retrofit이라고 한다.
Retrofit2는 네트워크 요청을 처리하기 위해서 OkHttp를 사용하고 enqueue와 excute를 사용하여 동기, 비동기 처리를 지원한다. 또한 JSON에서 Java 객체로 변환 해 줄 JSON 변환기가 내장되어있지 않기 때문에 JSON 변환기 라이브러리에 대한 지원을 제공하고 있다.

## Retrofit 장점
Retrofit의 장점에는 속도, 편의성, 가독성이 있다.
1. 속도
   Acynctask를 통해 비동기 실행하는 OKHttp에 비해 Retrofit에서는 Acynctask를 사용하지 않고, 자체적인 비동기 실행과 스레드 관리를 통해 속도가 빠르다는 장점이 있다.
2. 편의성
   OkHttp에서도 쿼리스트링, request, response 설정 등 반복적인 작업이 필요한데, Retrofit에서는 이런 과정을 모두 라이브러리에 넘겨서 처리하도록 하였다. 따라서 사용자는 함수 호출시에 파라미터만 넘기면 되기 때문에 작업량이 훨씬 줄어들고 사용하기 편리하다.
3. 가독성
  인터페이스 내에 annotation을 사용하여 호출할 함수를 파라미터와 함께 정의해놓고, 네트워크 통신이 필요한 순간에 구현없이 해당 함수를 호출하기만 하면 통신이 이루어지기에 코드를 읽기가 매우 편하다. Asynctask를 쓰지 않기에 불필요하게 코드가 길어질 필요도 없으며, 콜백 함수를 통해 결과가 넘어오도록 되어있어 매우 직관적인 설계가 가능하다.

## Retrofit 사용방법

### 1. Gradle 의존성 추가
```Gradle
// Retrofit 라이브러리
implementation 'com.squareup.retrofit2:retrofit:$retrofit_version'

// Gson 변환기 라이브러리
implementation 'com.squareup.retrofit2:converter-gson:$retrofit_version'    

// Scalars 변환기 라이브러리
implementation 'com.squareup.retrofit2:converter-scalars:$retrofit_version'
```
- Gson Converter : JSON 타입의 응답결과를 객체로 변환해주는 Converter
- Scalars Converter : 응답결과를 String으로 받아서 보여주는 Converter(사용자가 직접 변환시 사용)
- 이 외에 Moshi, Jackson 등 다양한 Converter가 있다.

### 2. Manifests 설정
```XML
<uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.CtrlF"
        android:usesCleartextTraffic="true">
```
- Manifests 파일에 인터넷 권한을 추가한다.
- 안드로이드에서는 http 주소를 허용하지 않기 때문에 android:usesCleartextTraffic="true"로 설정하여 http URL에 대해서 접근을 허용해줘야한다.

### 3. DTO(POJO) - 모델 클래스 생성
[REST API 응답 JSON 데이터 구조]
```JSON
{
	"page_id": 1,
	"title": "page title",
	"content": "page content"
}
```
[DTO 모델 - Data Class로 선언]
```Kotlin
data class PageDao(
    @SerializedName("page_id")
    val id: Int,
    val title: String,
    val content: String
)
```
- REST API의 응답 데이터 구조에 맞게 DTO 클래스를 구현한다.
- 응답 데이터의 변수명과 자료형은 반드시 일치해야 한다.
- SerializedName 어노테이션을 사용하면 응답 데이터의 변수명과 다르게 지정할 수 있다.

### 4. Interface 정의
```Kotlin
interface PageApi {
    @GET("pages/{page_id}")
    fun getPage(
        @Path("page_id") pageId: Int,
    ): Call<PageDao>
}
```
- API 통신을 위한 인터페이스를 정의한다.
- @GET("pages/{page_id}")의 요청메소드는 GET이고, baseUrl에 연결된 EndPoint는 pages/{pageid}가 된다.
- 메소드명은 자유롭게 설정이 가능하며, API 통신에 영향을 미치지 않는다.
- 매개변수   @Path("page_id") pageId: Int @Get 내부 {page_id}에 대입되는 매개변수이며, Int 형이다.
- 반환형 PageDao는 API 통신 응답이 오게되면 Callback으로 불려지는 반환형(위에서 만든 Data Class)이다.

### 5. Retrofit 클라이언트 생성
```Kotlin

private val retrofit = Retrofit.Builder()
    .addConverterFactory(GsonConverterFactory.create())
    .baseUrl(BASE_URL)
    .build()

object PageService {
    val retrofitService : PageApi by lazy {
        retrofit.create(PageApi::class.java)
    }
}
```
- BASE_URL 에는 절대 변하지 않을 api 주소를 넣는다.
- Retrofit.Builder를 생성하면서 addConverterFactory에 GsonConverter를 추가한다.(Gson은 JSON 구조의 직렬화된 데이터를 JAVA Object로 역직렬화, 직렬화 해주는 자바 라이브러리이다.)

### 6. Retrofit 통신
```Kotlin
private fun loadPage(pageId: Int){
        PageService.retrofitService.getPage(pageId).enqueue(object : Callback<PageDao>{
            override fun onResponse(call: Call<PageDao>, response: Response<PageDao>){
                if(response.isSuccessful)
                    _pageInfo.value = response.body()
                else
                    Log.e(Tag,"request failure: ${response.message()}")
            }

            override fun onFailure(call: Call<PageDao>, t: Throwable) {
                Log.e(Tag, "throwable: ${t.message}")
            }
        })
    }
```
- 위 함수는 pageId를 PageService의 메서드인 getPage()를 호출해 Retrofit 통신을 하는 함수이다.
- Retrofit은 enqueue()를 이용해 비동기 통신이 가능하고, execute()로 동기적 통신도 가능하다.
- enqueue() 인자에는 응답이 왔을 때 동작하는 콜백 함수를 등록한다.
- onResponse는 통신 성공시 동작하는 Callback 함수이며 메인 스레드에서 작업하는 부분이기 때문에 UI작업이 가능하다.
- onResponse가 항상 응답에 성공하는것이 아니기 때문에 조건문을 통해 확인해줘야 한다.
-onFailure은 통신 실패(네트워크 문제와 같이 시스템적인 이유로 실패)시 동작하는 Callback함수이다.



---
## Reference
- [[안드로이드] Retrofit2 '레트로핏' - 기본 사용법](https://jaejong.tistory.com/33?category=873924)
