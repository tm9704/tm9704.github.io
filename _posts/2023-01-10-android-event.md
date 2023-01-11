---
title: "Android(9) - 이벤트 처리(1)"
excerpt: "Android Event(1)"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-10 09:23:00"
last_modified_at: "2023-01-10 10:35:00"
---

## 1. 터치와 키 이벤트

1. **터치 이벤트**<br/>
   앱의 화면에서 발생하는 사용자 이벤트는 **터치**이다.<br/>
   터치란 손가락으로 화면을 눌렀다가 떼는 행위인데, 앱에서는 사용자가 화면을 눌렀는지
   손가락을 뗐는지 스와이프 했는지에 따라 알맞게 동작하도록 구현해야한다.<br/><br/>

   이런 사용자 터치 이벤트를 처리하고 싶으면 액티비티 클래스에 터치 이벤트의 콜백 함수인 onTouchEvent()를
   선언하면 된다.<br/>
   콜백 함수란 어떤 이벤트가 발생하거나 시점에 도달했을 때 시스템에서 자동으로 호출하는 함수를 말한다.<br/>

   ```kotlin
   class MainActivity : AppCompatActivity() {
       ...
       override fun onTouchEvent(event: MotionEvent?): Boolean {
           return super.onTouchEvent(event)
       }
   }
   ```

   액티비티에 onTonchEvent() 함수를 재정의해서 선언만 해놓으면 사용자가 이 액티비티 화면을 터치하는 순간
   onTouchEvent()함수가 자동으로 호출된다.<br/>
   이 함수에 전달되는 매개변수는 MotionEvent 객체이고 이 객체에 터치의 종류와 발생 지점
   (좌푯값)이 담긴다.<br/>

   1. 터치 이벤트 종류<br/>

      1. ACTION_DOWN: 화면을 손가락으로 누른 순간의 이벤트
      2. ACTION_UP: 화면에서 손가락을 떼는 순간의 이벤트
      3. ACTION_MOVE: 화면을 손가락으로 누른 채로 이동하는 순간의 이벤트<br/>

      ```kotlin
      override fun onTouchEvent(event: MotionEvnet?): Boolean{
        when(event?.action){
            MotionEvent.ACTION_DOWN -> {
                Log.d("kkang", "Touch down event")
            }
            MotionEvent.ACTION_UP -> {
                Log.d("kkang", "Touch up event")
            }
        }
        return super.onTouchEvent(event)
      }
      ```

   2. 터치 이벤트 발생 좌표 얻기<br/>
      터치 이벤트를 처리할 때에는 이벤티의 종류뿐만 아니라 이벤트가 발생한 지점을 알아야 하는 경우도 있다.<br/>
      이 좌표도 onTouchEvent() 함수의 매개변수인 MotioinEvent 객체로 얻는다.<br/>

      1. x: 이벤트가 발생한 뷰의 X좌표
      2. y: 이벤트가 발생한 뷰의 Y좌표
      3. rawX: 화면의 X 좌표
      4. rawY: 화면의 Y 좌표

      ```kotlin
      override fun onTouchEvent(event: MotionEvent?): Boolean {
          when(event?.action){
              MotionEvent.ACTION_DOWN -> {
                  Log.d("kkang",
                      "Touch down event x: ${event.x}, rawX: ${event.rawX}")
              }
          }
          return super.onTouchEvent(event)
      }
      ```

      x는 터치 이벤트가 발생한 뷰에서의 좌표값, rawX는 스크린, 즉 화면에서의 좌표값이다.<br/>

