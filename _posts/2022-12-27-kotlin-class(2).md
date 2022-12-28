---
title: "Kotlin(9) - 클래스와 객체(2)"
excerpt: "Kotlin Class & Object"
toc: true
toc_sticky: true
categories:
  - Kotlin
tags:
  - Kotlin
date: "2022-12-27 09:40:00"
last_modified_at: "2022-12-27 10:40:00"
---

## 1. super와 this의 참조

클래스를 상위와 하위 클래스로 설계하다 보면 상위와 현재 클래스의 특정 메서드나 프로퍼티, 생성자를 참조해야 하는 경우가 생긴다.<br/>
상위 클래스는 super 키워드로, 현재 클래스는 this 키워드로 참조가 가능하다.<br/>

1. super로 상위 객체 참조하기
   메서드를 오버라이딩하려고 할 때 상위 클래스에서 구현한 내용을 그대로 사용하고 거기에 필요한 내용만 추가하고 싶을 경우, 상위 클래스를 가리키는 super 키워드를 사용한다.<br/>
   super를 사용하면 상위 클래스의 프로퍼티나 메서드, 생성자를 사용할 수 있다.<br/>

   ```
   open class Bird(var name: String, var wing: Int, var beak: String, var color: String){
       fun fly() = println("Fly wing: $wing")
       open fun sing(vol: Int) = println("Sing vol: $vol")
   }

   class Parrot(name: String, wing: Int = 2, beak: String, color: String,
               var language: String = "natural") : Bird(name, wing, beak, color) {
       fun speak() = println("Speak! $language")

       override fun sing(vol: Int){ // 1
           super.sing(vol)
           println("I'm a parrot! The volume level is $vol")
           speak()
       }
   }
   ```

   1번처럼 Parrot 클래스의 sing() 메서드는 오버라이딩 되었다. 여기에 super.sing()을 호출해 상위 클래스인 Bird 클래스의 sing() 메서드를 먼저 실행할 수 있다.<br/>
   이후 Parrot 클래스에서 재정의한 sing()을 확장해서 println()과 speak를 호출했다.<br/>
   이처럼 일부는 상위 클래스의 동작을, 일부는 현재 클래스에서 새롭게 재정의할 수 있다.<br/>
   상위 클래스의 동작을 위해 super.메서드()와 같이 호출하거나, 상위 클래스의 프로퍼티를 super.프로퍼티로 참조해서 사용하거나, super()를 사용해 상위 클래스의 생성자를 호출할 수 있다.<br/>

2. this로 현재 객체 참조하기
   this로는 현재 객체를 참조할 수 있다.<br/>

   ```
   open class Person {
       constructor(firstName: String) {
           println("[Person] firstName: $firstName")
       }
       constructor(firstName: String, age: Int) { // 3
           println("[Person] firstName: $firstName, $age")
       }
   }

   class Developer : Person {
       constructor(firstName: String): this(firstName, 10) { // 1
           println("[Developer] $firstName")
       }
       constructor(firstName: String, age: Int): super(firstName, age) { // 2
           println("[Developer] $firstName")
       }
   }

   fun main(){
       val sean = Developer("Sean")
   }
   ```

   main() 함수에서 Developer("Sean")을 통해 객체를 생성하고 있다. 이때 인자가 1개이므로 먼저 Developer 클래스의 1번 생성자로 진입한다. 코드를 실행하기 전에 앞에 this()에 의해 firstName과 10을 인자로 가지고 Developer 클래스의 다른 생성자를 호출한다.(부 생성자 옆에 : this() 같은 게 있을 경우 다른 생성자를 해당 부분에서 호출한다.)<br/>
   해당 this()는 2개의 인자를 가진 부 생성자인 2번을 가리킨다. 여기에 다시 super()가 사용되었는데, super는 상속하고 있는 상위 클래스를 가리키기 때문에 2개의 인자를 처리할 수 있는 Person클래스의 3번 부 생성자를 호출한다.<br/><br/>

   상속을 통해서 클래스를 만드는 경우에는 상위 클래스의 생성자가 있다면 반드시 하위 클래스에서 호출해야 한다. 따라서 생성자 코드를 실행하기 전에 현재 클래스를 가리키는 this나 상위 클래스를 가리키는 super를 사용해 위임하여 다른 생성자를 처리할 수 있게 된다.<br/>

