---
title: "Android(4) - 코틀린 람다 & 고차함수"
excerpt: "Android Kotlin Lambda"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-04 20:00:00"
last_modified_at: "2023-01-04 21:15:00"
---

## 1. 람다 함수와 고차 함수

람다 함수는 코틀린뿐만 아니라 많은 프로그래밍 언어에서 제공하는 익명 함수 정의 기법이다.<br/>
주로 함수를 간단하게 정의할 때 이용하고 람다식이라고도 한다.<br/><br/>

코틀린에서는 고차 함수를 지원하는데, 고차 함수는 매개변수나 반환값으로 함수를 이용한다.<br/>
람다 함수는 주고받을 함수를 간단하게 정의할 때 사용하기 때문에, 람다 함수는 고차 함수를 이해하고
사용하려면 반드시 알아야할 부분이다.<br/>

1. **람다 함수 선언과 호출**<br/>
   일반적으로 함수는 fun 키워드로 선언하는데, 람다 함수는 fun 키워드를 사용하지 않고 함수 이름이 없다.
   람다함수는 중괄호({})로 표현한다.<br/>

   ```kotlin
   {매개변수 -> 함수 본문}
   ```

   코틀린에서 람다 함수를 사용하는 규칙은 아래와 같다.<br/>

   1. 람다 함수는 {}로 표현한다.<br/>
   2. {} 안에 화살표(->)가 있으며 화살표 왼쪽은 매개변수, 오른쪽은 함수 본문<br/>
   3. 함수의 반환값은 함수 본문의 마지막은 표현식이다.<br/>

   ```kotlin
   fun sum1(no1: Int, no2: Int): Int{
       return no1 + no2
   }

   val sum2 = {no1: Int, no2: Int -> no1 + no2}
   ```

   위 코드에서 sum1은 일반 함수, sum2는 람다 함수 선언이고 둘 다 같은 기능을 한다.<br/>
   람다 함수의 경우 중괄호 {} 영역에 람다 함수를 선언하고, 함수 선언문인데도 fun 키워드를 사용하지 않았으며
   함수 이름도 없다.<br/>
   중괄호 안의 화살표를 기준으로 왼쪽에는 Int타입의 매개변수 2개를 선언했고, 오른쪽에는 실행문을 작성했다.
   이 함수를 호출하면 실행문의 결괏값을 반환한다.<br/>
   람다 함수는 본문에서 마지막 실행문의 결과가 함수의 반환값이다.<br/><br/>

   람다 함수는 이름이 없기 때문에 함수명으로는 호출할 수 없고 보통 람다 함수를 변수에 대입해서 사용한다.<br/>

   ```kotlin
   sum2(10,20)
   ```

   위의 선언 코드에서 sum2라는 변수에 람다 함수를 대입했기 때문에 변수명을 이용해 호출했다.<br/>
   람다 함수는 변수에 대입해서 사용하는 방법 말고도, 선언과 동시에 호출할 수 있다.<br/>

   ```kotlin
   {no1: Int, no2:Int -> no1 + no2} (10,20)
   ```

   중괄호 부분이 람다 함수이고 함수의 경우 호출을 할 때 소괄호를 이용해 호출해야 하는데, 람다 함수인 중괄호 뒤에 소괄호를 이용해 함수를 선언하자마자 호출한다.<br/>
   소괄호 안은 람다 함수에 선언한 매개변수에 맞추어 인자를 전달한 것이다.<br/>

   1. 매개변수 없는 람다 함수<br/>
      함수에 매개변수가 항상 있어야 하는 것은 아니다.<br/>
      람다 함수에서 화살표 왼쪽이 매개변수를 정의하는 부분인데 매개변수가 없을 때는 그냥 비워두면 된다.
      매개변수가 없다면 화살표 생략도 가능하다.<br/>

      ```kotlin
      {-> println("function call")}

      {println("function call")}
      ```

   2. 매개변수가 1개인 람다 함수<br/>
      람다 함수를 선언할 때 매개변수는 중괄호 안 화살표 왼쪽에 선언한다.<br/>
      람다 함수의 매개변수가 1개일 때는 매개변수를 선언하지 않아도 함수로 전달된 값을 쉽게 이용할 수 있다.<br/>
      ```kotlin
      fun main() {
          val some = {no: Int -> println(no)}
          some(10)
      }
      ```
      ```kotlin
      fun main() {
          val some: (Int) -> Unit = {println(it)}
          some(10)
      }
      ```
      위 코드 두개는 똑같은 함수이다.<br/>
      아래쪽 코드를 보면 람다 함수의 중괄호 안에 화살표가 없어서 매개변수가 없는 것처럼 보일 수도 있는데,
      람다 함수 앞에 (Int) -> Unit이 매개변수 1개인 람다 함수임을 알려준다.<br/>
      (함수 타입)<br/>
      이처럼 람다 함수의 매개변수가 1개일 때는 중괄호 안에서 매개변수 선언을 생략하고 println(it) 처럼 it 키워드로 매개변수를 이용할 수 있다.<br/>
      여기서 주의할 점이 람다 함수에서 it을 이용해 매개변수를 사용할 때는 해당 매개변수의 타입을 식별할 수
      있을 때만 가능하다.<br/>
      ```kotlin
      val some = {println(it)} //오류
      val some: (Int) -> Unit = {println(it)} //가능
      ```
   3. 람다 함수의 반환<br/>
      람다 함수도 함수이므로 자신을 호출한 곳에 결괏값을 반환해야 할 때가 있다.<br/>
      그런데 람다 함수에서는 return을 사용할 수 없음.<br/>
      ```kotlin
      val some = {no1: Int, no2: Int -> return no1 * no2} //오류
      ```
      람다 함수의 반환값은 본문에서 마지막 줄의 실행 결과임.<br/>
      ```kotlin
      fun main() {
          val some = {no1: Int, no2: Int ->
              println("in lambda function")
              no1 * no2
          }
          println("result: ${some(10, 20)}")
      }
      ```

