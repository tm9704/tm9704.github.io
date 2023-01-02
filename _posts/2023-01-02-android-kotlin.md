---
title: "Android(2) - 코틀린"
excerpt: "Android Kotlin"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-02 14:25:00"
last_modified_at: "2023-01-02 15:25:00"
---

## 1. 코틀린 언어

코틀린은 JetBrains에서 개발한 프로그래밍 언어이다.<br/>
자바의 가상머신인 JVM에 기반을 둔 언어라서 코틀린으로 작성한 프로그램은 JVM에서 실행이 가능.<br/><br/>

코틀린은 확장자가 .kt인데, 코틀린 컴파일러가 코틀린 파일을 컴파일하면 자바 바이트 코드가 만들어진다.<br/>
즉, 코틀린으로 코드를 작성하지만 컴파일하면 자바 클래스가 만들어지고 JVM이 이를 실행한다.<br/><br/>

코틀린의 이점은

1. 표현력과 간결함<br/>
   자바보다 코틀린의 로직이 훨씬 간결함
2. 안전한 코드<br/>
   코틀린은 널 안전성을 지원하는 언이이다.<br/>
   객체지향 프로그램잉에서는 객체는 널 상태일 수 있고, 이때 런타임 오류인 널 포인트 예외(NPE)가 발생할 수 있다.
   따라서 객체가 널인 상황을 고려해 개발해야 하는데, 코틀린에서는 변수를 널 허용과 널 불허용으로 구분해 선언한다.<br/>
   이렇게 널과 관련된 여러 부분을 컴파일러가 해결해줌.
3. 상호 운용성<br/>
   코틀린과 자바는 100% 호환이 가능함.
4. 구조화 동시성<br/>
   코틀린 언어가 제공하는 **코루틴**이라는 기법을 이용하면 비동기 프로그래밍을 간소화 할 수 있다.<br/>

5. **코틀린 파일 구성**<br/>

   ```kotlin
    package com.example.test3

    import java.text.SimpleDateFormat
    import java.util.*

    var data = 10

    fun formatData(date: Date): String {
        val sdformat = SimpleDateFormat("yyyy-mm-dd")
        return sdformat.format(date)
    }

    class User {
        var name = "hello"

        fun sayHello() {
            println("name : $name")
        }
    }
   ```

   위 코드는 User.kt라는 예제 코틀린 파일이다.<br/>
   package 구문은 해당 파일을 컴파일했을 때 만들어지는 클래스 파일의 위치를 나타내며 소스 파일에서 맨 처음
   한 줄로 선언한다. kt파일의 위치와는 상관없는 별도의 이름으로 선언이 가능하다.<br/>
   다른 이름으로 선언하면 컴파일된 클래스 파일이 package 구문 뒤에 있는 위치에 생성됨.<br/><br/>

   import 구문은 package 아래 여러 줄 작성 가능.<br/>
   import 아래에 변수, 함수, 클래스를 선언할 수 있고, 변수와 함수는 클래스 안뿐 아니라 클래스 밖
   (최상위)에도 선언이 가능하다.<br/><br/>

## 2. 변수와 함수

코틀린에서 변수는 val, var 키워드로 선언함.<br/>
val은 초깃값이 할당되면 변경이 불가능한 변수를 선언할 때 사용하고,<br/>
var은 초깃값이 할당된 후에도 값을 바꿀 수 있는 변수를 선언할 때 사용한다.<br/>

```kotlin
val data1 = 10
var data2 = 10

fun main() {
    data1 = 20 //오류
    data2 = 20 //가능
}
```

1. **타입 지정과 타입 추론** <br/>
   변수명 뒤에는 콜론(:)을 추가해 타입을 명시할 수 있고, 대입하는 값에 따라 타입을 유추할 수 있을 때는
   생략이 가능하다.<br/>

   ```
    val data1: Int = 10
    val data2 = 10
   ```

   val data2: Int = 10과 val data2 = 10은 동일한 코드임.<br/>

2. **초깃값 할당**<br/>
   최상위에 선언한 변수나 클래스의 멤버 변수는 선언과 동시에 초깃값을 할당해야 하며, 함수 내부에 선언한
   변수는 선언과 동시에 초깃값을 할당하지 않아도 된다.<br/>
   (변수를 이용하려면 값을 할당해야됨)<br/>

   ```kotlin
   val data1: Int //오류
   val data2 = 10 //성공

   fun someFun() {
       val data3: Int
       println("data3 : $data3")//오류
       data3 = 10
       println("Data3 : $data3")//성공
   }

   class User {
       val data4: Int //오류
       val data5: Int = 10 //성공
   }
   ```