3. 주 생성자와 부 생성자 함께 사용하기
   만약 주 생성자와 부 생성자가 함께 있다면 this를 사용해 주 생성자를 가리킬 수 있다.<br/>

   ```
   class Person(firstName: String,
               out: Unit = println("[Primary Constructor] Parameter")) { // 2
       val fName = println("[Property] Person fName: $firstName") // 3

       init{
           println("[init] Person init block") // 4
       }

       constructor(firstName: String, age: Int,
                   out: Unit = println("[Secondary Constructor] Parameter")): this(firstName){
           println("[Secondary Constructor] Body: $firstName, $age)
       }
   }

   fun main() {
       val p1 = Person("Kildong", 30) // 1 -> 2 호출, 3->4->5 실행
       println()
       val p2 = Person("Dooly")
   }
   ```

   위 코드의 this는 주 생성자를 가리킨다. 생성하는 객체의 인자 수에 따라 부 생성자 또는 주 생성자를 호출한다.<br/>

4. 바깥 클래스 호출하기
   클래스를 선언할 때 클래스 안에 다시 클래스를 선언하는 것이 가능하다.<br/>
   이때 특정 클래스 안에 선언된 클래스를 이너 클래스(Inner Class)라고 한다.<br/>
   일단 여기서는 이너 클래스에서 바로 바깥 클래스를 참조하는 방법을 먼저 보겠음.<br/><br/>

   만약 이너 클래스에서 바깥 클래스의 상위 클래스를 호출하려면 super 키워드와 함께 @ 기호 옆에 바깥 클래스 이름을 작성한다.<br/>

   ```
   open class Base {
       open val x: Int = 1
       open fun f() = println("Base Class f()")
   }

   class Child : Base() {
       override val x: Int = super.x + 1
       override fun f() = println("Child Class f()")

       inner class Inside {
           fun f() = println("Inside Class f()")
           fun test() {
               f() // 1
               Child().f() // 2
               super@Child.f() // 3
               println("[Inside] super@Child.x: ${super@Child.x}") // 4
           }
       }
   }

   fun main() {
       val c1 = Child()
       c1.Inside().test()
   }
   ```

   Child 클래스는 Base 클래스를 상속하고 있다. Child 클래스 안에 inner 키워드로 선언된 이너 클래스인 Inside 클래스가 있다.<br/>
   c1 객체에 이너 클래스인 Inside()에 생성자 표기로 접근하고 다시 test() 메서드에도 각각 점 표기법으로 접근하고 있음. test() 메서드는 1번 현재 이너 클래스의 f()를 접근해 실행한다. 2번 Child().f()에서 바로 바깥 클래스의 f()를 실행하고, 3번에서 super@Child.f()는 Child 클래스의 상위 클래스인 Base 클래스의 f()를 실행하는 것이다.<br/>
   4번도 3번과 같은 원리로 Base 클래스의 프로퍼티에 접근하는 것.<br/>

5. 인터페이스에서 참조하기
   인터페이스(Interface)는 일종의 구현 약속으로 인터페이스를 참조하는 클래스는 인터페이스가 가지고 있는 내용을 구현해야 하는 **가이드**를 제시한다.<br/>
   따라서 인터페이스 자체로는 객체로 만들 수 없고 항상 인터페이스를 구현하는 클래스에서 생성해야 한다.<br/>
   일단 인터페이스에 접근하는 부분에 초점을 맞춰서 보겠음.<br/><br/>

   코틀린은 다중 상속이 되지 않지만, 인터페이스로는 필요한 만큼 다수의 인터페이스를 지정해 구현할 수 있다.<br/>
   이때 각 인터페이스의 프로퍼티나 메서드의 이름이 중복될 수 있는데, 앵글 브래킷(<>)을 사용해 접근하려는 클래스나 인터페이스의 이름을 정해주면 된다.<br/>

   ```
    open class A {
        open fun f() = println("A Class f()")
        fun a() = println("A Class a()")
    }

    interface B { //기본적으로 open
        fun f() = println("B Interface f()")
        fun b() = println("B Interface b()")
    }

    class C : A(), B { // 1
        override fun f() = println("C Class f()")

        fun test () {
            f() //2
            b() //3
            super<A>.f() // 4
            super<B>.f() //5
        }
    }

    fun main() {
        val c = C()
        c.test()
    }
   ```

   1번에서 지정한 것처럼 클래스와 인터페이스를 지정해 클래스 C를 선언했음. 이때 클래스는 1개만 상속이 가능하고 인터페이스는 여러 개 지정할 수 있다.<br/>
   인터페이스의 프로퍼티나 메서드를 사용할 수 있는데 이때 f() 메서드의 이름이 중복되고 있음. 3번 인터페이스의 b() 처럼 이름이 중복되지 않은 경우에는 상관 없지만, 중복된 f()같은 경우는 super<A>.f()와 super<B>.f()로 구분할 수 있음. 앵글 브래킷 없이 그냥 사용하는 경우에는 현재 클래스의 f()가 호출된다.<br/>

