---
title: "Android(1) - 안드로이드 앱의 기본 구조"
excerpt: "Android Basic"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2022-12-30 12:10:00"
last_modified_at: "2022-12-30 13:10:00"
---

요즘 안드로이드 어플 만들면서 Kotlin 언어만 공부하고 안드로이드 관련 지식이 많이 부족하다고 느껴서 어제 책을 한권 사서 포스팅을 해보려고 한다.<br/>
공부하는 책은 **깡샘의 안드로이드 앱 프로그래밍** 이라는 책인데, Kotlin으로 되어 있어서 구매했다.<br/>
어제 책을 조금 읽어봤는데 1장은 설치, 배포 관련 내용이라서 따로 포스팅은 안하고 필요할 때 다시 보려고 한다.<br/>

## 1. 안드로이드 소개

안드로이드는 리눅스 커널 기반으로 구글에서 제작한 모바일 OS이다.<br/>

1. 안드로이드의 특징

   1. 안드로이드는 리눅스 기반
   2. 자바나 코틀린 언어를 이용해 개발한다
   3. 안드로이드 OS 주요 부분과 라이브러리 코드들은 대부분 공개되어 있다
      <br/>

   **안드로이드 운영체제의 구조**<br/>
   ![android_stack](/images/android_stack.png)<br/>
   (출처: developer.android.com)<br/>

   1. 리눅스 커널<br/>
      안드로이드는 리눅스에 기반을 둔 오픈소스 소프트웨어 스택
   2. 하드웨어 추상화 레이어(hardware abstraction layer, HAL)<br/>
      상위의 자바 API프레임 워크에서 하드웨어 기능을 이용할 수 있게 표준 인터페이스를 제공한다.
   3. 안드로이드 런타임(Android runtime)<br/>
      ART라고 부르며 앱을 실행하는 역할을 한다.<br/>
      안드로이드 앱은 DEX 파일로 빌드되는데, 이 DEX 파일을 해석해서 실행하는 주체가 ART.<br/>
      안드로이드는 자바와 다르게 자바 소스를 컴파일해서 클래스 파일(자바 바이트 코드)로 만들고,
      이 클래스 파일을 런타임 때 그대로 실행하지 않고 DEX 파일로 컴파일을 한다.<br/>
      그리고 이 DEX 파일을 ART(Android Runtime)에서 실행한다.<br/>
      DEX 파일은 개발자가 직접 만들 필요는 없음.
   4. 네이티브 C/C++ 라이브러리<br/>
      안드로이드 앱은 대부분 자바 프레임워크로 개발하지만 네이티브 C/C++ 라이브러리를 이용할 수도 있는데,
      이거를 **안드로이드 NDK**라고 한다.<br/>
   5. 자바 API 프레임 워크<br/>
      앱을 개발할 때 사용하는 자바 API
      <br/><br/>

2. 안드로이드 버전<br/>
   안드로이드 버전은 12.0, 13.0처럼 운영체제 버전을 가리키지만, 앱을 개발할 때 사용하는 버전은
   **API 레벨(SDK)**임.<br/>
   운영체제 버전별로 API 레벨이 지정되어 있어서 소스코드에서는 대부분 API 레벨을 사용한다. 즉,
   개발자는 운영체제 버전과 API 레벨을 함께 알고 있어야 한다.<br/>

## 2. 안드로이드 앱 개발의 특징

안드로이드 앱 개발의 핵심은 **컴포넌트**<br/>

1. 컴포넌트란<br/>
   컴포넌트는 어플리케이션의 구성요소라고 할 수 있다.<br/>
   컴포넌트 자체가 어플리케이션이 아니라, 어플리케이션을 구성하는 단위이다. 즉,
   하나의 어플리케이션은 여러 컴포넌트로 구성됨.<br/>

   안드로이드 앱의 기본구조도 컴포넌트에 기반을 두기 때문에 하나의 앱은 여러 컴포넌트로 구성된다.<br/>
   그리고 안드로이드에서는 클래스로 컴포넌트를 개발한다.<br/> 즉, 하나의 클래스 == 하나의 컴포넌트.<br/>