3. **초기화 미루기**</dr>
   변수를 선언할 때 초깃값을 할당할 수 없는 경우가 있음.<br/>
   이때는 값을 이후에 할당할 것이라고 컴파일러에게 알려야함.<br/>
   lateinit이나 lazy 키워드를 사용한다.<br/>

   ```kotlin
   lateinit var data1: Int //오류
   lateinit val data2: String //오류
   lateinit var data3: String //성공
   ```

   lateinit은 이후에 초깃값을 할당할 것임을 명시적으로 선언한다.<br/>
   lateinit은 두가지 규칙이 있는데,

   1. var 키워드로 선언한 변수에만 사용이 가능
   2. Int, Long, Short, Double, Float, Boolean, Byte타입에는 사용이 불가능<br/><br/>

   lazy 키워드는 변수 선언문 뒤에 by lazy {} 형식으로 선언하며, 소스에서 변수가 최초로 이용되는 순간
   중괄호로 묶은 부분이 자동으로 실행되어 그 결과값이 변수의 초깃값으로 할당된다.<br/>
   lazy문의 중괄호 부분을 여러 줄로 작성한다면 마지막 줄의 실행 결과가 변수의 초깃값이 된다.<br/>

   ```kotlin
   val data4: Int by lazy {
       println("in lazy....")
       10
   }

   fun main() {
       println("in main....")
       println(data4 + 10)
       println(data4 + 10)
   }
   ```

   실행 결과는,<br/>
   in main....<br/>
   in lazy....<br/>
   20<br/>
   20<br/>

4. **데이터 타입**<br/>
   코틀린의 모든 변수는 객체임. 즉, 모든 타입은 객체 타입이다.<br/>
   정수를 다루는 타입인 Int는 기초 데이터 타입이 아니라 클래스이다.<br/>

   ```kotlin
   fun someFun(){
       var data1: Int = 10
       var data2: Int? = null //null 대입 가능

       data1 = data1 + 10
       data1 = data1.plus(10) //객체의 메서드 이용 가능
   }
   ```

   위 코드는 data1과 data2 변수를 Int타입으로 선언.<br/>
   만약 Int 타입이 기초 데이터 타입이라면 변수에 null을 할당할 수 없고, 메서드 호출이 불가능.<br/>
   코틀린에서는 모든 타입이 객체이므로 null 대입이 가능하고 메서드 호출이 가능함.<br/>

   1. 기초 타입 객체 <br/>
      Int, Short, Long, Double, Float, Byte, Boolean이 있다.<br/>
   2. 문자와 문자열<br/>
      Char은 문자를 표현하는 타입으로 작은 따옴표로 감싸서 사용<br/>
      String은 문자열으 표현하는 타입으로 큰따옴표, 삼중 따옴표로 감싸서 표현한다.<br/>
      String 타입의 데이터에 변수값이나 어떤 연산식의 결과값을 포함해야 할 때는 $를 사용한다.<br/>
      (문자열 템플릿)<br/>
   3. Any<br/>
      Any는 코틀린에서 최상위 클래스.<br/>
      즉, 모든 클래스는 Any의 하위 클래스이고 Any타입으로 선언한 변수는 모든 타입의 데이터 할당이 가능.<br/>
   4. Unit<br/>
      Unit은 다른 타입과 다르게 데이터 형식이 아닌 특수한 상황을 표현하려는 목적으로 사용<br/>
      Unit타입으로 선언한 변수에는 Unit 객체만 대입할 수 있다.<br/>
      즉, Unit으로 선언이 가능하지만 의미가 없음.<br/>
      주로 함수의 반환 타입으로 사용하며, 반환문이 없음을 명시적으로 나타낼 때 사용함.<br/>
      ```kotlin
      fun some(): Unit {
          println(10 + 20)
      }
      ```
   5. Nothing<br/>
      Nothing도 Unit과 마찬가지로 의미 있는 데이터가 아니라 특수한 상황을 표현한다.<br/>
      Nothing으로 선언한 변수에는 null만 대입이 가능하다.<br/>
      즉 Nothing으로 선언한 변수도 데이터로서는 의미가 없음.<br/>
      ```kotlin
      val data1: Nothing? = null
      ```
      주로 함수의 반환 타입에 사용하고 항상 null만 반환하는 함수라든가,
      예외를 던지는 함수의 반환 타입을 Nothing으로 선언한다.<br/>
      ```kotlin
      fun some1(): Nothing? {
          return null
      }
      fun some2(): Nothing {
          throw Exception()
      }
      ```
   6. 널 허용과 불허용<br/>
      코틀린의 모든 타입은 객체이므로 변수에 null을 대입할 수 있음.<br/>
      null은 값이 할당되지 않은 상황을 의미한다.<br/>
      null허용과 불허용은 변수를 선언할 때 타입 뒤에 물음표로 구분함.<br/>
      물음표가 있다면 널 허용, 없으면 불허용<br/>

      ```kotlin
      var data1: Int = 10
      data1 = null //오류

      var data2: Int? = 10
      data2 = null // 가능
      ```

