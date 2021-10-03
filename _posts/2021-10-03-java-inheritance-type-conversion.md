---
title: "Java(24) - 상속에서 클래스 생성과 형 변환"
excerpt: "Java Inheritance & Type Conversion"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-03 13:40:00"
last_modified_at: "2021-10-03 14:00:00"
---

## 1. 하위 클래스가 생성되는 과정

하위 클래스가 생성될 때 상위 클래스가 먼저 생성이 됩니다.<br/>

1. 상위 클래스의 생성자가 호출되고
2. 하위 클래스의 생성자가 호출됩니다.

하위 클래스의 생성자에서는 무조건 상위 클래스의 생성자가 호출되어야 합니다.<br/>

하위 클래스에서 상위 클래스의 생성자를 호출하는 코드가 없는 경우,<br/>
컴파일러는 상위 클래스의 기본 생성자를 호출하기 위한 super()을 추가합니다.<br/>
(컴파일 단계에서)<br/>

super()로 호출되는 생성자는 상위클래스의 기본 생성자입니다.<br/>

만약 상위 클래스의 기본 생성자가 없는 경우(매개변수가 있는 생성자만 존재하는 경우)<br/>
하위 클래스는 명시적으로 상위 클래스의 생성자를 호출해야 합니다.<br/>

```java
class Parent{
    int num;
    Parent(int num){
        this.num = num;
    }
}

class Child extends Parent{

    // Child(){
    //     ...
    // } 이건 안됨.
    Child(int num){
        super(num);
    }
}
```

이런식으로 super(생성자에 맞는 인자) 명시적으로 호출해야합니다.<br/>
super()은 프로그램이 추천해주는 경우도 있고,<br/>
보통 하위 클래스 생성자로 매개변수를 받고 그 매개변수로 상위 클래스의 생성자를 호출하는게 좋습니다.<br/>

## 2. super()의 의미

super()의 의미는, 우리는 this가 자기 자신의 메모리를 가리킨다고 배웠습니다.<br/>
super() 키워드 역시 비슷하게 상위 클래스의 메모리 위치, 참조값을 지닙니다.<br/>
this()의 다른 생성자 호출 기능처럼, super()는 상위 클래스의 생성자가 호출됩니다.<br/>

super() 키워드 정리

1. 하위 클래스에서 가지는 상위 클래스의 참조값
2. super()는 상위 클래스의 기본 생성자 호출
3. 하위 클래스에서 명시적으로 상위 클래스의 생성자를 호출하지 않으면 super()가 자동으로 호출
4. super을 이용해 상위 클래스의 멤버 변수나, 메서드 참조 가능

## 3. 상속에서의 메모리 상태

상속을 받는 경우,

1. 상위 클래스의 인스턴스가 먼저 생성되고,
2. 하위 클래스의 인스턴스가 생성됩니다.

메모리 구조를 보면,<br/>
![상속의 메모리상태](/images/inheritance_memory.png)<br/>

즉 앞서 배운 super()의 의미가 상위클래스의 공간을 가리킵니다.<br/>
(위 그림은 논리적 구조입니다.)<br/>

## 4. 상위 클래스로의 묵시적 형 변환(업캐스팅)

다음으로 상위 클래스로의 묵시적 형 변환에 대해 알아보겠습니다.<br/>

1. 상위 클래스 형으로 변수를 선언하고 하위 클래스 인스턴스를 생성할 수 있습니다.
2. 하위 클래스는 상위 클래스의 타입을 내포하고 있으므로 상위 클래스로의 형 변환이
   가능합니다.
3. 상속 관계에서 모든 하위 클래스는 상위 클래스로 묵시적 형 변환이 가능합니다.<br/>
   (그 역은 성립하지 않습니다.)

```java
Customer vc = new VIPCustomer();
```

위의 코드가 에러가 안나는 이유는 VIPCustomer가 Customer 상속받았기 때문에 Customer타입을 내포하기 때문입니다.<br/>

```java
Customer customerLee = new Customer();
VIPCustomer customerKim = new VIPCustomer();

customerLee = customerKim;
```

Customer로 변수가 선언되지 않더라도 상속을 받은 클래스의 경우 상위클래스로 묵시적 형 변환이 됩니다.<br/>
(업 캐스팅)<br/>
즉 상위 클래스가 먼저 생성되고 그 다음, 생성된 하위 클래스는 내부적으로<br/>
상위 클래스의 타입을 내포하고 있습니다.<br/>
VIPCustomer는 VIPCustomer임과 동시에 Customer을 상속받았기 때문에,<br/>
Customer 타입을 내포하고 있습니다.<br/>

## 5. 형 변환에서의 메모리

```java
Customer vc = new VIPCustomer();
```

의 경우 vc가 가리키는 것에 대해 알아보겠습니다.<br/>
(VIPCustomer은 Customer을 상속받은 상태입니다.)<br/>

![형 변환 메모리](/images/inheritance_type_memory.png)<br/>

상위 클래스 타입으로 변수가 선언된 경우 인스턴스들은 모두 생성이 됩니다.<br/>
그러나 가리키는 범위는 상위 클래스로 제한됩니다.<br/>
즉 메모리는 다 만들어졌지만, 변수의 타입이 Customer이므로 사용할 수 있는 것은 Customer의 멤버들 입니다.<br/>

## 6. 클래스 계층 구조가 여러 단계일 경우

다음은 클래스 계층 구조가 여러 단계일 경우를 보겠습니다.<br/>

![클래스 계층 구조](/images/inheritance_multi_class.png)<br/>

이 경우 Human은 내부적으로 Primate, Mammal의 자료형을 모두 내포합니다.<br/>
(업캐스팅 둘 다 가능)<br/>
