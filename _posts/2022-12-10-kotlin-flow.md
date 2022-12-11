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
   when문 안에서도 in 연산자와 범위 연산자를 사용할 수 있음.<br/>

   ```
   when(x){
       in 1..10 -> print("x는 1 이상 10 이하입니다.")
       !in 10..20 -> print("x는 10 이상 20 이하의 범위에 포함되지 않습니다.")
       else -> print("x는 어떤 범위에도 없습니다.")
   }
   ```

   in 연산자 앞에 느낌표를 사용하면 해당 범위 이외의 요소를 가리키게 됨.<br/>

7. when과 is 키워드 함께 사용하기
   is 키워드를 사용하면 특정 자료형을 검사할 수 있음.<br/>
   when문과 is 키워드를 사용하면 어떤 변수에 할당된 값의 자료형을 검사하여 자료형에 따라
   문장을 실행하도록 프로그램을 구성할 수 있음<br/>

   ```
   val str = "안녕하세요"
   val result = when(str) {
       is String -> "문자열입니다."
       else -> false
   }
   ```

   변수 str의 자료형이 String인 경우 뒤에 있는 문자열을 result에 할당함.<br/>

8. 인자가 없는 when문
   when문에 인자가 주어지지 않으면 else if문처럼 각각의 조건을 실행할 수 있음 <br/>

   ```
   fun main(){
       print("Enter the score: ")
       var score = readLine()!!.toDouble()
       var grade: Char = 'F'

       when {
           score >= 90.0 -> grade = 'A'
           score in 80.0..89.9 -> grade = 'B'
           score in 70.0..79.9 -> grade = 'C'
           score < 70.0 -> grade = 'F'
       }

       println("Score: $score, Grade: $grade")
   }
   ```

   이런식으로 인자를 두지 않는 경우 조건이나 표현식을 직접 만들어서 쓸 수 있음<br/>

9. 다양한 자료형의 인자 받기

   ```
   fun main() {
       cases("Hello")
       cases(1)
       cases(System.currentTimeMillis())
       cases(MyClass())
   }

   fun cases(obj: Any){
       when(obj){
           1 -> println("Int: $obj")
           "Hello" -> println("String: $obj")
           is Long -> println("Long: $obj")
           !is String -> println("Not a String")
           else -> println("Unknown")
       }
   }
   ```

## 2. 반복문

반복문은 반복문 블록 안에 있는 코드를 반복하여 실행하는 명령문.<br/>

1. for문
   변수를 선언하고 조건식에 따라 변수 값을 반복해서 증감하는 구문.<br/>
   java와는 다르게 in 연산자를 사용해 for문을 사용<br/><br/>

   for문은 변수의 값이 범위 안에 있다면 계속해서 for의 본문을 수행함<br/>
   범위 외의 값이 되면 for의 본문을 탈출함<br/>

   ```
   for(x in 1..5){
       println(x)
   }
   ```

   위 처럼 본문이 한줄이라면 중괄호 생략도 가능함.<br/>

2. 하행, 상행 및 다양한 반복 방법
   하행<br/>

   ```
   for (i in 5 downTo 1) print(i)
   ```

   2단계 증가<br/>

   ```
   for (i in 1..5 step 2) print(i)
   ```

3. while 문
   while문은 조건식이 true를 만족하는 경우 while문의 블록을 무한히 반복.<br/>
   조건식이 false가 되면 실행문이 중단되어 루프를 빠져나감.<br/>

   ```
   var i = 1
   while (i<=5){
       println("$i")
       ++i
   }
   ```

4. do~while문
   while은 조건식을 먼저 검사한 후 반복을 진행하기 때문에 조건식이 false인 경우
   작업이 한번도 진행되지 않음<br/>
   do~while의 경우 do블록에 작성한 본문을 한 번 실행한 다음 마지막에 조건식을 검사해
   true가 나오면 작업을 반복함<br/>

   ```
   fun main () {
       do{
           print("Enter an integer: ")
           val input = readLine()!!.toInt()

           for(i in 0..(input - 1)){
               for(j in 0..(input-1)) print((i+j) % input + 1)
               println()
           }
       } while(input != 0)
   }
   ```

## 3. 흐름의 중단과 반환

1. return 문
   보통 return문은 값을 반환하는데 사용<br/>

   ```
   fun add(a: Int, b: Int): Int {
       return a + b
       println("이 코드는 실행되지 않음")
   }
   ```

   return 이후의 코드는 실행되지 않는데, return이 사용되면 코드 흐름을 중단하고
   함수의 역할을 끝내기 때문<br/>