2. 안드로이드 앱을 구성하는 클래스는 모두 컴포넌트인가<br>
   클래스로 컴포넌트를 개발한다고 해서 모든 클래스가 컴포넌트인 것은 아니다.<br/>
   앱은 여러 클래스로 구성되는데 크게 **컴포넌트 클래스**와 **일반 클래스**로 이루어져 있음<br/>
   두 클래스 모두 개발자가 만들지만 런타임 때 누가 생명주기를 관리하는지에 따라 차이가 있다.<br/><br/>

   앱이 실행될 때 클래스의 객체 생성부터 소멸까지 생명주기 관리를 **개발자 코드**에서 한다면 **일반클래스**<br/>
   즉, 안드로이드 앱의 구성요소인 컴포넌트가 아닌 개발자가 임의의 목적으로 만든 클래스를 의미.<br/>
   생명주기를 **안드로이드 시스템**에서 관리한다면 **컴포넌트 클래스**.<br/>

3. 컴포넌트의 종류

   1. 액티비티<br/>
      화면을 구성하는 컴포넌트<br/>
      앱의 화면을 출력하려면 액티비티를 만들어야 한다.<br/>
   2. 서비스<br/>
      백그라운드에서 작업을 하는 컴포넌트<br/>
      화면 출력 기능은 없어서 서비스가 실행되더라도 화면에 출력되지는 않는다.<br/>
      백그라운드에서 장시간 실행해야 할 업무를 담당한다.<br/>
   3. 콘텐츠 프로바이더<br/>
      앱의 데이터를 공유하는 컴포넌트<br/>
      하나의 앱이 자신의 데이터를 다른 앱에 공유하려면 콘텐츠 프로바이더를 만들어야 하고,
      다른 앱에서는 그 콘텐츠 프로바이더를 이용해 데이터에 접근한다.<br/>
   4. 브로드캐스트 리시버<br/>
      시스템 이벤트가 발생할 때 실행되게 하는 컴포넌트<br/>
      여기서 이벤트는 시스템에서 발생하는 특정 상황을 의미 (시스템 콜)<br/>

   이런 컴포넌트를 구분하는 방법은 개발자가 컴포넌트 클래스를 만들 때는 클래스를 상속받아야 하는데,<br/>
   **상위 클래스**를 보고 구분할 수 있다.<br/>
   액티비티는 Activity 클래스를, 서비스는 Service, 콘텐츠 프로바이더는 ContentProvider, 브로드캐스트 리시버는
   BroadcastReceive 클래스를 상속받아서 만든다.<br/>

4. 컴포넌트는 앱 안에서 독립된 실행의 단위<br/>
   컴포넌트는 어플리케이션 안에서 독립된 실행 단위라는 중요한 특징이 있는데,<br/>
   컴포넌트끼리 서로 종속되지 않아서 코드 결합이 발생하지 않는다는 의미이다.<br/><br/>

   예를 들어 카카오톡의 목록 화면과 채팅 화면이 있다고 가정하자.<br/>
   목록 화면을 ListActivity라는 클래스명, 채팅 화면을 ChatActivity라는 클래스 명으로 작성하고,
   ListActivity에서 ChatActivity를 실행해 화면 전환을 한다고 가정한다.<br/><br/>

   ListActivity에서 ChatActivity를 실행해야 하므로 ListActivity에서 ChatActivity의 객체를 생성해서 실행하면
   될 것 같지만 안드로이드에서 해당 방법은 불가능하다.<br/>
   즉 두 클래스를 결합해서 실행하는게 불가능 하다는 건데, 컴포넌트의 생명주기를 안드로이드 시스템에서 관리하므로
   코드에서 직접 객체를 생성해서 실행하는게 불가능 하다.<br/>
   해결 방법은 안드로이드 시스템에 의뢰해서 시스템이 ChatActivity를 실행해야 한다.<br/>
   즉, 두 클래스가 서로 종속되지 않게 독립해서 실행되게 해야한다.<br/><br/>

   이렇게 컴포넌트가 앱 내에서 독립해서 실행되는 특징 덕분에 **앱의 실행 시점**이 다양하다.<br/>
   즉, ListActivity가 먼저 실행될 수도 있고 ChatActivity가 먼저 실행될 수도 있음.<br/>
   그래서 안드로이드 앱에는 메인 함수(앱의 단일 시작점) 개념이 없다고 할 수 있다.<br/>