## 2. 정보 은닉 캡슐화

클래스를 작성할 때 숨겨야 하는 속성이나 기능이 있을 수 있는데, 이런 것들을 숨기는게 캡슐화라고 한다.<br/>

1. 가시성 지시자
   각 클래스나 메서드, 프로퍼티의 접근 범위를 **가시성**이라고 한다.<br/>
   이 범위는 가시성 지시자를 통해 제어할 수 있음.<br/>

   1. private: 이 요소는 외부에서 접근할 수 없음
   2. public: 이 요소는 어디에서든 접근이 가능 (default)
   3. protected: 외부에서는 접근할 수 없으나 하위 요소에는 접근이 가능
   4. internal: 같은 정의의 모듈 내부에서는 접근이 가능

   지시자는 전역 변수, 함수, 클래스, 프로퍼티, 메서드, 인터페이스 등에 붙여서 사용이 가능하다. 아무런 지시자가 정의되어 있지 않으면 기본 값으로 public이 들어가게 된다.<br/>
   주 생성자 앞에 가시성 지시자를 사용하는 경우, constructor 키워드는 생략이 불가능하다.<br/>

2. private
   private는 접근 범위가 선언된 요소에 한정하는 가시성 지시자<br/>

   ```
   private class PrivateClass {
       private var i = 1
       private fun privateFunc() {
           i += 1 //접근 허용
       }

       fun access() {
           privateFunc() //접근 허용
       }
   }

   class OtherClass {
       val opc = PrivateClass()  //불가
       fun test(){
           val pc = PrivateClass() // 가능
       }
   }

   fun main() {
       val pc = PrivateClass() //가능
       pc.i //불가
       pc.privateFunc() //불가
   }

   fun TopFunction() {
       val tpc = PrivateClass() //가능
   }
   ```

   여기서 PrivateClass 클래스는 private로 선언되어 있기 때문에 다른 파일에서 접근이 불가능하다. 같은 파일에서 PrivateClass의 객체를 생성할 수 있다. 만약 다른 클래스에서 프로퍼티로서 PrivateClass의 객체를 지정하려면 똑같이 private로 선언해야 한다.<br/>
   객체를 생성했다고 하더라도 PrivateClass의 멤버인 i와 privateFunc() 메서드가 private로 선언되어 있기 때문에 다른 클래스나 main() 같은 최상위 함수에서 접근이 불가능하다. 즉, Private 멤버는 해당 클래스 내부에서만 접근이 가능함<br/>

3. protected
   protected 지시자는 최상위에 선언된 요소에는 지정할 수 없고 클래스나 인터페이스와 같은 요소의 멤버에만 지정할 수 있다.<br/>
   멤버가 클래스인 경우에는 protected로 선언이 가능하다.<br/>

   ```
   open class Base { //최상위 클래스에는 protected를 사용할 수 없음
       protected var i = 1
       protected fun protectedFunc() {
           i += 1 //허용
       }
       fun access() {
           protectedFunc() // 접근 허용
       }
       protected class Nested // 내부 클래스에는 지시자 허용
   }

   class Derived : Base() {
       fun test(base: Base): Int {
           protectFunc() // 접근 가능
           return i //접근 가능
       }
   }

   fun main() {
       val base = Base() //생성 가능
       base.i //불가
       base.protectedFunc() //불가
       base.access() //가능
   }
   ```

   위에서 protected 멤버 프로퍼티인 i와 메서드 protectedFunc()는 하위 클래스인 Derived 클래스에서 접근할 수 있다. protected로 지정된 멤버는 상속된 하위 클래스에서는 자유롭게 접근이 가능하다.<br/>
   외부 클래스나 객체 생성 후 점 표기법을 통해 protected 멤버에 접근하는 것은 허용되지 않는다.<br/>

4. internal
   internal은 프로젝트 단위의 모듈을 가리키기도 한다. 즉, 모듈이 달라지면 접근이 불가능함.<br/>
   만약 프로젝트에 또 다른 모듈이 없이 하나만 있는 경우 internal의 접근 범위는 프로젝트 전체범위가 된다.<br/>
   만약 패키지 이름이 다를 경우 import 구문을 사용해 필요한 클래스를 임포트 해야 해당 클래스를 사용할 수 있다.<br/>

## 3. 클래스와 클래스의 관계
