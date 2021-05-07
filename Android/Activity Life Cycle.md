# 액티비티 생명주기 (life-cycle)
<!--Table of Contents-->
- Activity life-cycle이란?
    - 생명 주기 콜백 메서드

## You can answer
- 액티비티의 생명주기를 간략히 설명하세요

<!--Contents-->

---
## Activity life-cycle 란 ? 
사용자가 앱을 탐색하고, 앱을 나가고 , 앱에 다시 돌아가는 등의 활동을 통한 앱의 Acticity 인스턴스는 생명 주기 안에서 서로 다른 상태를 통해 전환된다.  
Activity 클래스는 여러 콜백을 제공하는데 
사용자의 활동이 작동하는 방식을 이 생명 주기 콜백 메서드에서 선언할 수 있다.  
즉, 각 콜백은 앱이 더욱 안정적으로 기능할 수 있도록 하기위해 상태 변화에 적합한 특정 작업을 실행할 수 있도록 한다.

### 생명 주기 콜백 메서드
* Activity 클래스는 onCreate(), onStart(), onResume(), onPause(), onStop(), onDestroy() 와 같이 6가지 콜백 메서드를 제공한다.
![lifecylce](https://developer.android.com/guide/components/images/activity_lifecycle.png?hl=ko)  
  [Activity의 생명주기]
  
![state](https://camo.githubusercontent.com/ecceb23622438ed1883ff03de5c63cea022d1c7f274b329bd1b777ea5ebf653f/68747470733a2f2f646576656c6f7065722e616e64726f69642e636f6d2f696d616765732f746f7069632f6c69627261726965732f6172636869746563747572652f6c6966656379636c652d7374617465732e737667)  
  [Activity 생명주기에 따른 상태]   
<br>
#### onCreate() 
- 이 콜백은 시스템이 액티비티를 생성할 때 실행되는 것으로 필수적으로 구현해야한다.  
- 액티비티가 생성되면 Created 상태가 되고 startup 로직을 실행한다.  
  예를 들어, 데이터를 리스트에 바인딩, Activity와 ViewModel 연결, 일부 클래스 변수를 인스턴스화 한다.  
- saveInstanceState 매개변수로 이전 활동 상태를 저장한 Bundle 객체를 수신한다.  
- 이 Activity의 생명주기와 연결된 구성요소가 있다면 그 구성요소는 ON_CREATE 이벤트를 수신한다.
- onCreate() 가 실행을 완료하면 Started 상태가 되고 onStart() 와 onResume() 메서드를 호출한다.

#### onStart()
- Activity가 Started 상태가 되면 시스템은 onStart()를 호출한다.
- 앱은 Activity를 foreground에 보내 상호작용할 수 있도록 준비한다.  
  예를 들어, 앱이 UI를 관리하는 코드를 초기화 한다.  
- 이 Activity의 생명주기와 연결된 구성요소가 있다면 그 구성요소는 ON_START 이벤트를 수신한다.
- onStart() 메서드는 빠르게 완료되고 이 콜백이 완료되면 Resumed 상태가 되며 onResume() 메서드를 호출한다.

#### onResume() 
- Activity가 Resumed 상태가 되면 foreground에 표시되고 시스템이 onResume()을 호출한다. 
- 이 상태가 되면 앱이 사용자와 상호작용한다. 어떤 이벤트가 발생하여 앱에서 포커스가 떠날 때까지 
  앱이 이 상태에 머무른다.  
  예를 들어 전화가 오거나, 사용자가 다른 Activity로 이동하거나 화면이 꺼지는 이벤트 등이 해당된다.  
- 이 Activity의 생명주기와 연결된 구성요소가 있다면 그 구성요소는 ON_RESUME 이벤트를 수신한다.
- 방해되는 이벤트가 발생하면 Activity는 Paused 상태가 되고 시스템은 onPause() 콜백을 호출한다.
- Paused 상태에서 Resumed 상태로 돌아오면 시스템이 onResume() 메서드를 다시 호출한다.

#### onPause()
- 시스템은 사용자가 Activity를 떠나는 것을 나타내는 첫 신호로 이 메서드를 호출한다.   
  이는 Activity가 더 이상 foreground에 있지 않다는 것을 나타낸다. ( 사용자가 멀티 윈도우 모드에 있을 때는 표시될 수도 있다. )  
- 이 Activity의 생명주기와 연결된 구성요소는 ON_PAUSE 이벤트를 수신한다.  
  이 때 구성요소가 foreground에 있지 않을 때 실행 할 필요가 없는 기능을 모두 정지할 수 있다.  
  또, onPause() 메서드로 모든 리소스를 해제할 수도 있다. 
- onPause()는 잠깐 실행되므로 저장 작업은 지양해야한다. 부하가 큰 종료 작업은 onStop()에서 실행해야한다.
- onPause() 메서드의 실행이 완료되더라도 Activity가 Paused 상태로 남아 있다면 사용자에게 완전히 보이지 않게 될 때까지 현 상태를 유지한다.  
  Activity가 다시 시작되면 시스템은 다시 onResume() 콜백을 호출하고 Paused 상태에서 Resumed 상태로 
  돌아오면 시스템은 Activity 인스턴스를 메모리에 남겨두고, 시스템이 onResume() 을 호출할 때 
  인스턴스를 다시 호출한다.  
- Activity가 완전히 보이지 않게 되면 시스템은 onStop()을 호출한다.

#### onStop() 
- Activity가 사용자에게 더 이상 표시되지 않으면 Stopped 상태에 들어가게 되고 시스템이 onStop() 콜백을 호출한다.
- 이 Activity의 생명주기와 연결된 구성요소는 ON_STOP 이벤트를 수신한다.
- onStop() 메서드로 앱은 필요하지 않은 리소스를 해제하거나 조정해야하며, CPU를 많이 소모하는 종료 작업을 실행해야한다.
- Activity가 Stopped 상태에 들어가면 Activitiy 인스턴스는 메모리 안에 머무르게 된다.  
Activity가 다시 시작되면 시스템은 onRestart()를 호출하고 , Activity가 실행을 종료하면 시스템은 onDestroy()를 호출한다.
  
#### onDestroy() 
- onDestroy() 는 두 가지 경우 중 하나에 해당될 때 호출된다 .
    1. 사용자가 액티비티를 완전히 닫거나 액티비티에서 finish()가 호출되어 종료되는 경우
    2. 구성변경(기기 회전 또는 멀티윈도우)로 인해 시스템이 일시적으로 액티비티를 소멸시키는 경우
- 이 Activity의 생명주기와 연결된 구성요소는 ON_DESTROY 이벤트를 수신한다.
- Activity가 구성변경으로 인해 다시 생성될 경우 ViewModel이 그대로 보존되어 다음 액티비티 인스턴스에 전달되므로 
추가작업 필요하지는 않다.  
- Activity가 다시 생성되지 않을 경우에는 ViewModel은 onCleared() 메서드로 액티비티가 소멸되기 전에 모든 데이터를 정리해야한다.   

---
## Reference
- [Activity Life Cycle](https://developer.android.com/guide/components/activities/activity-lifecycle)


