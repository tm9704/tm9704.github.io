---
title: "Android(5) - 뷰를 이용한 화면 구성(1)"
excerpt: "Android Kotlin View(1)"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-05 10:30:00"
last_modified_at: "2023-01-05 11:35:00"
---

## 1. 화면을 구성하는 방법

1. **액티비티 - 뷰 구조**<br/>
   안드로이드 앱의 기본 구조는 컴포넌트를 기반으로 한다.<br/>
   컴포넌트는 액티비티, 브로드캐스트리시버, 콘텐츠 프로바이더가 있고, 이 중에서 액티비티만
   화면을 출력하는 컴포넌트는 액티비티 뿐이다.<br/><br/>

   액티비티는 화면을 출력하는 컴포넌트일 뿐이지 그 자체가 화면은 아니다.<br/>
   화면에 내용을 표시하려면 뷰(View) 클래스를 이용해 구성해야 한다.<br/>

2. **액티비티 코드로 화면 구성하기**<br/>
   액티비티에서 뷰로 화면을 구성하는 방법은 2가지이다.<br/>

   1. 액티비티 코드로 작성하는 방법<br/>
   2. 레이아웃 XML 파일로 작성하는 방법<br/>
      액티비티 코드로 작성하는 방법은 화면을 구성하는 뷰 클래스를 액티비티 코드에서 직접 생성한다.<br/>

   ```kotlin
   import android.graphics.Typeface
   import androidx.appcompat.app.AppComatActivity
   import android.os.Bundle
   import android.view.Gravity
   import android.view.ViewGroup.LayoutParams.WRAP_CONTENT
   import android.widget.ImageView
   import android.widget.LinearLayout
   import androidx.core.content.ContextCompat

   class MainActivity : AppCompatActivity() {
     override fun onCreate(savedInstanceState: Bundle?){
       super.onCreate(savedInstanceState)
       //이름 문자열 출력 TextView 생성
       val name = TextView(this).apply {
         typeface = Typeface.DEFAULT_BOLD
         text = "Lake Louise"
       }
       //이미지 출력 ImageView 생성
       val image = ImageView(this).also{
         it.setImageDrawable(ContextCompat.getDrawable(this, R.drawable.lake_1))
       }
       //주소 문자열 출력 TextView 생성
       val address = TextView(this).apply{
         typeface = Typeface.DEFAULT_BOLD
         text = "Lake Louise, AB, 캐나다"
       }
       val layout = LinearLayout(this).apply{
         orientation = LinearLayout.VERTICAL
         gravity = Gravity.CENTER
         //LinearLayout 객체에 TextView, ImageView, TextView 객체 추가
         addView(name, WRAP_CONTENT, WRAP_CONTENT)
         addView(image, WRAP_CONTENT, WRAP_CONTENT)
         addView(address, WRAP_CONTENT, WRAP_CONTENT)
       }
       //LinearLayout 객체를 화면에 출력
       setContentView(layout)
     }
   }
   ```

   위 코드에서는 필요한 뷰 객체를 코드로 직접 생성했고, 크기, 출력 데이터 등도
   일일이 객체에 대입했다. TextView 2개와 ImageView 1개를 추가한 LinearLayout
   객체를 액티비티 컴포넌트의 함수인 setContextView()로 전달해 화면을 출력했다.<br/><br/>

   코드에서 apply {}가 잇는데 apply 함수를 호출하는 구문이다.<br/>
   원래 함수 호출문은 apply() 처럼 소괄호를 사용해야 하는데, 호출하려는 함수가
   고차함수이고 마지막 전달인자가 람다 함수이면 소괄호를 생략해도 된다.<br/>

   ```kotlin
   val name = TextView(this).apply({})
   ```

   위는 apply 함수에 람다 함수를 전달한 코드이다. 이렇게 작성해도 괜찮지만,
   소괄호 생략도 가능하다.<br/><br/>

   매개변수로 람다 함수를 전달받는 모든 함수가 소괄호를 생략해도 되는건 아니다.<br/>

   ```kotlin
   fun some(arg1: () -> Unit){}
   ```

   ```kotlin
   some({println("hello")})
   some{println("hello")}
   ```

   위 두 코드는 매개변수가 1개이며, 람다 함수인 경우인데 아래 코드 처럼 소괄호 생략이 가능하다.<br/>

   ```kotlin
   fun some(arg1: Int, arg2: () -> Unit, arg3: () -> Unit){}
   ```

   매개변수가 3개인 함수이다.<br/>
   첫번째 매개변수는 Int, 두번째와 세번째는 함수 타입이다.<br/>

   ```kotlin
   some(10, {println("hello")}, {println("hello")}) //가능
   some(10, {println("hello")}) {println("hello")} //가능
   some 10, {println("hello")}, {println("hello")} //불가능
   some(10) {println("hello")} {println("hello")} //불가능
   ```

   위는 함수 호출 코드인데, 전체 인자를 소괄호로 묶거나 마지막 인자를 묶지 않은
   코드만 정상이다.<br/>
   즉 고차함수를 호출할 때 마지막 매개변수가 함수 타입이면 그에 전달한 람다 함수는 소괄호로 묶지 않아도 된다.<br/>

