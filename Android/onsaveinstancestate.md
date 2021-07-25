# onSaveInstanceState
<!--Table of Contents-->
- Activity의 종료

- onSaveInstanceState와 onRestoreInstanceState의 호출 시점

- onSaveInstanceState와 onRestoreInstanceState의 활용 예시

- 한계

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- onSaveInstanceState를 활용하는 경우
- onSaveInstanceState,onRestoreInstanceState 호출 시점

<!--Contents-->

---
##  Activity의 종료
 Activity는 다양한 경우로 종료가 됩니다.
1. 사용자가 ‘뒤로 가기(Back)’ 버튼을 눌러 Activity를 종료한 경우
2. Activity가 백그라운드에 있을 때 시스템 메모리가 부족해진 경우(OS가 강제 종료시킴)
3. 언어 설정을 변경할 때
4. 폰트 크기 or 폰트를 변경했을 때
5. 화면 회전

이렇게 Activity가 종료될 때 사용자의 의지로 꺼지는 경우가 있기도 하지만 부득이하게 꺼지는 경우가 있습니다. 부득이한 경우(2,3,4,5)에 사용자는 앱이 UI상태가 유지되기를 기대합니다. 이를 위해 onSaveInstanceState를 활용하여 사용자의 임시 UI 상태를 보존해야 합니다. 사용자가 뒤로 버튼을 누르거나(1) 활동이 finish() 메서드를 호출하여 자체적인 소멸 신호를 보내는 경우 해당 Activity 인스턴스에 관한 시스템과 사용자의 콘셉트가 모두 영구적으로 사라집니다. 이는 사용자의 의도가 시스템의 동작과 일치하므로 추가적인 작업이 필요하지 않습니다.

---

##  onSaveInstanceState와 onRestoreInstanceState의 호출 시점

![image](https://user-images.githubusercontent.com/22022393/126908472-53a2b2f9-2c3b-4582-b471-996a0fbe11f9.png)  

- 화면 회전, 언어 변경, 폰트 크기 또는 폰트 변경과 같은 app Configuration 발생 시  

onPause() -> onStop() -> onSaveInstanceState() -> onDestroy() -> onCreate() -> onStart() -> onRestoreInstanceState() -> onResume()  

onSaveInstanceState()의 경우 버전에 따라 onStop()이후에 호출되기도 함  

- 시스템 메모리 부족으로 background에 있는 activity가 시스템에 의해 강제로 종료되었다가 다시 복구되는 경우  

강제종료 -> onCreate() -> onStart() -> onRestoreInstanceState() -> onResume()  

---

## onSaveInstanceState와 onRestoreInstanceState의 활용 예시  

#### onSaveInstanceState()를 사용하여 UI 상태 저장  
```kotlin
override fun onSaveInstanceState(outState: Bundle?) {
    // Save the user's current game state
    outState?.run {
        putInt(STATE_SCORE, currentScore)
        putInt(STATE_LEVEL, currentLevel)
    }

    // Always call the superclass so it can save the view hierarchy state
    super.onSaveInstanceState(outState)
}

companion object {
    val STATE_SCORE = "playerScore"
    val STATE_LEVEL = "playerLevel"
}
```
activity가 정지되기 시작하면 인스턴스 상태 번들에 상태 정보를 저장할 수 있도록 시스템이 onSaveInstanceState() 메서드를 호출합니다. 인스턴스 상태 정보를 저장하려면 onSaveInstanceState()를 재정의하고, activity가 예상치 못하게 소멸될 경우 저장되는 Bundle 객체에 키-값 쌍을 추가해야 합니다.  

#### 저장된 인스턴스 상태를 사용하여 활동 UI 상태 복원  

activity가 이전에 소멸된 후 재생성되면, 시스템이 activity에 전달하는 Bundle로부터 저장된 인스턴스 상태를 복구할 수 있습니다. onCreate() 및 onRestoreInstanceState() 콜백 메서드 둘 다 인스턴스 상태 정보를 포함하는 동일한 Bundle을 수신합니다.  

onCreate() 메서드는 시스템이 activity의 새 인스턴스를 생성하든, 이전 인스턴스를 재생성하든 상관없이 호출되므로 읽기를 시도하기 전에 번들 상태가 null인지 반드시 확인해야 합니다. null일 경우, 시스템은 이전에 소멸된 activity의 인스턴스를 복원하지 않고 새 인스턴스를 생성합니다.

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState) // Always call the superclass first

    // Check whether we're recreating a previously destroyed instance
    if (savedInstanceState != null) {
        with(savedInstanceState) {
            // Restore value of members from saved state
            currentScore = getInt(STATE_SCORE)
            currentLevel = getInt(STATE_LEVEL)
        }
    } else {
        // Probably initialize members with default values for a new instance
    }
    // ...
}
```

시스템은 복원할 저장 상태가 있을 경우에만 onRestoreInstanceState()를 호출하기 때문에 위의 경우와 다르게 Bundle이 null인지 확인할 필요가 없습니다.  

```kotlin
override fun onRestoreInstanceState(savedInstanceState: Bundle?) {
    // Always call the superclass so it can restore the view hierarchy
    super.onRestoreInstanceState(savedInstanceState)

    // Restore state members from saved instance
    savedInstanceState?.run {
        currentScore = getInt(STATE_SCORE)
        currentLevel = getInt(STATE_LEVEL)
    }
}
```


---
## 한계
Activiy가 다시 생성될때 사용할 데이터를 인스턴스라고 하는데 인스턴스는 Bundle이라는 객체에 '키-밸류' 형태로 저장됩니다.  
그리고 이 Bundle은 주로 onSaveInstanceState() 메소드에서 관리할 수 있기 때문에 화면 회전 등과 같은 과정에서 onSaveInstanceState()를 재정의,사용하여 인스턴스를 저장합니다.  
onSaveInstanceState()는 onStop() 메서드가 호출될때 호출되며 호출 순서의 경우 버전에 따라 차이가 있습니다.  
Bundle에 onSaveInstanceState()를 활용하여 데이터를 저장하는 과정에서 직렬화 과정을 거치는데 이는 큰 데이터 처리에는 알맞지 않으므로 한계가 있습니다. 또한, onSaveInstanceState()는 메인 쓰레드에서 동작해야하기 때문에 여기서 데이터를 저장하는데 시간을 많이 사용하게 되면 그만큼 UI에 버벅거림이 생기게 됩니다.


---
## Reference
- [android developer](https://developer.android.com/guide/components/activities/activity-lifecycle#asem)
