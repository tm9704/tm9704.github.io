---
title: "Kotlin(8) - 클래스와 객체(1)"
excerpt: "Kotlin Class & Object"
toc: true
toc_sticky: true
categories:
  - Kotlin
tags:
  - Kotlin
date: "2022-12-23 11:40:00"
last_modified_at: "2022-12-23 12:40:00"
---

## 1. 클래스와 객체의 정의

객체 지향 프로그래밍(OOP)은 프로그램의 구조를 객체 간 상호작용으로서 표현하는 프로그래밍 방식.<br/>
코틀린은 함수형 프로그래밍과 더불어 객체 지향 프로그래밍 기법을 지원한다.<br/>

1. 객체 지향 프로그래밍과 용어
   자바에서는 클래스에 포함된 기능을 나타내는 함수를 메서드(Method), 변수를 필드(Field)라고 한다.<br/>
   메서드나 필드는 클래스 내부에 정의되므로, 클래스의 멤버 메서드, 멤버 필드라고도 한다.<br/><br/>
   코틀린에서는 필드 대신 프로퍼티(Property)라는 용어를 쓰는데, 그 이유는 변수 또는 필드에 내부적으로 접근 메서드가 포함되어 있기 때문이다.<br/>
   즉, 코틀린에서는 프로퍼티는 변수의 이름과 접근 함수가 포함된 형태.<br/>

2. 클래스의 추상화
   추상화는 목표로 하는 대상에 대해 필요한 만큼 속성과 동작을 정의하는 과정이다.<br/>
   예를 들어 **새**라는 기본 개념을 정의하고 새롭게 발견한 개별적인 새의 **일반적인 동작(함수)**과 **특징(속성)**을 알아내는 것.<br/><br/>

즉, 여러 종류의 새가 있을텐데, 그들의 공통된 점인 **새**라는 클래스를 정의하는 것을 추상화라고 한다.<br/>

3. 클래스 선언하기
   클래스를 선언하려면 class 키워드가 필요하다.<br/>

```
class Bird {} // 내용이 비어 있는 클래스 선언
class Bird // 중괄호 생략 가능
```

위의 코드는 **Bird**라는 이름의 클래스를 선언한 것인데, 특별히 프로퍼티와 메서드를 정의하지 않고 빈 형태로 클래스를 선언할 수 있다. 이때는 중괄호도 생략이 가능하다.<br/>

```
class Bird { // 1
  // 2
  var name: String = "mybird"
  var wing: Int = 2
  var beak: String = "short"
  var color: String = "blue"

  //3
  fun fly() = println("Fly wing: $wing")
  fun sing(vol: Int) = println("Sing vol: $vol")
}

fun main() {
  val coco = Bird() // 4
  coco.color = "blue" // 5

  println("coco.color: ${coco.color}") // 6
  coco.fly() // 7
  coco.sing(3)
}
```

1번의 클래스의 정의 부분을 살펴보면 class 키워드를 사용해 클래스 이름 Bird를 정의했다.<br/>
클래스의 본문에는 2번처럼 변수 선언과 같은 방법으로 프로퍼티를 선언했음. 이때 프로퍼티는 반드시 초기화되어 있어야 한다.<br/>
3번에서는 함수를 선언하는 방법과 동일하게 메서드를 정의했다.<br/><br/>

Bird라는 클래스를 사용하기 위해 4번처럼 coco라는 이름으로 객체를 만들고, 이 객체는 5번처럼 프로퍼티에 값을 할당하거나 6번처럼 이 객체의 프로퍼티로부터 값을 읽거나 7번처럼 메서드를 실행할 수 있게 된다.<br/>
이때 점(.) 표기법으로 해당 객체의 멤버 베서드에 접근할 수 있다.<br/>

4. 객체와 인스턴스
   위에서 Bird라는 클래스란 일종의 선언일 뿐 실제 메모리에 존재해 실행되고 있는 것은 아니다.<br/>
   이 클래스로부터 객체(Object)를 생성해야만 클래스라는 개념의 실체인 객체가 물리적인 메모리 영역에서 실행된다.<br/>
   이것을 인스턴스화 되었다고 이야기하고 메모리에 올리간 객체를 인스턴스라고 한다.<br/>

## 2. 생성자

**생성자(Constructor)**란 클래스를 통해 객체가 만들어질 때 기본적으로 호출되는 함수를 말한다.<br/>
클래스 안의 프로퍼티 값을 직접 입력하여 초기화해도 되지만 이렇게 하면 항상 같은 프로퍼티 값을 가지는 객체가 만들어진다.<br/>
이렇게 하지 않기 위해 외부에서 인자를 받아 초기화할 수 있도록 특별한 함수 constructor()를 정의한다.<br/>