3. **레이아웃 XML로 화면 구성하기**<br/>

   ```xml
   <?xml version = "1.0" encoding = "utf-8"?>
   <LinearLayout xmlns:android = "https//schemas.android.com/apk/res/android"
     android: layout_width = "match_parent"
     android: layout_height = "match_parent"
     android: orientation = "vertical"
     android: gravity = "center">
     <TextView
       android: layout_width = "wrap_content"
       android: layout_height = "wrap_content"
       android: textStyle = "bold"
       android: text = "Lake Louise"/>
     <ImageView
       android: layout_width = "wrap_content"
       android: layout_height = "wrap_content"
       android: src = "@drawable/lake_1"/>
     <TextView
       android: layout_width = "wrap_content"
       android: layout_height = "wrap_content"
       android: textStyle = "bold"
       android: text = "Lake Louise, AB, 캐나다"/>
     >
   ```

   앞에서 액티비티 코드로 구현한 화면을 XML 파일로 구현한 예이다.<br/>
   이처럼 XML을 이용해 화면을 구현하면 액티비티 코드에 이 내용을 작성하지
   않아도 괜찮지만 코드에서 화면을 구현한 XML을 명시해 어떤 화면을 출력할지 알려줘야 한다.<br/>

   ```kotlin
   class MainActivity: AppCompatActivity() {
     override fun onCreate(savedInstanceState: Bundle?) {
       super.onCreate(savedInstanceState)
       //화면 출력 XML 명시
       setContentView(R.layout.activity_main)
     }
   }
   ```

## 2. 뷰 클래스

