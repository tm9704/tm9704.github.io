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

![상속](/images/inheritance.png/)<br/>

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

(수정중)
