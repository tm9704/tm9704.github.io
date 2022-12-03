---
title: "Kotlin(6) - 함수와 함수형 프로그래밍(2)"
excerpt: "Kotlin Function"
toc: true
toc_sticky: true
categories:
  - Kotlin
tags:
  - Kotlin
date: "2022-12-03 15:40:00"
last_modified_at: "2021-12-03 16:40:00"
---

## 1. 고차 함수와 람다식의 사례

그냥 여기서는 이정도 있다 정도만 알고 넘어가겠음<br/>

1. 동기화를 위한 코드 구현
   동기화란 변경이 일어나면 안 되는 특정 코드를 보호하기 위한 잠금 기법<br/>
   동기화로 보호되는 코드는 임계영역(Critical Section)이라고도 부른다.<br/><br/>

   보통 프로그래밍에서는 특정 공유 자원에 접근한다고 했을 때, 공유 자원이 여러 요소에 접근해서 망가지는 것을 막기 위해 임계 영역의 코드를 잠가 두었다가 사용한 후 풀어주어야 한다.<br/>

   ```
   Lock lock = new ReentrantLock();
   lock.lock(); // 잠금
   try{
       //보호할 임계영역 코드
       //수행할 작업
   }finally{
       lock.unlock() //해제
   }
   ```

   위 코드는 Lock 을 활용해 임계 영역을 보호함<br/>
   먼저 lock()을 통해 Lock을 걸고 보호하려는 코드는 try블록(임계 영역)에 둔다.<br/>
   임계 영역의 코드가 끝나면 반드시 finally 블록에서 unlock()을 통해 잠금을 해제해야 한다.<br/><br/>

   위 코드를 특정 함수를 보호하기 위한 고차 함수를 만들고 활용한 코드

   ```
   fun<T> lock(reLock: ReentrantLock, body: () -> T): T{
       reLock.lock()
       try{
           return body()
       }finally{
           reLock.unlock()
       }
   }
   ```

   잠금을 위한 lock() 함수를 fun<T> lock() 형태인 제네릭 함수로 설계하고 있음<br/>
   제네릭은 데이터 자료형을 일반화해서 어떤 자료형이던 지정할 수 있는 자료형<br/><br/>

   try{...} finally{...} 블록의 실행과정을 간단히 보면,<br/>
   try 블록의 모든 내용이 처리된 후 finally블록을 처리함.<br/>
   만약 try블록에 문제가 발생해도 finally 블록은 항상 수행됨.<br/>
   보통 try에서 메모리 할당이나 파일 열기 같은 작업을 한다면 finally 에서 무언가를 해제하거나 닫는 처리를 작성함<br/><br/>

   사용하는 공유 자원을 보호하기 위한 코드

   ```
   import java.util.concurrent.locks.ReentrantLock

   var sharable = 1 //보호가 필요한 공유 자원

   fun main() {
       val reLock = RentrantLock()

       lock(reLock, {criticalFunc()}) // 1
       lock(reLock){criticalFunc()} // 2
       lock(reLock, ::criticalFunc) // 3

       println(sharable)
   }

   fun criticalFunc(){
       // 공유 자원 접근 코드 사용
       sharable += 1
   }

   fun <T> lock(reLock: ReentrantLock, body: () -> T): T{
       reLock.lock()
       try{
           return body()
       }finally{
           reLock.unlock()
       }
   }
   ```

   main() 함수 바깥에 전역 변수로 선언되어 있는 sharable 변수는 여러 루틴에서 접근할 수 있음<br/>
   즉, 해당 변수에 특정 연산을 하고 있을 때는 보호가 필요<br/>
   이 공유 자원에 1씩 더하는 특정 연산을 하는 함수로 criticalFunc()이 선언되어 있는데, 이 함수를 보호해야 한다.<br/>
   보호를 위해서 제네릭 함수인 lock() 함수를 구현함. 이 함수에 criticalFunc()을 두번째 인자로 넘겨서 처리하면 람다식 함수의 매개변수 body에 의해 잠금 구간에서 공유 자원인 sharable 변수가 다른 루틴의 방해 없이 안전하게 처리됨.<br/>

2. 네트워크 호출 구현
   네트워크로부터 무언가를 호출하고 성공하거나 실패했을 때 특정 콜백 함수를 처리하는 프로그램<br/>
   콜백함수는 특정 이벤트가 발생하기까지 처리되지 않다가 이벤트가 발생하면 즉시 호출되어 처리되는 함수. 즉 사용자가 아닌 시스템이나 이벤트에 따라 호출 시점을 결정함<br/><br/>

   -> 이부분은 다음에

