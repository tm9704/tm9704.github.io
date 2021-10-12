---
title: "Spring(3) - 디자인 패턴"
excerpt: "Spring Design Pattern"
toc: true
toc_sticky: true
categories:
  - Spring
tags:
  - Spring
date: "2021-10-12 15:45:00"
last_modified_at: "2021-10-12 16:10:00"
---

## 1. 디자인 패턴

자주 사용하는 설계 패턴을 정형화해서 이를 유형별로 가장 최적의 방법으로 개발을<br/>
할 수 있도록 정해둔 설계입니다.<br/>

알고리즘과 유사하지만, 알고리즘은 명확하게 정답이 있지만,<br/>
디자인패턴은 명확하게 정답이 있는 형태가 아니며, 프로젝트의 상황에 맞추어 적용이 가능합니다.<br/>

## 2. 디자인 패턴의 장단점

장점으로는<br/>

1. 개발자(설계자) 간의 원활한 소통이 가능함
2. 소프트웨어의 구조 파악이 용이함
3. 재사용을 통한 개발 시간 단축
4. 설계 변경 요청에 대한 유연한 대처가 가능함
   <br/>

단점으로는<br/>

1. 객체지향으로 설계하고 구현해야하기 때문에 객체지향을 이해하고 있어야함
2. 초기 투자 비용 부담

## 3. 디자인 패턴의 종류

1. 생성 패턴 (생성자 관련)<br/>
   객체를 생성하는 것과 관련된 패턴으로, 객체의 생성과 변경이 전체 시스템에 미치는 영향을<br/>
   최소화 하고, 코드의 유연성을 높여줍니다.<br/>
   - Singleton
   - Builder
   - Chaining
   - ...
2. 구조 패턴<br/>
   프로그램 내의 자료구조나 인터페이스 구조 등 프로그램 구조를 설계하는데 활용될 수 있는 패턴,<br/>
   클래스, 객체들의 구성을 통해서 더 큰 구조를 만들 수 있게 해줍니다.<br/>
   큰 규모의 시스템에서는 많은 클래스들이 서로 의존성을 가지게 되는데, 이런 복잡한 구조를 개발하기 쉽게<br/>
   만들어 주고, 유지 보수 하기 쉽게 만들어 줍니다.<br/>
   - Adapter
   - Decorator
   - Facade
   - Proxy
   - ...
3. 행위 패턴<br/>
   반복적으로 사용되는 객체들의 상호작용을 패턴화한 것으로, 클래스나 객체들이 상호작용하는 방법과 책임을<br/>
   분산하는 방법을 제공합니다.<br/>
   행위 패턴은 행위 관련 패턴을 사용하여 독립적으로 일을 처리하고자 할 때 사용합니다.<br/>
   - Observer
   - Strategy
   - ...

## 4. Singleton Pattern

싱글톤 패턴(Singleton Pattern)은 어떠한 클래스(객체)가 유일하게 1개만 존재할 때 사용합니다.<br/>
(ex)여러명의 사람, 여러대의 PC가 한대의 프린터를 사용할 때 프린터)<br/>

이를 주로 사용하는 곳은 서로 자원을 공유할 때 사용하는데, 실물 세계에서는 프린터가 해당되며,<br/>
실제 프로그래밍에서는 TCP Socket 통신에서 서버와 연결된 connect 객체에 주로 사용합니다.<br/>
(Socket은 보통 1개를 사용하기 때문에)<br/>

Spring에서는 bin이라는 클래스 또는 객체는 기본적으로 singleton으로 관리됩니다.<br/>

![Singleton](/images/singleton.png)<br/>

싱글톤 패턴을 만드는 방법은<br/>
default 생성자를 private로 막고, getInstance 메소드(static)를 통해 생성되어 있는<br/>
객체를 가지고 오거나, 없으면 생성합니다.<br/>

static 메서드인 getInstance()를 통해 어떠한 클래스에서도 동일한 객체를 얻을 수 있습니다.<br/>

```java
public class SocketClient {

    private static SocketClient socketClient = null;

    // default 생성자 막기
    private SocketClient(){}

    public static SocketClient getInstance(){ //static 메소드 이므로 바로 접근가능

        if(socketClient == null){
            socketClient = new SocketClient();
            System.out.println("socket new instance");
        }

        return socketClient;
    }

    public void connect(){
        System.out.println("socket");
    }

}
```

위의 코드가 Singleton Pattern을 사용한 SocketClient 클래스입니다.<br/>

우선 private static으로 딱 한개만 존재하는 자기 자신 객체를 생성합니다.<br/>

기본 생성자를 default로 막았고,<br/>
static 메서드인 getInstance()에서 if문으로 객체가 존재하지 않을 경우 객체를 생성하고,<br/>
있다면 그대로 반환합니다.<br/>

```java
class AClazz {

    private SocketClient socketClient;

    public AClazz(){
        this.socketClient = SocketClient.getInstance();
    }

    public SocketClient getSocketClient() {
        return socketClient;
    }

}

class BClazz {

    private SocketClient socketClient;

    public BClazz(){
        this.socketClient = SocketClient.getInstance();
    }

    public SocketClient getSocketClient() {
        return socketClient;
    }
}

public class Main {

    public static void main(String[] args) {

        AClazz aClazz = new AClazz();
        BClazz bClazz = new BClazz();

        SocketClient aClient = aClazz.getSocketClient();
        SocketClient bClient = bClazz.getSocketClient();

        System.out.println("두개의 객체가 동일한가.");
        System.out.println(aClient.equals(bClient));
    }
}
```

각각 AClazz, BClazz에서 단 한개의 SocketClient 객체를 받고<br/>
Main에서 같은지 확인한 코드입니다.<br/>
결과는 true로, 단 한개의 동일한 객체를 공유합니다.<br/>
