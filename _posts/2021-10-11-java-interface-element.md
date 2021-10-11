---
title: "Java(32) - 인터페이스의 요소들"
excerpt: "Java Interface Element"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-11 13:45:00"
last_modified_at: "2021-10-09 14:10:00"
---

앞선 포스트에서 인터페이스의 요소들에 대해 간단히 보고 넘어갔습니다.<br/>
이번 포스트에서 자세하게 공부하도록 하겠습니다.<br/>

## 1. 인터페이스의 요소

1. 상수<br/>
   인터페이스에 선언된 모든 변수는 상수로 처리됩니다.
2. 메서드<br/>
   모든 메서드는 추상 메서드가 됩니다.
3. 디폴트 메서드<br/>
   디폴트 메서드는 기본 구현을 가지는 메서드입니다.<br/>
   인터페이스의 경우 기본적으로 추상메서드만을 제공하기 때문에,<br/>
   인터페이스를 구현하는 클래스들이 공통적으로 지녀야할 메서드들을<br/>
   구현하는 클래스마다 구현을 각각 해줘야 했습니다.<br/>

   즉 인터페이스가 기본적으로 제공하는 메서드가 디폴트 메서드입니다.<br/>
   (구현하는 클래스에서 필요 시 재정의가 가능합니다.)<br/>

   디폴트 메서드는 선언시 default 키워드를 앞에 붙여주면 됩니다.<br/>
   나중에 인터페이스를 구현한 클래스가 있고 그 클래스가 인스턴스화 된 이후에 호출해서 사용합니다.

4. 정적(static) 메서드<br/>
   인스턴스의 생성과 관계없이 인터페이스 타입으로 호출하는 메서드입니다.<br/>

   정적 메서드는 선언시 앞에 static 키워드를 붙여주면 됩니다.<br/>
   사용 시 인스턴스 변수로 가져다 사용하는 것이 아닌, 인터페이스 타입으로 사용합니다.

5. private 메서드<br/>
   인터페이스 내에서 사용하기 위해 구현한 메서드입니다.<br/>
   구현하는 클래스에서 재정의는 불가능합니다.<br/>
   (디폴트 메서드나 정적 메서드에서 사용하기 위해 선언한 메서드)<br/>

   private 메서드에는 2가지 종류가 있는데, 일반 메서드와 static메서드가 있습니다.<br/>
   일반 메서드는 디폴트 메서드, static으로 선언한 것은 static 메서드에서 사용하면 됩니다.<br/>

## 2. 각 메서드 예시

```java
interface Calc {
	double PI = 3.14;
	int ERROR = -999999999;

	int add(int num1, int num2);
	int substract(int num1, int num2);
	int times(int num1, int num2);
	int divide(int num1, int num2);

	default void description() { //디폴트 메서드
 		System.out.println("점수 계산기를 구현합니다.");
 		myMethod();
 	}

 	static int total(int[] arr) { //정적(static) 메서드
 		int total = 0;

 		for(int i: arr) {
 			total += i;
 		}

 		mystaticMethod()
 		return total;
 	}

//private 메서드
 	private void myMethod() {
 		System.out.println("private method");
 	}

 	private static void mystaticMethod() {
 		System.out.println("dfs");
 	}
}

abstract class Calculator implements Calc {

	@Override
	public int add(int num1, int num2) {
		return num1 + num2;
	}

	@Override
	public int substract(int num1, int num2) {
		return num1 - num2;
	}
}

class CompleteCalc extends Calculator{

	@Override
	public int times(int num1, int num2) {
		return num1 * num2;
	}

	@Override
	public int divide(int num1, int num2) {
		if(num2 == 0)
			return ERROR;
		else
			return num1 / num2;
	}

	public void showInfo() {
		System.out.println("모두 구현하였습니다.");
	}

	@Override
 	public void description() { //디폴트 메서드 overriding
 		System.out.println("재정의 한 description");
 	}
}

public class CalcTest {

	public static void main(String[] args) {
		CompleteCalc calc = new CompleteCalc();

		int n1 = 10;
		int n2 = 2;

		System.out.println(calc.add(n1, n2));
		System.out.println(calc.substract(n1, n2));
		System.out.println(calc.times(n1, n2));
		System.out.println(calc.divide(n1, n2));

		calc.description();

 		int[] arr = {1,2,3,4,5};
 		int sum = Calc.total(arr);
 		System.out.println(sum);
	}
}
```

## 3. 여러개의 인터페이스 구현하기

인터페이스는 구현 코드가 없으므로 하나의 클래스가 여러 인터페이스를 구현할 수 있습니다.<br/>

디폴트 메서드의 이름이 중복되는 경우에는 재정의를 해야합니다.<br/>
(overriding)<br/>

여러 인터페이스를 구현한 클래스는 인터페이스 타입으로 형 변환 되는 경우,<br/>
해당 인터페이스에 선언된 메서드만 사용이 가능합니다.

![인터페이스 구현](/images/interface2.png)<br/>

위의 그림은 Customer 클래스가 Buy와 Sell, 두개의 인터페이스를 구현한 그림입니다.<br/>