```
class 클래스 이름 constructor(필요한 매개변수..) { //주 생성자
  ...
  constructor(필요한 매개변수..){ // 부생성자
    // 프로퍼티 초기화
  }
  [constructor(필요한 매개변수..){...}] //추가 부생성자
}
```

생성자는 주 생성자와 부 생성자로 나뉘며 필요에 따라 주 생성자 또는 부 생성자를 사용할 수 있다.<br/>
부 생성자는 필요하면 매개변수를 다르게 여러 번 정의할 수 있다.

1. 부 생성자
   부 생성자는 클래스의 본문에 함수처럼 선언한다.<br/>

```
class Bird {
  // 1. 프로퍼티 - 선언만
  var name: String
  var wing: Int
  var beak: String
  var color: String

  // 2. 부 생성자 - 매개변수를 통해 초기화할 프로퍼티에 지정
  constructor(name: String, wing: Int, beak: String, color: String){
    this.name = name // this는 선언된 현재 클래스의 프로퍼티
    this.wing = wing
    this.beak = beak
    this.color = color
  }

  fun fly() = println("Fly wing: $wing")
  fun sing(vol: Int) = println("Sing vol: $vol")
}

fun main() {
  val coco = Bird("mybird", 2, "short", "blue")

  coco.color = "yellow"
  println("coco.color = ${coco.color}")
  coco.fly()
  coco.sing(3)
}
```

프로퍼티는 1번처럼 선언하기만 하고 2번과 같이 부 생성자의 매개변수를 통해 초기화 할 수 있다. 부 생성자를 이용하면 프로퍼티를 선언할 때 초기화 할 필요가 없음.<br/>
부 생성자인 constructor()블럭에서 매개변수로 전달받은 것들과 프로퍼티의 선언된 것들을 구분하기 위해 Bird 클래스를 가리키는 this 키워드를 사용해 프로퍼티를 지정함.<br/>
this는 객체 자신에 대한 참조로 클래스 내부에 있는 함수에서 프로퍼티를 참조할 수 있음.<br/><br/>

부 생성자를 여러 개 사용할 떄는 매개변수를 다르게 정의해야 한다.<br/>

```
class Bird {
  var name: String
  var wing: Int
  var beak: String
  var color: String

  // 첫 번째 부생성자
  constructor(name: String, wing: Int, beak: String, color: String){
    this.name = name // this는 선언된 현재 클래스의 프로퍼티
    this.wing = wing
    this.beak = beak
    this.color = color
  }

  //두 번째 부생성자
  constructor(name: String, beak: String){
    this.name = name
    this.wing = 2
    this.beak = beak
    this.color = "grey"
  }
  ...
}
```

객체를 생성할 때 인자의 개수에 따라 생성자를 다르게 호출할 수 있음.<br/>

2. 주 생성자
   주 생성자는 클래스 이름과 함께 생성자 정의를 사용할 수 있는 기법.<br/>
   주 생성자는 클래스 이름과 블록 시작 부분 사이에 선언.<br/>

```
class Bird constructor(_name: String, _wing: Int, _beak: String, _color: String){
  var name: String = _name
  var wing: Int = _wing
  var beak: String = _beak
  var color: String = _color

  fun fly() = println("Fly wing: $wing")
  fun sing(vol: Int) = println("Sing vol: $vol")
}
```

주 생성자의 선언은 클래스 이름 오른쪽에 constructor 키워드로 시작함.<br/>
주 생성자의 constructor 키워드는 아래와 같이 생략이 가능함.<br/>

```
class Bird(_name: String, _wing: Int, _beak: String, _color: String){
  ...
}
```

3. 프로퍼티를 포함한 주 생성자
   내부의 프로퍼티를 생략하고 생성자의 매개변수에 프로퍼티 표현을 함께 넣을 수 있음.<br/>
   val, var를 사용해서 매개변수를 선언하면 생성자에 this 키워드를 사용하거나 매개변수 이름에 언더스코어를 붙인 다음 생성자에서 인자를 할당할 필요가 없다.<br/>

```
class Bird(val name: String, val wing: Int, val beak: String, var color: String){
  ...
}
```

더 긴 예제를 보면,<br/>

```
class Bird(var name: String, var wing: String, var beak: String, var color: String){
  //프로퍼티 선언은 생략

 fun fly() = println("Fly wing: $wing")
  fun sing(vol: Int) = println("Sing vol: $vol")
}

fun main() {
  val coco = Bird("mybird", 2, "short", "blue")

  coco.color = "yellow"
  println("coco.color: ${coco.colr}")
  coco.fly()
  coco.sing(3)
}
```

주 생성자의 매개변수에 프로퍼티가 선언되었으므로 본문에서 프로퍼티 선언이 생략됨.<br/>

