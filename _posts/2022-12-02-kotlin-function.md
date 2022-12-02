---
title: "Kotlin(5) - 함수와 함수형 프로그래밍(1)"
excerpt: "Kotlin Function"
toc: true
toc_sticky: true
categories:
  - Kotlin
tags:
  - Kotlin
date: "2022-12-02 16:30:00"
last_modified_at: "2021-12-02 17:00:00"
---

## 1. 함수 선언하고 호출하기

1. 함수란?

   함수는 여러 값(인자)을 입력받아 기능을 수행하고 결괏값 하나를 반환하는 코드의 모음<br/><br/>

   함수를 사용하는 이유는 코드를 재사용할 수 있기 때문이다.<br/>
   동일한 함수의 인자만 다른 것을 넣어주면서 함수를 재사용할 수 있음<br/>

2. 함수의 구조

   ```
   fun sum(a: Int, b: Int): Int{
       var sum = a + b
       return sum
   }
   ```

   우선 fun이라는 키워드로 함수 선언을 알린다.<br/>
   fun 키워드 뒤에 함수명을 써주고, 보통 카멜 표기법으로 지음<br/><br/>

   함수 이름 오른쪽에 소괄호를 두고 그 안에 매개변수를 정의함<br/>
   매개변수는 함수에서 입력 받는 변수를 말하고, 여러개 지정이 가능함.<br/>
   콜론(:)과 함께 자료형을 명시해줘야함.<br/>
   함수가 반환하는 값이 있다면 반환값의 자료형도 명시해줘야함<br/><br/>

   중괄호 안에 함수 호출시 실행할 코드를 적어주고, return에 수행 후 반환할 값을 적어준다.<br/>

   ```
   fun 함수명 (변수명: 자료형) : 반환 자료형 {
       함수 본문
       return 반환값
   }
   ```

   위에서 매개변수, 리턴 타입, 리턴 값 생략이 가능함<br/>
   함수 본문이 한줄일 경우 중괄호 생략 가능<br/>

   ```
   fun sum(a: Int, b: Int): Int = a + b
   ```

   위처럼 return 생략시 대입 연산자로 대체가 가능함.<br/>

   ```
   fun sum(a: Int, b: Int) = a + b
   ```

   반환값의 자료형이 명백하다면, 반환값의 자료형도 생략이 가능<br/>

3. 함수 호출과 프로그램의 실행 순서
   보통 함수를 사용하는 것을 **함수를 호출한다**라고 한다.<br/>

   ```
   fun sum(a: Int, b: Int): Int {
       var sum = a+b
       return sum
   }

   fun main() {
       val result1 = sum(3,2)
       val result2 = sum(6,7)

       println(result1)
       println(result2)
   }
   ```

   위의 코드의 실행 순서는,<br/>

   1. 프로그램의 진입점 main()함수
      프로그램을 실행하면 main()이 가장 먼저 실행됨.<br/>
   2. sum()함수 호출
      sum()함수들을 호출함, 처음으로 result1의 값이 될<br/> sum(3,2)가 호출되고 인자 3, 2가 a, b로 전달됨
   3. 값 전달
      sum()함수를 호출하면 흐름이 main()에서 sum()으로 넘어가고 sum()수행 후 3+2를 sum으로 반환하고 다시 함수의 흐름이 main()으로 돌아감 <br/>
   4. 나머지 반복