```java
interface Buy {
	void buy();

 	default void order() {
 		System.out.println("구매 주문");
 	}
}

interface Sell {
	void sell();

 	default void order() {
 		System.out.println("판매 주문");
 	}
}

 class Customer implements Buy, Sell{

	@Override
 	public void sell() {
 		System.out.println("customer sell");
 	}

 	@Override
 	public void buy() {
 		System.out.println("customer buy");
 	}

// 	@Override
// 	public void order() {
// 		System.out.println("customer order");
// 	}

 	@Override
 	public void order() {
 		// TODO Auto-generated method stub
 		Buy.super.order();
 	}

 	public void sayHello() {
 		System.out.println("hello");
 	}

}

public class CustomerTest {

	public static void main(String[] args) {
		Customer customer = new Customer();

 		customer.buy();
 		customer.sell();
 		customer.order();
 		customer.sayHello();

 		Buy buyer = customer;
 		buyer.buy();
 		//인스턴스의 오더가 호출됨.
 		buyer.order();

 		Sell seller = customer;
 		seller.sell();
 		seller.order();
	}

}
```

Customer의 타입으로 인스턴스화 한 Customer 인스턴스의 경우,<br/>
order()메서드는 Customer에서 재정의하거나 Buy.super.order() or Sell.super.order()한<br/>
order()메서드를 호출합니다.<br/>

Buy, Sell 타입으로 인스턴스화 되거나 형 변환이 된 Customer 인스턴스의 경우 <br/>
Buy나 Sell의 order()메서드가 호출됩니다.<br/>
(오버라이딩과 관계없이)

## 4. 인터페이스 상속

인터페이스 간에도 상속이 가능합니다.<br/>

인터페이스의 경우 일반 클래스 상속과 다르게 구현이 없기 때문에,<br/>
extends 뒤에 여러 인터페이스를 상속받는 것이 가능합니다.<br/>
(다중 상속)<br/>

구현 내용이 없으므로 타입 상속(type inheritance)라고 합니다.<br/>
(여러 인터페이스가 있을 때 여러 인터페이스를 상속 받아서 하나의 인터페이스가 만들어질 수 있습니다.)<br/>

![인터페이스 상속](/images/interface_inheritance.png)<br/>

위의 그림은 MyInterface라는 인터페이스가 X, Y라는 인터페이스를 상속받고<br/>
MyClass가 MyInterface를 구현한 것입니다.<br/>

```java

interface X{
    void x();
}

interface Y{
    void y();
}

interface MyInterface extends X, Y{
    void myMethod();
}

class MyClass implements MyInterface{
    @Override x(){
        ...
    }

    @Override y(){
        ...
    }

    @Override myMethod(){
        ...
    }
}
```

여기서 MyClass는 X 타입, Y 타입이기도 하면서, MyInterface 타입이기도 합니다.<br/>

## 5. 인터페이스 구현과 클래스 상속 함께 사용하기

![인터페이스의 구현, 상속](/images/interface_extends_implements.png)<br/>

위의 그림은 BookShelf 클래스가 Shelf클래스를 상속받고, Queue 인터페이스를 구현한 것입니다.<br/>

```java
import java.util.ArrayList;

class Shelf {

	protected ArrayList<String> shelf;

	public Shelf() {
		shelf = new ArrayList<String>();
	}

	public ArrayList<String> getShelf() {
		return shelf;
	}

	public int getCount() {
		return shelf.size();
	}
}

interface Queue {

	void enQueue(String title);
	String deQueue();

	int getSize();
}

class BookShelf extends Shelf implements Queue{

	@Override
	public void enQueue(String title) {
		shelf.add(title);
	}

	@Override
	public String deQueue() {

		return shelf.remove(0);
	}

	@Override
	public int getSize() {
		return getCount();
	}

}

public class BookTest {

	public static void main(String[] args) {
		Queue bookQueue = new BookShelf();
		bookQueue.enQueue("태백산맥1");
		bookQueue.enQueue("태백산맥2");
		bookQueue.enQueue("태백산맥3");

		System.out.println(bookQueue.deQueue());
		System.out.println(bookQueue.deQueue());
		System.out.println(bookQueue.deQueue());
	}

}
```

위 코드는 원래 선반이 있고, 그 선반을 상속받은 책 선반이 있고,<br/>
해당 책 선반이 Queue라는 자료구조에 의해 책이 정리된다고 했을 때<br/>
Queue라는 자료구조가 선언되어 있고 그것을 구현해서 사용한다는 코드입니다.<br/>

## 6. Diamond Problem

Java에서는 일반 클래스의 경우 다중 상속이 불가능합니다.<br/>
그 이유가 Diamond Problem 즉 다이아몬드 문제입니다.<br/>

Person이라는 클래스가 있고 Father와 Mother이라는 클래스가 Person을 상속받는다고 가정합시다.<br/>
그리고 Child라는 클래스가 Father과 Mother 둘 다 상속받는다고 가정할 때,<br/>
Child는 Father과 Mother의 멤버 변수들과 메서드를 상속받습니다.<br/>

그런데 Father과 Mother에 같은 메서드가 있을 경우,<br/>
Child에서 그 메서드를 Child타입으로 선언된 Child참조변수.해당메서드() 로 호출할 경우<br/>
컴파일 에러가 발생합니다.<br/>

그 이유는 Child가 Father과 Mother중 어디에 있는 메서드를 상속받아야 하는지 모르기 떄문입니다.<br/>
