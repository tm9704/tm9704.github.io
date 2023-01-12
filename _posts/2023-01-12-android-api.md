---
title: "Android(11) - API 레벨 & 퍼미션 설정"
excerpt: "Android API & Permission"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-12 23:10:00"
last_modified_at: "2023-01-12 23:40:00"
---

## 1. API 레벨 호환성 고려하기

build.gradle에 SDK 버전을 설정하는 targetSdk와 minSdk를 봤었다.<br/>
여기에 설정하는 값은 API 레벨을 의미하며 targetSdk는 여기에 설정한 버전의 API로 앱을 개발한다는 의미이고
minSdk는 여기에 설정한 버전의 기기부터 설처할 수 있다는 의미이다.<br/>
즉 targetSdk로 앱을 개발하지만 minSdk까지 오류가 발생하지 않고 동작해야한다.<br/><br/>

앱을 개발할 때 minSdk 설정값보다 상위 버전에서 제공하는 API를 사용한다면 호환성을 고려해야 한다.<br/>
API 레벨 호환성 문제가 발생하는 클래스나 함수를 사용하면 안드로이드 스튜디오에서 경고나 오류메세지를 표시함.<br/>
API 레벨 호환성에 문제가 있는 API를 사용할 때는 @ 기호로 시작하는 애너테이션을 추가해 오류를 해결할 수 있다.<br/>

```kotlin
@RequiresApi(Build.VERSION_CODES.S)
fun noti() {
    ...
    val builder: Notification.Builder = Notification.Builder(this, "1")
        .setStyle(
            Notification.CallStyle.forIncomingCall(caller, declineIntent, answerIntent)
        )
    ...
}
```

API 레벨 호환성에 문제가 있는 API를 사용한 함수나 클래스 선언부 위에 @RequiresApi 애너테이션을 추가하면
안드로이드 스튜디오에서 오류가 발생하지 않고 @RequiresApi 대신 @TargetApi를 이용해도 된다.<br/><br/>

API 호환성 애너테이션은 오류를 무시하는 설정일 뿐이며 앱이 실행될 때 API 레벨 호환성을 막으려면
직접 코드로 처리해야 한다.<br/>

```kotlin
if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.S){
    val builder: Notification.Builder = Notification.Builder(this, "1")
        .setStyle(
            Notification.CallStyle.forIncomingCall(caller, declineIntent, answerIntent)
        )
}
```

Build.VERSION.SDK_INT는 앱이 실행되는 기기의 API 레벨이다.<br/>

## 2. 퍼미션 설정하기

퍼미션이란 앱의 특정 기능에 부여하는 접근을 의미한다.<br/>

1. **퍼미션 설정과 사용 설정**<br/>
   어느 앱에 퍼미션을 설정하려면 해당 앱의 매니패스트 파일에 permission 태그로 퍼미션을 설정해야 한다.<br/>
   그리고 퍼미션을 설정한 컴포넌트를 다른 앱에서 사용하고자 할 때 매니패스트 파일에 uses-permission 태그를 설정해야 한다.<br/>

   1. permission: 기능을 보호하려는 앱의 매니페스트 파일에 설정
   2. uses-permission: 퍼미션으로 보호된 기능을 사용하려는 앱의 매니페스트 파일에 설정<br/>

   매니페스트 파일에 퍼미션을 설정할 때는 permission 태그와 다음 속성을 사용.<br/>

   1. name: 퍼미션 이름
   2. label, description: 퍼미션 설명
   3. protectionLevel: 보호 수준<br/>

   ```xml
   <permission android:name="com.example.permission.TEST_PERMISSION"
        android:label="Test Permission"
        android:description="@string/permission_desc"
        android:protectionLevel="dangerous"/>
   ```

   name은 퍼미션을 구별하는 식별자 역할을 하며, 개발자가 직접 작성함.<br/>
   label과 description은 이 퍼미션을 이용하는 외부 앱에서 권한 인증 화면에 출력할 퍼미션의 정보.<br/>
   protectionLevel 속성은 보호 수준을 의미하고 다음과 같은 값을 지정할 수 있다.<br/>

   1. normal: 낮은 수준의 보호, 사용자에게 권한 요청을 하지 않아도 된다.
   2. dangerous: 높은 수준의 보호, 사용자에게 권한 요청을 해야한다.
   3. signature: 같은 키로 인증한 앱만 실행
   4. signatureOrSystem: 안드로이드 시스템 앱이거나 같은 키로 인증한 앱만 실행.<br/>

   보통 위에 3개만 사용함. 모든 protectionLevel의 속성값들은 퍼미션 사용 설정은 해야함.<br/><br/>

   매니페스트 파일에 permission을 설정했다고 해서 컴포넌트가 보호되지는 않는다.<br/>
   permission으로 설정한 다음 이 퍼미션으로 보호하려는 컴포넌트에 적용해야 한다.<br/>
   android:permission 속성을 이용함.<br/>

   ```xml
   <activity android:name=".OneActivity" android:permission="com.example.TEST_PERMISSION">
        <intent-filter>
            <action android:name="android.intent.action.PICK"/>
        </intent-filter>
    </activity>
   ```

   위 컴포넌트는 설정한 퍼미션에 의해 보호되고 이 컴포넌트를 이용하는 곳에서는 매니페스트 파일에
   uses-permission을 선언해야 정상으로 실행된다.<br/>

   ```xml
   <uses-permission android:name="com.example.TEST_PERMISSION"/>
   ```

   외부 앱과 연동하는 거 말고도 안드로이드 시스템에서 보호하는 기능을 사용할 때도 마찬가지로 해줘야한다.<br/>
   (시스템이 보호하는 기능은 검색)<br/>

2. **퍼미션 허용 확인**<br/>
   사용자가 퍼미션을 허용했는지 확인하려면 checkSelfPermission() 함수를 이용한다.<br/>

   ```kotlin
   open static fun checkSelfPermission(
       @NonNull context: Context,
       @NonNull permission: String
   ): Int
   ```

   두 번째 매개변수가 퍼미션을 구분하는 이름이며 결과값은 <br/>

   1. PackageManager.PERMISSION_GRANTED: 권한을 허용한 경우
   2. PackageManager.PERMISSION_DENIED: 권한을 거부한 경우<br/>

   이 중 하나의 상수로 전달됨.<br/>

   ```kotlin
   val status = ContextCompat.checkSelfPermission(this,
        "android.permission.ACCESS_FINE_LOCATION")
    if(status == PackManager.PERMISSION_GRANTED) {
        Log.d(...)
    }else {
        Log.d(...)
    }
   ```

   퍼미션이 거부된 상태라면 퍼미션을 허용해 달라고 요청해야 하는데, ActivityResultLauncher를 이용하면 된다.<br/>
   이 클래스는 액티비티에서 결과를 돌려받아야 할 때 사용하고 대표적으로 퍼미션 허용 요청과 다른 액티비를
   실행하고 결과를 돌려받을 때이다.<br/>
   ActivityResultLauncher 객체는 registerForActivityResult() 함수를 호출해서 만든다.<br/>

   ```kotlin
   public final <I, O> Activity
   ```
