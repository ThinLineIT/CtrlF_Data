# ListView
<!--Table of Contents-->
- ListView
    - getView() 구현
    - convertView 이용
- 정리
    - listView를 사용하는 경우


<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- listView를 사용하는 경우

<!--Contents-->

---
## ListView
### 1. getView() 의 기본적인 구현
```kotlin
override fun getView(position: Int, convertView: View?, parent: ViewGroup?): View {
  val item = mInflater.inflate(R.layout.text_item_view, null)
  item.findViewById<TextView>(R.id.textView).text = "test"
  return item
}
```
listView는 스크롤 할때마다 getView()를 계속해서 호출하여 새로운 view를 리스트의 각 항목에 inflate 하게 된다.  
findViewById도 연속적으로 발생되고 이는 view의 재사용성이 떨어져 비효율적이다.

### 2. ConvertView로 view 재사용
```kotlin
override fun getView(position: Int, convertView: View?, parent: ViewGroup?): View {
  val item = if (convertView == null) {
    mInflater.inflate(R.layout.text_item_view, null).apply {
      tag = ViewHolder(findViewById(R.id.textView))
    }
  } else {
    convertView
  }
  val holder = item.tag as ViewHolder
  holder.textView.text = dataList[position]
  
  return item
}
```
그래서 위와 같이 convertView를 사용하여 ViewHolder 패턴을 적용한다.   
convertView == null 일 때만 view가 생성된다.

### 정리
ListView를 사용하면 좋은 경우 : 
1) 리스트의 항목이 간단한 경우 (ex: 텍스트로만 이루어진 리스트)
2) 리스트의 크기가 작고 고정되어있는 경우 
3) 데이터가 많이 바뀌지 않는 경우  <br><br>
*그러나, listView는 사실상 deprecated 되었음.*
---
## Reference
