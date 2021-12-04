---
title: "Kotlin(4) - 코틀린 자료형 검사 & 변환"
excerpt: "Kotlin NPE Test"
toc: true
toc_sticky: true
categories:
  - Kotlin
tags:
  - Kotlin
date: "2021-12-04 18:30:00"
last_modified_at: "2021-12-04 19:00:00"
---

코틀린은 변수를 사용할 때 반드시 값이 할당되어 있어야 한다는 원칙이 있습니다.<br/>

만약, 값이 할당되지 않은 변수를 사용하면 오류가 발생하는데,<br/>
값이 없는 상태를 null이라고 부릅니다.<br/>

null 상태인 변수를 허용하려면 물음표(?) 기호를 사용해 선언해야 합니다.<br/>

## 1. null을 허용한 변수 검사하기

프로그램이 실행되는 도중에 값이 null인 변수에 접근하려 하면 NPE 예외 오류가 발생합니다.<br/>
그러나 코틀린은 변수에 아예 null을 허용하지 않습니다.<br/>

1. 변수에 null 할당하기<br/>

   ```
    var str1 : String = "Hello Kotlin"
    //str1 = null => 오류 null을 허용하지않음
    println("str1: $str1")
   ```

   위의 코드를 실행하면 null 값이 할당될 수 없다는 오류 메세지가 나타납니다.<br/>
   변수에 null 할당을 허용하려면 자료형 뒤에 물음표(?) 기호를 명시해야 합니다.<br/>

   ```
    var str1 : String? = "Hello Kotlin"
    str1 = null
    println("str1: $str1")
   ```

   str1 변수에 null을 할당할 수 있으며, 변수의 null 허용 여부에 따라<br/>
   String과 String?은 서로 다른 자료형입니다.<br/>

2. 세이프 콜과 non-null 단정 기호<br/>

   ```
   var str1: String? = "Hello Kotlin"
   str1 = null
   println("str1: $str1 length: ${str1.length}") //null을 허용하면 length가 실행될 수 없음
   ```

   위 코드는 실행이 불가능하며, String?형에서는 세이프 콜(?.)이나 non-null 단정 기호(!!.)만 허용합니다.<br/>

   1. 세이프콜<br/>

      null이 할당되어 있을 가능성이 있는 변수를 검사하여 안전하게 호출하도록 도와주는 기법<br/>

      ```
      var str1: String? = "Hello Kotlin"
      str1 = null
      println("str1: $str1 length: ${str1?.length}") // str1을 세이프 콜로 안전하게 호출
      ```

      str1을 검사한 다음 null이 아니면 str1의 멤버 변수인 length에 접근해 값을 읽습니다.<br/>
      반면, str1에 아무런 값이 없을 경우 length에 접근하지 않고 그대로 null을 출력합니다.<br/>

   2. non-null 단정기호<br/>

      non-null 단정 기호는 변수에 할당된 값이 null이 아님을 단정하는 기호입니다.<br/>
      그러므로 컴파일러가 null 검사 없이 무시합니다.<br/>

      따라서 변수에 null이 할당되어 있어도 컴파일은 진행되나, 실행 중에 NPE가 발생합니다.<br/>

      ```
      println("str1: $str1 length: ${str1!!.length}") //NPE 강제 발생
      ```

3. 조건문을 활용한 null을 허용한 변수 검사<br/>

   세이프 콜이나 단정 기호를 사용하는 방법 대신 조건문으로 null을 허용한 변수를 검사해도 됩니다.<br/>
   즉, null을 허용한 변수의 null 상태 가능성을 검사하기만 하면 코틀린 컴파일러는 오류를 발생시키지 않습니다.<br/>

   ```
   fun main(){
       var str1: String? = "Hello Kotlin"
       str1 = null

       var len = if(str1 != null) str1.length else -1
       println("str1: $str1 length: ${len}")
   }
   ```

4. 세이프 콜과 엘비스 연산자 활용<br/>

   null을 허용한 변수를 조금 더 안전하게 사용하려면 세이프 콜(?.)과<br/>
   엘비스(Elvis) 연산자 ?:를 함께 사용하면 됩니다.<br/>

   엘비스 연산자는 변수가 null인지 아닌지 검사하여 null이 아니라면,<br/>
   그대로 왼쪽 식을 실행하고 null이면 오른쪽 식을 실행합니다.<br/>

   ```
    var str1: String? = "Hello Kotlin"
    str1 = null
    println("str1 = $str1 length: ${str1?.length ?: -1}") //세이프 콜과 엘비스 연산자 활용
   ```

   위의 **_${str1?.length ?: -1}_**은 **_if(str1!=null) str1.length else -1_**과 동일합니다.<br/>

## 2. 자료형 비교하고 검사하고 변환하기

코틀린의 자료형은 모두 참조형으로 선언한다고 배웠습니다.<br/>
하지만 컴파일될 때 Int, Long, Short와 같은 자료형은 기본형 자료형으로 변환됩니다.<br/>

각각 저장방식이 다르기 때문에 자료형을 비교하거나 검사할 때는 이런 특징을 잘 이해해야 합니다.<br/>

코틀린은 자료형이 서로 다른 변수를 비교하거나 연산할 수 없습니다.<br/>

1. 자료형 변환<br/>

   코틀린은 자바같은 언어처럼 자료형이 다른 변수에 재할당 하면 자동 형변환이 되지 않습니다.<br/>
   (Type Mismatch 오류 발생)<br/>

   ```
    val a: Int = 1 //Int형 변수 a를 선언하고 1을 할당
    val b: Double = a //자료형 불일치로 오류 발생
    val c: Int = 1.1 //자료형 불일치 오류 발생
   ```

   만약 자료형을 변환해 할당하고 싶다면 자료형 변환 메서드가 필요합니다.<br/>

   ```
   val b: Doulbe = a.toDouble()
   ```

   그러나 표현식에서 자료형이 서로 다른 값을 연산할 때는 자료형이 표현할 수 있는<br/>
   범위가 큰 자료형으로 자동 형 변환하여 연산합니다.<br/>

   ```
   var result = 1L + 3 //Long + Int -> result는 Long
   ```

2. 기본형과 참조형 자료형의 비교 원리<br/>

   자료형을 비교할 때 단순히 값만 비교하는 방법과 참조 주소까지 비교하는 방법이 있습니다.<br/>

   단순히 값만 비교할 경우 이중 등호 (==)를 사용하며<br/>
   (주소는 상관 없음)<br/>
   참조 주소를 비교하려면 삼중 등호(===)를 사용합니다.<br/>
   (값은 상관 없음)<br/>

   ```
   val a: Int = 128
   val b: Int = 128
   println(a == b)
   println(a === b)
   ```

   위의 코드의 경우는 Int형 같은 경우 컴파일러가 자동으로 int로 변환해주기 때문에,<br/>
   삼중 등호가 비교하는 값도 저장된 값이 128입니다.<br/>

   참조 주소까지 달라지는 것은 null을 허용한 변수의 경우 같은 값을 저장해도<br/>
   이중등호와 삼중등호를 사용한 결과가 다릅니다.<br/>

   ```
   val a: Int = 128 //스택 저장
   val b: Int? = 128 // 참조형으로 저장

   println(a == b)
   println(a === b)
   ```