2. **키 이벤트**<br/>
   키 이벤트는 사용자가 폰의 키를 누르는 순간에 발생한다.<br/>
   액티비티에서 키 이벤트를 처리하기 위해서는 콜백함수를 재정의해야 하는데 종류로는<br/>

   1. onKeyDown: 키를 누르는 순간의 이벤트
   2. onKeyUp: 키를 떼는 순간의 이벤트
   3. onKeyLongPress: 키를 오래 누르는 순간의 이벤트<br/>

   ```kotlin
   class MainActivity2 : AppCompatActivity() {
    ...
    override fun onKeyDown(keyCode: Int, event: KeyEvent?): Boolean {
        Log.d("kkang", "onKeyDown")
        return super.onKeyDown(keyCode, event)
    }
    override fun onKeyUp(keyCode: Int, event: KeyEvent?): Boolean {
        Log.d("kkang", "onKeyUp")
        return super.onKeyUp(keyCode, event)
    }
   }
   ```

   키 이벤트 함수의 첫 번째 매개변수는 키의 코드이며 이 값으로 사용자가 어떤 키를 눌렀는지 식별할 수 있다.<br/>

   ```kotlin
   override fun onKeyDown(keyCode: Int, event: KeyEvent?): Boolean {
    when(keyCode){
        KeyEvent.KEYCODE_0 -> Log.d("kkang", "0")
        KeyEvent.KEYCODE_A -> Log.d("kkang", "A")
    }
    return super.onKeyDown(keyCode, event)
   }
   ```

   키 이벤트가 발생하는 키는 폰에서 제공하는 소프트 키보드의 키를 의미하는 것은 아니다.<br/>
   소프트 키보드의 키는 키 이벤트로 처리할 수 없다으며 액티비티에 onKeyDown() 등의 함수를 선언해
   놓더라도 사용자가 소프트 키보드의 키를 눌렀을 때 이벤트 함수가 호출되지 않는다.<br/><br/>

   이런 키 이벤트들은 하드웨어 키보드의 키를 입력하면 키 이벤트로 처리할 수 있으며, 안드로이드 시스템
   버튼도 키로 취급하므로 이 버튼의 이벤트를 처리하는 데도 사용된다.<br/>
   안드로이드 시스템 버튼의 종류로는 보통 전원 버튼, 볼륨 조절 버튼, 기기 아래쪽에
   뒤로가기, 홈, 오버뷰 버튼이 있다.<br/>
   이 중에서 뒤로가기, 볼륨 조절은 키로 취급해 키 이벤트로 처리가 가능하지만 전원, 홈, 오버뷰 버튼은
   액티비티에 onKeyDown() 함수를 선언해 놓아도 사용자가 버튼을 눌렀을 때 호출되지 않는다.<br/>
   즉, 앱에서 이벤트를 처리할 수 없는 버튼이다.<br/><br/>

   제스처 내비게이션을 사용하고 있다면 사용자 제스처로 뒤로 가기를 하면 <뒤로가기>버튼을 누른 것과 같다.<br/>
   사용자 제스처 뒤로 가기도 onKeyDown() 함수를 선언해 이벤트 처리가 가능하다.<br/>

   ```kotlin
   override fun onKeyDown(keyCode: Int, event: KeyEvent?): Boolean {
    when(keyCode){
        KeyEvent.KEYCODE_BACK -> Log.d("kkang", "Back")
        KeyEvent.KEYCODE_VOLUME_UP -> Log.d("kkang", "Volume")
        KeyEvent.KEYCODE_VOLUME_DOWN -> Log.d("kkang", "Volume")
    }
   }
   ```

   뒤로가기 버튼 이벤트는 onBackPressed() 함수를 이용할 수도 있다.<br/>

   ```kotlin
   override fun onBackPressed() {
    Log.d("kkang", "Back")
   }
   ```

## 2. 뷰 이벤트

액티비티의 화면은 TextView, EditText, ImageView, Button등의 뷰로 화면을 구성하고 구현한다.<br/>
이런 뷰를 사용자가 터치했을 때 이벤트 처리는 앞에서 본 터치 이벤트를 사용하지 않는다.<br/>
각 뷰에서 이벤트를 별도로 제공함.<br/>
(뷰의 이벤트를 터치 이벤트로 처리할 경우 프로그래밍이 복잡해질 수 있다.)<br/>

