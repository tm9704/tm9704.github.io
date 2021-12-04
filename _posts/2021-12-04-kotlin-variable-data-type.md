---
title: "Kotlin(3) - 코틀린 변수와 자료형"
excerpt: "Kotlin Variable & Data Type"
toc: true
toc_sticky: true
categories:
  - Kotlin
tags:
  - Kotlin
date: "2021-12-04 15:50:00"
last_modified_at: "2021-12-04 16:20:00"
---

변수란 값을 넣을 수 있는 상자라고 이해하고 시작하겠습니다.<br/>

## 1. 변수 선언, 자료형 추론

코틀린에서 변수는 val, var 키워드를 이용해 선언할 수 있습니다.<br/>
우선 val, var의 차이점을 보겠습니다.<br/>

1. val : 최초로 지정한 변수의 값으로 초기화 하고 더 이상 바꿀 수 없는 읽기 전용 변수
2. var : 최초로 지정한 변수의 초깃값이 있더라도 값을 바꿀 수 있는 변수

변수를 선언하는 방법은<br/>

**_val username: String = "Kildong"_**<br/>

위 처럼 선언 키워드 변수명: 자료형 = "값" 순으로 선언하면 됩니다.<br/>

코틀린은 자료형을 지정하지 않고 변수를 선언하면 변수에 할당된 값을 보고 알아서 자료형을 지정할 수 있습니다.<br/>
이것을 자료형 추론이라고 합니다.<br/>

**_val username = "Kildong"_**<br/>

자료형을 지정하지 않은 변수는 반드시 값을 지정해야 합니다.(초기화 필수)<br/>

## 2. 코틀린 자료형

코틀린의 자료형 종류에 대해 공부하기 전에 코틀린의 자료형에 대해 설명하겠습니다.<br/>

코틀린은 보통 프로그래밍 언어처럼 기본형 자료형을 바로 사용하는게 아니라 참조형 자료형을 사용합니다.<br/>

1. 기본형 자료형(Primitive Data Type)<br/>
   가공되지 않은 순수한 자료형을 말합니다.<br/>
   프로그래밍 언어 내에 저장되어 있습니다.<br/>

2. 참조형 자료형(Reference Type)<br/>
   객체를 생성하고 동적 메모리 영역에 데이터를 둔 다음 이것을 참조하는 자료형입니다.<br/>

코틀린은 참조형만을 사용하지만, 참조형으로 선언된 변수는 성능 최적화를 위해 컴파일러에서 다시 기본형으로 대체합니다.<br/>

## 3. 자료형 종류

1. 정수 자료형<br/>

   정수 자료형은 부호가 있는 것과 부호가 없는 것으로 나눌 수 있습니다.<br/>

   예시를 보겠습니다.<br/>

   ```
   fun main(){
    val num05 = 127 //Int형 추론
    val num06 = -32768 //Int형 추론
    val num07 = 2147483647 //Int형 추론
    val num08 = 9223372036854775807 //Long형 추론
   }
   ```

   위 코드 처럼 정수를 사용할 때 그냥 숫자를 사용하면 10진수를 나타내고, 각 정수의 크기에 맞게 추론합니다.<br/>

   ```
   val exp01 = 123 //Int형 추론
   val exp02 = 123L //접미사 L을 사용해 Long형으로 추론
   val exp03 = 0x0F //접두사 0x를 사용해 16진 표기가 사용된 Int형으로 추론
   val exp04 = 0b00001011 //접두사 0b를 사용해 2진 표기가 사용된 Int형으로 추론
   ```

   접미사나 접두사를 활용해 2진수나 16진수의 표현도 가능합니다.<br/>

   ```
   val exp08: Byte = 127 //명시적으로 자료형을 지정 (Byte)
   val exp09 = 32767 //명시적으로 자료형을 지정하지 않으면 Short형 범위의 값도 Int형으로 추론
   val exp10: Short = 32767 //명시적으로 자료형을 지정(Short)
   ```

   보통 숫자는 Int로 추론되기 때문에 좀 더 작은 범위의 자료형을 사용하고 싶다면,<br/>
   직접 자료형을 명시해야 합니다.<br/>

   ```
    val uint: UInt = -153u
    vla ulong: ULong = 46322342uL
   ```

   부호가 없는 자료형을 사용할 때는 위와 같이 값에 식별자를 사용하면 됩니다.<br/>

