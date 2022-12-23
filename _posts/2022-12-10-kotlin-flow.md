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

   when 블록의 안을 보면 화살표(->) 왼쪽에는 일치하는 값, 표현식, 범위로 조건을 나타내고, 오른쪽에는 수행할 문장을 사용함<br/>

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

2. return으로 Unit 반환하기
   리턴하는 값 없이 return만 사용하게 될 경우, Unit이라는 자료형 자체를 반환한다.<br/>

   ```
   //1. Unit을 명시적으로 반환
   fun hello(name: String): Unit {
       println(name)
       return Unit
   }

   //2. Unit 이름을 생략한 반환
   fun hello(name: String): Unit {
       println(name)
       return
   }

   //3. return문 자체를 생략
   fun hello(name: String) {
       println(name)
   }
   ```

   위 세개의 함수들은 모두 동일한 결과를 반환함.<br/>

3. 람다식에서 return 사용하기
   인라인(inline)으로 선언되지 않은 람다식에서는 return을 그냥 사용할 수 없음.<br/>
   return@label과 같이 라벨(label) 표기와 함께 사용해야 한다.<br/>
   라벨이란 코드에서 특정한 위치를 임의로 표시한 것으로, @ 기호와 이름을 붙여서 사용한다.<br/>
   인라인으로 선언된 함수에서 람다식을 매개변수로 사용하면 람다식에서 return을 사용할 수 있음.<br/>

   ```
   fun main(){
    retFunc()
   }

   inline fun inlineLambda(a: Int, b: Int, out: (Int, Int) -> Unit) {
    out(a,b)
   }

   fun retFunc() {
    println("start of retFunc") //1
    inlineLambda(13,3) { a,b -> //2
        val result = a + b
        if(result > 10) return //3
        println("result: $result") //4
    }
    println("end of retFunc") //5
   }
   ```

   inlineLambda() 함수는 람다식을 매개변수로 사용하는 인라인 함수.<br/>
   retFunc() 함수가 호출되어 실행될 때 1번의 내용을 출력하고, 2번으로 진입해 람다식을 인자로 사용하고 있음.<br/>
   만약 a와 b인자의 합이 10을 넘는 경우에는 return을 사용한다.<br/>
   return이 호출되면 람다식 바깥의 retFunc() 함수까지 빠져 나가게 된다.<br/>
   4,5번은 실행되지 않고, 이런 반환을 비지역 반환이라고 한다.<br/>

4. 람다식에서 라벨과 함께 return 사용하기
   비지역 반환을 방지하고 가장 가까운 함수인 retFunc() 함수 위치로 빠져나가게 하려면 어떻게 해야하는가.<br/>
   람다식에서 라벨을 정의해 return을 사용해야 한다.<br/>
   라벨을 지정하기 위해서는 @ 기호를 라벨 뒤에 붙여 **라벨이름@**과 같이 지정하고, 사용할 때는 앞부분에 **return@라벨이름** 으로 지정함.<br/>

   ```
   fun main(){
       retFunc()
   }

   fun inlineLambda(a: Int, b: Int, out: (Int, Int) -> Unit) { // inline 제거
       out(a,b)
   }

   fun retFunc(){
       println("start of retFunc")
       inlineLambda(13, 3) lit@{a,b -> // 1
           val result = a + b
           if(result > 10) return@lit //2
           println("result: $result")
       } //3

       println("end of retFunc")//4
   }
   ```

   위 코드의 inlineLambda() 함수는 코드 앞에 inline을 삭제했으므로 이제 인라인 함수가 아님.
   따라서 inline을 지운 순간 return에 오류가 생긴다.<br/>
   이때 1번처럼 람다식 블록 앞에 라벨(lit@)을 정의함. 그리고 2번처럼 return에 @으로 시작하는 라벨(@lit)을 붙여줌.<br/>
   그러면 오류가 사라지고 result가 10보다 큰 경우 3으로 빠져나온다.<br/>

