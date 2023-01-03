---
title: "Android(3) - 코틀린 객체지향"
excerpt: "Android Kotlin OOP"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-03 11:00:00"
last_modified_at: "2023-01-03 12:15:00"
---

## 1. 클래스와 생성자

1. **클래스 선언**<br/>
   코틀린에서 클래스는 class 키워드로 선언한다.<br/>

   ```kotlin
   class User {}
   ```

   위 코드에서 class User가 선언부, {}가 본문이다. 이 본문에 입력할 내용이 없다면,
   중괄호 생략이 가능하다.<br/>
   클래스의 멤버는 생성자, 변수, 함수, 클래스로 구성된다. 이 중에서 생성자는 constructor이라는
   키워드로 선언하는 **함수**임. (클래스 안에 또 다른 클래스 선언이 가능)<br/>

   ```kotlin
   class User {
       var name = "kkang"
       constructor(name: String){
           this.name = name
       }
       fun someFun(){
           println("name: $name")
       }
       class SomeClass {}
   }
   ```

   클래스는 객체를 생성해 사용하고 객체로 클래스의 멤버에 접근한다.<br/>
   객체 생성은 Java와 조금 다른데, 클래스 이름과 같은 함수로 객체를 생성한다.<br/>

   ```kotlin
   val user = User("kim")
   user.someFun()
   ```

   객체를 생성할 때 생성자가 자동으로 호출되므로 소괄호 안에 전달한 인자는 클래스에 선언된
   생성자의 매개변수와 맞아야한다.<br/>

2. **주생성자**<br/>
   코틀린 클래스는 생성자를 주 생성자와 보조 생성자(부 생성자)로 구분한다.<br/>
   한 클래스 안에 주 생성자만 혹은 보조 생성자만 선언할 수 있고, 둘 다 선언도 가능하다.<br/><br/>

   주 생성자는 constructor 키워드로 클래스 선언부에 선언한다. 필수는 아니고 한 클래스에 하나만 가능.<br/>

   ```kotlin
   class User constructor() {}
   ```

   주 생성자를 선언할 때는 constructor 키워드 생략이 가능함.<br/>

   ```kotlin
   class User() {}
   ```

   클래스의 주 생성자를 선언하지 않으면 컴파일러가 매개변수가 없는 주 생성자를 자동으로 추가한다.<br/><br/>

   1. 주 생성자의 매개변수<br/>
      주 생성자를 선언할 때 필요에 따라 매개변수를 선언할 수도 있다.<br/>
      ```kotlin
      class User(name: String, count: Int){}
      ```
      위 코드는 User 클래스를 선언하면서 주 생성자에 매개변수를 2개 선언했다.<br/>
      이렇게 선언하면 사용시 주 생성자에 맞게 객체를 생성해야 한다.<br/>
      ```kotlin
      val user = User("kkang", 10)
      ```
   2. 주 생성자의 본문 - init<br/>
      주 생성자를 이용해 객체를 생성할 때 특정한 로직을 수행할 수 있다.<br/>

      ```kotlin
      class User(name: String, count: Int){

      }{

      }
      ```

      위 같은 형식으로 주 생성자의 본문을 중괄호를 이용해 사용할 수 없다.<br/>
      주 생성자는 클래스 선언부에 있기 때문에 중괄호를 사용할 수 없고 init 키워드를 사용해야 한다.<br/><br/>

      코틀린의 클레스 안에서 init으로 지정한 영역은 객체를 생성할 때 자동으로 실행된다.<br/>
      클래스에서 init영역은 필수가 아니라서 주 생성자의 본문을 구현할 때 주로 사용한다.<br/>

      ```kotlin
      class User(name: String, count: Int){
          init{
              println("i am init...")
          }
      }

      fun main() {
          val user = User("kkanga", 10)
      }
      ```

   3. 생성자의 매개변수를 클래스의 멤버변수로<br/>
      생성자의 매개변수는 기본적으로 생성자에서만 사용할 수 있는 지역변수.<br/>

      ```kotlin
      class User(name: String, count: Int){
          init{ // 가능
              println("name: $name, count: $count")
          }
          fun someFun() { // 오류
              println("name: $name, count: $count")
          }
      }
      ```

      생성자를 호출할 때 init 영역이 실행되므로 init에서는 생성자의 매개변수에 접근이 가능함.<br/>
      그러나 생성자의 매개변수는 **지역 변수**이므로 다른 함수에서는 사용이 불가능하다.<br/>
      즉 위 코드의 someFun()에서는 사용이 불가능함.<br/>
      생성자의 매개변수를 클래스의 멤버 변수처럼 다른 함수에서 사용하려면 멤버 변수를 선언하고
      init에서 멤버 변수에 매개변수를 할당해야 한다.<br/>

      ```kotlin
      class User(name: String, count: Int){
          var name: String
          var count: Int
          init {
              this.name = name
              this.count = count
          }

          for someFun() {
              println("name: $name, count: $count") // 가능
          }
      }

      fun main() {
          val user = User("kkang", 10)
          user.someFun()
      }
      ```

      위 방법 말고도 주 생성자의 매개변수를 var이나 val 키워드로 선언하면 클래스의 멤버 변수가 된다.<br/>

      ```kotlin
      class User(val name: String, val count: Int) {
          fun someFun() {
              println("name: $name, count: $count") // 가능
          }
      }
      fun main() {
          val user = User("kkang", 10)
          user.someFun()
      }
      ```

      원래 함수는 매개변수를 선언할 때 var, val 키워드를 추가할 수 없지만 주 생성자에서만 유일하게
      var, val 키워드로 매개변수를 선언할 수 있고 이렇게 선언하면 멤버변수가 된다.<br/>