4. 함수 호출과 메모리
   프로그램이 실행되면 메모리에 프로그램을 위한 공간이 만들어짐<br/>

   ```
   fun main() {
       val num1 = 10
       val num2 = 3
       val result: Int

       result = max(num1, num2)
       println(result)
   }

   fun max(a: Int, b: Int) = if(a>b) a else b
   ```

   **함수와 스택 프레임**<br/>
   함수의 각 정보들은 프레임(Frame)이라는 정보로 스택(Stack) 메모리의 높은 주소부터 거꾸로 자라나듯 채워져 나간다.<br/><br/>
   위 코드에서 main()함수의 스택 프레임이 가장 먼저 생김<br/>
   main()의 스택 프레임에는 args, num1, num2, result를 위한 공간이 생긴다.<br/><br/>

   처음에 main() 스택 프레임이 생긴 후 num1, num2에 값이 들어감<br/>
   그 후 max()함수가 호출되고, max() 함수의 스택 프레임이 생성됨.<br/>
   max에서 a, b에 전달된 매개변수의 값이 들어가게 되고 연산 후 값을 반환하고 스택 프레임이 소멸됨<br/>
   반환값은 main() 스택 프레임의 result로 들어가게 됨.<br/>
   이후 반복함<br/><br/>

   여기서 스택 프레임은 함수가 호출되면 생성되고, 스택 프레임은 각각 분리되어 있음.<br/>
   이 프레임에서 분리된 변수들을 지역변수라고 하고 지역변수는 다른 스택 프레임에서는 해당 변수명으로 접근이 불가능함<br/><br/>

   함수를 호출하면 호출 순으로 스택에 스택 프레임이 생성되고, 생성한 반대 순서로 소멸됨<br/>

5. 반환값이 없는 함수
   함수의 반환값은 생략할 수 있는데, 예를 들어 두 인자를 그대로 출력하는 함수는 값을 반환하지 않아도 됨.<br/>
   즉 return문을 생략해도 되는데, 그 대신 반환값의 자료형을 Unit으로 지정하거나 생략이 가능함<br/>
   Unit은 코틀린에서 다루는 특수한 자료형으로, 반환값이 없을 때 사용<br/>

   ```
   fun printSum(a: Int, b: Int): Unit {
       println("sum of $a and $b is ${a + b}")
   }
   ```

   ```
   fun printSum(a: Int, b: Int) {
       println("sum of $a and $b is ${a + b}")
   }
   ```

   Unit은 자바의 void와는 다르니 주의해야함.<br/>

6. 매개변수 활용
   함수의 매개변수에는 기본값을 지정할 수 있음<br/>

   ```
   fun add(name: String, email: String = "default"){
       ...
   }
   ```

   위의 코드처럼 매개변수에 대입연산자를 사용해서 기본값을 넣어주면 됨.<br/>
   기본 값을 제공한 경우, 함수 호출시 해당 매개변수에 대한 인자를 생략해도 됨.<br/>

   ```
   add("Min") // 이런식으로 생략이 가능
   ```

   <br/>
   매개변수가 많은 함수를 호출하다가 어떤 인자를 어떤 매개변수에 전달했는지 헷갈리는 경우가 있을 수 있음<br/>
   이때 매개변수의 이름과 함께 인자를 전달할 수 있음<br/>

   ```
   fun main(){
       namedParam(x = 200, z = 100)
       namedParam(z = 150)
   }

   fun namedParam(x: Int = 100, y: Int = 200, z: Int){
       println(x+y+z)
   }
   ```

   위의 코드처럼 인자에 매개변수 명을 써주면 됨.<br/><br/>

   함수의 인자로 1,2,3을 전달하면 1,2,3이라 출력하고,<br/>
   1,2,3,4를 전달하면 1,2,3,4라고 출력하기 위해서는 **가변 인자**를 사용하면 됨<br/>
   가변인자를 사용하면 함수는 하나만 정의해 놓고 여러 개의 인자를 받을 수 있음<br/>

   ```
   fun main(){
       normalVarargs(1,2,3,4)
       normalVarargs(4,5,6)
   }

   fun normalVarargs(vararg counts: Int){
       for(num in counts){
           print("$num")
       }

       print("\n")
   }
   ```

   위의 코드처럼 매개변수 앞에 vararg라는 키워드를 붙여 가변 인자로 지정.<br/>
   이렇게 되면 해당 매개변수가 배열이 된다.<br/>

## 2. 함수형 프로그래밍

코틀린은 함수형 프로그래밍과 객체 지향 프로그래밍을 모두 지원하는 다중 패러다임 언어<br/>
두 프로그래밍의 장점은 코드를 간략하게 만들 수 있다는 것.<br/>