5. 어플리케이션 라이브러리 사용<br/>
   어플리케이션 라이브러리란 다른 어플리케이션을 라이브러리처럼 사용하는 것을 의미한다.<br/>
   예를 들면 카카오톡에서 사진을 전송할 때 카메라 앱을 이용해 사진을 찍고 보낼 수 있는데,<br/>
   카카오톡 앱이 카메라 앱을 라이브러리처럼 사용한 것이다.<br/>

6. 리소스를 활용한 개발<br/>
   안드로이드 앱 개발의 또 다른 특징은 리소스를 많이 활용한다는 점이다.<br/>
   **리소스**란 코드에서 정적인 값을 분리한 것을 의미하는데<br/>
   앱에서 발생하는 데이터, 사용자 이벤트 같은 동적인 값이 아니라 항상 똑같은 값이라면 따로 분리해서
   개발하는게 생산성, 유지 보수에 좋기 때문이다.<br/>

## 3. 앱 구성 파일 분석

1. 프로젝트의 폴더 구성<br/>
   안드로이드 앱 프로젝트를 만들면 많은 폴더와 파일이 생성되는데, 대부분 빌드 도구와 관련된 것이고<br/>
   우리가 관심을 가져야 하는 부분은 main폴더 안에 있는 친구들이다.<br/>
   뭐 파일 탐색기 써서 굳이 안들어가도 되고 안드로이드 스튜디오를 실행하면 프로젝트 탐색창에 개발자에게
   필요한 폴더와 파일들만을 보여주기 때문에 이거만 봐도 괜찮음.<br/><br/>
   ![android_file](/images/android_file.png)<br/>
   위처럼 프로젝트를 만들면 **app**이라는 모듈이 자동으로 생성됨.<br/>
   모듈 하나가 앱 하나이며 프로젝트는 여러 모듈을 묶어서 관리하는 개념이다.<br/>
   하나의 프로젝트에는 app말고 여러 모듈을 추가할 수 있는데, 모듈은 앱 단위이므로 새로운 모듈을 추가하는 것은
   새로운 앱을 개발하는 것과 같은 의미이다.<br/>