3. **보조 생성자**<br/>
   보조 생성자는 클래스의 본문에 constructor 키워드로 선언하는 함수이다.<br/>
   클래스 본문에 선언하므로 여러 개를 추가할 수 있다.<br/>
   보조 생성자도 객체를 생성할 때 자동으로 호출되고, 생성자 본문을 중괄호로 묶어서 객체 생성과
   동시에 실행할 영역을 지정할 수 있다.<br/>

   ```kotlin
   class User {
       constructor(name: String){
           println("constructor(name: String) call...")
       }
       constructor(name: String, count: Int){
           println("constructor(name: String, count: Int) call...")
       }
   }

   fun main() {
       val user1 = User("kkang")
       val user2 = User("kkang", 10)
   }
   ```

   1. 보조 생성자에 주 생성자 연결<br/>
      클래스를 선언할 때 주 생성자나 보조 생성자 하나만 선언하면 문제가 없지만, 둘 다 선언한다면
      생성자들 끼리 연결해 주어야 한다.<br/>

      ```kotlin
      class User(name: String) {
          constructor(name: String, count: Int){ // 오류
              ...
          }
      }
      ```

      주 생성자가 있으므로 보조 생성자에서 주 생성자를 호출해 주어야 한다.<br/>
      보조 생성자는 객체를 생성할 때 호출되며, 이 때 클래스 내에 주 생성자가 있다면 this() 구문을
      이용해 주 생성자를 호출해야 한다.<br/>

      ```kotlin
      class User(name: String){
          constructor(name: String, count: Int): this(name) {//성공
              ...
          }
      }
      fun main() {
          val user = User("kkang", 10)
      }
      ```

      만약 주 생성자가 있는 상태에서 보조 생성자를 여러 개 선언한다면 보조 생성자에서 this()로 다른
      보조 생성자를 호출할 수도 있다.<br/>
      그런데 이때에도 보조 생성자로 객체를 생성한다면 어떤 식으로든 주 생성자가 호출되게 해야 한다.<br/>

## 2. 상속

