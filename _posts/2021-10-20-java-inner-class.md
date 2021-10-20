---
title: "Java(45) - 내부 클래스"
excerpt: "Java Inner Class"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-20 15:10:00"
last_modified_at: "2021-10-20 15:30:00"
---

## 1. 내부 클래스

내부 클래스란,<br/>

- 클래스 내부에 구현한 클래스 (중첩된 클래스)<br/>
  (해당 클래스를 감싸고 있는 외부 클래스와 밀접한 연관이 있는 경우가 많음)
- 클래스 내부에서 사용하기 위해 선언하고 구현하는 클래스<br/>
  (주로 어떤 클래스의 내용이 외부에서 쓸 일이 별로 없고 어떤 클래스 내부에서만 쓸 경우)
- 주로 외부 클래스 생성자에서 내부 클래스를 생성

종류로는,<br/>

- 인스턴스 내부 클래스
- 정적(static) 내부 클래스
- 지역(local) 내부 클래스
- 익명(anonymous) 내부 클래스

가 있습니다.

## 2. 내부 클래스의 유형

보통 내부 클래스는 변수와 동일하게 생각하면 됩니다.<br/>

1. **인스턴스 내부 클래스 (멤버 변수)<br/>**

   인스턴스 내부 클래스는 내부적으로 사용할 클래스입니다.(private 권장)<br/>
   외부 클래스가 생성된 후 생성되며, private가 아닌 내부 클래스는 다른 외부 클래스에서<br/>
   생성이 가능합니다.<br/>

   ex) OutClass outClass = new OutClass();<br/>
   OutClass.InClass inClass = outClass.new InClass();<br/>

   - 구현 위치: 외부 클래스
   - 사용할 수 있는 외부 클래스 변수<br/>
     외부 인스턴스 변수, 외부 전역 변수
   - 생성 방법<br/>
     외부 클래스를 먼저 만든 후 내부 클래스 생성

2. **정적(static) 내부 클래스 (static 변수)<br/>**

   정적 내부 클래스는 외부 클래스 생성과 무관하게 사용이 가능합니다.<br/>
   정적 변수와 정적 메서드를 사용합니다.<br/>

   - 구현 위치: 외부 클래스 멤버 변수와 동일
   - 사용할 수 있는 외부 클래스 변수<br/>
     외부 전역 변수
   - 생성 방법<br/>
     외부 클래스와 무관하게 생성<br/>

3. **지역(local) 내부 클래스 (메서드 안 변수)<br/>**

   지역 내부 클래스는 지역 변수와 같이 메서드 내부에서 정의하여 사용하는 클래스입니다.<br/>
   메서드의 호출이 끝나면, 메서드에 사용된 지역변수의 유효성은 사라집니다.<br/>

   메서드 호출 이후에도 사용해야 하는 경우가 있을 수 있으므로, 지역 내부 클래스에서<br/>
   사용하는 메서드의 지역변수나 매개변수는 final로 선언됩니다.<br/>

   - 구현 위치: 메서드 내부에 구현
   - 사용할 수 있는 외부 클래스 변수<br/>
     외부 인스턴스 변수, 외부 전역 변수
   - 생성 방법<br/>
     메서드를 호출할 때 생성<br/>

4. **익명 내부 클래스 (변수 중에는 없음)<br/>**

   익명 내부 클래스는 내부 클래스 중 가장 많이 사용하는 내부 클래스입니다.<br/>

   이름이 없는 클래스를 의미하며, 클래스의 이름을 생략하고 주로 하나의 인터페이스나,<br/>
   하나의 추상 클래스를 구현하여 반환합니다.<br/>

   인터페이스나 추상 클래스 자료형 변수에 직접 대입하여 클래스를 생성하거나<br/>
   지역 내부 클래스의 메서드 내부에서 생성하여 반환할 수 있습니다.<br/>

   - 구현 위치: 메서드 내부에 구현, 변수에 대입하여 직접 구현
   - 사용할 수 있는 외부 클래스 변수<br/>
     외부 인스턴스 변수, 외부 전역 변수
   - 생성 방법<br/>
     메서드를 호출할 때 생성되거나,<br/>
     인터페이스 타입 변수에 대입할 때 new 예약어를 사용하여 생성<br/>

## 3. 인스턴스 내부 클래스 예시

```java
class OutClass{
	private int num = 10;
	private static int sNum = 20;
	private InClass inClass;

	public OutClass(){
		inClass = new InClass();
	}

	class InClass{ //인스턴스 내부 클래스
		int iNum = 100;
//		static int sInNum = 200;
		void inTest() {
			System.out.println("OutClass num = " + num + "(외부 클래스의 인스턴스 변수)");
			System.out.println("OutClass sNum = " + sNum + "(외부 클래스의 스태틱 변수)");
			System.out.println("InClass inNum = " + iNum + "(내부 클래스의 인스턴스 변수)");
		}
	}

	public void usingInner() {
		inClass.inTest();
	}
}

public class InnerTest {

	public static void main(String[] args) {

		OutClass outClass = new OutClass();

		outClass.usingInner();

		OutClass.InClass myInClass = outClass.new InClass();
		myInClass.inTest();
	}
}
```

