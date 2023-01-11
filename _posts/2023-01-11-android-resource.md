---
title: "Android(10) - 리소스(1)"
excerpt: "Android Resource(1)"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-11 20:40:00"
last_modified_at: "2023-01-10 21:40:00"
---

## 1. 리소스의 종류와 특징

안드로이드 앱 개발에서 리소스는 정적인 자원을 의미한다.<br/>
앱이 동작하면서 동적으로 발생하거나 변경되는 콘텐츠는 코드로 작성해야 하지만,
정적인 콘텐츠들은 코드에 작성하지 않고 리소스로 분리해서 외부 파일로 만들어 이용한다.<br/>
이렇게 이용하면 코드가 더 간결해지고 유지,보수가 편해진다.<br/>
앱에서 이용하는 리소스는 크게 **앱 리소스**와 **플랫폼 리소스**로 나뉜다.<br/>

1. **앱 리소스 사용하기**
   앱 리소스는 개발자가 직접 추가한 리소스를 의미한다.<br/>
   앱을 개발하기 위해 모듈을 만들면 자동으로 res라는 디렉토리가 생기고 하위에
   drawable, layout, mipmap, values라는 디렉토리 4개가 생긴다.<br/>
   여기다가 리소스 파일을 만듦.<br/><br/>

   자동으로 생기는 디렉토리말고 더 많은 리소스 파일 종류가 있는데 그건 알아서 res 아래에 디렉토리를
   만들어서 사용하면 된다.<br/>

   1. animator: 속성 애니메이션 XML
   2. anim: 트윈 애니메이션 XML
   3. color: 색상 상태 목록 정의 XML
   4. drawable: 이미지 리소스
   5. mipmap: 앱 실행 아이콘 리소스
   6. layout: 레이아웃 XML
   7. menu: 메뉴 구성 XML
   8. raw: 원시 형태로 이용되는 리소스 파일
   9. values: 단순 값으로 이용되는 리소스
   10. xml: 특정 디렉토리가 정의되지 않은 나머지 XML 파일
   11. font: 글꼴 리소스<br/>

   리소스 디렉터리와 파일은 이름을 지을 때 규칙이 있다.<br/>
   위에서 소개한 리소스 디렉터리 명은 고정이고 리소스 파일 명은 values에 넣는 거를 제외하면
   자바의 이름 작성 규칙을 지켜야 하고 알파벳 대문자는 사용할 수 없다.<br/>
   이렇게 하는 이유는 리소스 디렉터리와 파일을 코드에서 그대로 사용하는게 아니라 R 파일에 식별자로
   등록해서 사용하기 때문이다.<br/>

   1. 레이아웃 리소스 - layout 디렉터리<br/>
      레이아웃 XML 파일을 저장하는 디렉터리<br/>
   2. 이미지 리소스 - drawable 디렉터리<br/>
      이미지 리소스를 저장하는 디렉토리.<br/>
      이곳에는 PNG, JPG, GIF, 9.PNG 파일들을 저장할 수 있으며 XML로 작성한 이미지도 저장이 가능하다.<br/>

      ```xml
      // XML로 작성한 이미지
      <shape xmlns:android="http://schemas.android.com/apk/res/android"
          android:shape="rectangle">
          <gradient
              android:startColor="#FFFF0000"
              android:endColor="#80FF00FF"
              android:angle="45"/>
          <corners android:radius="8dp"/>
      </shape>
      ```

      위 XML 파일은 XML 파일이지만 이미지 리소스처럼 ImageView 등에서 이용이 가능함.<br/>
      XML 이미지를 만들때는 다음 태그들을 사용함.<br/>

      1. shape
         도형을 의미하고 android:shpae = "rectangle"처럼 shape 속성을 이용해 도형의 타입을 지정,
         shape 값은 rectangle, oval, line, ring중 선택이 가능함.
      2. corners
         둥근 모서리를 그리는 데 사용한다. shape 값이 rectangle일 때만 적용이 가능
      3. gradient
         그라데이션 색상 지정
      4. size
         도형의 크기 지정
      5. solid
         도형의 색상 지정
      6. stroke
         도형의 윤곽선 지정<br/>

   3. mipmap 디렉토리<br/>
      앱을 기기에 설치하면 나타나는 실행 아이콘의 이미지 리소스가 저장되는 디렉토리.<br/>

   4. values 디렉토리
      값으로 이용되는 리소스를 저장하는 디렉터리이고 문자열, 색상, 크기, 스타일, 배열 등의 값을
      XML로 저장할 수 있다.<br/>
      values에 저장되는 리소스는 다른 디렉터리의 리소스와 이용 방법이 조금 다르다.<br/>
      다른 디렉터리의 리소스는 파일명이 R인 파일에 식별자로 추가되므로 코드에서 이 식별자로 구분해서
      사용한다.<br/>
      예를 들어 layout 디렉터리에 activity_main.xml 이라는 파일이 있을 경우, R.layout.activity_main으로 이용함.<br/>
      그러나 values에 문자열 리소스 파일 strings.xml이 있다고 가정했을 때 위 처럼 사용하지 않는다.<br/><br/>

      즉, values 디렉터리의 리소스 파일은 파일명이 R인 파일에 식별자로 등록되지 않고 리소스 파일에 값을 지정한
      태그의 name 속성값이 등록된다.<br/>

      ```xml
      <resources>
          <string name = "app_name">Test9</string>
          <string name = "txt_data1">Hello</string>
          <string name = "txt_data2">World</string>
      </resources>
      ```

      위 처럼 저장된 string.xml파일을 사용할 때는

      ```xml
      <TextView
          android:id="@+id/textView"
          android:layout_widht="wrap_content"
          android:layout_height="wrap_content"
          android:text="@string/txt_data1"/>
      ```

      ```kotlin
      binding.textView.text = getString(R.string.txt_data2)
      ```

      이렇게 name 속성에 지정한 값이 R파일에 식별자로 기록되기 때문에 위처럼 사용한다.<br/><br/>

      values 디렉터리에 저장되는 리소스는 문자열 이외에도 색상, 크기, 스타일 등이 있는데,<br/>
      색상은 color태그, 크기는 dimen 태그, 스타일은 style태그를 이용해서 사용한다.<br/>
      스타일 속성은 뷰에 설정되는 여러 속성을 스타일에 등록하여 한꺼번에 적용하거나, 여러 뷰에
      중복되는 속성을 스타일로 정의해 재사용하는 용도로 쓰인다.<br/>

      ```xml
      <resources>
          <style name="MyTextStyle">
              <item name="android:textSize">@dimen/txt_size</item>
              <item name="android:textSize">@color/txt_color</item>
          </style>
          <style name="MyTextStyleSub" parent="MyTextStyle">
              <item name="android:textColor">#0000FF</item>
              <item name="android:background">@color/txt_bg_color</item>
          </style>
      </resources>
      ```

      스타일 정의 시 다른 스타일을 상속받아 재정의할 수 있다.<br/>

   5. color 디렉토리<br/>
      color 디렉토리는 색상 리소스를 등록한다.<br/>
      values에도 등록이 가능하지만, color 디렉토리에 등록하는 이유는 values에 등록하는 색상은
      색상 하나를 리소스에 등록해 사용하겠다는 의미이고 color 디렉토리의 리소스는 특정 뷰의 상태를 표현하고
      그 상태에 적용되는 색상을 등록할 때 사용한다.<br/>
      예를 들면 어떤 버튼을 눌렀을 때와 누르지 않았을 때의 색상을 리소스로 등록하고자 한다고 가정하자.<br/>
      이걸 values에 등록할 수도 있지만 버튼의 상태에 따른 색상을 하나의 XML에 등록해 적용하면 더 편리하다.<br/>
      이때 color 디렉토리를 사용한다.<br/>

      ```xml
      <?xml version="1.0" encoding="utf-8"?>
      <selector xmlns:android="http://schemas.android.com/apk/res/android">
          <item android:state_pressed="true"
              android:color="#ffff0000"/>
          <item android:state_focused="true"
              android:color="#ff0000ff"/>
          <item android:color="#ff000000"/>
      </selector>
      ```

      위 코드는 selector 태그 아래에 item 태그를 이용해 이 리소스가 적용될 뷰의 상태와
      그 상태일 때의 색상을 지정했다.<br/>

   6. font 디렉터리<br/>
      글꼴 리소스를 저장하고 TTF나 OTF파일을 저장한 후 글꼴을 적용할 뷰에서 이용하면 된다.<br/>