1. **뷰의 이벤트 처리 구조**<br/>
   뷰 이벤트는 일정한 구조에 따라 처리된다.<br/>
   앞에서 본 터치 이벤트의 경우 이벤트 콜백 함수인 onTouchEvent()만 액티비티에 선언해 놓으면 처리할 수 있다.<br/>
   키 이벤트 역시 콜백함수인 onKeyDown()만 액티비티에 선언해 놓아도 이벤트를 처리할 수 있다.<br/>
   그러나 뷰 이벤트는 이벤트 콜백 함수만 선언해서는 처리할 수 없다.<br/><br/>

   뷰 이벤트 처리는 **이벤트 소스**와 **이벤트 핸들러**로 역할이 나뉘고 이 둘을 **리스너**로 연결해야
   이벤트를 처리할 수 있다.<br/>

   1. 이벤트 소스: 이벤트가 발생한 객체
   2. 이벤트 핸들러: 이벤트 발생 시 실행할 로직이 구현된 객체
   3. 리스너: 이벤트 소스와 이벤트 핸들러를 연결해주는 함수<br/>

   즉, 이벤트 소스에 리스너로 이벤트 핸들러를 등록해 놓으면 이벤트가 발생할 때 실행되는 구조이다.<br/>

   ```kotlin
   binding.checkbox.setOnCheckedChangeListener(obejct: CompoundBotton.OnCheckedChangeListener){
        override fun onCheckedChanged(p0: CompoundButton?, p1: Boolean) {
            Log.d("kkang", "체크박스 클릭")
        }
   })
   ```

   위 코드는 체크박스의 체크 상태가 변경될 때 이벤트 처리를 작성한 코드이다.<br/>
   checkbox 객체가 이벤트가 발생하는 이벤트 소스이고, 이벤트 처리 내용이 담긴 이벤트 핸들러는
   OnCheckedChangeListener 인터페이스를 구현한 객체이다.<br/><br/>

   대부분 이벤트 핸들러는 이름 형식이 OnXXXListener인 인터페이스를 구현해서 만든다.<br/>
   대표적으로 OnClickListener, OnLongClickListener, OnItemClickListener 등의 인터페이스를 제공한다.<br/><br/>

   위 코드에서는 checkbox가 이벤트 소스, setOnCheckedListener가 리스너, OnCheckedChangeListener가 이벤트 핸들러이다.<br/>
   인터페이스를 구현한 object 클래스를 이벤트 핸들러로 만들었지만,<br/>
   액티비티 자체에서 인터페이스를 구현할 수도 있고, 이벤트 핸들러를 별도의 클래스로 만들어 처리할 수도 있고,
   코틀린의 SAM기법을 이용할 수도 있음.<br/>
   (자바 인터페이스를 간단하게 사용하기 위해 제공하는 기법)<br/>

   ```kotlin
   class MainActivity : AppCompatActivity(), CompoundButton.OnCheckedChangeListener {
       override fun onCreate(savedInstatnceState: Bundle?) {
           super.onCreate(savedInstatnceState)
           val binding = ActivityMain3Binding.inflate(layoutInflater)
           setContentView(binding.root)
           binding.checkbox.setOnCheckedChangeListener(this)
       }
       override fun onCheckedChanged(p0: CompoundButton?, p1: Boolean){
           Log.d("kkang", "체크박스 클릭")
       }
   }
   ```

   액티비티 인터페이스를 구현한 예시<br/>

   ```kotlin
    class MyEventHandler : CompoundButton.OnCheckedChangeListener {
        override fun onCheckedChanged(p0: CompoundButton?, p1: Boolean){
            Log.d("kkang", "체크박스 클릭")
        }
    }

   class MainActivity : AppCompatActivity() {
       override fun onCreate(savedInstatnceState: Bundle?) {
           super.onCreate(savedInstatnceState)
           val binding = ActivityMain3Binding.inflate(layoutInflater)
           setContentView(binding.root)

           binding.checkbox.setOnCheckedChangeListener(MyEventHandler())
       }
   }
   ```

   이벤트 핸들러를 별도의 클래스로 만든 예시<br/>

   ```kotlin
   class MainActivity : AppCompatActivity() {
       override fun onCreate(savedInstatnceState: Bundle?) {
           super.onCreate(savedInstatnceState)
           val binding = ActivityMain3Binding.inflate(layoutInflater)
           setContentView(binding.root)

           binding.checkbox.setOnCheckedChangeListener{
            compoundButton, b ->
            Log.d("kkang", "체크박스 클릭")
           }
       }
   }
   ```

   SAM 기법으로 구현한 예시<br/>

