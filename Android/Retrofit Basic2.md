# Retrofit2
<!--Table of Contents-->
- 요청메소드
  - GET
  - POST
  - PUT
  - PATCH
  - DELETE

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- Jetpack Compose 란 무엇인가요 ?

<!--Contents-->

---
## 요청 메소드
- Http 통신에 사용되는 요청메소드에는 CRUD 방식의 메소드 4개(+1)와 그 외에 3개의 메소드로 구성되어 있다.
- CRUD 방식에는 GET,POST,PUT,PATHCH,DELETE 가 있고, 그 외에 HEAD, TRACE, OPTIONS가 있다.
### Get
- 서버에 데이터 검색을 요청할 때 사용되는 메소드이다.
- 동일한 요청에는 동일한 데이터를 응답한다.(변경하지않고, 단순 조회만 가능)
- 파라메터로 사용가능한 어노테이션 : @Path, @Query, @QueryMap

 [@Path]
 ```Kotlin
 @GET("pages/{page_id}")
 fun getPage(
     @Path("page_id") pageId: Int,
 ): Call<PageDao>
 ```
 - @Path 는 동적인 URI를 가능하게 해주는 어노테이션이다.
 - @GET(EndPoint)에서 EndPoint 안에 중괄호로 감싸진 변수에 매핑되도록 알려주는 역할을 합니다.

 [@Query]
 ```Kotlin
 @GET("issues")
    fun listIssue(
        @Query("search") search: String,
        @Query("note_id") noteId: String,
        @Query("topic_id") topicId: String,
        @Query("limit") limit: Int
    ): Call<List<IssueDao>>
 ```
 - @Query 는 URI에 쿼리스트링을 추가해서 보낼 수 있도록 해주는 어노테이션이다.
 - URI에 쿼리스트링을 추가해서 원하는 데이터를 조회할 수 있다.
 - ex)"http://kongjingoo.pythonanywhere.com/api/issues?search=algorithm&note_id=1&topic_id=1&limit=1

 [@QueryMap]
 ```Kotlin
 @GET("issues")
    fun listIssue(
        @QueryMap parm: Map<String,String>,
        @Query("limit") limit: Int
    ): Call<List<IssueDao>>
 ```
 - @QueryMap 은 다중 쿼리가 있을 때 사용된다.

### POST
- 서버에 데이터를 생성할 때 사용되는 메소드이다.
- 파라메터로 사용가능한 어노테이션 : @Path, @Body, @Field, @FieldMap

[@Body]
```Kotlin
data class PostIssue(
  val title : String,
  val note_id : Int,
  val topic_id : Int,
  val content : String
  )
////////////////////////
@POST("issues")
fun addIssue(
    @Body body : PostIssue,
)
```
- @Body 는 데이터를 Object 통째로 전송이 가능하다.
- Request로 넘겨주는 값이 Json 형식일 경우 사용한다.

[@Field]
```Kotlin
@FormUrlEncoded
    @POST("issues")
    fun addIssue(
        @Field("title") title: String,
        @Field("note_id") noteId: Int,
        @Field("topic_id") topicId: Int,
        @Field("content") content: String
    )
```
- @Field 는 FormUrlEncoded 방식으로 전송한다.
- @FormUrlEncoded 어노테이션을 반드시 추가해줘야한다.
- form-urlencoded는 key=value&key=value와 같은 형태로 데이터를 전달하는 것을 말한다.

[@FieldMap]
```Kotlin
@FormUrlEncoded
    @POST("issues")
    fun addIssue(
        @FieldMap parm : Map<String,String>,
        @Field("note_id") noteId: Int,
        @Field("topic_id") topicId: Int,
    )
```
- @FieldMap 은 다중 필드가 있을 때 사용된다. QueryMap과 사용방법이 비슷하다.


### PUT & PATCH
- Udpate, 서버 내 데이터를 수정하는 역할로 새로 생성하는 개념이 아닌 변경/수정의 의미이다.
- 파라메터로 사용가능한 어노테이션은 POST와 동일하다.
- Post와 차이점은 변경할 대상을 정해줘야한다.(Path)

```Kotlin
@FormUrlEncoded
    @PATCH("issues/{issue_id}")
    fun updateIssue(
        @Path("issue_id") issueId: Int,
        @Field("title") title: String,
        @Field("note_id") noteId: Int,
        @Field("topic_id") topicId: Int,
        @Field("content") content: String
    )
```
#### PUT, PATCH 차이점
- PUT : 리소스의 모든것을 업데이트한다.
- PATHCH : 리소스의 일부를 업데이트한다.

아래와 같은 리소스가 있을 때,
|member|1|
|---|---|
|name|Lee|
|age|26|
|gender|M|

아래와 같은 요청을 PUT으로 보내게 되면
```Json
PUT/members/1
{
  name: "Sin",
  age: 25,
  gender: "M"
}
```

보낸 모든 값들이 변하게된다.
|member|1|
|---|---|
|name|Sin|
|age|25|
|gender|M|

위의 리소스에 아래와 같은 요청을 PUT으로 보내게 되면
```Json
PUT/members/1
{
  name: "Hong",
}
```
아래 리소스와 같이 보내지 않은 요청은 null값이 된다.
|member|1|
|---|---|
|name|Hong|
|age||
|gender||


아래와 같은 리소스가 있을 때,
|member|1|
|---|---|
|name|Lee|
|age|26|
|gender|M|

아래와 같은 요청을 PATCH로 보내면
```Json
PATCH/members/1
{
  name: "Sin",
}
```
요청에 포함되어 있는 부분만 변경된다.
|member|1|
|---|---|
|name|Sin|
|age|26|
|gender|M|

### DELETE
- 서버에 데이터 삭제를 요청하는 메소드이다.
- 반환되는 DTO 데이터는 없으며, 삭제에 성공하게 되면 응답코드로 200이 오게 된다.
- 삭제를 위해서 사용되기 때문에 삭제 대상을 지정하기 위한 @Path 어노테이션만 사용된다.

```Kotlin
    @DELETE("issues/{issue_id}")
    fun deleteIssue(
        @Path("issue_id") issueId: Int
    )
```
---
## Reference
- [[안드로이드] Retrofit2 기본 사용법2 -'GET/POST/PUT/DELETE'](https://jaejong.tistory.com/38?category=873924)
- [HTTP 메소드 PUT , PATCH 차이](https://programmer93.tistory.com/39)