## 2. 코트린의 다양한 함수 알아보기

1. 익명 함수
   익명 함수는 일반 함수이지만 이름이 없는 함수.<br/>

   ```
   fun(x: Int, y: Int): Int = x + y
   ```

   함수 선언 키워드 fun이 존재하지만 이름이 없는게 특징.<br/>
   변수 선언에 그대로 사용할 수 있음<br/>

   ```
   val add: (Int, Int) -> Int = fun(x,y) = x + y
   val result = add(10, 2)
   ```

   이 익명 함수에서 선언 자료형을 람다식 형태로 써 주면 변수 add는 람다식 함수처럼 add()와 같이 사용할 수 있음.<br/>
   만약 매개변수에 자료형을 써 주면 선언부에 자료형은 생략할 수 있음

   ```
   val add = fun(x: Int, y: Int) = x + y
   ```

   람다식 표현법과 매우 유사하며 다음과 같이 동일하게 만들 수 있음<br/>

   ```
   val add = {x: Int, y: Int -> x + y}
   ```

   굳이 람다식을 안쓰고 익명함수를 쓰는 이유는 람다식에서는 return, break, continue처럼 제어문을 사용하기 어려움.<br/>
   함수 본문 조건식에 따라 함수를 중단하고 반환해야 하는 경우에는 익명 함수를 사용해야 함<br/>

2. 인라인 함수
   인라인 함수는 이 함수가 호출되는 곳에서 함수 본문의 내용을 모두 복사해 넣어 함수의 분기 없이 처리되기 때문에 코드의 성능을 높일 수 있음<br/>
   인라인 함수의 코드 자체가 복사되어 들어가기 때문에 보통은 짧게 작성함<br/>
   인라인 함수는 람다식 매개변수를 가지고 있는 함수에서 동작함<br/>

   ```
   fun main() {
       shortFunc(3) {println("First call: $it")}
       shortFunc(3) {println("Second call: $it")}
   }

   inline fun shortFunc(a: Int, out: (Int) -> Unit){
       println("Before calling out()")
       out(a)
       println("After calling out()")
   }
   ```

   shortFunc() 함수가 2번 호출되는 것으로 보이지만 함수 내용이 복사되어 들어감<br/>

3. 인라인 함수 제한하기
   인라인 함수의 매개변수로 사용한 람다식의 코드가 너무 길거나 인라인 함수의 본문 자체가 너무 길면 컴파일러에서 성능 경고를 할 수 있음.<br/>
   그리고 인라인 함수가 너무 많이 호출되면 코드 양이 늘어나 오히려 좋지 않을 수 있음<br/>

   ```
   inline fun sub(out1: () -> Unit, out2: () -> Unit){...}
   ```

   위 코드의 경우 out1, out2의 람다식이 그대로 복사되어 들어가기 때문에 코드의 양이 많아짐<br/>
   일부 람다식을 인라인되지 않게 하기 위해서는 noinline 키워들를 사용해야함<br/>

   ```
   inline fun sub(out1: () -> Unit, noinline out2: () -> Unit){...}
   ```

   noinline이 잇는 람다식은 인라인으로 처리되지 않고 분기하여 호출함

4. 인라인 함수와 비지역 반환
   코틀린에서는 익명 함수를 종료하기 위해서 return을 사용할 수 있음.<br/>
   이 때는 특정 반환값 없이 return만을 사용해야 함<br/><br/>

   인라인 함수에서 사용한 람다식에서는 return문을 사용할 수 있음.<br/>

   ```
   fun main() {
       shortFunc(3){
           println("First call: $it")
           return //1
       }
   }

   inline fun shortFunc(a: Int, out: (Int) -> Unit){
       println("Before calling out()")
       out(a)
       println("After calling out()") //2
   }
   ```

   out(a)는 인라인되어 대체되기 때문에 1번의 return문까지 포함됨.<br/>
   따라서 2번의 println("After calling out()") 문장은 실행되지 않음<br/>
   이러한 반환을 비지역반환이라고 부름<br/><br/>

   만약 shortFunc()가 inline 키워드로 선언되지 않으면 return문은 람다식 본문에 사용할 수 없으므로 return문을 허용할 수 없다는 오류가 남<br/>
   그 밖에 out()을 직접 호출해 사용하지 않고 또 다른 함수에 중첩하면 실행 문맥이 달라지므로 return을 사용할 수 없음.<br/>
   이때 비지역 반환을 금지하는 방법이 있음.<br/>

   ```
   fun main(){
       shortFunc(3){
           println("First call: $it")
           // return 사용 불가
       }
   }

   inline fun shortFunc(a: Int, crossinline out: (Int) -> Unit){
       println("Before calling out()")
       nestedFunc{out(a)}
       println("After calling out()")
   }

   fun nestedFunc(body: () -> Unit){
       body()
   }
   ```

   crossinline 키워드는 비지역 반환을 금지해야 하는 람다식에 사용함.<br/>
   위와 같이 문맥이 달라져 인라인이 되지 않는 중첩된 람다식 함수는 nestedFunc() 함수 때문에 return을 금지해야 한다.<br/>
   그래서 crossinline을 사용하면 람다식에서 return문이 사용되었을 때 코드 작성 단계에서 오류를 보여줘 잘못된 비지역 반환을 방지할 수 있음.<br/>

