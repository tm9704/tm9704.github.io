---
title: "Android(8) - 레이아웃(1)"
excerpt: "Android Kotlin Layout(1)"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-09 15:35:00"
last_modified_at: "2023-01-09 16:35:00"
---

## 1. LinearLayout

1. **LinearLayout 배치 규칙**<br/>
   LinearLayout은 뷰를 가로나 세로 방향으로 나열하는 레이아웃 클래스이다.<br/>
   orientation이라는 속성에 horizontal이나 vertical값으로 방향을 지정한다.<br/>

   ```xml
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical">
       <Button
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="BUTTON1"/>
       <Button
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="BUTTON2"/>
   </LinearLayout>
   ```

   orientation을 vertical로 하면 버튼들이 세로로, horizontal로 지정하면 가로로 배치된다.<br/>
   LinearLayout은 방향만 설정하면 뷰를 추가한 순서대로 나열한다.<br/>
   화면에서 벗어나도 줄을 자동으로 바꾸지는 않는다.<br/><br/>

   LinearLayout도 중첩이 가능한데, 예를 들면 가로 방향으로 배치하는 LinearLayout에 이미지 뷰와 텍스트 뷰뿐만 아니라
   세로 방향으로 배치하는 LinearLayout을 추가할 수 있다.<br/>

   ```xml
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical">
       <Button
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="BUTTON1"/>
       <Button
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="BUTTON2"/>
       <LinearLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="horizontal">
            ...
        </LinearLayout>
   </LinearLayout>
   ```

2. **layout_weight 속성**<br/>
   화면에 뷰를 배치하다 보면 가로나 세로 방향으로 여백이 생길 수 있는데,
   이 여백을 뷰로 채울 수 있다.<br/>

   1. 뷰 1개로 전체 여백 채우기<br/>
      여백을 뷰로 채우려면 layout_weight 속성을 사용한다.<br/>
      따로 수치를 계산하지 않아도 각 뷰에 설정한 가중치로 여백을 채울 수 있다.<br/>
      ```xml
      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:orientation="horizontal">
          <Button
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="BUTTON1"
              android:layout_weight="1"/>
          <Button
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="BUTTON2"/>
      </LinearLayout>
      ```
      모든 여백을 채우는 걸 볼 수 이다.<br/>
   2. 뷰 여러 개로 여백을 나누어 채우기<br/>
      ```xml
      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="BUTTON1"
            android:layout_weight="1"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="BUTTON2"
            android:layout_weight="3"/>
      </LinearLayout>
      ```
      layout_weight 속성에 지정한 숫자는 가중치를 나타낸다. 즉 뷰가 차지하는 여백의
      비율을 의미한다.<br/>
      위의 예제같은 경우 BUTTON1이 1/4, BUTTON2가 3/4를 차지한다.<br/>
   3. 중첩된 레이아웃에서 여백 채우기<br/>
      ```xml
      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:orientation="vertical">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">
            <Button
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="BUTTON1"
                android:layout_weight="1"/>
            <Button
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="BUTTON2"
                android:layout_weight="3"/>
        </LinearLayout>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="BUTTON3"
            android:layout_weight="1"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="BUTTON4"/>
      </LinearLayout>
      ```
      위의 예제를 보면 LinearLayout을 중첩해서 화면을 구성하고 각 버튼에 layout_weight값을 1,3,1로 지정했다.<br/>
      결과화면을 보게 되면, BUTTON1과 BUTTON2에 설정한 가중치와 BUTTON3에 설정한 가중치가 서로 영향을 미치지 않음.<br/>
      즉 layout_weight 속성은 같은 영역에 있는 뷰끼리만 여백을 나누어 차지함.<br/>
      layout_weight는 자신이 속한 레이아웃의 여백을 대상으로 작용한다.<br/>
   4. 여백 채우기로 뷰의 크기 설정하기<br/>
      layout_weight는 여백을 채우는 속성이지만, 뷰의 크기를 0으로 하고 layout_weight값만 설정하기도 한다.<br/>
      ```xml
      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:orientation="vertical">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="0"
            android:text="BUTTON1"
            android:layout_weight="1"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="0"
            android:text="BUTTON2"
            android:layout_weight="1"/>
      </LinearLayout>
      ```
      위의 경우 layout_height가 다 0이지만 layout_weight값이 둘 다 1이므로 각각 50%씩 비율을 차지한다.<br/>

3. **gravity, layout_gravity**<br/>
   뷰를 정렬할 때는 gravity, layout_gravity 속성을 사용한다.<br/>
   이 속성들을 사용하지 않을 때 기본 정렬은 left/top이다.<br/>

   1. 뷰에 gravity와 layout_gravity 속성 적용하기<br/>

      ```xml
      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <TextView
            android:layout_width="150dp"
            android:layout_height="150dp"
            android:background="#FF0000"
            android:textSize="15dp"
            android:textStyle="bold"
            android:textColor="#FFFFFF"
            android:text="HelloWorld"
            android:gravity="right|bottom"
            android:layout_gravity="center_horizontal"/>
      </LinearLayout>
      ```

      gravity 속성의 정렬 대상은 콘텐츠이고 layout_gravity는 뷰 자체를 정렬하는 속성이다.<br/>

   2. 레이아웃에 gravity 속성 적용하기
      위 코드에서 layout_gravity속성을 center_vertical 값으로 지정하면 정렬이 적용되지 않는다.<br/>
      LinearLayout 자체가 방향으로 뷰를 배치하는 레이아웃이므로 Linearlayout의 orientation 속성에
      설정한 방향과 같은 방향으로는 layout_gravity속성이 적용되지 않는다.<br/>
      뷰들을 화면 가운데에 표시하고 싶으면 레이아웃에 gravity="center"로 지정하면 된다.<br/>