1. **상속과 생성자**<br/>
   클래스를 선언할 때 다른 클래스를 참조해서 선언하는 것을 상속(Inheritance)라고 한다.<br/>
   코틀린에서 어떤 클래스를 상속받으려면 선언부에 콜론(:)과 함께 상속받을 클래스 이름을 입력한다.<br/><br/>

   상속 관계에서 상속 대상이 되는 클래스를 **상위 클래스**, 상속 받는 클래스를 **하위 클래스**라고 한다.<br/>
   다른 클래스에서 상속할 수 있게 선언하려면 open 키워드를 사용해야 한다.<br/>
   상위 클래스를 상속받은 하위 클래스의 생성자에서는 상위 클래스의 생성자를 호출해야 한다.<br/>

   ```kotlin
   open class Super(name: String){
   }
   class Sub(name: String) : Supper(name) {
   }
   ```

   위 처럼 상위 클래스의 생성자 호출문을 꼭 클래스 선언문에 작성할 필요는 없고 보조 생성자에서 호출해도 됨.<br/>

   ```kotlin
   open class Super(name: String){
   }
   class Sub : Super {
       constructor(name: String) : super(name) {
       }
   }
   ```

2. **오버라이딩 - 재정의**<br/>
   상속이 주는 최고 이점은 상위 클래스에 정의된 멤버(변수, 함수)를 하위 클래스에서 자신의 멤버처럼
   사용할 수 있다는 점이다.<br/>
   ```kotlin
   open class Super {
       var superData = 10
       fun superFun() {
           println("i am superFun: $superData")
       }
   }
   class Sub : Super()
   fun main() {
       val obj = Sub()
       obj.superData = 20
       obj.superFun()
   }
   ```
   때로는 상위 클래스에 정의된 멤버를 하위 클래스에서 재정의해야 할 수도 있다.<br/>
   즉, 상위 클래스에 선언된 변수나 함수를 같은 이름으로 하위 클래스에서 다시 선언하는 것이다.<br/>
   이를 오버라이딩이라고 한다.<br/>
   변수도 오버라이딩으로 재정의할 수 있지만 주로 함수를 재정의하는데 사용.<br/>
   같은 함수명으로 하위 클래스에서 새로운 로직을 추가하고 싶을 때 오버라이딩을 사용한다.<br/>
   ```kotlin
   open class Super {
       var superData = 10
       fun superFun() {
           println("i am superFun: $superData")
       }
   }
   class Sub : Super() {
       override var someData = 20
       override fun someFun() {
           println("i am sub class function: $someData")
       }
   }
   fun main() {
       val obj = Sub()
       obj.someFun()
   }
   ```
   코틀린에서 오버라이딩 규칙은 먼저 상위 클래스에서 오버라이딩을 허용할 변수나 함수 선언 앞에
   open 키워드를 추가해야 한다.<br/>
   open 키워드로 선언하지 않으면 하위 클래스에서 재정의가 불가능하다.<br/>
   그리고 하위 클래스에서 재정의를 할 때는 override 키워드가 필요함.<br/>
3. **접근 제한자**<br/>
   접근 제한자란 클래스의 멤버를 외부의 어느 범위까지 이용하게 할 것인지를 결정하는 키워드.<br/>
   코틀린에서 제공하는 접근 제한자에는 public, internal, protected, private가 있다.<br/>
   1. public<br/>
      접근 제한이 없음을 나타내며, default값임.<br/>
      최상위에서 이용시 - 모든 파일에서 접근 가능<br/>
      클래스 멤버에서 이용시 - 모든 클래스에서 접근 가능<br/>
   2. internal<br/>
      모듈은 프로젝트 단위나 같은 세트 단위를 나타냄<br/>
      최상위에서 이용시 - 같은 모듈 내에서 접근 가능<br/>
      클래스 멤버에서 이용시 - 같은 모듈 내에서 접근 가능<br/>
   3. protected<br/>
      최상위에서 이용시 - 사용 불가<br/>
      클래스 멤버에서 이용시 - 상속 관계의 하위클래스에서만 접근 가능<br/>
   4. private<br/>
      최상위에서 이용시 - 파일 내부에서만 접근 가능<br/>
      클래스 멤버에서 이용시 - 클래스 내부에서만 접근 가능<br/>