2. **클릭과 롱클릭 이벤트 처리**
   안드로이드는 다양한 뷰를 제공하지만, 이벤트 처리 구조는 동일하다.<br/>
   이벤트 소스와 이벤트 핸들러를 리스너로 연결하는 구조만 이해하면 어떤 뷰 이벤트라도
   어렵지 않게 처리가 가능하다.<br/><br/>

   대표적으로 ClickEvent, LongClickEvent를 보겠음.<br/>
   두 이벤트는 뷰의 최상위 클래스인 View에 정의된 이벤트이다.<br/>

   1. open fun setOnClickListener(l: View.OnClickListener?): Unit
   2. open fun setOnLongClickListener(l: View.OnLongClickListener?): Unit<br/>
      ClickEvent는 OnClickListener를 구현한 객체를 이벤트 핸들러로 등록해야 하고,<br/>
      LongClickEvent는 OnLongClickListener를 구현한 객체를 이벤트 핸들러로 등록해야한다.<br/>

   ```kotlin
   binding.button.setOnClickListener {
       Log.d("kkang", "클릭 이벤트")
   }

   binding.button.setOnLongClickListener{
       Log.d("kkang", "롤클릭 이벤트")
       true
   }
   ```

   위 코드는 위에서 설명한 뷰 이벤트의 처리 구조를 보면 이벤트 핸들러는 setOnClickListener() 함수로
   등록하고 여기에 OnClickListener 인터페이스를 구현한 객체를 지정해야 하는데, 위 코드는 그냥 setOnClickListener에
   람다 함수를 매개변수로 지정한 것처럼 보인다.<br/>
   예를 들어 이벤트 핸들러를 자바로 작성한다면,<br/>

   ```java
   binding.btn.setOnClickListener(new View.OnClickListener()){
       @Override
       public void onClick(View view){
       }
   };
   ```

   위 코드를 코틀린으로 바꾸면,<br/>

   ```kotlin
   binding.btn.setOnClickListener(object: View.OnClickListener){
       override fun onClick(p0: View?) {
       }
   })
   ```

   여기서 코틀린의 SAM 기법을 이용하면 위 코드를 조금 더 간단하게 작성이 가능하다.<br/>
   SAM은 자바 API를 코틀린에서 활용할 때 람다 표현식으로 쉽게 이용할 수 있게 해주는 기법.<br/>
   SAM은 하나의 추상 함수를 포함하는 인터페이스를 활용하는 방법이다.<br/><br/>

   인터페이스가 자바에 작성되어 있고 그 인터페이스를 등록하는 세터함수도 자바에 작성되어 있으면
   코틀린에서 세터 함수를 이용해 인터페이스를 구현한 객체를 등록할 때 람다 함수로 쉽게 작성할 수 있다.<br/>
   아래 처럼 자바에 선언된 인터페이스가 있다고 가정.<br/>

   ```java
   public interface JavaInterface1 {
       void callback();
   }
   ```

   그리고 이 인터페이스 타입의 객체를 등록하는 함수도 다음처럼 자바에 선언되어 있다고 가정.<br/>

   ```java
   public class SAMTest{
       JavaInterface1 callback;
       public void setInterface(JavaInterface1 callback){
           this.callback = callback;
       }
   }
   ```

   이처럼 자바 함수인 setInterface()를 코틀린에서 이용하려면 인터페이스를 구현한 객체를 매개변수로
   지정해야한다.<br/>

   ```kotlin
   obj.setInterface(object: JavaInterface1 {
       override fun callback() {
           println("hello kotlin")
       }
   })
   ```

   위 코드를 SAM 기법을 이용하면,<br/>

   ```kotlin
   obj.setInterface{println("hello SAM")}
   ```

   추상 함수 하나를 포함하는 인터페이스만 SAM 기법으로 이용할 수 있고, 코틀린에서는 이러한 SAM
   기법을 이용해 많은 이벤트 핸들러 등록 코드를 다음처럼 작성한다.<br/>

   ```kotlin
   binding.btn.setOnClickListener {
    ...
   }
   ```
