# Fragment 생명주기

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- 프래그먼트의 생명주기를 간단히 서술하세요

<!--Contents-->

---
## Fragment life-cycle 
![프래그먼트](https://developer.android.com/images/guide/fragments/fragment-view-lifecycle.png)
Fragment의 생명주기는 위의 그림과 같이 Fragment 자체의 생명주기에 따른 상태와 View 생명주기에 따른 상태를 가진다.

### Fragment CREATED
프래그먼트가 CREATED 상태에 도달하면 `FragmentManager`에 추가되었으며 
onAttach() 메서드가 이미 호출된 상태이다. 
이 상태에서 프래그먼트의 view는 아직 생성되지 않았으며 
view와 관련된 모든 상태는 view가 생성된 이후에
복원되어야한다. 
onCreate() 메서드가 호출되면 이전 상태가 저장된 `savedInstanceState` Bundle argument를 
받게되고 프래그먼트가 처음 생성된 것이라면 `savedInstanceState` 는 null일 것이다. 

### Fragment CREATED & View INITIALIZED
프래그먼트의 view 생명주기는 프래그먼트의 유효한 view 인스턴스를 제공할 때만 생성된다.
onCreateView()를 재정의하여 view를 확장하거나 생성하도록 할 수 있지만,
대부분의 경우 적절한 시간에 자동으로 view를 확장해주는 프래그먼트 생성자를 이용할 수 있다.

view가 null 이외의 인스턴스화 된 경우 프래그먼트에 set 되고 getView()를 사용하여 검색할 수 있다.
그런 다음 getViewLifecycleLiveData()가 프래그먼트의 view에 해당하는 새 INITIALIZED lifecycleOwner로 업데이트 된다. 
onViewCreated() 메서드도 동시에 호출된다.

view의 초기 상태를 설정하고, 프래그먼트 뷰를 업데이트하는 `LiveData` 인스턴스 관찰을 시작하고
프래그먼트 뷰의 임의 RecyclerView 또는 ViewPager2 인스턴스에 어탭터를 설정하기에 적합한 위치이다.

### Fragment CREATED & View CREATED
프래그먼트의 view가 생성된 후에는 이전 상태가 복원되고나서 view의 생명주기로 이동한다.
여기서 fragment의 view와 관련된 추가 상태를 복원해야한다.
그리고 onViewStateRestored() 메서드를 호출하게된다.

### Fragment STARTED & View STARTED 
Lifecycle-aware 구성요소를 프래그먼트 STARTED 상태에 연결하는 것을 추천한다.
이 상태는 프래그먼트의 view가 생성된 경우 사용할 수 있고, 
자식 프래그먼트의 FragmentTransaction를 안전하게 수행하는 것을 보장한다.
프래그먼트의 view가 null이 아니라면, 프래그먼트 뷰의 생명주기는 프래그먼트의 생명주기가 STARTED가 되는 즉시
STARTED가 된다.

### Fragment RESUMED & View RESUMED
프래그먼트가 보이게 되면 모든 Animator와 Transition은 완료되고 프래그먼트는 사용자와의 상호작용을 준비한다.
프래그먼트의 생명주기가 RESUME 상태가 되며 onResume() 메서드를 호출한다, 

RESUMED 상태로 전환하는 것은 사용자에게 이제 프래그먼트와 상호작용 할 수 있음을 알리는 적절한 신호와 같다.
RESUMED 되지 않은 프래그먼트들은 수동으로 view에 초점을 맞추거나 입력방법 가시성을 처리하려 하면 안된다.

### Downward state transitions
프래그먼트가 낮은 수명주기 상태로 하향 이동할 때, 관련된 생명주기 event는 view의 생명주기에 의해 관찰자들에게 전송된다.
만약, 인스턴스화 된 경우에는 프래그먼트의 생명주기에 의해 전달된다.
프래그먼트의 생명주기 이벤트가 전달된 후에 프래그먼트는 관련 생명주기 콜백을 호출한다.

### Fragment STARTED & View STARTED
사용자가 프래그먼트를 떠나기 시작하고, 여전히 프래그먼트가 보이면, 프래그먼트와 view의 생명주기는 다시 STARTED 상태가 되고
관찰자들에게 ON_PAUSE 이벤트를 전달한다. 그리고 프래그먼트는 onPause() 콜백을 호출한다.

### Fragment CREATED & View CREATED 
프래그먼트가 더이상 보이지 않게되면, 생명주기는 CREATED 상태가 되고 ON_STOP 이벤트를 관찰자들에게 전송한다.
부모 액티비티나 프래그먼트가 멈추거나 상태를 저장하면서 이 상태로 전환되도록 한다.  

### Fragment CREATED & View DESTROYED
모든 애니메이션과 전환이 완료된 후에 프래그먼트의 view가 화면에서 분리되면 프래그먼트 view의 생명주기는
DESTROYED 상태가 되고 onDestroyView()를 호출한다. 
이 시점에서 프래그먼트 view에 대한 모든 참조를 제거해야 garbage가 수집될 수 있다.

### Fragment DESTROYED
프래그먼트가 제거되거나 fragmentManager가 destroyed 되면, 프래그먼트 생명주기는 DESTROYED 상태가 되며,  
onDestroy() 를 호출하고 프래그먼트의 생명주기가 끝난다.

---
## Reference
- [Developer - fragment lifecycle](https://developer.android.com/guide/fragments/lifecycle)


