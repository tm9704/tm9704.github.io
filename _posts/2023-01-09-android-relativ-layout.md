---
title: "Android(8) - 레이아웃(2)"
excerpt: "Android Kotlin Layout(2)"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-09 17:00:00"
last_modified_at: "2023-01-09 17:35:00"
---

## 1. RelativLayout

1. **RelativeLayout 배치 규칙**<br/>
   RelativeLayout은 상대 뷰의 위치를 기준으로 정렬하는 레이아웃 클래스이다.<br/>
   즉, 화면에 이미 출력된 특정 뷰를 기준으로 방향을 지정하여 배치한다.<br/>
   이때 아래와 같은 속성을 사용하고 각 속성에 입력하는 값은 기준이 되는 뷰의 id이다.<br/>

   1. android:layout_above:기준 뷰의 위쪽에 배치
   2. android:layout_below:기준 뷰의 아래쪽에 배치
   3. android:layout_toLeftOf: 기준 뷰의 왼쪽에 배치
   4. android:layout_toRightOf: 기준 뷰의 오른쪽에 배치<br/>

   ```xml
   <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
      <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@mipmap/ic_launcher"/>
      <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="HELLO"/>
   </RelativeLayout>
   ```

   위의 코드 같은 경우 LinearLayout 처럼 자동으로 가로세로 방향으로 배치되지 않으므로
   이미지 뷰 위에 버튼이 겹쳐서 나온다.<br/>
   위의 예에서 버튼을 이미지 뷰 오른쪽에 배치하고 싶으면 android:layout_toRightOf 속성을 이용한다.<br/>

   ```xml
   <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
      <ImageView
        android:id="@+id/testImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@mipmap/ic_launcher"/>
      <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="HELLO"
        android:layout_toRightOf="@id/testImage"/>
   </RelativeLayout>
   ```

2. **align 속성**<br/>
   위의 예제의 결과를 보면 이미지의 세로 크기가 버튼보다 클 경우 버튼이 이미지 위쪽에 맞춰질 수 있음.<br/>
   상대 뷰의 어느쪽에 맞춰서 정렬할지 정하는 속성들이 있는데, 이 속성에 입력하는 값 역시 기준이 되는 뷰의 id.<br/>

   1. android:layout_alignTop: 기준 뷰와 위쪽을 맞춤
   2. android:layout_alignBottom: 기준 뷰와 아래쪽을 맞춤
   3. android:layout_alignLeft: 기준 뷰와 왼쪽을 맞춤
   4. android:layout_alignRight: 기준 뷰와 오른쪽을 맞춤
   5. android:layout_alignBaseLine: 기준 뷰와 텍스트 기준선을 맞춤<br/>

   ```xml
   <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <ImageView
         android:id="@+id/testImage"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:src="@mipmap/ic_launcher"/>
       <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="HELLO"
         android:layout_toRightOf="@id/testImage"
         android:layout_alignBottom="@id/testImage"/>
   </RelativeLayout>
   ```

   또한 상위 레이아웃을 기준으로 맞춤 정렬하는 속성도 있다. 뷰를 부모 영역의 오른쪽
   또는 아래쪽에 붙이고 싶을 때 사용하는 속성.<br/>

   1. android:layout_alignParentTop: 부모의 위쪽에 맞춤
   2. android:layout_alignParentBottom: 부모의 아래쪽에 맞춤
   3. android:layout_alignParentLeft: 부모의 왼쪽에 맞춤
   4. android:layout_alignParentRight: 부모의 오른쪽에 맞춤
   5. android:layout_centerHorizontal: 부모의 가로 방향 중앙에 맞춤
   6. android:layout_centerVertical: 부모의 세로 방향 중앙에 맞춤
   7. android:layout_centerInparent: 부모의 가로, 세로 중앙에 맞춤<br/>

   ```xml
   <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
     android:layout_width="match_parent"
     android:layout_height="match_parent">
     <ImageView
       android:id="@+id/testImage"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:src="@mipmap/ic_launcher"/>
     <Button
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="HELLO"
       android:layout_toRightOf="@id/testImage"
       android:layout_alignParentRight="true"/>
   </RelativeLayout>
   ```

## 2. FrameLayout

FrameLayout은 뷰를 겹쳐서 출력하는 레이아웃 클래스이다.<br/>
카드를 쌓아서 뷰를 추가한 순서대로 위에 겹쳐서 계속 출력하는 레이아웃.<br/>

```xml
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="match_parent">
  <Button
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="BUTTON1"/>
  <ImageView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@mipmap/ic_launcher"/>
</FrameLayout>
```

FrameLayout을 이용하면 버튼과 이미지가 겹쳐서 출력된다.<br/>
LinearLayout처럼 가로세로 방향으로 배치하지 않고, RelativeLayout처럼 상대 위치를 조절하는
속성도 없고 단순히 겹쳐서 출력하는 레이아웃이므로 특별한 속성도 없다.<br/><br/>

FrameLayout은 똑같은 위치에 여러 뷰를 겹쳐서 놓고, 어떤 순간에 하나의 뷰만 출력할 때 사용한다.<br/>
따라서 대부분 뷰의 표시 여부를 설정하는 visibility 속성을 함께 사용한다.<br/>