내부 클래스는 컴파일이 되면 OutClass$InClass.class 이런식으로 생성이됩니다.<br/>

주로 OutClass(외부 클래스) 내부 생성자에서 InClass(내부 클래스)를 생성합니다.<br/>
ex) private InClass inClass;<br/>
public OutClass(){<br/>
inClass = new InClass();<br/>
}<br/>

내부 클래스가 static 클래스가 아닐 경우 static 변수를 내부 클래스에서 선언하면 error가 발생합니다.<br/>
(내부 클래스는 생성 이후에 사용이 가능하기 때문 => 내부 클래스는 외부 클래스가 생성된 후 생성됩니다)<br/>

내부 클래스(private)의 메서드를 사용하기 위해서는 OutClass에 사용 메서드가 따로 필요합니다.<br/>
(usingInner())<br/>

외부에서도 내부 클래스의 생성이 가능합니다.<br/>
ex) OutClass.InClass myInClass = outClass.new InClass();<br/>

외부에서 생성할 때 중요한 점은 OutClass가 생성되어 있어야 하고,<br/>
OutClass.InClass처럼 직접 접속해서 생성합니다.<br/>
그리고 생성되어져 있는 OutClass의 참조변수에 new 해서 사용합니다.<br/>
Inner클래스가 private인 경우 생성이 불가능합니다.<br/>
(이렇게 많이 사용하지는 않음, 내부 클래스는 외부 클래스안에서만 사용하기 위해 보통 만들기 때문)<br/>

위 코드는 단순히 사용방법을 보기 위한 인스턴스 내부 클래스 예시입니다.<br/>

## 4. static 내부 클래스 예시

```java
class OutClass{

    private static int sNum = 20;

	static class InStaticClass{
		int inNum = 100;
		static int sInNum = 200;

		void inTest() {
			System.out.println(inNum);
			System.out.println(sInNum);
			System.out.println(sNum);
		}

		static void sTest() {
			System.out.println(sInNum);
			System.out.println(sNum);
		}
	}
}

public class InnerTest {

	public static void main(String[] args) {

		OutClass outClass = new OutClass();

        //위의 외부 클래스 생성과는 상관 없음
		OutClass.InStaticClass sInClass = new OutClass.InStaticClass();
		OutClass.InStaticClass.sTest();
		sInClass.inTest();

	}
}
```

정적 내부 클래스의 특징은 OutClass 내부에 만들긴 하지만,<br/>
OutClass가 생성되고 쓰는게 아닙니다. (생성 여부와 관계없이 사용 가능)<br/>

정적 내부 클래스 내부에는 staitc 변수, static 메서드 선언이 가능합니다.<br/>
그러나 정적 내부 클래스 내부에 선언된 인스턴스 변수는<br/>
정적 내부 클래스 내부에 선언된 static 메서드 내부에서는 사용이 불가능합니다.<br/>
(static 메서드는 생성과 관계가 없기 때문)<br/>

생성 방법은<br/>
ex) OutClass.InStaticClass sInClass = new OutClass.InStaticClass();<br/>
이렇게 외부 클래스의 생성과 관계없이 바로 생성이 가능합니다.<br/>

정적 내부 클래스 내부의 static 메서드 사용방법은<br/>
ex) OutClass.InStaticClass.sTest();<br/>
외부 클래스를 생성하지 않고 바로 사용이 가능합니다.<br/>

정적 내부 클래스 내부의 일반 메서드 사용방법은<br/>
정적 내부 클래스를 생성하고 참조해서 사용합니다.<br/>
ex)sInClass.inTest();<br/>

## 5. 지역 내부 클래스 예시

```java
class Outer{

	int outNum = 100;
	static int sNum = 200;

	Runnable getRunnable(int i) {

		int num = 100;

		class MyRunnable implements Runnable{

			@Override
			public void run() {
//				num += 10;
//				i = 200;
				System.out.println(num);
				System.out.println(i);
				System.out.println(outNum);
				System.out.println(Outer.sNum);
			}
		}
		return new MyRunnable();
	}
}

public class LocalInnerClassTest {

	public static void main(String[] args) {

		Outer outer = new Outer();
		Runnable runnable = outer.getRunnable(50);

		runnable.run();
	}
}
```

위 코드에서 Runnable getRunnable(int i){} 메서드는 Runnable한 객체를 반환하는 메서드입니다.<br/>

해당 메서드 내부에 Runnable한 객체를 클래스로 선언합니다.<br/>
(Runnable한 객체는 쓰레드를 공부할 때 다루겠습니다.)<br/>

