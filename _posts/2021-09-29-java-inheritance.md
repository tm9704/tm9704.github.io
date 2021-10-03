---
title: "Java(23) - 상속"
excerpt: "Java Inheritance"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-09-29 19:40:00"
last_modified_at: "2021-09-29 20:00:00"
---

이번 포스트에서는 상속에 대해 공부하겠습니다.<br/>

## 1. 클레스에서 상속의 의미

상속은 기존에 정의된 클래스에 메소드와 변수를 추가하여 새로운 클래스를 정의하는 것입니다.<br/>

즉 새로운 클래스를 정의할 때 이미 구현된 클래스를 상속(inheritance)받아서<br/>
속성이나 기능이 확장되는 클래스를 구현하는 것입니다.<br/>
(새로운 클래스를 완전히 처음부터 생성하는 것이 아닌 기능과 속성만 상속)<br/>

속성이나 기능이 확장되는 곳에서 쓰이기 때문에 서로 이질적인 클래스에서는 사용하지 않습니다.<br/>

![상속](/images/inheritance.png)<br/>

상속하는 클래스를 상위클래스, parent class, super class 등으로 부릅니다.<br/>
상속 받는 클래스는 하위클래스, child class, subclass 등으로 부릅니다.<br/>

```java
class B extends A{
  ...
}
```

위의 코드처럼 B가 A를 상속받을 경우 extends 키워드를 사용해줍니다.<br/>
Java에서는 extends 키워드 뒤에는 상속받을 클래스는 단 한개만 위치할 수 있습니다.<br/>
(single inheritance)<br/>

코드 재사용이 되긴 하지만, 코드 재사용을 목적으로 사용하는것이 아닌 구체적인 클래스를 만들기 위해 사용합니다.<br/>

## 2. 상속을 사용하는 경우

상속을 사용하는 경우에 대해 알아보겠습니다.<br/>

1. 상위클래스는 하위클래스보다 일반적인 개념과 기능을 가짐
2. 하위클래스는 상위클래스보다 구체적인 개념과 기능을 가짐

즉 하위클래스가 상위클래스의 개념과 기능을 기본적으로 가지고 있고, 더 구체적인 개념이 필요할 경우 사용합니다.<br/>

예시로는,<br/>
포유류 <- 사람이 있습니다.<br/>

```java
class Mammal{
  ...
}

class Human extends Mammal{
  ...
}
```

## 3. protected 예약어

접근 제한자를 공부했을 때 봤던 protected 키워드입니다.<br/>

외부 클래스에는 private으로, 하위클래스에서는 public의 기능을 구현하고 싶을 때 사용합니다.<br/>
상위 클래스에 protected로 선언된 변수나 메서드는 다른 외부 클래스에서는 사용할 수 없지만,<br/>
하위클래스에서는 사용이 가능합니다.<br/>

상위클래스에 있는 private 변수나 메소드 역시 상속이 됩니다.<br/>
그러나 앞선 포스트에서 공부했듯이 private로 선언된 변수나 메소드는 선언된 클래스 내에서만 접근이 가능합니다.<br/>

즉 우리는 간접적으로 접근을 해야합니다.<br/>
예를 들면 상위 클래스에 생성자로 private 변수를 초기화하게 하고,<br/>
하위 클래스에서 super로 초기화 해주는 방법이 있습니다.<br/>
(아니면 상위 클래스에서 제공하는 protected 메소드로 접근)<br/>

## 4. static 변수와 메소드의 상속

static변수와 메소드는 생성되는 인스턴스마다 독립적으로 존재하는 멤버가 아니고,<br/>
생성되는 인스턴스들이 함께 공유하는 변수, 메소드라고 배웠습니다.<br/>

즉 static변수와 메소드는 상속에 중점을 두는 것이 아니라 어떻게 접근할 것인지에 대해 생각해야합니다.<br/>

상속 관계에서 상위 클래스에 public으로 선언된 static 변수나 메소드가 있다면<br/>
하위 클래스에서는 변수명이나 메소드명 만으로 접근이 가능합니다.<br/>
(또한, 하위클래스 이름을 통해서도 상위클래스의 static변수 및 메소드에 접근이 가능합니다.)<br/>

```java
class  A{
  public static int total = 0;
}

class B extends A{
  public void addNum(int num){total += num;}
  public void showTotal(){System.out.println(total);}
}

class AddTest{
  public static void main(String[] args){
    A ad = new A();
    B af = new B();

    af.addNum(1);

    B.val += 5;
    af.showTotal();
  }
}
```

위 코드의 실행 결과는 6입니다.<br/>

## 5. 생성자의 상속

일반적으로 자바의 생성자는 상속이 되지 않는다고 말합니다.<br/>

우선 예시를 보겠습니다.<br/>

```java
class AAA{
  int num;
  AAA(int n){num = n;}
}

class BBB extends AAA{
  BBB(){super(0);}
}

class Test{
  public static void main(String() args){
    BBB b1 = new BBB(); //가능
    BBB b2 = new BBB(1); //불가능
  }
}
```

위의 코드의 경우 b2는 BBB에 적절한 생성자가 없으므로 생성이 되지 않습니다.<br/>
즉 생성자가 상송이 되지 않는다는 것은 b2가 참조하는 인스턴스의 생성이 불가능함을 두고 말하는 것입니다.<br/>

즉 생성자의 상속이란, 위에서 정의한 AAA클래스의 다음 생성자가,<br/>
AAA(int n){num = n;}<br/>
AAA 클래스를 상속하는 BBB 클래스에게 다음의 형태로 상속됨을 뜻합니다.<br/>
BBB(int n){num = n;}<br/>

즉 생성자에서 상속은 지금까지 공부한 상속과 약간의 차이를 보이지만 비슷한 관접입니다.<br/>
(나중 포스트에서 더 정확히 다루겠습니다.)