4. 초기화 블록을 가진 주 생성자
   생성자는 기본적으로 함수를 표현하는 기능이기 때문에 변수를 초기화 하는 것 말고도 특정한 작업을 하도록 코드를 작성할 수 있다. 단, 클래스 이름 다음에 주 생성자를 표현하는 경우 클래스 블록 ({}) 안에 코드를 넣을 수 없음.<br/>
   따라서 초기화에 꼭 사용해야 할 코드가 있다면 init{...} 초기화 블록을 클래스 선언부에 넣어야 한다.<br/>

```
class Bird(var name: String, var wing: Int, var beak: String, var color: String){
  // 1. 초기화 블록
  init {
    println("----초기화 블록 시작----")
    println("이름은 $name, 부리는 $beak")
    this.sing(3)
    println("----초기화 블록 끝----")
  }

  fun fly() = println("Fly wing: $wing")
  fun sing(vol: Int) = println("Sing vol: $vol")
}

fun main(){
  val coco = Bird("mybird", 2, "short", "blue") // 2

  coco.color = "yellow"
  println("coco.color: ${coco.color}")
  coco.fly()
}
```

1번 init 초기화 블록에서 name과 beak를 출력하고 sing()메서드를 사용했다. init 초기화 블록에서는 출력문이나 프로퍼티, 메서드 등과 같은 코드를 사용할 수 있다.<br/>
초기화 블록에서 명시한 내용은 2번 객체 생성과 함께 같이 실행된다.<br/>

5. 프로퍼티의 기본값 지정
   생성자의 매개변수에 함수에서 하는 것 처럼 기본값을 사용할 수 있음.<br/>

```
class Bird(var name: String = "NONAME", var wing: Int = 2, var beak: String, var color: String){
  ...
}

fun main() {
  val coco = Bird(beak = "long", color = "red")
  ...
}
```

## 3. 상속과 다형성

클래스는 자식 클래스를 만들 때 상위 클래스(부모 클래스)의 속성과 기능을 물려받아 계승하는데, 이것을 상속(Inheritance)이라고 한다. 상속을 이용하면 하위 클래스는 일부러 상위 클래스의 모든 내용을 다시 만들지 않아도 된다.<br/>
다형성(Polymorphism)이란 메서드가 같은 이름을 사용하지만, 구현 내용이 다르거나 매개변수가 달라서 하나의 이름으로 다양한 기능을 수행할 수 있는 개념을 의미한다.<br/>

1. 상속과 클래스의 계층
   모든 클래스는 Any 클래스의 하위 클래스가 되며, 상위 클래스를 명시하지 않으면 Any 클래스를 상속받게 된다.<br/>
   상위 클래스가 상속할 수 있는 상태가 되려면 **open** 키워드와 함께 선언해야 한다. open 없이 기본으로 클래스를 선언하면 상속할 수 없는 기본 클래스가 된다.<br/>
   즉, open 키워드로 선언하지 않은 클래스들은 최종 클래스로서 상속할 수 없다.<br/><br/>

하위 클래스를 선언하는 방법은,<br/>

```
open class 기반 클래스 이름 {
  ...
}
class 파생 클래스 이름 : 기반 클래스 이름() {
  ...
}
```

위에서 나타나지는 않았지만, 모든 클래스는 묵시적으로 Any를 상속받는다. 즉, 아무런 표기가 없어도 모든 클래스는 Any를 최상위 클래스로 가진다.<br/>

```
// 1
open class Bird(var name: String, var wing: Int, var beak: String, var color: String){
  fun fly() = println("Fly wing: $wing")
  fun sing(vol: Int) = println("Sing vol: $vol")
}

//2
class Lark(name: String, wing: Int, beak: String, color: String)
: Bird(name, wing, beak, color){
    fun singHitone() = println("Happy Song!")
}

//3
class Parrot : Bird {
  val language: String

  constructor(name: String,
              wing: Int,
              beak: String,
              color: String,
              language: String) : super(name, wing, beak, color){
                this.language = language
              }
    fun speak() = println("Speak! $language")
}

fun main() {
  val coco = Bird("mybird", 2, "short", "blue")
  val lark = Lark("mylark", 2, "long", "brown")
  val parrot = Parrot("myparrot", 2, "short", "multiple", "korean")

  ...
  lark.singHitone()
  parrot.speak()
  ...
}
```

1번에서 상속을 위해 open 키워드를 사용하여 Bird 클래스를 정의함.<br/>
2번에서는 주 생성자를 사용하는 방법으로 파생 클래스를 선언했다. 이때 상위 클래스인 Bird 클래스에 생성자에 사용하는 매개변수와 인자들을 지정해야 한다.<br/>
3번에서는 부 생성자를 사용하는 방법으로 파생 클래스를 선언했는데, 이때는 본문 내부에 constructor()를 이용한다.(super은 뒤에서)<br/>
이렇게 하위 클래스는 상위 클래스의 메서드나 프로퍼티를 그대로 상속하면서 상위 클래스에는 없는 자신만의 프로퍼티나 메서드를 확장할 수 있다.<br/>