1. 함수형 프로그래밍
   함수형 프로그래밍은 순수 함수를 작성하여 프로그램의 부작용을 줄이는 프로그래밍 기법<br/>
   함수형 프로그래밍에서는 람다식과 고차함수를 사용함<br/><br/>

   **순수함수**<br/>
   어떤 함수가 같은 인자에 대하여 항상 같은 값을 반환하면 **부작용이 없는 함수** 라고 한다.<br/>
   그리고 부작용이 없는 함수가 함수 외부의 어떤 상태도 바꾸지 않는다면 **순수함수** 라고 한다.<br/>

   ```
   fun sum(a: Int, b: Int): Int{
    return a + b
   }
   ```

   위의 코드가 순수 함수의 예로, 동일 인자인 a,b를 입력받아 항상 a+b를 출력함<br/><br/>

   **람다식**<br/>

   ```
   {x,y -> x+y}
   ```

   위의 식을 보면 함수의 이름이 없고 화살표가 사용되었음<br/>
   수학에서 람대 대수는 이름이 없는 함수로 2개 이상의 입력을 1개의 출력으로 단순화 한다는 개념<br/>
   함수형 프로그래밍의 람다식은

   1. 다른 함수의 인자로 넘기는 함수
   2. 함수의 결과값을 반환하는 함수
   3. 변수에 저장하는 함수
      를 말함.<br/><br/>

   **일급 객체**<br/>
   함수형 프로그래밍에서는 함수를 일급 객체로 생각함<br/>

   1. 일급 객체는 함수의 인자로 전달할 수 있음
   2. 일급 객체는 함수의 반환값에 사용할 수 있음
   3. 일급 객체는 변수에 담을 수 있음
      위가 일급 객체의 특징인데, 람다식은 일급 객체의 특징을 가진 이름 없는 함수라고 할 수 있음<br/><br/>

   **고차함수**
   다른 함수를 인자로 사용하거나 함수를 결과값으로 반환하는 함수를 의미함<br/>

   ```
   fun main(){
       println(highFunc({x,y -> x+y}, 10, 20))
   }

   fun highFunc(sum: (Int, Int) -> Int, a: Int, b: Int): Int = sum(a,b)
   ```

   sum(a,b)는 람다식의 표현문에 따라 a+b를 반환합니다.<br/><br/>

   함수형 프로그래밍의 정의와 특징을 정리하면

   1. 순수 함수를 사용해야 한다.
   2. 람다식을 사용할 수 있다.
   3. 고차 함수를 사용할 수 있다.

## 3. 고차함수와 람다식

1. 고차 함수의 형태
   고차 함수는 인자나 반환값으로 함수를 사용함<br/>

   - 일반 함수를 인자나 반환값으로 사용하는 고차 함수

     ```
     fun main(){
         val res1 = sum(3,2)
         val res2 = mul(sum(3,3), 3)

         println("res1: $res1, res2: $res2)
     }

     fun sum(a: Int, b: Int) = a + b
     fun mul(a: Int, b: Int) = a * b
     ```

     위 코드는 함수의 인자로 함수를 사용한 예제<br/>

     ```
     fun main(){
         println("funcFunc: ${funcFunc()}")
     }

     fun sum(a: Int, b: Int) = a + b

     fun funcFunc(): Int{
         return sum(2, 2)
     }
     ```

     위 코드는 funcFunc() 함수의 반환값으로 sum()함수를 사용함.<br/>

   - 람다식을 인자나 반환값으로 사용하는 고차함수

     ```
     fun main(){
         val result: Int
         val multi = {x: Int, y: Int -> x * y}
         result = multi(10, 20)
         println(result)
     }
     ```

     multi에는 x와 y를 인자로 받아 곱하여 반환하는 람다식이 할당<br/>
     람다식이 변수에 할당되면 변수 이름으로 함수 형태로 사용이 가능함<br/>
     (multi() 이런식으로)<br/><br/>

     람다식을 자세히 보면<br/>
     람다식의 자료형 선언 = {람다식의 매개변수 -> 람다식의 처리 내용}<br/>
     이렇게 되는데, 람다식의 선언 자료형은 람다식 매개변수에 자료형이 명시된 경우 생략이 가능<br/>
     람다식의 매개변수의 자료형은 선언 자료형이 명시되어 있으면 생략이 가능함<br/>
     람다식의 처리 내용은 표현식이 여러 줄인 경우 마지막 표현식이 반환됨<br/>

     ```
     val multi2: (Int, Int) -> Int = {x: Int, y: Int ->
         println("x * y")
         x * y
     }
     ```

     생략 한 예제 <br/>

     ```
     val multi: {x: Int, y: Int -> x * y}
     val multi: (Int, Int) -> Int = {x, y -> x * y}
     ```

     반환자료형이 없가나 매개변수가 하나만 있을 경우 예제<br/>

     ```
     val greet: () -> Unit = {println("Hello World")}
     val square: (Int) -> Int = {x ->  x * x}
     ```

     람다식 안에 람다식을 넣은 예제<br/>

     ```
     val nestedLambda: () -> () -> Unit = {{println("nested")}}
     ```

     위의 두 코드 생략 예제<br/>

     ```
     val greet = {println("Hello World")} // 추론 가능
     val square = {x: Int -> x * x} // x의 자료형을 명시했기 때문에 생략 가능
     val nestedLambda = {{println("nested")}}
     ```

     람다식을 매개변수에 사용한 고차함수 예제<br/>

     ```
     fun main(){
         var result: Int
         result = highOrder({x, y -> x + y}, 10, 20)
         println(result)
     }

     fun highOrder(sun: (Int, Int) -> Int, a: Int, b: Int) : Int {
         return sum(a, b)
     }
     ```