## 3. 코틀린의 클래스 종류

1. **데이터 클래스**<br/>
   데이터 클래스는 data 키워드로 선언하며 자주 사용하는 데이터를 객체로 묶어준다.<br/>
   데이터 클래스는 VO(value-object) 클래스를 편리하게 이용하게 해준다.<br/>

   ```kotlin
   class NonDataClass(val name: String, val email: String, val age: Int)

   data class DataClass(val name: String, val email: String, val age: Int)
   ```

   위 코드에서 NonDataClass는 일반 클래스로, DataClass는 data 키워드를 이용해 데이터 클래스로
   선언했다. 두 클래스의 주 생성자는 매개변수 구성이 같음.<br/>

   ```kotlin
   fun main() {
       val non1 = NonDataClass("kkang", "a@a.com", 10)
       val non2 = NonDataClass("kkang", "a@a.com", 10)

       val data1 = DataClass("kkang", "a@a.com", 10)
       val data2 = DataClass("kkang", "a@a.com", 10)
   }
   ```

   1. 객체의 데이터를 비교하는 equals() 함수<br/>
      VO 클래스는 데이터를 주요하게 다루므로 객체의 데이터가 서로 같은지 비교할 때가 많음.<br/>
      객체가 같은지가 아니라 **객체의 데이터**가 같은지 비교하는 경우임.<br/>
      이때 equals()함수를 사용한다.<br/>

      ```kotlin
      println("non data class equals : ${non1.equals(non2)}")
      println("data class equals : ${data1.equals(data2)}")
      ```

      equals() 함수로 일반 클래스의 객체를 비교하면 객체 자체를 비교하므로 결과값은 false<br/>
      그러나 데이터 클래스의 객체를 비교하면 객체 자체가 아니라 객체의 데이터를 비교하므로 true이다.<br/><br/>

      데이터 클래스는 주로 주 생성자에 val, var 키워드로 매개변수를 선언해 클래스의 멤버 변수로 활용한다.<br/>
      데이터 클래스 본문에 변수나 함수를 추가할 수도 있지만 객체의 데이터를 비교할 때 이용하는 equlas() 함수를
      주 생성자에 선언한 멤버 변수의 데이터만 비교 대상으로 삼는다.<br/>

      ```kotlin
      data class DataClass(val name: String, val email: String, val age: Int){
          lateinit var address: String
          constructor(name: String, email: String, age: Int, address: String):
              this(name, email, age){
                  this.address = address
              }
      }
      fun main() {
          val obj1 = DataClass("kkang", "a@a.com", 10, "seoul")
          val obj2 = DataClass("kkang", "a@a.com", 10, "busan")
          println("obj1.equals(obj2) : ${obj1.equals(obj2)}")
      }
      ```

      위 코드의 결과값은 true인데 두 객체의 일부 멤버 변수값은 다르지만, 주 생성자에 선언한 멤버 변숫값이
      같으면 true가 나온다.<br/>
      즉, 데이터 클래스의 equals() 함수는 주 생성자의 멤버 변수가 같은지만 판단함.<br/>

   2. 객체의 데이터를 반환하는 toString() 함수 <br/>
      데이터 클래스를 사용하면서 객체가 가지는 값을 확인해야 할 때가 많은데, 이때 데이터 클래스와
      일반 클래스의 toString() 함수는 반환값이 다름.<br/>
      ```kotlin
      fun main() {
          class NonDataClass(val name: String, val email: String, val age: Int)
          data class DataClass(val name: String, val email: String, val age: Int)
          val non = NonDataClass("kkang", "a@a.com", 10)
          val data = DataClass("kkang", "a@a.com", 10)
          println("non data class toString: ${non.toString()}")
          println("data class toString: ${data.toString()}")
      }
      ```
      일반 클래스로 생성한 객체의 toString() 함수가 출력하는 값은 의미 있는 데이터가 아님.<br/>
      데이터 클래스의 toString()함수는 **객체가 포함하는 멤버 변수의 데이터를 출력**<br/>
      즉, 객체의 데이터를 확인할 때 유용하게 사용이 가능하다.<br/>
      데이터 클래스의 toString() 함수 역시 주 생성자의 매개변수에 선언된 데이터만 출력 대상이다.<br/>

