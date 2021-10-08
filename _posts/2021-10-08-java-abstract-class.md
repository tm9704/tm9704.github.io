---
title: "Java(28) 추상 클래스"
excerpt: "Java Abstract Class"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-08 12:40:00"
last_modified_at: "2021-10-07 13:00:00"
---

## 1. 추상 클래스

추상 클래스 즉 abstract 클래스는 완벽하지 않은 클래스를 말합니다.<br/>
구체적이지 않고, 형태는 없고 개념만 존재합니다.<br/>

- 추상 클래스는 추상 메서드를 포함한 클래스입니다.<br/>
  추상 메서드란

  ```java
  public int calcPrice(int price){
    bonusPoint += price * bonusRatio;
    return price;
  }
  ```

  위의 코드를 보게되면 메서드의 body가 존재합니다 (괄호 안에 있는 실행부분)<br/>
  추상 메서드는 이 body가 존재하지 않고 선언부만 존재하는 메서드 입니다.<br/>

- 추상 클래스는 abstract 키워드를 사용합니다.<br/>
- 추상 클래스는 new(인스턴스화)할 수 없습니다.<br/>
  (new해서 인스턴스화 했을 때 메서드의 body가 없어서 불려질 수 있는 부분이 없음, 완전하지 않은 클래스)

## 2. 추상 클래스 구현

우선 코드를 보겠습니다.<br/>

```java
public abstract class Computer{
    public abstract void display();
    //display(); 와 display(){}는 다른 코드입니다.
    public abstract void typing();
    ...
}
```

위의 코드처럼 abstract 키워드를 통해 추상 클래스, 추상 메서드임을 나타냅니다.<br/>

추상 메서드 외에 구현된 메서드가 있어도 클래스에 abstract 키워드가 존재하면 추상 클래스가 됩니다.<br/>
(추상 클래스는 단독으로 쓰이지 않고 상위 클래스용으로 사용)<br/>

추상 클래스는 인스턴스의 생성이 불가능하다는 점만 제외하면 모든 부분에서 일반 클래스와 동일합니다.<br/>
추상 클래스 역시 참조변수의 선언도 가능하고 메소드 오버라이딩의 원리가 그대로 적용됩니다.<br/>

## 3. 추상 클래스 사용하기

추상 클래스는 주로 상속의 상위 클래스로 사용됩니다.<br/>

- 추상 메서드: 하위 클래스가 구현해야 하는 메서드
- 구현된 메서드: 하위 클래스가 공통으로 사용하는 기능의 메서드, 하위 클래스에 따라 재정의 가능

즉 구현해야할 메서드를 상위 클래스에 선언해두고, 구현의 책임을 하위 클래스에 위임합니다.<br/>

상속을 받은 하위 클래스는

1. 추상 클래스가 되거나
2. 추상 메서드들을 구현해야합니다.

만약 추상 메서드가 여러개고 한개만 하위클래스가 구현한 경우<br/>
하위 클래스는 추상 클래스가 되어야 합니다.<br/>

추상 클래스 역시 업캐스팅이 가능합니다.<br/>
(위에서 설명했듯이 인스턴스화가 불가능한 점을 빼고 동일)<br/>
추상 클래스 하나를 상속 받은 여러 클래스를 동일한 상위 클래스 타입으로 핸들링이 가능합니다.<br/>

추상 클래스의 예시를 하나 보고 마치겠습니다.<br/>

```java
abstract class Computer {

	public abstract void display();
	public abstract void typing();

	public void turnOn() {
		System.out.println("전원을 킵니다.");
	}

	public void turnOff() {
		System.out.println("전원을 끕니다.");
	}
}

class DeskTop extends Computer{

	@Override
	public void display() {
		System.out.println("DeskTop display");
	}

	@Override
	public void typing() {
		System.out.println("DeskTop typing");
	}

}

abstract class NoteBook extends Computer{

	public void typing() {
		System.out.println("NoteBook typing");
	}
}

class MyNoteBook extends NoteBook{

	@Override
	public void display() {
		System.out.println("MyNoteBook display");
	}

}

public class ComputerTest {

	public static void main(String[] args) {
		Computer computer = new DeskTop();
		computer.display();
		computer.turnOff();

		Computer computer2 = new MyNoteBook();
		computer2.display();
		computer2.turnOff();

	}
}
```
