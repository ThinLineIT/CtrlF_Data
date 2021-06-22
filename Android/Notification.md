# 시작하기 전에
 - 지원 라이브러리 추가
다음 종속 항목을 모듈 수준 build.gradle에 추가한다.

```kotlin
dependencies {
  implementation "com.android.support:support-compat:28.0.0"
}
```

---

# 알림 컨텐츠 설정
알림을 생성하기 위해서는 NotificationCompat.Builder 객체를 사용하여 알림 콘텐츠와 채널을 설정해야 한다.
```kotlin
var builder = NotificationCompat.Builder(this, CHANNEL_ID)
          .setSmallIcon(R.drawable.notification_icon)
          .setContentTitle(textTitle)
          .setContentText(textContent)
          .setPriority(NotificationCompat.PRIORITY_DEFAULT)
```
- **setSmallIcon()** 으로 설정한 작은 아이콘. 사용자가 볼 수 있는 유일한 필수 콘텐츠다.
- **setContentTitle()** 로 설정한 제목
- **setContentText()** 로 설정한 본문 텍스트
- **setPriority()** 로 설정한 알림 우선순위. 우선순위에 따라 Android 7.1이하에서 알림이 얼마나 강제적이어야 하는지가 결정된다.

**NotificationCompat.Builder** 생성자의 경우 채널 ID를 제공해야 한다. Android 8.0(API수준 26)이상에서는 호환성을 유지하기 위해 필요하지만 이전 버전에서는 무시한다. 채널 ID는 사용자가 원하는 String 형태로 설정하면 된다.

# 채널 만들기
Android 8.0 이상에서 알림을 제공하려면 먼저 **NotificationChannel** 인스턴스를 **createNotificationChannel()** 에 전달하여 앱의 알림 채널을 시스템에 등록해야 한다.

```kotlin
private fun createNotificationChannel() {
        // Create the NotificationChannel, but only on API 26+ because
        // the NotificationChannel class is new and not in the support library
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val name = getString(R.string.channel_name)
            val descriptionText = getString(R.string.channel_description)
            val importance = NotificationManager.IMPORTANCE_DEFAULT
            val channel = NotificationChannel(CHANNEL_ID, name, importance).apply {
                description = descriptionText
            }
            // Register the channel with the system
            val notificationManager: NotificationManager =
                getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
            notificationManager.createNotificationChannel(channel)
        }
    }
```

# 알림의 탭 작업 설정
알림을 탭했을 때의 활동을 설정하는 방법이다. 이 작업을 하려면 **PendingIntent** 객체로 정의된 콘텐츠 인텐트를 지정하여 **setContentIntent()** 에 전달한다.

```kotlin
// Create an explicit intent for an Activity in your app
    val intent = Intent(this, AlertDetails::class.java).apply {
        flags = Intent.FLAG_ACTIVITY_NEW_TASK or Intent.FLAG_ACTIVITY_CLEAR_TASK
    }
    val pendingIntent: PendingIntent = PendingIntent.getActivity(this, 0, intent, 0)

    val builder = NotificationCompat.Builder(this, CHANNEL_ID)
            .setSmallIcon(R.drawable.notification_icon)
            .setContentTitle("My notification")
            .setContentText("Hello World!")
            .setPriority(NotificationCompat.PRIORITY_DEFAULT)
            // Set the intent that will fire when the user taps the notification
            .setContentIntent(pendingIntent)
            .setAutoCancel(true) // 알림을 탭하면 자동으로 알림을 삭제하는 기능
```

# 알림 표시
알림을 표시하려면 **NotificationManagerCompat.notify()** 를 호출하여 알림의 고유 ID와 **NotificationCompat.Builder.build()** 의 결과를 전달한다.

```kotlin
with(NotificationManagerCompat.from(this)) {
        // notificationId is a unique int for each notification that you must define
        notify(notificationId, builder.build())
    }
```
NotificationManagerCompat.notify()에 전달하는 알림 ID를 저장해놔야 한다. 해당 알림을 업데이트하거나 삭제하려면 나중에 필요하기 때문이다.

# 구현 예시
- 채널 생성
```kotlin
val CHANNEL_ID = "CHANNEL"

fun createNotificationChannel() {
  if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
    val name = "Channel Name"
    val descriptionText = "Channel Description"
    val importance = NotificationManager.IMPORTANCE_DEFAULT
    val channel = NotificationChannel(CHNNEL_ID, name,importance).apply {
      description = descriptionText
    }
    val notificationManager : NotificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
    notificationManager.createNotificationChannel(channel)
  }
}
```

- 버튼 클릭시 알림 표시
```kotlin
val notificationId = 99

// 채널 생성
createNotificationChannel()

// 알림 클릭시 PendingActivity로 전환하는 Intent 생성
val intent = Intent(this, PendingActivity::class.java).apply {
  flags = Intent.FLAG_ACTIVITY_NEW_TASK or Intent.FLAG_ACTIVITY_CLEAR_TASK
}
val pendingIntent : PendingIntent = PendingIntent.getActivity(this, 0, intent, 0)

val builder = NotificationCompat.Builder(this, CHANNEL_ID)
        .setSmallIcon(R.mipmap.ic_launcher)
        .setContentTitle("Content Title")
        .setContentText("Content Text")
        .setPriority(NotificationCompat.PRIORITY_DEFAULT)
        .setContentIntent(pendingIntent) // 위에서 생성한 pendingIntent 적용
        .setAutoCancel(true) // 알림을 탭하면 자동으로 알림을 삭제하는 기능

button.setOnclickListener {
  with(NotificationManagerCompat.from(this)) {
    // notificationId는 각 notification 마다 유일하게 존재하는 것으로 임의로 정의해야한다.
    // 알림 실행
    notify(notificationId, builder.build())
  }
}
```
---
# Reference
- [Android Developers](https://developer.android.com/training/notify-user/build-notification?hl=ko)
- [[안드로이드]Notification 정리 및 예제](https://youngest-programming.tistory.com/491)