## 4. 람다식과 고차 함수 호출하기

함수의 내용을 할당하거나 인자 혹은 반환값을 자유롭게 넘기려면 호출 방법을 이해해야 한다.<br/>
기본형 변수로 할당된 값은 스택에, 참조형 변수로 할당된 객체는 참조 주소가 스택, 객체는 힙에 존재.<br/>
각각 함수에 전달할 때, 기본형은 값이 전달되고 참조형은 주소가 전달됨<br/><br/>

JVM에서 실행되는 자바나 코틀린은 함수를 호출할 때 인자의 값만 복사하는 **값의 의한 호출**이 일반적임<br/>
자바는 객체가 전달될 때 주소 자체를 전달하는 것이 아닌 값을 복사하는데, 참조에 의한 호출처럼 보이지만 그 값이 주소일 뿐임<br/>

1. 값에 의한 호출
   코틀린에서 값에 의한 호출은 함수가 또 다른 함수의 인자로 전달된 경우 람다식 함수는 값으로 처리되어 그 즉시 함수가 수행된 후 값을 전달함<br/>

   ```
   fun main() {
       val result = callByValue(lambda())
       println(result)
   }

   fun callByValue(b: Boolean): Boolean {
       println("callByValue function")
       return b
   }

   val lambda: () -> Boolean = {
       println("lambda function")
       true
   }
   ```

   위 예제의 경우 callByValue(lambda())가 호출됨과 동시에 인자로 전달된 lambda()가 먼저 수행되고 lambda()의 수행문을 먼저 수행하고 true를 반환해서 true가 callbyValue 함수의 인자로 전달된 후 callByValue를 수행하고 값을 반환함<br/>

2. 이름에 의한 람다식 호출
   람다식의 이름이 인자로 전달될 때 실행되지 않고 실제로 호출될 때 실행되도록 하는 예제

   ```
   fun main() {
       val result = callByName(otherLambda)
       println(result)
   }

   fun callByName(b: () -> Boolean): Boolean {
       println("callByValue function")
       return b()
   }

   val otherLambda: () -> Boolean = {
       println("otherLambda function")
       true
   }
   ```

   위 코드의 경우 callByName()함수의 매개변수 b는 람다식 자료형으로 선언되어 있음<br/>
   즉, 람다식 자체가 매개변수 b에 복사되어 있고, callByName()함수를 수행하면서 b()처럼 사용되기 전까지 람다식이 실행되지 않음<br/>