2. **함수 타입과 고차 함수**<br/>
   코틀린에서는 함수를 변수에 대입해서 사용할 수 있음.<br/>
   그런데, 변수는 타입을 가지며 타입을 유추할 수 있을 때를 제외하고는 생략이 불가능함.<br/>
   그래서 변수에 함수를 대입하려면 변수를 **함수 타입**으로 선언해야 한다.<br/>

   1. 함수 타입 선언<br/>
      함수 타입이란 함수를 선언할 때 나타내는 매개변수와 반환 타입을 의미한다.<br/>

      ```kotlin
      fun some(no1: Int, no2: Int): Int {
          return no1 + no2
      }
      ```

      위에서 오른쪽은 Int형 매개변수를 2개 받아서 결괏값을 Int로 반환하는 함수 선언.<br/>
      이거를 (Int, Int) -> Int로 표현할 수 있음.<br/>
      함수를 대입할 변수를 선언할 때 이러한 함수 타입을 선언하고 이에 맞는 함수를 대입해야 한다.<br/>

      ```kotlin
      val some: (Int, Int) -> Int = {no1: Int, no2: Int -> no1 + no2}
      ```

      앞에 (Int, Int) -> Int가 함수 타입, 뒤에 람다 함수가 함수 내용.<br/>

   2. 타입 별칭 - typealias<br/>
      함수 타입을 typealias를 이용해 선언할 수 있다.<br/>
      typealias는 타입의 별칭을 선언하는 키워드로, 함수 타입뿐만 아니라 데이터 타입을 선언할 때도
      사용이 가능하다.<br/>

      ```kotlin
      typealias MyInt = Int
      fun main() {
          val data1: Int = 10
          val data2: MyInt = 10
      }
      ```

      ```kotlin
      typealias MyFunType = (Int, Int) -> Boolean

      fun main() {
          val someFun: MyFunType = {no1: Int, no2: Int ->
              no1 > no2
          }
          println(someFun(10,20))
          println(someFun(20,10))
      }
      ```

   3. 매개변수 타입 생략<br/>
      람다 함수를 정의할 때 매개변수의 타입을 유추할 수 있다면 타입 선언 생략 가능<br/>

      ```kotlin
      typealias MyFunType = (Int, Int) -> Boolean
      val someFun: MyFunType = {no1, no2 -> no1 > no2}
      ```

      ```kotlin
      val someFun: (Int, Int) -> Boolean = {no1, no2 -> no1 > no2}
      ```

      ```kotlin
      val someFun = {no1: Int, no2: Int -> no1 > no2}
      ```

   4. 고차 함수<br/>
      **고차 함수**란 함수를 매개변수로 전달받거나 반환하는 함수를 의미함.<br/>
      함수를 매개변수나 반환값으로 이용하는 함수를 고차 함수라고 함.<br/>
      ```kotlin
      fun hofFun(arg: (Int) -> Boolean): () -> String {
          val result = if(arg(10)) {
              "valid"
          }else{
              "invalid"
          }
          return {"hofFun result: $result"}
      }
      fun main() {
          val result = hofFun({no -> no > 0})
          println(result())
      }
      ```
      위 코드에서 hofFun()은 고차함수.<br/>
      result변수에서 hofFun()을 호출함. hofFun의 인자로 들어간 람다 함수가 arg: (Int) -> Boolean (매개변수)로 들어감. 그리고 반환 값인 람다 함수를 result에 넣음.<br/>

## 2. 널 안전성