2. 다형성
   상위 클래스의 메서드나 프로퍼티를 상속할 때 하위 클래스에서 똑같은 이름의 메서드나 프로퍼티를 지정할 때는 매개변수를 다르게 하거나 기능 구현부를 다르게 작성해야 한다.<br/>
   이렇게 이름은 동일하지만 매개변수가 서로 다른 형태를 취하거나 실행 결과를 다르게 가질 수 있는 것을 **다형성**이라고 한다.<br/><br/>

동작은 동일하지만 인자의 형식만 달라지는 것을 **오버로딩(Overloading)**이라고 한다.<br/>
상위와 하위 클래스에서 메서드나 프로퍼티의 이름은 같지만 기존의 동작을 다른 동작으로 재정의 하는 것을 **오버라이딩(Overriding)**이라고 한다.<br/><br/>

3. 오버로딩
   동일한 클래스 안에서 같은 이름의 메서드가 매개변수만 달리해서 여러 번 정의될 수 있는 개념으로, 반환값은 동일하거나 달라질 수 있음. 구현되는 동작은 **대부분 동일**하다.<br/>

```
fun main() {
  val calc = Calc()
  println(calc.add(3,2))
  println(calc.add(3.2, 1.3))
  println(clac.add(3,3,2))
  println(calc.add("Hello", "World"))
}

class Calc {
  fun add(x: Int, y: Int): Int = x + y
  fun add(x: Double, y: Double): Double = x + y
  fun add(x: Int, y: Int, z: Int): Int = x + y + z
  fun add(x: String, y: String): String = x + y
}
```

위는 서로 다른 매개변수를 가진 add() 함수들이 있다.<br/>
같은 클래스 안에서 이름은 모두 동일하지만 사용하는 인자에 따라 호출되는 메서드가 달라진다.<br/>

4. 오버라이딩
   하위 클래스에서 새로 만들어지는 메서드가 **이름**이나 **매개변수**, **반환값**이 이전 메서드와 똑같지만 새로 작성되는 것을 의미한다.<br/>
   하위의 새로운 메서드는 상위 클래스의 메서드의 내용을 완전히 새로 만들어 다른 기능을 하도록 정의한다.(즉, 재정의)<br/><br/>

코틀린에서는 기반 클래스의 내용을 파생 클래스가 오버라이딩하기 위해 기반 클래스에서는 open 키워드, 파생클래스에는 **override** 키워드를 각각 사용한다.<br/>
메서드 뿐만 아니라 프로퍼티도 오버라이딩할 수 있다.<br/>

```
open class Bird {
  ...
  fun fly() {...} // 1
  open fun sing() {...} // 2
}

class Lark() : Bird() {
  fun fly() {...}
  override fun sing() {...} // 3
}
```

open이 사용된 상속 가능한 Bird 클래스에는 fly() 메서드와 sing() 메서드가 있다.<br/>
1번의 fly() 메서드 앞에는 open 키워드가 없음, 이렇게 정의한 메서드는 오버라이딩이 불가능한 최종 메서드이다. 메서드를 오버라이딩 하려면 open 키워드가 메서드 이름 앞에 있어야 한다.<br/><br/>

2번의 경우 open을 사용해 sing() 메서드를 정의했으므로 오버라이딩이 가능한 메서드가 된다.<br/>
3번과 같이 하위 클래스에서는 override라는 키워드를 사용해 재정의됨을 알려야 한다. 그러나 fly() 메서드는 Bird 클래스에 open 키워드가 없으므로 재정의할 수 없기 때문에 메서드 본문을 재정의하면 오류를 발생한다.<br/><br/>

오버라이딩을 아예 막고자 할 때는 override키워드 앞에 final 키워드를 사용해 하위 클래스에서 재정의 되는 것을 막을 수 있다.<br/>

```
open class Lark() : Bird() {
  final override fun sing() {...} //하위 클래스에서의 재정의를 막음
}
```

추가 예제<br/>

```
open class Bird(var name: String, var wing: Int, var beak: String, var color: String){
  fun fly() = println("Fly wing: $wing")
  open fun sing(vol: Int) = println("Sing vol: $vol")
}

class Parrot(name: String,
            wing: Int,
            beak: String,
            color: String,
            var language: String = "natural") : Bird(name, wing, beak, color){
  fun speak() = println("Speak! $language")
  override fun sing(vol: Int){
    println("I'm a parrot! The volume level is $vol")
    speak()
  }
}

fun main() {
  val parrot = Parrot(name = "myparrot", beak = "short", color = "multiple")
  parrot.language = "English"

  println("Parrot: ${parrot.name}, ${parrot.wing}, ${parrot.beak}, ${parrot.color},
  ${parrot.language"})
  parrot.sing(5)
}
```