1.  **뷰 클래스의 기본 구조**<br/>
    안드로이드에서 화면을 만들어 표시하는 컴포넌트는 액티비티이며 액티비티가
    실행되면서 뷰 클래스를 이용해 화면을 구성한다.<br/>
    안드로이드에서는 많은 뷰 클래스를 제공하고 이 뷰 클래스들이 어떻게 설계
    되었는지 이해하면 여러 가지 뷰를 사용할 때 도움이 된다.<br/>

    1.  뷰 객체의 계층 구조<br/>
        액티비티 화면을 구성할 때 사용하는 클래스는 모두 View의 하위 클래스.<br/>
        화면 구성과 관련한 클래스를 통칭해서 **뷰 클래스**라고 한다.<br/>
        ![view](/images/view.png)<br/>
        위 그림은 뷰 클래스의 계층 구조를 나타낸다.<br/>

        1. View<br/>
           모든 뷰 클래스의 최상위 클래스.<br/>
           액티비티는 View의 서브 클래스만 화면에 출력한다.<br/>
        2. ViewGroup<br/>
           View의 하위 클래스지만 자체 UI는 없어서 화면에 출력해도 아무것도 나오지 않는다.<br/>
           일종의 그릇 역할을 하는 클래스로, 일반적으로 컨테이너 기능을 담당한다.<br/>
           실제로는 서브클래스인 레이아웃 클래스를 사용함.<br/>
        3. TextView<br/>
           특정 UI를 출력할 목적으로 사용하는 클래스, 그 중에서 문자열을 출력하는 뷰이다.<br/>
           이거 말고도 다양한 클래스가 존재함.<br/>

        ```xml
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:orientation="vertical">
        </LinearLayout>
        ```

        객체의 계층 구조에서 중요할 역할을 하는게 레이아웃 클래스.<br/>
        위처럼 코드를 작성해도 화면에는 아무것도 출력되지 않는다.<br/>
        ViewGroup클래스의 하위 클래스인 레이아웃 클래스는 다른 뷰 객체 여러 개를 담아서
        한꺼번에 제어할 목적으로 사용하기 때문에 레아이웃 클래스만 사용하면 아무 것도 출력되지 않는다.<br/>

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
            android:layout_widht="wrap_content"
            android:layout_height="wrap_content"
            android:text="BUTTON2"/>
        </LinearLayout>
        ```

        위처럼 버튼 객체를 여러 개 생성하고 레이아웃에 추가하면 레이아웃을 이용해 뷰 객체를 계층으로 묶어
        한꺼번에 출력하거나 정렬하는 등 편리하게 제어가 가능하다.<br/>

    2.  레이아웃 중첩<br/>
        뷰의 계층 구조는 레이아웃 객체를 중첩해서 복잡하게 구성할 수 있다.<br/>

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
          android:layout_widht="wrap_content"
          android:layout_height="wrap_content"
          android:text="BUTTON2"/>
        <LinearLayout
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:orientation="horizontal">
          <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="BUTTON3"/>
          <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="BUTTON4"/>
        </LinearLaytout>
      </LinearLayout>
    ```

    위 코드는 LinearLayout 객체에 또 다른 LinearLayout 객체를 포함하여 화면을 구성한 예제.<br/>
    이처럼 객체를 계층 구조로 만들어 이용하는 패턴을 **컴포지트 패턴** 또는 **문서 객체 모델** 이라고 한다.<br/>

2.  **레이아웃 XML의 뷰를 코드에서 사용하기**<br/>
    화면 구성을 레이아웃 XML 파일에 작성하고 액티비티에서 setContentView() 함수로 XML 파일을 지정하면 화면을 출력한다.<br/>
    때로는 XML에 선언한 객체를 코드에서 사용해야 할 때가 있는데, 이 때는 객체를 어떻게 부를 것인지 식별자를 부여하고
    그 식별자로 객체를 얻어와야 한다.<br/>
    이때 사용하는 속성이 id이다.<br/>
    id는 레이아웃 XML에 선언한 뷰를 구별할 필요가 없을때는 생략이 가능하다.<br/>

```xml
<TextView
  android:id="@+id/text1"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:text="hello"/>
```

id 속성은 android:id="@+id/text1" 형태로 추가한다.<br/>
여기서 text1이 id값이며 이 값은 식별자로 이용하기 때문에 앱에서 유일해야 한다.<br/>
이처럼 XML에 id 속성을 추가하면 자동으로 R.java 파일에 상수 변수로 추가된다.<br/><br/>

id 속성값은 "@+id/text1" 형태로 추가하는데 XML 속성값이 @로 시작하면 R.java 파일을 의미한다.<br/>
즉 이 표현식은 R.java 파일에 text1이라는 상수 변수를 추가하라는 의미.<br/>

```kotlin
//XML 화면 출력
setContentView(R.layout.activity_main)
//id값으로 뷰 객체 획득
val textView1: TextView = findViewById(R.id.text1)
```

setContentView()는 액티비티의 화면을 출력하는 함수이며 호출하는 것만으로도 XML의 내용이 액티비티 화면에 출력된다.<br/>
화면에 뷰의 내용이 나왔다는 것은 뷰 객체가 생성되었다는 의미이고 생성된 뷰 객체를 findViewById(R.id.text1)처럼 가져오면 된다.<br/>

```kotlin
val textView1: TextView = findViewById<TextView>(R.id.text1)
```

위 처럼 findViewById() 함수로 얻은 뷰 객체의 타입을 제네릭으로 명시해도 됨.<br/>

