---
title: "Java(27) - 다운캐스팅"
excerpt: "Java Downcasting"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-07 12:50:00"
last_modified_at: "2021-10-07 13:20:00"
---

우리는 상속을 공부할 때 하위클래스에서 상위클래스로 형 변환되는 경우를 배웠습니다.<br/>
(업 캐스팅 (묵시적))<br/>

이번 포스트에서는 하위클래스로 형 변환이 되는 다운캐스팅에 대해 공부하겠습니다.<br/>

## 1. 다운캐스팅

다운캐스팅을 사용하는 경우는<br/>
묵시적으로 상위클래스로 형 변환된 인스턴스가 원래 자료형인 하위 클래스로<br/>
변환되어야 하는 경우 사용합니다.<br/>

업캐스팅과는 다르게 명시적으로 이루어집니다.<br/>

```java
Customer vc = new VIPCustomer(); //묵시적 (업캐스팅)
VIPCustomer vCustomer = (VIPCustomer)vc; //명시적 (다운캐스팅)
```

즉 형변환이 된 상태에서 다시 원래 자신의 데이터 타입으로 돌아가는게 다운캐스팅입니다.<br/>

이 경우 vCustomer 앞의 VIPCustomer 클래스 타입과 괄호 안의 VIPCustomer,<br/>
두개만 확인하기 때문에 명시적으로 적어준 클래스 타입이 인스턴스 타입과 완전히 다른 경우<br/>
에러가 발생할 수 있습니다.<br/>

## 2. Instanceof

위에서 설명한 에러를 방지하기 위해 Instanceof 키워드를 사용합니다.<br/>

![instanceof](/images/instanceof.png)<br/>

```java
Animal hAnimal = new Human();
if(hAnimal instanceof Human){
    Human human = (Human)hAnimal;
}
```

위의 코드는 hAnimal 인스턴스 자료형이 Human형 이라면<br/>
인스턴스 hAnimal을 Human형으로 다운캐스팅하는 코드입니다.<br/>

즉 instanceof는 어느 객체가 정말 해당 타입의 객체인지 확인하고<br/>
true, false를 반환합니다.<br/>

상위클래스에서 오버라이딩으로 해결할 수 있는 경우는<br/>
다형성을 사용하는게 좋습니다.<br/>

오버라이딩이 정 안되는 경우에만 다운캐스팅을 사용하고<br/>
다운캐스팅의 안정성을 높이기 위해 instanceof를 사용합니다.