1. **널 안전성이란?**<br/>
   널은 객체가 선언되었지만 초기화되지 않은 상태를 의미한다.<br/>
   객체는 데이터가 저장된 주소를 참조하므로 흔히 참조 변수라고 한다.<br/>
   데이터가 메모리에 저장되면 어디에 저장됐는지 알아야 이용할 수 있는데, 이때 해당 메모리 위치를 식별하는 것이
   주소이다.<br/>
   객체에는 주소가 저장되고 이 주소로 메모리에 접근해서 데이터를 이용한다.<br/>
   그런데 널은 객체가 주소를 가지자 못한 상태를 나타낸다.<br/>

   ```kotlin
   val data1: String = "hello"
   val data2: String? = null
   ```

   위 코드에서 data1 변수에는 "hello"라는 문자열 데이터가 저장된 주소가 대입되고, 그 주소로 문자열 데이터를
   사용한다.<br/>
   data2에는 null을 대입했는데, 그러면 data2는 주솟값을 가지지 못한다.<br/>
   즉 변수가 선언되었지만 이용할 수 없는 상태를 의미함.<br/><br/>

   이처럼 널인 상태의 객체를 사용하면 널 포인터 예외(NPE)가 발생한다.<br/>
   널 포이터 예외는 널인 상태의 객체를 이용할 수 없다는 오류임.<br/><br/>

   이때 널 안전성이란 널 포인터 예외가 발생하지 않도록 코드를 작성하는 것을 의미함.<br/>

   ```kotlin
   fun main() {
       var data: String? = null
       val length: if(data == null) {
           0
       }else{
           data.length
       }
       println("data length: $length")
   }
   ```

   위 처럼 조건문을 사용해 null을 확인하면 널 포인터 예외가 발생하지 않지만 매번 일일이 점검하는 코드를 작성해야 하기 때문에 힘듦.<br/>

   ```kotlin
   fun main() {
       var data: String? = null
       println("data length: ${data?.length ?: 0}")
   }
   ```

   data가 null이면 0을 반환하고 null이 아니면 length를 반환하는 코드.<br/>
   위 처럼 작성하면 null 점검 코드를 작성하지 않아도 널 안전성 확보 가능.<br/>

2. **널 안전성 연산자**<br/>

   1. 널 허용 - ? 연산자<br/>
      코틀린에서는 변수 타입을 널 허용과 불허로 구분.<br/>
      즉, 변수에 null을 대입할 수 있는지를 선언할 때 타입으로 구분한다는 이야기.<br/>

      ```kotlin
      var data1: String = "kkang"
      data1 = null //오류

      var data2: String? = "kkang"
      data2 = null //성공
      ```

      오른쪽 코드에서 data1 변수는 String 타입으로 선언했다.<br/>
      이렇게 하면 널 불허로 선언하므로 이 변수에 null을 대입하면 오류가 발생함.<br/>
      data2 변수는 String 타입으로 선언했지만 뒤에 ? 연산자를 추가했으므로 널 허용 변수가 되어 null
      대입이 가능하다.<br/>

   2. 널 안전성 호출 - ?. 연산자<br/>
      널 허용으로 선언한 변수의 멤버에 접근할 때는 반드시 ?. 연산자를 이용해야 한다.<br/>
      ?. 연산자는 변수가 null이 아니면 멤버에 접근하지만 null이면 멤버에 접근하지 않고 null을 반환.<br/>
      ```kotlin
      var data: String? = "kkang"
      var length = data.length //오류
      ```
      언제든지 null이 대입될 수 있으므로 오류 발생.<br/>
      ```koltin
      var data: String? = "kkang"
      var length = data?.length //성공
      ```
   3. 엘비스 - ?: 연산자
      엘비스 연산자란 ?: 기호를 말한다.<br/>
      이 연산자는 변수가 널이면 널을 반환한다.<br/>
      어떤 경우에는 변수가 널일 때 대입해야 하는 값이나 실행해야 하는 구문이 있을 수도 있는데,
      이때 엘비스 연산자를 사용한다.<br/>
      ```kotlin
      fun main() {
          var data: String? = "kkang"
          println("data = $data : ${data?.length ?: -1}")
          data = null
          println("data = $data : ${data?.length ?: -1}")
      }
      ```
   4. 예외 발생 - !! 연산자
      !!는 객체가 널일 때 예외를 일으키는 연산자.<br/>
      객체가 널일 때 ?. 또는 ?: 연산자를 이용해 널 포인트 예외가 발생하지 않게 작성할 수도 있지만,
      또 어떤 경우에는 예외를 발생시켜야 할 때도 있음. 이때 !! 연산자를 사용한다.<br/>
      ```kotlin
      fun some(data: String?): Int {
          return data!!.length
      }
      fun main() {
          println(some("kkang"))
          println(some(null))
      }
      ```
      위 소스는 some() 함수에 문자열을 전달하면 오류 없이 정상으로 실행되고, null을 전달하면
      data!!.length 코드로 예외 메세지를 출력하는 것을 보여준다.<br/>
