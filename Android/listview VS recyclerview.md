# ListView vs RecyclerView
<!--Table of Contents-->
- 차이점
    - ViewHolder Pattern
    - LayoutManager
    - Item Decoration & Item Animator
    - Notifying Adapter
- 정리

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- ListView와 RecyclerView의 차이 ?

<!--Contents-->

---
## 차이점
가장 보편적으로 생각할 수 있는 view 객체의 재사용
![images](https://miro.medium.com/max/1400/1*yVuye_4Sx2Y24pVOp0By5A.png)
그러나 listView.md에서 언급했듯이 listView 역시 재사용 가능케 만들 수 있음
### ViewHolder Pattern
    recyclerView에서 강제적인 ViewHolder의 사용 
    데이터를 bind 하기 위한 findViewById의 호출을 한번만 함으로써 
    오버헤드를 줄일 수 있음
### LayoutManager
    recyclerView의 3가지 LayoutManager 
    Linear(가로세로) , Grid(한줄에 1개 이상의 이미지 표시),staggeredGrid(이미지 크기를 다양하게 적용)와 같이 
    다양한 타입의 리스트를 제공
    Custom 도 가능함
![](https://themarshmallowz.files.wordpress.com/2017/06/imagess-e1497550061396.png?w=615)
### Item Decoration & Item Animator
    listView에서는 XML로 divide하지만 recyclerView에서는 ItemDecoration 클래스로
    원하는 아이템에 divider를 추가할 수 있음
    또, listView에는 없었던 애니메이션 기능을 ItemAnimator 클래스로 추가함.
    item을 삽입하거나, 삭제, 이동하는 등의 animation을 추가할 수 있음
```kotlin
val decoration = DividerItemDecoration(
  mbinding.PBrcv.context,
  LinearLayoutManager(this.context).orientation
)
mbinding.PBrcv.addItemDecoration(decoration)
```

### ViewType
    RecyclerView는 각 칸마다 ViewType을 기반으로 layout을 핸들링 할 수 있음
    어떤 layout을 형성 할 지 onCreateViewHolder에서 판단한 뒤,
    ViewHolder를 return 하게 됨. 
    ViewType의 value는 개발자가 직접 지정할 수 있으며 
    지정하지 않았을 때는 Default Value로 처리. 

ex) view 별로 색깔을 다르게 하는 recycler view 생성을 위한 Adapter
```kotlin
class ItemsAdapter(private var items: List<Model.Item>) : RecyclerView.Adapter<RecyclerView.ViewHolder>() {

  // 뷰타입 별 분기 
  override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerView.ViewHolder {
    when (viewType) {
      TYPE_RE -> {
        var binding = ItemType01Binding.inflate(LayoutInflater.from(parent.context), parent, false)
        return ItemRedHolder(binding)
      }
      TYPE_OR -> {
          var binding = ItemType02Binding.inflate(LayourInflater.from(parent.context), parent, false)
        return ItemOrangeHolder(binding)
      }
      else -> {
          var binding = ItemType07Binding.inflate(LayoutInflater.from(parent.context), parent, false)
        return ItemPurpleHodler(binding)
      }
    }
  }
  
  // getItemCount() 
  override fun getItemCount(): Int {
      return items.size 
  }
  
  // holder에 뷰 바인딩
  override fun  onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
      if(holder is ItemRedHolder) {
          holder.bind(items.get(position))
      } else if (holder is ItemOrangeHolder) {
          holder.bind(items.get(position))
      } else  if (holder is ItemPurpleHolder) {
          holder.bind(item.get(position))
      }
  }
  
  // 각 아이템의 viewType을 반환함 
  override fun getItemViewType(position: Int): Int {
      item.let{
          return it[position].viewType
      } 
  }

  companion object {
    const val TYPE_RE = 1;
    const val TYPE_OR = 2;
    const val TYPE_PU = 3;

  }
}
```
onCreateViewHolder / getItemCount / onBindViewHolder 외 getItemViewType을 추가로 재정의 해야함.

### Notifying Adapter
    데이터의 변경이 일어났을 때, recyclerView가 layout을 새로 그리게 되는데
    이때 notifyDataSetChanged() 를 호출하게 되면 레이아웃이 전체가 새로 그려지게 됨
    변경된 데이터만 새로 그리고 싶을 때 ListAdapter를 사용하면 리스트를 비교해서 업데이트하는것이
    좀 더 편리해짐. 
    ListAdapter를 사용하기 위해 DiffUtil.ItemCallBack 클래스를 구현
    이러한 콜백은 각각의 타입에 맞는 notification (notifyItemInserted(), notifyItemRemoved(), notifyItemChanged()) 와
    같은 메서드들을 사용하지 않아도 간단하게 핸들링 할 수 있음

```kotlin
class SleepNightAdapter(private val clickListener: SleepnightListener) :
  ListAdapter<DataItem, RecyclerVeiw.ViewHolder>(SleepNightDiffCallback())

class SleepNightDiffCallback : DiffUtil.ItemCallback<DataItem>() {
  //check the id is same
  override fun areItemsTheSame(oldItem: DataItem, newItem: DataItem): Boolean {
    return oldItem.id == newItem.id
  }

  // if id is same, check the content is same
  @SuppressLint("DiffUtilEquals")
  override fun areContentsTheSame(oldItem: DataItem, newItem: DataItem): Boolean {
    return oldItem == newItem
  }
}

```
### 정리
| |RecyclerView|ListView|
|------|---|---|
|ViewHolder|ViewHolder 패턴 필수적으로 사용|ViewHolder 사용 가능하나 필수는 아님|
|LayoutManager|Grid, Linear, StaggeredGrid|세로방향만 가능|
|Decoration|RecyclerView.ItemDecoration 객체 사용하여 구분선 |android:divider 속성을 이용하여 구분|
|Item Animation|item 애니메이션 처리 클래스 ||
|Click Detection|View에서 클릭 이벤트를 다루지 않고 itemView에서 이벤트를 통한 처리|리스트의 개별항목에 대한 클릭이벤트에 바인딩 하기위한 AdapterView.onItemClickListener 인터페이스 존재|
|View Type|viewtype을 여러 개 두어 각각의 layout을 핸들링 할 수 있음||

---
## Reference
- [recyclerview click event](https://purple-wood-lights.tistory.com/19)