그리고 해당 메서드에서 객체를 반환합니다.<br/>

여기서 MyRunnable 클래스는 메서드 내부에 있기 때문에 지역 내부 클래스가 됩니다.<br/>
해당 클래스 내부의 메서드에서는 외부 클래스의 멤버변수, static 변수의 사용이 가능합니다.<br/>

그러나 지역 내부 클래스 내부에서 지역 내부 클래스를 감싸고 있는 메서드의 변수들은 사용이 불가능합니다.<br/>
이유는 메서드 내부의 지역변수들은 해당 메서드가 호출되고 반환될 때까지만 유효하기 때문이고,<br/>
지역 내부 클래스가 반환된 후에 지역 내부 클래스 내부의 메서드는 언제든지 불릴 수 있기 때문에<br/>
유효성의 주기 차이로 변수들이 유효하지 않게 됩니다.<br/>

추가적으로, 지역 내부 클래스가 쓰이는 메서드에서 지역 변수들은 자동으로 상수가 됩니다.(final)<br/>
즉 참조는 가능하지만 변경은 불가능합니다.<br/>
(직접 명시는 안해줘도됨(final))<br/>

마지막으로 정리하자면,<br/>

위 코드에서 i, num 변수는 getRunnable의 지역변수이고, stack 메모리에 생성이됩니다.<br/>
그러나 지역 내부 클래스 내부에서 변경시 오류가 나는데, 메서드가 호출되는 시점과<br/>
class의 생성 주기가 다르기 때문입니다.<br/>

메서드는 호출후 반환을 하게되면 사라지지만, 클래스 내부의 run()메서드의 경우는 다시 호출될 여지가<br/>
있는데, 메서드의 지역변수들은 없을 수 있습니다.<br/>

즉 i,num은 stack에 잡히면 안되고 final로 선언됩니다.<br/>

지역 내부 클래스 사용부분을 보면<br/>

```java
public class LocalInnerClassTest {

	public static void main(String[] args) {

		Outer outer = new Outer();
		Runnable runnable = outer.getRunnable(50);

		runnable.run();
	}
}
```

getRunnable(50)에 해당하는 메서드는 호출되고 사라집니다.<br/>
그러나 아래의 run()메서드는 언제든지 호출될 수 있기 때문에<br/>
메서드 내부의 지역 변수는 변경이 불가능한 final로 선언이 되는 것입니다.<br/>

해당 코드에서 MyRunnable 이라는 클래스의 이름은 getRunnable메서드 안에서만 사용되고<br/>
다른 곳에서는 사용하지 않습니다.<br/>

그래서 우리는 익명 내부 클래스라는 이름이 없는 클래스를 사용합니다.<br/>

## 6. 익명 내부 클래스

우선 예시를 보겠습니다.<br/>

```java
class Outer2{

	int outNum = 100;
	static int sNum = 200;

	Runnable getRunnable(int i) {
		int num = 100;
		//1
		return new Runnable(){
			@Override
			public void run() {
				System.out.println(num);
				System.out.println(i);
				System.out.println(outNum);
				System.out.println(Outer.sNum);
			}
		};
	}
	//2
	Runnable runner = new Runnable() {
		@Override
		public void run() {
			System.out.println("test");
		}
	};
}

public class AnonymousInnerClassTest {

	public static void main(String[] args) {
		Outer2 outer = new Outer2();
		outer.runner.run();
		Runnable runnable = outer.getRunnable(50);

		runnable.run();
	}
}
```

지역 내부 클래스 예시에서 봤듯이, MyRunnable이라는 클래스의 이름은 해당 클래스를 감싸는<br/>
메서드 내부에서만 사용이되기 떄문에, 해당 클래스를 없애고 바로 반환과 구현을 해줍니다.<br/>

```java
return new Runnable(){
	@Override
	public void run() {
		System.out.println(num);
		System.out.println(i);
		System.out.println(outNum);
		System.out.println(Outer.sNum);
	}
};
```

Runnable() 인터페이스를 바로 구현해서, run() 메서드를 구현하고 반환합니다.<br/>
즉 class 이름이 사라지지만 실행결과는 동일합니다.<br/>
(;를 마지막에 써주는 이유는 구현의 끝을 알리기 위해)<br/>

2 주석 밑에 또 다른 구현 방법이 있습니다.<br/>

```java
Runnable runner = new Runnable() {
		@Override
		public void run() {
			System.out.println("test");
		}
	};
```

익명 내부 클래스는 인터페이스나 추상 클래스에 대한 생성을 바로 할 수 있습니다.<br/>

원래 인터페이스나 추상 클래스를 생성하려면 상속받은 클래스를 만들고 생성해서 사용해야 하는데,<br/>
단 한개의 인터페이스나 추상 클래스는 익명 내부 클래스로 생성해서 주로 사용합니다.<br/>