2. **오브젝트 클래스**<br/>
   코틀린에서 오브젝트 클래스는 **익명 클래스**를 만들 목적으로 사용.<br/>
   익명 클래스는 이름이 없는 클래스라서 선언하면서 동시에 객체를 생성해야함.<br/>

   ```kotlin
   val obj = object {
       var data = 10
       fun some() {
           println("data: $data")
       }
   }
   fun main() {
       obj.data = 20 //오류
       obj.some() //오류
   }
   ```

   object 키워드를 사용해 멤버 변수와 함수를 포함한 클래스를 선언.<br/>
   이렇게 하면 클래스 선언과 동시에 객체가 생성되며 그 객체를 obj에 저장함.<br/>
   이 obj 객체로 클래스에 선언한 멤버에 접근을 시도하면 오류가 발생한다.<br/><br/>

   그 이유는 클래스 타입 때문인데, object 키워드로 클래스를 선언했지만 타입은 명시하지 않았으므로
   코틀린의 최상위 타입인 Any로 취급함.<br/>
   Any 타입 객체에는 data, some()이라는 멤버가 없어서 오류가 발생한다.<br/><br/>

   그래서 object{} 형태로 익병 클래스를 선언할 때 보통 타입까지 함께 입력해 선언한다.<br/>
   오브젝트 클래스의 타입은 object 뒤에 콜론(:)을 입력하고 그 뒤에 클래스의 상위 또는 인터페이스를 입력한다.
   예를 들어 object: A {} 형태로 선언하면 클래스를 A 타입으로 선언한 것이다.<br/>
   만약 A가 클래스면 A 클래스를 상속받은 하위 클래스를 익명으로 선언한 것이고, 인터페이스 이면 A 인터페이스를
   구현한 익명 클래스를 선언한 것이다. 즉, object 객체를 A 타입으로 이용할 수 있게 된다.<br/>

   ```kotlin
   open class Super {
       open var data = 10
       oen fun some() {
           pritnln("i am super some(): $data")
       }
   }
   val obj = object {
       override var data = 20
       override fun some() {
           println("object some() data: $data")
       }
   }
   fun main() {
       obj.data = 30 //가능
       obj.some() //가능
   }
   ```

3. **컴패니언 클래스**<br/>
   컴패니언 클래스는 멤버 변수나 함수를 클래스 이름으로 접근하고자 할 때 사용한다.<br/>
   일반적으로 클래스의 멤버는 객체를 생성해서 접근해야 한다. 그런데 컴패니언 클래스는 객체를 생성하지 않고서도
   클래스 이름으로 특정 멤버를 이용할 수 있음.<br/>

   ```kotlin
   class MyClass {
       var data = 10
       fun some() {
           println(data)
       }
   }

   fun main() {
       val obj = MyClass()
       obj.data = 20 //성공
       obj.some() //성공
       MyClass.data = 20//오류
       MyClass.some() //오류
   }
   ```

   위 처럼 접근하면 오류.<br/>
   클래스 이름으로 멤버에 접근할 수 있게 하려면 companion 키워드 사용.<br/>

   ```kotlin
   class MyClass {
      companion object {
        var data = 10
        fun some() {
            println(data)
        }
      }
   }

   fun main() {
       MyClass.data = 20
       MyClass.some()
   }
   ```

   클래스 내부에 companion object {} 형태로 선언하면 이 클래스를 감싸는 클래스 이름(MyClass)으로 멤버에
   접근할 수 있다.<br/>
