# 인텐트 (Intent)
<!--Table of Contents-->
- 인텐트 (Intent) 란?
    - 정의
- 인텐트 유형
    - 명시적 인텐트
    - 암시적 인텐트
    - 인텐트 필터란?
- 인텐트 빌드 
    - 인텐트에 포함된 기본 사항
    - 예시 

<!-- 어떤 질문을 대답할 수 있어야 하는지-->
## You can answer
- Intent 란 무엇인가요 ?

<!--Contents-->

---
## 인텐트 (Intent) 란? 
### 정의
  * 인텐트는 메시징 객체로 컴포넌트가 운영체제와 통신하는데 사용할 수 있다.
  인텐트가 다목적 통신 도구로, 이것을 추상화한 Intent 클래스는 인텐트의 용도에 따라 
  서로 다른 생성자들을 제공한다. 
    1) 액티비티 시작  
      Activity는 앱 안의 단일 화면으로 새 인스턴스를 시작하려면 Intent를 startActivity()로 전달하면 된다.  
       Intent는 시작할 액티비티를 설명하고 모든 필수 데이터를 담는다.  
       액티비티가 완료되었을 때 startActivityForResult()를 호출해서 결과를 수신한다.
       액티비티는 해당 결과를 이 액티비티의 onActivityResult() 콜백에서 별도의 Intent 객체로 수신한다.
    2) 서비스 시작  
        Service는 Android 5.0(API 레벨 21) 이상부터 JobScheduler로 시작할 수 있다.
       Android 5.0(API 레벨 21) 이하 버전은 Service 클래스의 메서드를 사용해서 시작할 수 있다.  
    3) 브로드캐스트 전달  
       시스템은 시스템 이벤트에 대한 다양한 브로드캐스트를 전달한다.  
       인텐트를 sendBroadcast() 또는 sendOrderedBroadcast()에 전달하면 다른 앱의 브로드캐스트에 전달할 수 있다. 
## 인텐트의 유형
  * 명시적 인텐트  
    - 인텐트를 충족하는 어플리케이션이 무엇인지 지정한다. 대상 앱의 패키지 이름 또는 완전히 자격을 갖춘 요소 클래스 이름을 제공한다.  
      일반적으로 앱 안에서 구성 요소(액티비티 등)를 시작할 때 사용한다.
  * 암시적 인텐트 
    - 특정 구성요소의 이름을 대지 않지만, 수행할 일반적인 작업을 선언하여 다른 앱의 컴포넌트가 이를 처리할 수 있도록 도와준다.  
    - 암시적 인텐트를 사용하면 Android 시스템에서 시작할 적절한 구성요소를 찾는다. 이때 인텐트의 내용은 기기에 있는 다른 여러앱의 manifest 파일에서 선언된 <i>intent-filter</i> 와 비교하는 방법을 사용한다.
  

  ### Intent-filter 란 ?
      앱의 manifest 파일에 들어있는 표현으로 해당 구성요소가 수신하고자 하는 인텐트의 유형을 나타낸다.
      인텐트 필터를 선언하지 않는 경우, 명시적 인텐트로만 시작할 수 있다. 

 ※ 앱의 보안을 보장하려면 Service를 시작할 때는 항상 명시적 인텐트만 사용하고 service에 대한  
인텐트 필터는 선언하지 마세요! Android 5.0(API 레벨 21)부터 시스템은 개발자가 암시적 인텐트로  
bindService()를 호출하면 예외를 발생시킵니다. 

## 인텐트 빌드 
### Intent에 포함된 기본 사항
1. 구성 요소 이름  
   - 시작할 구성요소의 이름 
   - 인텐트를 명시적인 것으로 만들어주는 중요한 정보 
   - 구성 요소 이름을 설정하려면 setComponent(), setClass(), setClassName()을 사용하거나 Intent 생성자 사용
2. 작업 
   - 수행할 일반적인 작업을 나타내는 문자열
   - 본인의 앱 내에 있는 인텐트가 사용할 작업을 직접 정할 수도 있지만, 보통은 Intent 클래스나 다른 프레임워크 클래스가 정의한 작업 상수를 지정해야함
   - 작업을 지정하려면 setAction() 또는 Intent 생성자 사용
3. 데이터
   - 작업을 수행할 데이터 및 또는 해당 데이터의 MIME 유형을 참조하는 URI 객체 
   - 데이터 URI만 설정하려면 setData() 를 호출, MIME 유형을 설정하려면 setType() 호출
     필요한 경우 setDataAndType()을 사용하여 두 가지 모두 명시적으로 설정 가능   
     ( setData() 와 setType()을 동시에 호출하는 것은 서로를 무효화하므로 주의! )
4. 카테고리
   - 인텐트를 처리해야 하는 구성 요소의 종류에 관한 추가 정보를 담은 문자열
   - addCategory()로 지정
5. 엑스트라
   - 요청된 작업을 수행하는데 필요한 추가 정보가 담긴 키-값 쌍
   - 다양한 putExtra() 메서드로 엑스트라 데이터 추가 가능하며  
     모든 엑스트라 데이터를 포함한 Bundle 객체를 만든 다음 Bundle을 Intent에 putExtras()로 삽입 
6. 플래그
   - Intent 클래스에서 정의되며 인텐트에 대한 메타데이터와 같은 기능  
     ex) Android 시스템에 액티비티를 시작할 방법에 대한 지침을 줄 수도 있고 시작한 다음 어떻게 처리해야하는지도 알려줌
     
### 예시
* 암시적 인텐트 예시   
```kotlin   
    val intent = Intent(Intent.ACTION_VIEW)
    intent.setData(Uri.parse("https://github.com/ThinLineIT"))  
    startActivity(intent)  
```

* 명시적 인텐트 예시  
```kotlin  
    // SubActivity에 엑스트라 추가하여 실행  
    val intent = Intent(this, SubActivity::class.java)  
            .putExtra("key", "data")  
    startActivity(intent)
```
```kotlin  
    // apply 함수를 이용해서 가독성 높이기 
    // 실행된 SubActivity로부터 결과를 받고 싶다면 startActivityForResult()  
    val REQUEST_CODE = 100  
    val intent = Intent(this, SubActivity::class.java)
    
    intent.apply{
        this.putExtra("firstKey", 1)
        this.putExtra("secondKey", 2)
    }
    
    startActivityForResult(intent,REQUEST_CODE)  
```
---
## Reference
- [Android developer - Intent and Intent Filters](https://developer.android.com/guide/components/intents-filters)