2. **플렛폼 리소스 사용하기**<br/>
   앱 개발자가 res 디렉터리에 추가하는 것은 앱 리소스이다.<br/>
   개발자가 리소스를 따로 준비하지 않아도 이미 안드로이드 플랫폼이 제공하는 많은 리소스가 있음.<br/>
   앱을 개발할 때 이러한 플랫폼 리소스를 활용할 수 있다.<br/><br/>

   플랫폼 리소스는 안드로이드 스튜디오의 프로젝트 탐색 창에서 보기 옵션을 [Packages]로 설정한 후
   [Libraries] 항목을 보면 확인할 수 있다.<br/><br/>

   플랫폼의 라이브러리를 보면 anim, color, drawable 등 많은 리소스 디렉터리가 있는데, 이곳에
   있는 리소스가 플렛폼 리소스이다.<br/>
   플랫폼 리소스도 R 파일에 등록된 식별자로 이용할 수 있는데, 플랫폼 리소스는 앱에 있는 리소스가 아니므로
   앱의 R 파일이 아니라 android.R이라는 플랫폼 라이브러리의 R 파일에 등록되어 있다.<br/>
   따라서 android.R 파일을 이용해 플랫폼 리소스를 이용할 수 있다.<br/>

   ```kotlin
   binding.imageView.setImageDrawable(ResourceCompat.getDrawable(resources,
        android.R.drawable.alert_dark_frame, null))
    binding.textView.text=getString(android.R.string.emptyPhoneNumber)
   ```

   XML에서 앱 리소스는 @기호로 R 파일의 리소스를 이용했다.<br/>
   XML에서 drawable의 save.png 파일을 이용한다면 @drawable/save처럼 작성한다.<br/>
   그런데 플랫폼 리소스를 XML에서 이용할 때는 @android: 패턴으로 한다.<br/>

## 2. 리소스 조건 설정

1. **리소스 조건 설정이란**<br/>
   리소스 조건 설정이란 어떤 리로스를 특정 환경에서만 적용되도록 설정하는 것이다.<br/>
