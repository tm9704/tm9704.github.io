---
title: "Kotlin(7) - 프로그램 흐름 제어"
excerpt: "Kotlin Condition"
toc: true
toc_sticky: true
categories:
  - Kotlin
tags:
  - Kotlin
date: "2022-12-10 20:00:00"
last_modified_at: "2022-12-10 20:40:00"
---

## 1. 조건문

조건문은 주어진 조건에 따라 다른 결과를 반환하는 코드.

1. if문과 if-else문

   ```
   if(조건식){
       수행할 문장
   }
   ```

   조건식에는 Boolean 자료형으로 참(true) 또는 거짓(false)값을 받을 수 있는 조건식을 구성해야 함.<br/>
   조건식이 true일 경우 if문 블록안에 있는 문장을 수행함.<br/>
   수행할 문장이 하나일 경우 중괄호 생략 가능<br/>

   ```
   if(조건식) {
       수행할 문장
   } else {
       수행할 문장
   }
   ```

   조건식이 false인 경우 수행하고자 하는 문장이 있다면 else문의 블록에 코드를 작성.<br/>

   블록의 표현식이 길어질 경우 중괄호로 감쌈.<br/>
   람다식처럼 블록의 마지막 표현식이 변수에 반환되어 할당됨.<br/>

   ```
   fun main() {
       val a = 12
       val b = 7

       val max = if(a>b) {
           println("a 선택")
           a
       } else {
           println("b 선택")
           b
       }

       println(max)
   }
   ```

2. else if문으로 조건문 중첩하기
   여러가지 조건을 적용하기 위해 else if를 적용할 수 있음.<br/>
   너무 많이 사용하면 코드가 읽기 어려워짐.<br/>

   ```
   val number = 0
   val result = if (number > 0)
           "양수 값"
       else if(number < 0)
           "음수 값"
       else
           "0"
   ```

   - 코틀린의 표준 라이브러리 함수 readLine()
     콘솔로부터 문자열을 입력받는 함수
     ```
     val score = readLine()!!.toDouble()
     ```
     위 코드는 readLine()으로 입력받는 것의 자료형은 문자열이기 때문에 형변환을 해준 것.<br/>
     문자열을 입력받을 때 실수형으로 변환을 하지 못하는 경우 예외가 발생할 수 있기 때문에 단정기호 사용.<br/>

3. in 연산자와 범위 연산자로 조건식 간략하게 만들기

   ```
   fun main() {
       print("Enter the score: ")
       val score = readLine()!!.toDouble()
       var grade: Char = 'F'

       if(score >= 90.0){
           grade = 'A'
       }else if(score >= 80.0 && score <= 89.9){
           grade = 'B'
       }else if(score >= 70.0 && score <= 79.9){
           grade = 'C'
       }

       println("Score: $score, Grade: $grade")
   }
   ```

   조건에 비교 연산자와 논리 연산자를 이용해 범위를 지정할 수 있음<br/><br/>

   이렇게 비교 연산자와 논리 연산자를 다 사용하는게 번거로울 수 있는데,<br/>
   포함 여부 확인을 위한 in 연산자와 2개의 점(..)으로 구성된 범위(range) 연산자를 대신 사용할 수 있음<br/>

   ```
   if(score >= 90){
       grade = 'A'
   } else if (score in 80.0 .. 89.9){
       grade = 'B'
   } else if (score in 70.0 .. 79.9){
       grade = 'C'
   }
   ```

4. when문으로 다양한 조건 처리하기
   when문을 사용하면 더 편리하고 간단한 문장을 구성할 수 있음.<br/>
   when문은 함수처럼 인자가 있는 경우, 없는 경우 2가지임<br/>

   ```
   when(인자) {
       인자에 일치하는 값 혹은 표현식 -> 수행문
       인자에 일치하는 범위 -> 수행할 문장
       ...
       else -> 수행할 문장
   }
   ```

   when 블록의 안을 보면 화살표(->) 왼쪽에는 일치하는 값, 표현식, 범위로 조건을 나타내고,<br/>
   오른쪽에는 수행할 문장을 사용함<br/>

   ```
   when(x) {
       1 -> println("x === 1")
       2 -> println("x === 2")
       else -> {
           print("x는 1,2가 아닙니다.")
       }
   }
   ```

   만약 화살표 (->) 기호 오른쪽의 문장이 여러줄이라면 중괄호를 사용해 블록으로 구성할 수 있다.<br/>
   switch - case 문과 비슷하지만 break가 필요 없음<br/><br/>

   여러 조건을 한번에 표현하려면 콤마를 사용하면 된다.<br/>

   ```
   when(x) {
       0,1 -> print("x == 0 or x == 1")
       else -> print("기타")
   }
   ```

5. when문에 함수의 반환값 사용하기

   ```
   when(x) {
       parseInt(s) -> print("일치함")
       else -> print("기타")
   }
   ```

   parseInt() 함수는 인자로 지정된 s를 정수형으로 변환해 주는 함수.<br/>
   x의 값과 parseInt(s)의 반환값이 일치하면 print("일치함")을 수행함.

6. when문에 in 연산자와 범위 지정자 사용하기