3. 다른 함수의 참조에 의한 일반 함수 호출
   람다식이 아닌 일반 함수를 또 다른 함수의 인자에서 호출하는 고차함수 예제<br/>

   ```
   fun main() {
       val res1 = funcParam(3,2,::sum)
       println(res1)

       hello(::text)

       val likeLambda = ::sum
       println(likeLambda(6,6))
   }

   fun sum(a: Int, b: Int) = a + b

   fun text(a: String, b: String) = "Hi $a, $b"

   fun funcParam(a: Int, b: Int, c: (Int, Int) -> Int): Int {
       return c(a,b)
   }

   fun hello(body: (String, String) -> String): Unit {
       println(body("Hello", "World"))
   }
   ```

   위에서 sum() 함수는 람다식이 아니므로 이름으로 바로 호출이 불가능함.<br/>
   하지만 sum(), funcParam()의 매개변수 c의 선언부 구조를 보면 인자 수와 자료형의 개수가 동일함<br/>
   이때 ::기호를 함수 이름 앞에 사용해 소괄호와 인자를 생략하고 사용할 수 있음

   ```
   hello(::text)
   hello({a,b -> text(a,b)})
   hello{a,b -> text(a,b)}
   ```

   위의 세 코드가 모두 동일한 결과를 나타냄<br/>

## 5. 람다식의 매개변수

매개변수 개수에 따라 람다식을 구성하는 방법<br/>
매개변수와 인자 개수에 따라 람다식의 생략된 표현이 가능함<br/>

1. 람다식에 매개변수가 없는 경우

   ```
   fun main() {
       noParam({"Hello World"})
       noParam{"Hello World"}
   }

   fun noParam(out: () -> String) = println(out())
   ```

   위 코드의 noParam() 함수의 매개변수는 람다식 1개를 가지고 있는데, 이때는 함수 사용 시 소괄호를 생략할 수 있음.<br/>
   main에서 noParam() 함수의 인자에는 람다식 표현식인 {"..."}형태의 인자가 있음<br/>
   이 람다식에는 매개변수가 없으므로 화살표 기호가 사용되지 않고 소괄호 생략이 가능<br/>

2. 람다식에 매개변수가 1개 있는 경우
   람다식에 매개변수가 1개 있을 경우에는 람다식에 화살표 기호 왼쪽에 필요한 변수를 써줘야함<br/>

   ```
   fun main() {
       ...
       oneParam({a -> "Hello World! $a"})
       oneParam{a -> "Hello World! $a"}
       oneParam{"Hello World! $it"}
   }
   ...

   fun oneParam(out: (String) -> String){
       println(out("OneParam"))
   }
   ```

   매개변수가 1개 들어간 람다식을 구성할 때는 변수와 화살표를 추가해야됨.<br/>
   매개변수가 1개인 경우에는 화살표 표기를 생략하고 it으로 대체할 수 있음<br/>

3. 람다식의 매개변수가 2개 이상인 경우

   ```
   fun main() {
       moreParam {a,b -> "Hello World! $a $b"}
   }

   fun morParam(out: (String, String) -> String){
       println(out("OneParam", "TwoParam"))
   }
   ```

   2개 이상인 경우에는 it을 사용해 변수를 생략할 수 없음<br/>
   특정 람다식의 매개변수를 사용하고 싶지 않을 때는 이름 대신 언더스코어로 대체 가능<br/>

4. 일반 매개변수와 람다식 매개변수를 같이 사용하기

   ```
   fun main() {
       withArgs("Arg1", "Arg2", {a,b->"Hello World! $a $b"})
       withArgs("Arg1", "Arg2"){a,b->"Hello World! $a $b"}
   }

   fun withArgs(a: String, b: String, out: (String, String) -> String){
       println(out(a,b))
   }

   ```

   마지막 매개변수가 람다식 함수라면 소괄호 밖에 써도 된다.<br/>

5. 일반함수에 람다식 매개변수를 2개 이상 사용하기
   일반 함수의 매개변수에 람다식을 2개 이상 사용할 때는 소괄호를 생략할 수 없음<br/>

   ```
   fun main() {
       twoLambda({a,b -> "First $a $b"}, {"Second $it"})
       twoLambda({a,b -> "First $a $b"}) {"Second $it"}
   }

   fun twoLambda(first: (String, String) -> String, second: (String) -> String){
       println(first("OneParam", "TwoParam"))
       println(second("OneParam"))
   }
   ```

   위 코드처럼 소괄호 생략은 불가능하지만, 마지막 매개변수가 람다식일 경우 하나는 밖으로 뺄 수 있음.