5. **함수 선언하기** <br/>
   코틀린에서 함수를 선언하려면 fun이라는 키워드를 사용함.<br/>
   반환 타입을 선언할 수 있고, 생략시 Unit이 자동으로 적용됨.<br/>

   ```kotlin
   fun some(data1: Int): Int{
       return data1 * 10
   }
   ```

   함수의 매개변수에는 var이나 val 키워드를 사용할 수 없음.<br/>
   val이 자동으로 적용되고 함수 안에서 매개변수를 변경할 수 없다.<br/>
   함수의 매개변수에는 기본값 선언이 가능하고 기본값이 있을 경우 인자로 값을 넘겨주지 않아도 된다.<br/><br/>

   함수의 매개변수가 여러 개면 호출할 때 전달한 인자를 순서대로 할당함.<br/>
   호출 시 매개변수명을 지정하면 매개변숫값의 순서를 바꿔도 됨.<br/>
   (매개변수 명을 지정해서 호출하는걸 명명된 매개변수라고 함)<br/>

6. **컬랙션 타입**<br/>
   컬랙션 타입은 여러 개의 데이터를 표현하는 방법이며 Array, List, Set, Map이 있다.<br/>

   1. Array - 배열<br/>
      코틀린에서 배열은 Array 클래스로 표현함.<br/>
      Array 클래스의 생성자에서 첫 번째 매개변수는 배열의 크기이고,
      두 번째 매개변수는 초깃값을 지정하는 함수.<br/>
      배열의 타입은 제네릭으로 표현한다.<br/>

      ```kotlin
      val data1: Array<Int> = Array(3, {0})
      ```

      위 코드는 두번째 인자가 0을 반환하는 람다식 함수이므로 0으로 초기화한 데이터를 3개 나열한 정수형 배열을 선언한 것.<br/>
      배열의 데이터에 접근할 때는 대괄호, set(), get()을 이용한다.<br/>

      ```kotlin
      fun main(){
          val data1: Array<Int> = Array(3,{0})
          data1[0] = 10
          data1[1] = 20
          data1.set(2,30) // 배열에서 2번째 데이터를 30으로 설정

          println(
              """
              array size: ${data1.size}
              array data: ${data1[0]},${data1[1]},${data1.get(2)}
              """
          )
      }
      ```

   2. 기초 타입의 배열<br/>
      앞에서 배열의 타입을 제네릭으로 명시한다고 했는데, 배열의 데이터가 기초 타입이라면 Array를 사용하지 않고
      각 기초 타입의 배열을 나타내는 클래스를 사용할 수 있음.<br/>
      또한, arrayOf()라는 함수를 사용하면 배열을 선언할 때 값을 할당할 수 있음.<br/>
      (배열 선언과 동시에 값 할당)

      ```kotlin
      fun main() {
          val data1: IntArray = IntArray(3, {0})
          val data2 = arrayOf<Int>(10,20,30)
          ...
      }
      ```

      arrayOf() 함수도 기초 타입을 대상으로 하는 함수들이 있다.<br/>
      (booleanArrayOf()...)<br/>

   3. List, Set, Map<br/>
      위의 세개는 Collection 인테페이스를 타입으로 표현한 클래스이고 통틀어 컬랙션 타입 클래스라고 한다.<br/>

      1. List - 순서가 있는 데이터의 집합, 데이터의 중복 허용
      2. Set - 순서가 없고, 데이터의 중복을 허용하지 않음
      3. Map - 키, 값의 쌍으로 이루어진 데이터의 집합. 순서가 없고 키의 중복을 허용하지 않음<br/>

      Collection 타입의 클래스는 가변, 불변 클래스로 나뉨.<br/>
      불변은 초기에 데이터를 대입하면 더 이상 변경이 불가능.<br/>
      가변은 초깃값을 대입한 이후에도 데이터를 추가하거나 변경이 가능하다.<br/><br/>

      리스트로 예를 들면 List는 불변 타입으로 size(), get() 함수만 제공하고 데이터를 추가하거나 변경하는 함수는 제공하지 않는다.<br/>
      MutableList는 가변타입으로 size(), get() 이외에 add(), set() 함수를 이용할 수 있음.<br/>
      (Set, Map도 마찬가지임)<br/>

      ```kotlin
      fun main() {
          var list = listOf<Int>(10,20,30)
          println(
              """
              list size : ${list.size}
              list data : ${list[0]}, ${list.get(1)},${list.get(2)}
              """
          )
      }
      ```

      위 코드는 List타입 사용 예제.(listOf()함수로 List 객체 생성)<br/>

      ```kotlin
      fun main() {
          var mutableList = mutableListOf<Int>(10,20,30)
          mutableList.add(3,40)
          mutableList.set(0,50)
          ...
      }
      ```

      위 코드는 mutableListOf()로 MutableList 객체를 생성해서 사용하는 예제.<br/>

      ```kotlin
      fun main(){
          var map = mapOf<String, String>(Pair("one", "hello"), "two" to "world")
          ...
      }
      ```

      Map 객체는 키와 값으로 이루어진 데이터의 집합이다.<br/>
      Map 객체의 키와 값은 Pair 객체를 이용할 수도 있고, '키 to 값' 형태로 이용할 수도 있음.<br/>
      위 코드에서는 mapOf() 함수를 이용해 Map 객체를 만듦.<br/>
