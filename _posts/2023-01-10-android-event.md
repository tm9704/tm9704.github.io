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