5. 암묵적 라벨
   람다식 표현식 블로게 직접 라벨을 쓰는 것이 아닌 람다식의 명칭 그대로를 라벨처럼 사용할 수 있는데, 이것을 암묵적 라벨이라고 함.<br/>

   ```
   fun retFunc() {
       println("start of retFunc")
       inlineLambda(13,3) {a,b ->
           val result = a + b
           if(result > 10) return@inlineLambda
           println("result: $result")
       }

       println("end of retFunc")
   }
   ```

   위의 코드처럼 람다식의 이름을 직접 라벨처럼 사용할 수 있음.<br/>

6. 익명 함수를 사용한 반환
   람다식 대신 익명함수를 넣을 수도 있음.<br/>
   이 때는 라벨을 사용하지 않고도 가까운 익명함수 자체가 반환되므로 위와 동일한 결과를 가질 수 있음.<br/>

   ```
   fun retFunc() {
       println("start of retFunc")
       inlineLambda(13, 3, fun (a,b) {
           val result = a + b
           if(result > 10) return
           println("result: $result")
       })
       }
       println("end of retFunc")
   }
   ```

   익명 함수는 fun(...){...} 형태로 이름 없이 특정 함수의 인자로 넣을 수 있음.<br/>
   이때는 일반 함수처럼 작동하기 때문에 return도 일반 함수에서 반환되는 것과 같이 사용할 수 있음<br/>
   람다식의 방법으로 return을 사용하려면 아래처럼 사용하면 된다.<br/>

   ```
   val getMessage = lambda@{num: Int ->
       if(num !in 1..100){
           return@lambda "Error" //라벨을 통한 반환
       }
       "Success" //마지막 식 반환
   }
   ```

   위의 코드를 익명함수로 바꾼 예제(아래)

   ```
   val getMessage = fun(num: Int): String {
       if(num !in 1..100){
           return "Error"
       }
       return "Success"
   }
   ...
   var result = getMessage(99)
   ```

   익명함수를 사용하면 2개의 return이 확실하게 구별된다.<br/>
   보통의 경우 람다식을 사용하고 return과 같이 명시적으로 반환해야 할 것이 여러 개라면 익명 함수를 쓰는게 좋다.<br/>

7. 람다식과 익명 함수를 함수에 할당할 때 주의할 점
   람다식은 특정 함수에 할당할 때 주의하며 사용해야 한다.<br/>
   익명 함수와 람다식은 할당하는 방법에서 약간의 차이가 있는데, 읽기에 따라 문제가 생길 수 있음.<br/><br/>

   먼저 함수에 람다식을 할당한다고 할 때는,<br/>

   ```
   fun greet() = {println("Hello")}

   greet()
   ```

   greet() 함수를 실행하면 Hello가 출력되지 않고 아무것도 출력되지 않는다.<br/>
   할당 연산자(=)에 의해 람다식 {println("Hello")} 자체가 greet()함수에 할당된 것 뿐임.<br/>
   greet()함수가 가지고 있는 함수를 사용하려면 다음과 같이 표기해야 한다.<br/>

   ```
   greet()()
   ```

   함수가 할당됨을 명시적으로 표현하려면 익명 함수를 써서 선언하는 것이 읽기에 더 좋을 수 있음.<br/>

   ```
   fun greet() = fun() {println("Hello")}
   ```

   결과는 동일하게 "Hello"를 반환함.<br/>

8. break문과 continue문
   break는 해당 키워들르 사용한 지점에서 반복문을 빠져나오게 된다.<br/>
   continue는 이후 본문을 계속 진행하지 않고 다시 반복 조건을 살펴봄

9. 예외처리
   프로그램 코드를 작성하다 보면 해당 코드가 제대로 작동하지 모하고 중단되는 현상이 발생할 수 있음.<br/>
   이걸 예외라고 하고 예외를 발생시키는 상황으로는,<br/>

   1. 운영체제의 문제
   2. 입력값의 문제
   3. 받아들일 수 없는 연산
   4. 메모리의 할당 실패 및 부족
   5. 컴퓨터 기계 자체의 문제
      등이 있다.<br/><br/>

   따라서 프로그램을 실행할 때 발생할 수 있는 예외에 대비해야 하는데, 이를 예외 처리라고 한다.<br/>
   잠재적으로 예외가 발생할 수 있는 코드를 try-catch문으로 감싸 놓고, try블록에서 예외가 발생하면 catch 블록에서 잡아서 예외를 처리함.<br/>
