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

   main() 함수에서 Developer("Sean")을 통해 객체를 생성하고 있다. 이때 인자가 1개이므로 먼저 Developer 클래스의 1번 생성자로 진입한다. 코드를 실행하기 전에 앞에 this()에 의해 firstName과 10을 인자로 가지고 Developer 클래스의 다른 생성자를 호출한다.
