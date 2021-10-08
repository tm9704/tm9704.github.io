---
title: "Java(29) - 추상 클래스 응용 : 템플릿 메서드"
excerpt: "Java Template Method"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-08 13:10:00"
last_modified_at: "2021-10-07 13:30:00"
---

## 1. 템플릿 메서드

- 템플릿(template) : 틀이나 견본

템플릿 메서드는 디자인패턴의 일종이며 템플릿의 정의를 그대로 활용한 메서드입니다.<br/>

추상 메서드나 구현된 메서드를 활용하여 전체의 흐름(시나리오)를 정의해 놓은 메서드입니다.<br/>
final로 선언하여 재정의할 수 없게 합니다.<br/>

즉 템플릿 메서드는 로직을 정의해 놓은 메서드라고 할 수 있습니다.<br/>
해당 순서대로 수행해라 하는 로직은 정해져 있고,<br/>
그 안에 구현된 메서드가 있을 수도, 추상 메서드가 있을 수도 있는데,<br/>
어떻게 구현하냐와 상관없이 로직의 흐름은 변경되지 않습니다.<br/>

총 정리를 하자면<br/>

1. 추상 클래스로 선언된 상위 클래스에서 추상 메서드를 이용해 전체 흐름 정의
2. 구체적인 각 메서드 구현은 하위 클래스에 위임
3. 하위 클래스들이 각각 다른 구현을 해도 템플릿 메서드에 정의된 시나리오대로 수행

## 2. 예제

템플릿 메서드를 활용한 예제를 보겠습니다.<br/>

```java
abstract class Car {
	public abstract void drive();
	public abstract void stop();

	public void startCar() {
		System.out.println("시동을 켭니다.");
	}

	public void turnOff() {
		System.out.println("시동을 끕니다.");
	}

	final public void run() {
		startCar();
		drive();
		stop();
		turnOff();
	}
}

class AICar extends Car{

	@Override
	public void drive() {
		System.out.println("자율 주행합니다.");
		System.out.println("자동차가 스스로 방향을 바꿉니다.");
	}

	@Override
	public void stop() {
		System.out.println("스스로 멈춥니다.");
	}
}

class ManualCar extends Car{

	@Override
	public void drive() {
		System.out.println("사람이 운전합니다.");
		System.out.println("사람이 핸들을 조작합니다.");
	}

	@Override
	public void stop() {
		System.out.println("브레이크를 밟아서 정지합니다.");
	}

}

public class CarTest {

	public static void main(String[] args) {
		Car aiCar = new AICar();
		aiCar.run();

		System.out.println("=================");

		Car manualCar = new ManualCar();
		manualCar.run();
	}

}
```

위의 코드에서 가장 상위클래스인 추상 클래스 Car를 보겠습니다.<br/>

각각 하위 클래스에서 구현해야 할 drive(), stop() 추상 메서드가 있고,<br/>
일반 메서드인 startCar(), turnOff() 메서드가 있습니다.<br/>

그 외에 final키워드가 있는 run() 메서드가 구현되어 있는데,<br/>
이게 템플릿 메서드입니다.<br/>

main 에서 각각 인스턴스들을 활용해 해당 run() 메서드를 실행시키면<br/>
run()메서드 로직에 따라 수행됩니다.<br/>

## 3. final 키워드

final은 변수를 상수화 시키는 키워드입니다.<br/>

final 변수는 값이 변경될 수 없는 상수가 됩니다.<br/>

```java
public static final double PI = 3.14;
```

즉 오직 한 번만 값을 할당할 수 있습니다.<br/>
값이 변할 일이 거의 없고 외부에서 정적으로 쓰는 경우가 있기 때문에<br/>
static 키워드와 함께 많이 사용됩니다.<br/>

final 메서드는 하위 클래스에서 재정의(overriding)할 수 없는 메서드입니다.<br/>

final 클래스는 상속이 불가능한 클래스입니다.<br/>
(ex)String 클래스)

## 4. public static final 상수 값 정의하여 사용하기

프로젝트 구현 시 여러 파일에서 공유해야 할 상수 값을 하나의 파일에 선언하여 사용하면 편리합니다.<br/>

```java
class define{
    public static final int MIN = 1;
    public static final int MAX = 99999;
    ...
}

public class UsingDefine{
    public static void main(String[] args){
        System.out.println("최솟값은 " + Define.MIN + " 입니다.");
        ...
    }
}
```

위의 코드처럼 static으로 상수값을 선언했으므로,<br/>
인스턴스를 생성하지 않고 클래스 이름으로 참조해서 사용이 가능합니다.<br/>