```xml
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="match_parent">
  <Button
    android:id="@+id/button"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="BUTTON1"/>
  <ImageView
    android:id="@+id/imageView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@mipmap/ic_launcher"
    android:visibility="invisible"
    android:clickable="true"/>
</FrameLayout>
```

처음에는 보이지 않지만 원하는 순간에 뷰의 visibility의 속성값을 바꾸어 조절 가능하다.<br/>

```kotlin
class MainActivity: AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?){
    super.onCreate(savedInstanceState)

    val binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.button.setOnClickListener {
      binding.button.visibility = View.INVISIBLE
      binding.imageView.visibility = View.VISIBLE
    }

    binding.imageView.setOnClickListener{
      binding.imageView.visibility = View.INVISIBLE
      binding.button.visibility = View.VISIBLE
    }
  }
}
```

## 3. GridLayout

1. **GridLayout 배치 규칙**<br/>
   GridLayout은 행과 열로 구성된 테이블 화면을 만드는 레이아웃 클래스이다.<br/>
   LinearLayout처럼 orientation 속성으로 가로세로 방향으로 뷰를 나열하는데,
   LinearLayout과는 다르게 줄바꿈을 자동으로 해준다.<br/>

   1. orientation: 방향설정
   2. rowCount: 세로로 나열할 뷰 개수
   3. columnCount: 가로로 나열할 뷰 개수<br/>

   rowCount, columncount 속성에 설정한 개수만큼 뷰를 추가하면 자동으로 줄바꿈해서
   다음 줄에 뷰를 출력한다.<br/>

   ```xml
   <GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
     android:layout_width="match_parent"
     android:layout_height="match_parent"
     android:orientation="horizontal"
     android:columnCount="3">
     <Button android:text="A"/>
     <Button android:text="B"/>
     <Button android:text="C"/>
     <Button android:text="D"/>
     <Button android:text="E"/>
   </GridLayout>
   ```

2. **GridLayout 속성**<br/>
   1. 특정 뷰의 위치 조정하기.<br/>
      GridLayout으로 뷰를 배치하면 추가한 순서대로 배치되는데,
      layout_row, layout_column 속성을 이용하면 특정 뷰의 위치를 조정할 수 있다.<br/>
      1. layout_row: 뷰가 위치하는 세로 방향 인덱스
      2. layout_column: 뷰가 위치하는 가로 방향 인덱스<br/>
      ```xml
      <GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:orientation="horizontal"
          android:columnCount="3">
          <Button android:text="A"/>
          <Button android:text="B"/>
          <Button android:text="C"
            android:layout_row="1"
            android:layout_column="1"/>
          <Button android:text="D"/>
          <Button android:text="E"/>
      </GridLayout>
      ```
   2. 특정 뷰의 크기 확장하기.<br/>
      특정 뷰의 크기를 확대하려면 layout_gravity속성을 사용한다.<br/>
      ```xml
      <GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:orientation="horizontal"
          android:columnCount="3">
          <Button android:text="A"/>
          <Button android:text="HELLOWORLD HELLOWORLD"/>
          <Button android:text="B"/>
          <Button android:text="C"/>
          <Button android:text="D"/>
          <Button android:text="E"
            android:layout_gravity="fill_horizontal"/>
          <Button android:text="F"/>
      </GridLayout>
      ```
      위의 경우 layout_gravity="fill_horizontal"으로 특정 뷰를 확장했음.<br/>
      여기서 공간이 충분하다면 여백에 다음 뷰를 넣어 한 칸에 뷰를 2개 표시할 수 있다.<br/>
      ```xml
      <GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:orientation="horizontal"
          android:columnCount="3">
          <Button android:text="A"/>
          <Button android:text="HELLOWORLD HELLOWORLD"/>
          <Button android:text="B"/>
          <Button android:text="C"/>
          <Button android:text="D"/>
          <Button android:text="E"/>
          <Button android:text="F"
            android:layout_row="1"
            android:layout_column="1"
            android:layout_gravity="right"/>
      </GridLayout>
      ```
   3. 열이나 행 병합하기<br/>
      어떤 뷰가 테이블에서 여러 칸을 차지하게 설정할 수 있다.<br/>
      이때 layout_columnSpan, layout_rowSpan 속성을 사용한다.<br/>
      1. layout_columnSpan: 가로로 열 병합
      2. layout_rowSpan: 세로로 행 병합<br/>
      ```xml
      <GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:orientation="horizontal"
          android:columnCount="3">
          <Button android:text="A"
            android:layout_columnSpan="2"
            android:layout_rowSpan="2"
            android:layout_gravity="fill"/>
          <Button android:text="B"/>
          <Button android:text="C"/>
          <Button android:text="D"/>
          <Button android:text="E"/>
          <Button android:text="F"/>
      </GridLayout>
      ```

## 3. ConstraintLayout

ConstraintLayout은 안드로이드 플랫폼이 아니라 androidx에서 제공하는 라이브러리이다.<br/>