5. 확장 함수
   클래스에는 다양한 함수가 정의되어 있음.<br/>
   이것을 클래스의 멤버 메서드라고 부름.<br/>
   기존의 멤버 메서드는 아니지만 기존의 클래스에 내가 원하는 함수를 하나 더 포함시켜 확장하고 싶을 때 **확장 함수**를 사용할 수 있음<br/>

   ```
   fun 확장 대상.함수 이름(매개변수, ...): 반환값{
       ...
       return 값
   }
   ```

   최상위 클래스인 Any에 확장 함수를 구현하면 모든 클래스에서 확장함수를 추가할 수 있음. 코틀린의 최상위 요소는 Any이기 때문에 여기에 확장함수를 추가하면 코틀린의 모든 요소에 상속되기 때문.<br/>

6. 중위 함수
   중위 표현법은 클래스의 멤버를 호출할 때 사용하는 점(.)을 생략하고 함수 이름 뒤에 소괄호를 붙이지 않아 직관적인 이름을 사용할 수 있는 표현법.<br/>
   즉, 중위 함수란 일종의 연산자를 구현할 수 있는 함수를 말함.<br/>
   중위 함수를 사용하려면 3가지 조건이 필요함<br/>

   1. 멤버 메서드 또는 확장 함수여야함
   2. 하나의 매개변수를 가져야함
   3. infix 키워드를 사용하여 정의

   ```
   fun main() {
    //일반 표현법
    //val multi = 3.multiply(10)

    //중위 표현법
    val multi = 3 multiply 10
    println("multi: $multi")
   }

    // Int를 확장해서 multiply() 함수를 하나 더 추가함
   infix fun Int.multiply(x: Int): Int { //infix로 선언되므로 중위 함수
    return this * x
   }
   ```

   위의 코드에서 multiply() 함수는 하나의 매개변수를 가진 Int의 확장함수.<br/>
   infix 키워드를 사용해 중위 함수임을 선언했기 때문에 연산자처럼 표현할 수 있다.<br/>

7. 꼬리 재귀 함수
   => 나중에

## 3. 함수와 변수의 범위

함수는 실행블록({})을 가지고 있음. 함수의 시작을 알리는 중괄호 시작 기호에서 함수가 실행되며 중괄호가 끝나는 지점에서 함수가 종료되면서 가지고 있는 지역 변수를 반환함<br/>
이와 마찬가지로 블록 내에 또 다른 함수를 정의해 넣으면 지역 함수가됨<br/>
단, 지역함수를 사용할 때는 항상 지역 함수를 먼저 선언해야 함<br/><br/>

지역 함수도 마찬가지로 블록이 끝나면 같이 삭제됨<br/>

1. 함수의 범위
   **최상위 함수와 지역함수** <br/>
   코틀린에서는 파일을 만들고 곧바로 main()함수나 사용자 함수를 만들 수 있음.<br/>
   이것을 최상위 함수라고 함<br/>
   함수 안에 또 다른 함수가 선언되어 있는 경우에는 지역함수 라고 함<br/>

   ```
   fun main(){
       ...
       fun secondFunc(a: Int){
           ...
       }

       userFunc(4) // 사용자 함수 사용 - 선언부 위치 관계 x
       secondFunc(2) //지역함수 -  선언부가 먼저 나와야함
   }

   fun userFunc(counts: Int){
       ...
   }
   ```

   사용자가 만든 최상위 함수 main() 함수의 앞이나 뒤에 선언해도 main()함수 안에서 함수를 사용하는데 아무런 지장이 없음<br/>