3. **뷰의 크기를 지정하는 방법**<br/>
   뷰의 레이아웃 XML에 등록하여 화면을 구성할 때 생략할 수 없는 속성이 바로 크기이다.<br/>
   layout_width, layout_height가 크기를 설정하는 속성임.<br/>

   ```xml
   <TextView
     android:id="@+id/text1"
     android:layout_weight="wrap_content"
     android:layout_height="wrap_content"
     andorid:text="hello"/>
   ```

   크기를 나타내는 속성값에는 3가지 중 하나를 선언한다.<br/>

   1. 수치
   2. match_parent
   3. wrap_content<br/>
      수치의 경우 100px 이런 식으로 사용한다. 즉, 직접적인 수치를 직접 정하며 px, dp등의 단위를 사용한다.<br/>
      match_parent는 부모 크기 전체를 의미한다.<br/>
      자신의 바로 상위 계층의 크기를 따라감.<br/>
      wrap_content는 자신의 콘텐츠를 화면에 출력할 수 있는 적절한 크기를 의미한다.<br/>
      TextView의 경우 문자열의 길이가 길어지거나 글꼴이 커지면 뷰 크기도 커진다.<br/>

   ```xml
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical"
       android:background="#ffff00">
       <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON1"
         android:backgroundTint="#00ffff"/>
       <Button
         android:layout_widht="match_parent"
         android:layout_height="wrap_content"
         android:text="BUTTON2"
         android:backgroundTint="#00ffff"/>
   </LinearLayout>
   ```

4. **뷰의 간격 설정**<br/>
   뷰의 간격은 margin과 padding 속성으로 설정한다.<br/>
   margin 속성은 뷰와 뷰 사이의 간격을, padding 속성은 뷰의 콘텐츠와 테두리 사이의 간격을 의미한다.<br/>
   그냥 margin과 padding속성을 사용한 경우 모든 방향에 적용되고, Left, Right등을 추가해 특정 방향에만 간격을 줄 수 있다.<br/>
   ```xml
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical">
       <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON1"
         android:backgroundTint="#00ffff"
         android:padding="30dp"/>
       <Button
         android:layout_widht="match_parent"
         android:layout_height="wrap_content"
         android:text="BUTTON2"
         android:backgroundTint="#00ffff"
         android:paddingBottom="50dp"
         android:layout_marginLeft="50dp"/>
   </LinearLayout>
   ```
5. **뷰의 표시 여부 설정**<br/>
   visibility 속성은 뷰가 화면에 출력되어야 하는지를 설정한다.<br/>
   값을 visible, invisible, gone으로 설정하며 기본값은 visible이다.<br/><br/>

   invisible과 gone은 모두 화면에 뷰가 보이지 않게 하지만, 자리를 차지하는 여부에 따라 차이가 있음.<br/>
   invisible은 뷰가 화면에 보이지 않지만 자리는 차지하고, gone으로 설정하면 자리조차 차지하지 않는다.<br/>

   ```xml
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
     android:layout_width="match_parent"
     android:layout_height="match_parent"
     android:orientation="horizontal">
     <LinearLayout
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON1"/>
      <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON2"/>
      <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON3"/>
    </LinearLayout>
    <LinearLayout
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON1"/>
      <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON2"
         android:visibility="invisible"/>
      <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON3"/>
    </LinearLayout>
    <LinearLayout
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON1"/>
      <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON2"
         android:visibility="gone"/>
      <Button
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:text="BUTTON3"/>
    </LinearLayout>
   </LinearLayout>
   ```

   만약 XML이 아닌 코드에서 뷰의 visibility 속성을 조정하려면 뷰의 visibility 속성값을 View.VISIBLE이나
   View.INVISIBLE로 설정하면 된다.<br/>

   ```kotlin
   visibleBtn.setOnClickListener{
    targetView.visibility = View.VISIBLE
   }
   invisibleBtn.setOnClickListener{
    targetView.visibility = View.INVISIBLE
   }
   ```

   코틀린의 변수는 자바와 다르게 필드가 아니라 프로퍼티이다.<br/>
   즉 변수 자체에 세터와 게터가 내장되어 있어서 그냥 변수처럼 이용해도 게터, 세터가 호출됨
