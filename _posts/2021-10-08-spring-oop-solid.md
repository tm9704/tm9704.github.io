---
title: "Spring(3) - 객체지향 설계 5원칙(SOLID)"
excerpt: "Spring OOP SOLID"
toc: true
toc_sticky: true
categories:
  - Spring
tags:
  - Spring
date: "2021-10-08 13:40:00"
last_modified_at: "2021-10-08 14:00:00"
---

객체지향 설계 5원칙인 SOLID에 대해 공부하겠습니다.<br/>

우선 응집도와 결합도에 대해 알아보고 가겠습니다.<br/>

좋은 소프트웨어의 설계를 위해서는 결합도(coupling)은 낮추고,<br/>
응집도(cohesion)은 높여야합니다.<br/>

1. 결합도(coupling)
   모듈(클래스)간의 상호 의존 정도를 나타내는 지표로써<br/>
   결합도가 낮으면 모듈간의 상호 의존성이 줄어들어서 객체의 재사용 및 유지보수가 유리합니다.
2. 응집도(cohesion)
   하나의 모듈 내부에 존재하는 구성 요소들의 기능적 관련성으로.<br/>
   응집도가 높은 모듈은 하나의 책임에 집중하고 독립성이 높아져, 재사용 및 유지보수가 용이합니다.

## 1. SRP(Single Responsibility Principle) : 단일 책임 원칙

단일 책임 원칙이란 어떠한 클래스를 변경해야 하는 이유는 한가지 뿐이여야 한다는 것입니다.<br/>

![SRP](/images/srp.png)<br/>

위의 그림을 보게되면 하나의 클라이언트에 너무 많은 기능이 들어있고<br/>
다른 모듈들이 FTP Client의 메서드들을 참조하고 있습니다.<br/>

이 경우 어느 한 메서드를 수정하면 그 메서드를 참조하고 있는 다른 모듈들을 모두 수정해야하는<br/>
문제점이 발생합니다.<br/>

여기서 우리는 단일 책임 원칙을 통해 각각 분리를 시킬 수 있습니다.<br/>

![SRP2](/images/srp2.png)<br/>

이런식을 각 기능들을 분리시켜준다면, 다른 모듈이 추가되어도 자신의 응집도가 높은,<br/>
자신의 기능에 대해서 확실하게 하고 있는 클래스 또는 모듈 객체가 있다면<br/>
재사용이 높아지고 결합도가 낮아집니다.<br/>

예시를 하나 보겠습니다.

```java
class Unit{
    private String name;
    private int speed;

    private void move(){}
}

class Zergling extends Unit{
    public void move(){
        this.speed += 3;
    }
}

class Tank extends Unit{
    public void move(){
        if("Tank Mode"){
            speed = 0;
        }else{
            speed = 10;
        }
    }
}

class Plane extends Unit{
    public void Plane(){
        this.충돌 = false;
    }

    pubic void move(){
        speed = 15;
    }
}
```

(위 코드는 실제 코드가 아니라 이해를 위한 코드입니다.)<br/>

위 코드는 단일 책임과 다형성을 이용한 코드입니다.<br/>
각각의 객체의 기능만 할 수 있도록 설계를 할 수 있습니다.<br/>

## 2. OCP(Open Closed Principle) : 개방 폐쇄 원칙

자신의 확장에는 열려 있고, 주변의 변화에 대해서는 닫혀 있어야 합니다.<br/>

상위 클래스 또는 인터페이스를 중간에 둠으로써, 자신의 변화에 대해서는 폐쇄적이지만,<br/>
인터페이스는 외부의 변화에 대해서 확장을 개방해 줄 수 있습니다.<br/>

이러한 부분은 JDBC와 Mybatis, Hibernate등 JAVA에서는 Stream(Input, Output)에서 찾아볼 수 있습니다.<br/>

![ocp](/images/ocp.png)<br/>

위의 그림에서,<br/>
Application의 입장에서 외부적으로는 굉장히 많은 DB가 있을 수 있는데,<br/>
이 많은 DB들을 직접 다 연결할 경우 DB가 추가될 때마다 Application을 수정해야합니다.<br/>
그렇기 때문에 중간에 JDBC 인터페이스를 둬서 내부적으로는 1개의 통로를 가지고 외부적으로는 n개의 통로로<br/>
확장될 수 있도록 한 것입니다.<br/>

## 3. LSP(Liskov Substitution Principle) : 리스코프 치환 원칙

서브 타입은 언제나 자신의 기반(상위) 타입으로 교체 할 수 있어야 합니다.<br/>

![lsp](/images/lsp.png)<br/>

위의 그림에서 LSP를 위반한 그림을 보면,<br/>
소나타와 그랜저는 아반떼를 대신할 수 없습니다.<br/>

반면 LSP를 잘 적용한 그림을 보면<br/>
정찰기와 수송기는 비행기를 대신할 수 있습니다.<br/>

## 4. ISP(Interface Segregation Principle) : 인터페이스 분리 원칙

클라이언트는 자신이 사용하지 않는 메서드에 의존 관계를 맺으면 안됩니다.<br/>

프로젝트 요구 사항과 설계에 따라서 SRP(단일 책임 원칙)과 ISP(인터페이스 분리 원칙)를 선택하면 됩니다.<br/>

![isp](/images/isp.png)<br/>

위의 그림에서 지도라는 객체는 여러가지 기능을 제공할 수 있습니다.<br/>
그렇다고 해서 자전거 내비게이션을 만든다고 지도를 상속받아서 개발을 한다면,<br/>
필요없는 기능들도 정의해야하는 상황이 발생할 수 있습니다.<br/>

이런것을 해결하기 위해 자전거 내비게이션이 지도를 상속받는게 아니라<br/>
자전거 길 안내라는 인터페이스를 정의해서 자전거 길 안내만 받게 하는 것이 ISP입니다.<br/>

## 5. DIP(Dependecy Inversion Principle) : 의존 역전 원칙

자신보다 변하기 쉬운 것에 읜존하지 말아야 합니다.<br/>

![dip1](/images/dip1.png)<br/>

위의 그림에서 사람은 옷에 의존할 수 있습니다.<br/>
그러나 옷은 계절마다 변할 수 있습니다. 즉 사람이 자신보다 변하기 쉬운 옷에 의존하고 있습니다.<br/>

![dip2](/images/dip2.png)<br/>

위의 그림은 사람은 옷이라는 인터페이스를 두고 해당 인터페이스를 의존합니다.<br/>
각 계절 옷도 옷을 의존하며 의존 역전이 일어납니다.<br/>

개발 폐쇄 원칙에서도 살펴본 부분인데,<br/>
SOLID는 객체 지향 4대 특성에 기반함으로써 유사한 모양을 지닙니다.<br/>