2. 실수 자료형<br/>

   실수 자료형은 실수를 저장하는데 사용합니다.<br/>

   ```
    val exp01 = 3.14 //Double형으로 추론(기본)
    val exp02 = 3.14F //Float형으로 추론 식별자 F에 의해
   ```

   실수를 표현하는 방식은 부동소수점 방식을 사용합니다.<br/>
   (부동소수점 방식은 Java 카테고리에서 다뤘습니다.)<br/>

   코틀린에서 소수점의 이동은 숫자 오른쪽에 e나 E와 함께 밑수인 10을 제외하고 지수만 적으면 됩니다.<br/>

   ```
    val exp03 = 3.14E-2 //왼쪽으로 소수점 2칸 이동, 0.0314
    val exp04 = 3.14e2 //오른쪽으로 소수점 2칸 이동, 314
   ```

   정수 자료형과 실수 자료형의 최솟값과 최댓값을 알 수 있는 방법은,<br/>

   ```
   fun main(){
       println("Byte min: " + Byte.MIN_VALUE + " max: " + Byte.MAX_VALUE)
       println("Short min: " + Short.MIN_VALUE + " max: " + Short.MAX_VALUE)
       println("Int min: " + Int.MIN_VALUE + " max: " + Int.MAX_VALUE)
       println("Long min: " + Long.MIN_VALUE + " max: " + Long.MAX_VALUE)
       println("Float min: " + Float.MIN_VALUE + " max: " + Float.MAX_VALUE)
       println("Double min: " + Double.MIN_VALUE + " max: " + Double.MAX_VALUE)
   }
   ```

   이런식으로 알 수 있습니다.<br/>

3. 논리 자료형<br/>

   논리 자료형(Boolean)은 조건을 검사할 때 많이 사용합니다.<br/>
   값은 true, false를 가집니다.<br/>

   ```
    val isOpen = true // isOpen은 Boolean형으로 추론
    val isUploaded: Boolean // 변수를 선언만 한 경우 자료형(Boolean)을 반드시 명시
   ```

4. 문자 자료형<br/>

   문자 자료형(Char)은 문자를 표현하기 위해 사용하며 문자 자료형 값은 작은따옴표(')로 감싸 표현합니다.<br/>

   ```
    val ch = 'c' // ch는 Char로 추론
    val ch2: Char // 변수를 선언만 한 경우 자료형(Char)을 반드시 명시
   ```

   컴퓨터는 문자 자료형 값을 저장할 때 문자세트를 참고하여 번호로 저장합니다.<br/>
   예를 들면 문자 A를 A로 저장하는 것이 아닌 65로 저장됩니다.<br/>

   ```
    val ch = 'A'
    println(ch+1) //B

    val chNum: Char = 65 // 오류, 숫자를 사용하여 선언하는건 금지
   ```

   정수 자료형을 이용하여 문자 자료형을 선언하는 방법을 보겠습니다.<br/>

   ```
    val code: Int = 65
    val chFromCode: Char = code.toChar() //code에 해당하는 문자를 할당
    println(chFromCode) //결과

    //val ch4: Char = 'ab' => 2개 이상의 문자는 Char에 담을 수 없음
   ```

5. 문자열 자료형<br/>

   문자열 자료형(String)에 대해 보겠습니다.<br/>
   문자열 자료형은 문자를 배열하여 저장할 수 있는 자료형입니다.<br/>

   문자 자료형처럼 기본형으로 처리되는 자료형이 아니라 기본형에 속하지 않는<br/>
   배열 형태로 되어 있는 특수한 자료형입니다.<br/>

   ```
   fun main(){
    var str1: String = "Hello"
    var str2 = "World"
    var str3 = "Hello"

    println("str1 === str2: ${str1 === str2}")
    println("str1 === str3: ${str1 === str3}")
   }
   ```

   저장 방식은 Java와 동일합니다. (참조 개념)<br/>

6. 표현식과 $ 기호 사용하여 문자열 출력하기

   변수의 값이나 표현식을 문자열 안에 넣어 출력하려면 $ 기호와 함께 변수나 표현식을 사용하면 됩니다.<br/>

   ```
   fun main(){
       var a = 1
       val str1 = "a = $a"
       val str2 = "a = ${a+2}" // 문자열에 표현식 사용

       println("str1: \"$str1\", str2: \"$str2\"")
   }
   ```

   문자열 안에 변수와 변수 앞에 $키워드를 사용하면 $변수명이 변수의 값으로 대체됩니다.<br/>
   표현식 역시 동일합니다.<br/>

7. 형식화된 다중 문자열 사용하기<br/>

   줄바꿈 문자, 탭 등 특수문자가 포함된 문자열을 표현하기 위해서는 \"\"\"기호를 사용하면 됩니다<br/>

   ```
   fun main(){
    val num = 10
    val formattedString = """
        var a = 6
        var b = "Kotlin"
        println(a + num)
        """
    //num의 값은 $num
    println(formattedString)
   }
   ```

8. 자료형에 별명 붙이기<br/>

   자료형에 별명을 붙이려면 typealias 키워드를 사용하면 됩니다.<br/>

   ```
   typealias Username = String // String을 Username이라는 별명으로 대체
   val user: Username = "Kildong" // Username과 String은 같은 표현
   ```