2. 모듈의 폴더 구성

   1. 그래들 빌드 설정 파일<br/>
      그래들(gradle)은 안드로이드 앱의 빌드 도구<br/>
      그래들의 설정 파일이 build.gradle이고 앱을 빌드하는 데 필요한 설정을 이 파일에서 등록한다.<br/>
      탐색 창을 보면 build.gradle이 2개가 있는데 하나는 프로젝트 수준, 하나는 모듈 수준의 파일이다.<br/>
      모듈 == 앱이므로 대부분의 빌드 설정은 모듈 수준 파일에서 작성하면 된다.<br/><br/>

      모듈 수준의 그래들 파일을 보면 몇 가지 설정이 자동으로 되어있음<br/>
      이 값을 수정하거나 새로 추가하면서 빌드 환경을 설정함.<br/>

      ```gradle
      plugins{
          id 'com.android.application'
          id 'kotlin-android'
      }
      ```

      위 코드는 플러그인 선언 부분이고 필요 시에 추가할 수 있음.<br/>

      ```gradle
      compileSdk 33
      ```

      위 코드는 앱을 컴파일하거나 빌드할 때 적용할 버전을 설정.<br/>
      안드로이드 SDK 33버전을 적용해서 컴파일하라는 의미.<br/>

      ```gradle
      applicationId "com.example.androidlab"
      ```

      applicationId는 앱의 식별자를 설정<br/>

      ```gradle
      minSdk 21
      targetSdk 33
      ```

      targetSdk는 개발할 때 적용되는 SDK버전을 의미.<br/>
      33으로 지정하면 33버전의 SDK로 개발한다는 의미.<br/>
      minSdk는 이 앱을 설치할 수 있는 기기의 최소 SDK 버전을 의미함.<br/>

      ```gradle
      versionCode 1
      versionName "1.0"
      ```

      위 코드는 앱의 버전을 설정.<br/>
      초깃값은 1이지만 앱이 사용자의 스마트폰에 설치되어 이용되다가 업데이트 될 때
      이 버전을 올려 다시 배포한다.<br/>

      ```gradle
      copileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
      }

      kotlinOptions {
        jvmTarget = '1.8'
      }
      ```

      위 코드는 개발 언어의 버전을 설정.<br/>

      ```gradle
      dependencies {
        implementation 'androidx.core:core-ktx: 1.7.0'
        implementation 'androidx.appcompat:appcompat:1.5.1'
        implementation 'com.google.android.material:material:1.4.0'
        implementation 'androidx.constraintlayout:constraintlayout:2.1.2'
        testImplementation 'junit:junit:4.+'
        androidTestImplementation 'androidx.test.ext:junit:1.1.3'
        androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
      }
      ```

      위 코드는 앱에서 이용하는 라이브러리의 버전을 설정.<br/>
      targetSdk에 명시한 SDK는 기본으로 적용되지만 그 외에 개발자가 추가하는 오픈 소스 라이브러리나
      구글의 androidx 라이브러리 등 SDK 라이브러리가 아닌 것들으 모두 dependecies에 선언해야 한다.<br/>
      실제 개발할 때는 dependencies에 많은 라이브러리를 선언한다.<br/>

   2. 메인 환경 파일<br/>
      AndroidManifest.xml은 안드로이드 앱의 메인 환경 파일이다.<br/>
      안드로이드 시스템은 이 파일에 설정한 대로 사용자의 폰에서 앱을 실행한다.<br/>

      ```xml
      <manifest xmlns:android="http://schemas.android.com/apk/res/android" xmlns:tools = "http://schemas.android.com/tools">
      ```

      manifest는 매니페스트 파일의 루트 태그.<br/>
      xmlns는 XML의 네임스페이스 선언이며 URL이 http://schemas.android.com/apk/res/android로 선언되었다면
      안드로이드 표준 네임스페이스이다.<br/>

      ```xml
       <application
            android:allowBackup="true"
            android:dataExtractionRules="@xml/data_extraction_rules"
            android:fullBackupContent="@xml/backup_rules"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/Theme.AndroidLab"
            tools:targetApi="31">
            (...생략)
        </application>
      ```

      application 태그는 앱 전체를 대상으로 하는 설정.<br/>
      해당 태그에는 앱의 아이콘을 설정하는 icon 속성이 있는데, 이곳에 지정한 이미지가 실행 아이콘이다.<br/>
      XML의 속성값이 @으로 시작하면 리소스를 의미.<br/><br/>

      label 속성에는 앱의 이름을 등록.<br/>
      theme 설정은 앱에 적용해야 하는 설정.<br/><br/>

      안드로이드 컴포넌트는 시스템에서 생명주기를 관리한다.<br/>
      그리고 시스템은 매니페스트 파일에 있는 대로 앱을 실행하기 때문에 컴포넌트는 매니페스트 파일에 등록해야
      시스템이 인지한다.<br/>

      ```xml
      <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
      ```

      액티비티는 activity 태그로, 서비스는 service, 나머지도 동일하게 태그로 등록한다.<br/>
      컴포넌트 하나당 태그 하나로 등록하며 activity가 10개라면 태그 10개 선언해야한다.<br/><br/>

      액티비티를 등록할 때 필수 속성은 name이다. name 속성에는 클래스 이름을 등록한다.<br/>
      위에서 android:name=".MainActivity"는 MainActivity 클래스를 액티비티로 등록하겠다는 의미.<br/>
      여기서 클래스 앞에 있는 점은 해당 클래스가 manifest 태그에 등록한 package 경로에 있다는 의미.<br/><br/>

   3. 리소스 폴더<br/>
      res 폴더는 앱의 리소스를 등록하는 목적으로 사용.<br/>
      기본적으로 4개가 있다.<br/>

      1. drawable: 이미지 리소스
      2. layout: UI 구성에 필요한 XML 리소스
      3. mipmap: 앱 아이콘 이미지
      4. values: 문자열 등의 값으로 이용되는 리소스<br/>

      res 폴더 아래에 리소스를 만들면 자동으로 R.java 파일에 상수 변수로 리소스가 등록되고
      코드에서는 이 상수 변수로 리소스를 이용한다.<br/>
      (R.java는 자동으로 만들어진다.)<br/><br/>

      리소스를 만들면 해당 리소스를 식별하기 위한 int형 변수가 R.java 파일에 만들어진다.<br/>
      이처럼 안드로이드 리소스 파일이 R.java 파일에 상수 변수로 등록되어 이용되면서 다음과 같은
      규칙이 생긴다.<br/>

      1. res 하위의 폴더명은 지정된 폴더명을 사용해야 한다.
      2. 각 리소스 폴더에 다시 하위 폴더를 정의할 수 없음
      3. 리소스 파일명은 자바의 이름 규칙을 위배할 수 없음
      4. 리소스 파일 명에는 알파벳 대문자를 이용할 수 없음

   4. 레이아웃 XML 파일<br/>
      res/layout 폴더 아래 기본으로 만들어지는 activity_main.xml 파일은 화면을 구성하는 파일<br/>

      ```xml
      <?xml version="1.0" encoding="utf-8"?>
      <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
          xmlns:app="http://schemas.android.com/apk/res-auto"
          xmlns:tools="http://schemas.android.com/tools"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          tools:context=".MainActivity">

          <TextView
              android:id="@+id/textView"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="Hello World!"
              app:layout_constraintBottom_toBottomOf="parent"
              app:layout_constraintEnd_toEndOf="parent"
              app:layout_constraintStart_toStartOf="parent"
              app:layout_constraintTop_toTopOf="parent" />

      </androidx.constraintlayout.widget.ConstraintLayout>
      ```

      이 파일을 열어 보면 androidx.constraintlayout.widget.ConstraintLayout 태그와 TextView태그가
      등록되어 있다. 일단 여기서는 화면을 구성하는 요소라는 정도만 알면 될 것 같음.<br/><br/>

   5. 메인 액티비티 파일

   ```kotlin
   class MainActivity : AppCompatActivity() {
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           setContentView(R.layout.activity_main)
       }
   }
   ```

   위 파일은 MainActivity.kt파일의 내용.<br/>
   내용을 간단하게 살펴보면 AppCompatActivity를 상속받아 MainActivity라는 클래스를 정의했다.<br/>
   AppCompatActivity는 Activity의 하위 클래스. 따라서 MainActivity는 액티비티 컴포넌트 클래스.<br/>
   즉, 이 클래스는 화면 출력을 목적으로 하는 액티비티 클래스라는 의미.<br/><br/>

   MainActivity 클래스가 실행되면 onCreate() 함수가 자동으로 호출되며 onCreate() 함수안의 구문을 실행함.<br/>
   여기서 setContentView() 함수는 매개변수에 지정한 내용을 액티비티 화면에 출력함.<br/>
