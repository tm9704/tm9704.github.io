---
title: "Android(6) - 뷰를 이용한 화면 구성(2)"
excerpt: "Android Kotlin View(2)"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-08 10:30:00"
last_modified_at: "2023-01-08 11:35:00"
---

## 1. 기본적인 뷰 살펴보기

1. **텍스트 뷰**<br/>
   TextView는 문자열을 화면에 출력하는 뷰이다.<br/>
   자주 사용하는 속성으로는<br/>
   1. android:text<br/>
      TextView에 출력할 문자열을 지정하는 속성.<br/>
      직접 문자열을 대입해도 되고, android:text="@string/hello"처럼 문자열 리소스를 지정해도 된다.<br/>
   2. android:textColor<br/>
      문자열의 색상을 지정하는 속성.<br/>
      android:textColor="#FF0000"처럼 16진수 RGB형식을 사용한다.<br/>
   3. android:textSize<br/>
      문자열의 크기를 지정하는 속성.<br/>
      android:textSize="20sp"처럼 값은 숫자를 사용하고 단위는 생략이 불가능하다.<br/>
      단위는 px, sp, dp등을 사용한다.<br/>
   4. android:textStyle<br/>
      문자열의 스타일을 지정한다.<br/>
      android:textStyle="bold"처럼 사용하고, 값으로는 bold, italic, normal중 선택.<br/>
   5. android:autoLink<br/>
      autoLink 속성은 TextView에 출력할 문자열을 분석해 특정 형태의 문자열에 자동 링크를 추가해준다.<br/>
      만약 android:autoLink="web"으로 설정하면 문자열에 웹 주소가 포함됐을 때 해당 문자열을 링크 모양으로 표시한다.<br/>
      속성값으로 web, phone, email등을 사용할 수 있고 여러 개를 함께 설정하려면 | 기호로 연결한다.<br/>
      ```xml
      <TextView
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:text="웹페이지 : https://github.com/tm9704"
          andorid:autoLink="web"/>
      ```
   6. android:maxLines<br/>
      TextView에 문자열을 출력할 때 긴 문자열은 자동으로 줄바꿈을 한다.<br/>
      그런데 특정 줄 까지만 나오도록 해야 하는 경우가 있는데 이 때 쓰는게 maxLines.<br/>
      android:maxLines="3"으로 설정하면 문자열이 3행까지만 출력된다.<br/>
   7. android:ellipsize<br/>
      maxLines 속성을 이용할 때 출력되지 않은 문자열이 더 있다는 것을 표시하려면 줄임표(...)를 넣는다.<br/>
      이때 ellipsize 속성을 사용한다. 속성 값으로는 end, middle, start등을 설정한다.<br/>
      end로 설정시 문자열 뒤에 줄임표가, start와 middle은 줄임표가 각각 앞, 중간 부분에 추가되는데
      end와 달리 singleLine="true"속성으로 문자열을 한 줄로 출력했을 때만 적용된다.<br/>
      ```xml
      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:orientation="vertical">
          <TextView
              android:layout_width="match_parent"
              android:layotu_width="wrap_content"
              android:text="@string/long_text"
              android:singleLine="true"
              android:ellipsize="middle"/>
          <View
              android:layout_width="match_parent"
              android:layout_height="2dp"
              android:background="#000000"/>
          <TextView
              android:layout_width="match_parent"
              android:layotu_width="wrap_content"
              android:text="@string/long_text"
              android:maxLines="3"
              android:ellipsize="end"/>
      </LinearLayout>
      ```
2. **이미지 뷰**<br/>
   ImageView는 이미지를 화면에 출력하는 뷰.<br/>
   자주 사용하는 속성으로는<br/>
   1. android:src<br/>
      ImageView에 출력할 이미지를 설정하는 속성.<br/>
      ImageView에는 리소스 이미지, 파일 이미지, 네트워크 이미지 등을 출력할 수 있다.<br/>
      android:src="@drawable/images3"처럼 리소스 이미지를 설정할 수 있음.<br/>
   2. android:maxWidth, android:maxHeight, android:adjustViewBounds<br/>
      ImageView가 출력하는 이미지의 최대 크기를 지정하는 속성.<br/>
      뷰의 크기는 layout_width, layout_height 속성으로 설정하지만 이 속승은 크기가 고정되어
      있어서 뷰에 넣을 때 이미지 크기가 다양하다면 이미지와 뷰의 크기가 맞지 않는 상황이 발생할 수 있다.<br/>
      또한 이미지가 클 때 layout_width, layout_height의 속성값을 wrap_content로 지정하면 뷰의 크기가 지나치게
      커지는 문제가 있다.<br/>
      ```xml
      <Imageview
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:background="#ff0000"
          android:src="@drawable/test"/>
      ```
      위 처럼 지정하게 되면 이미지가 화면에 출력되지만, 뷰가 과도하게 크게 출력될 수 있음.<br/>
      이렇게 되면 다른 뷰들과 함께 하면을 구성하기 힘들 수 있다.<br/>
      여기서 maxWidth이런거 쓰는거임.<br/>
      maxWidth, maxHeight 속성은 android:adjustViewBounds 속성과 함께 사용해야 하고
      이 속성을 true로 설정하면 이미지의 가로세로 길이와 비례해 뷰의 크기를 맞춘다.<br/>
      ```xml
      <Imageview
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:maxWidth="100dp"
          android:maxHeight="100dp"
          android:adjustViewBounds="true"
          android:background="#ff0000"
          android:src="@drawable/test"/>
      ```
      maxWidth, maxHeight에 설정한 값 만큼 이미지가 작게 출력되고, adjustViewBounds 속성으로 가로세로 비율도 유지된다.<br/>
      뷰자체의 크기도 maxWidth, maxHeight에 설정한 값으로 지정되어서 뷰 자체의 크기도 작아짐.<br/>
3. **버튼, 체크박스, 라디오버튼**<br/>
   Button은 사용자 이벤트를 처리하고 CheckBox는 다중 선택을, RadioButton은 단일 선택을 제공하는 뷰.<br/>
   체크박스는 한 화면에 여러 개가 나오더라도 다중 선택을 제공하므로 서로 영향을 미치지 않는다.<br/>
   라디오 버튼의 경우 화면에 여러 개가 나오면 하나만 선택할 수 있는 단일 선택이므로 여러 개를 묶어서 처리해야 한다.<br/>
   즉, 라디오 버튼은 RadioGroup과 함께 사용하고 그룹으로 묶은 라디오 버튼 중 하나만 선택이 가능하다.<br/>
   ```xml
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         android:orientation="vertical">
         <Button
            android:layout_width="wrap_cotent"
            android:layout_height="wrap_cotent"
            android:text="BUTTON1"/>
         <CheckBox
             android:layout_width="wrap_cotent"
             android:layout_height="wrap_cotent"
             android:text="check1"/>
         <CheckBox
             android:layout_width="wrap_cotent"
             android:layout_height="wrap_cotent"
             android:text="check2"/>
        <RadioGroup
            android:layout_widht="wrap_content"
            android:layout_height="wrap_content">
            <RadioButton
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="radio1"/>
            <RadioButton
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="radio2"/>
        </RadioGroup>
    </LinearLayout>
   ```
