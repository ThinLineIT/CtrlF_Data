# RecyclerView
<!--Table of Contents-->
- RecyclerView 구성
- Example

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- RecyclerView를 사용하는 이유?
- RecyclerView 구현

<!--Contents-->

---
## RecyclerView

- 구성

롤리팝(5.0) 버전에서 ListView 의 단점을 보완하여 나옴.  
성능이 향상되었고 더 유연하게 리스트를 구현할 수 있음.  
더 다양한 형태로 커스터마이징 할 수 있도록 제공됨
![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWMBZi%2FbtqDVwsNuXE%2FPARdLOKFOKsuphSPL9CnT1%2Fimg.png)

### Example
* 사진을 불러와 grid 형식으로 이미지를 띄우는 프래그먼트

#### 1) Declare dependencies
* build.gradle 파일에 dependencies 추가
```kotlin
dependencies {
    implementation("androidx.recyclerview:recyclerview:1.2.1")
    // For control over item selection of both touch and mouse driven selection
    implementation("androidx.recyclerview:recyclerview-selection:1.1.0")
}
```
#### 2) item layout
<item_grid.xml>
```kotlin
<androidx.constraintlayout.widget.ConstraintLayout
  android:layout_width = "wrap_content"
  android:layout_height = "wrap_content"
  xmlns:app = "http://schemas.android.com/apk/res-auto"
  android:paddingBottom = "2dp"
  android:paddingEnd = "2dp"
  android:orientation = "vertical" >

  <ImageView
    android:id = "@+id/imageView" 
    android:layout_width = "wrap_content"
    android:layout_height = "200dp"
    android:scaleType = "center"
    app:layout_constraintBottom_toBottomOf = "parent"
    app:layout_constraintEnd_toEndOf = "parent"
    app:layout_constraintStart_toStartOf = "parent"
    app:layout_constraintTop_toTopOf = "parent" />

</androidx.constraintlayout.widget.ConstraintLayout >
```
<fragment_gallery.xml>
```kotlin
<androidx.constraintlayout.widget.ConstraintLayout
  xmlns:app="http://schemas.android.com/apk/res-auto"
  xmlns:tools="http://schemas.android.com/tools"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  tools:context=".gallery.GalleryFragment">

  <androidx.recyclerview.widget.RecyclerView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/Gridrcv"
    app:layoutManager="androidx.recyclerview.widget.GridLayoutManager">

  </androidx.recyclerview.widget.RecyclerView>
</androidx.constraintlayout.widget.ConstraintLayout>

```
#### 3) ViewHolder
* view holder를 사용함으로써 findViewById를 매번 호출하지 않아도 되고  
  view holder가 가진 view의 정보를 보여줌
```kotlin
class ViewHolder(
  val binding: ItemGridBinding
) : RecyclerView.ViewHolder(binding.root) {
  fun bind(image: ImageModel) {
    Glide.with(itemView).load(image.download_url).into(binding.imageView)
      
  }
}
```
#### 4) Adapter
* ViewHolder의 데이터를 RecyclerView에 연결하는 곳
* 데이터를 어떻게 그릴지 결정함
```kotlin
class ImageAdapter() : RecyclerView.Adapter<ImageAdapter.ViewHolder>() {
  private var ImageData = ArrayList<ImageModel>()

  override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
    val view = ItemGridBinding.inflate(
      LayoutInflater.from(parent.context),
      parent,
      false
    )
    return ViewHolder(view)
  }

  override fun onBindViewHolder(holder: ViewHolder, position: Int) {
    holder.bind(ImageData[position])
    holder.binding.executePendingBindings()
  }

  override fun getItemCount(): Int {
    return ImageData.size
  }

  fun submitList(list: List<ImageModel>) {
    ImageData.clear()
    ImageData.addAll(list)
    notifyDataSetChanged()
  }
}
```
#### 5) fragment
* fragment에서 adapter 연결
```kotlin
fun responseSuccess(response: ImageList?) {
    val images: ArrayList<ImageModel> = arrayListOf()
    if (response != null) {
        try {
            for (image in response) {
                images.add(image)
            }
        } catch (e: JSONException) {
            e.printStackTrace()
        }
    }
    val imageAdapter = ImageAdapter()
    imageAdapter.submitList(images)
    mbinding.Gridrcv.adapter = imageAdapter
    mbinding.Gridrcv.layoutManager = GridLayoutManager(this.context, 4)
}
```

### 정리
ViewHolder 패턴의 사용을 강제하고 Adapter 클래스를 직접 구현하기 때문에   
View custom 작업에 대한 유연성이 listView 보다 더 편리하게 사용가능함.