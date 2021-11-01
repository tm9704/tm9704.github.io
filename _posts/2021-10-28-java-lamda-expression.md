---
title: "Java(53) - 자바 람다식"
excerpt: "Java Lamda Expression"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-28 13:10:00"
last_modified_at: "2021-10-27 14:00:00"
---

## 1. 람다식이란

람다식은 자바에서 함수형 프로그래밍 (functional programming)을 구현하는 방식입니다.<br/>

클래스를 생성하지 않고 함수의 호출만으로 기능을 수행하며,<br/>
함수형 인터페이스를 선언합니다.<br/>

여기서 함수형 프로그래밍이란,<br/>

- 함수를 베이스로한 프로그래밍
- 매개변수를 받아서 그 매개변수를 이용해서 프로그램을 하게 되면 외부 변수 사용을 하지않음<br/>
  (순수 함수형 프로그래밍)
- 외부 변수를 사용하지 않기 때문에 외부에 부수적인 영향(side effect)를 주지 않음
- 입력받은 자료를 기반으로 수행되고 외부에 영향을 미치지 않으므로<br/>
  병렬처리가 가능하며 안정적이고 확장성 있는 프로그래밍 방식

입니다.<br/>

내부적으로는 익명객체를 생성합니다.<br/>

## 2. 람다식 문법

1. 매개변수가 하나인 경우 괄호 생략이 가능합니다. (두개인 경우 생략 불가능)<br/>

   ```java
   str -> {System.out.println(str);}
   ```

   위에서 보듯이 함수의 이름이 없기 때문에 익명함수라는 것을 알 수 있습니다.<br/>
   -> 오퍼레이터는 앞에 매개변수를 이용해 뒤의 문장을 수행한다는 의미입니다.<br/>

2. 중괄호 안의 구현부가 한 문장인 경우 중괄호 생략이 가능합니다.<br/>

   ```java
   str -> System.out.println(str);
   ```

3. 중괄호 안의 구현부가 한 문장이라도 return문은 중괄호 생략이 불가능합니다.<br/>

   ```java
   str -> return str.length(); //오류
   ```

4. 중괄호 안의 구현부가 반환문 하나라면 return문과 중괄호 모두 생략이 가능합니다.<br/>

   ```java
   (x,y) -> x+y //두 값을 더하여 반환
   str -> str.length() //문자열 길이를 반환
   ```

## 3. 함수형 인터페이스

함수형 인터페이스는 람다식을 선언하기 위한 인터페이스입니다.<br/>

익명함수와 매개변수만으로 구현되므로 단 하나의 메서드만을 선언해야합니다.<br/>
(두개 이상의 메서드가 선언되면 어느 메서드의 호출인지 모호해짐)<br/>

@Fuctional Interface 애노테이션을 사용합니다.<br/>
(람다식을 위한 인터페이스라는 애노테이션)<br/>

원래는 어떤 인터페이스가 있고 그 인터페이스의 메서드를 호출한다 라고 하면<br/>
인터페이스를 구현한 클래스를 만들고 그 클래스의 인스턴스를 생성하고 참조변수를 통해 호출했습니다.<br/>

예시를 보겠습니다.<br/>
두개의 수 중 큰 수를 구하는 코드입니다.<br/>

```java
//MyMaxNumber.java

@FunctionalInterface
public interface MyMaxNumber {
	int getMaxNumber(int x, int y);
} //메서드를 선언하기 위해 인터페이스를 사용
```

위 코드에서 만약 void add(int x, int y)라는 함수가 하나 더 작성됐다고 가정합시다.<br/>
이런 상황에서 람다식을 사용하면 x, y를 두개의 함수가 모두 사용하기 때문에 어떤 것을 사용할지 모호해집니다.<br/>

```java
//TestMyNumber.java

public class TextMyNumber {
	public static void main(String[] args) {

		MyMaxNumber max = (x,y) -> (x>=y)?x:y;
    //메서드를 구현
		System.out.println(max.getMaxNumber(10,20));
    //사용
	}
}
```

두번째 예시를 보겠습니다.<br/>

람다식을 위한 함수형 인터페이스<br/>

```java
@FunctionalInterface
public interface StringConcat {
	public void makeString(String s1, String s2);
}
```

원래 람다식을 이용하지 않을 때<br/>

```java
public class StringConImp1 implements StringConcat{

	@Override
	public void makeString(String s1, String s2) {
		System.out.println (s1 + "," + s2);
	}
}
```

```java
public class TestString {

	public static void main(String[] args) {

		StringConImp1 imp1 = new StringConImp1();
		imp1.makeString("hello", "world");

		StringConcat concat = (s,v) -> System.out.println(s + "," + v);
		concat.makeString("hello", "world");

		StringConcat concat2 = new StringConcat() {

			@Override
			public void makeString(String s1, String s2) {
				System.out.println(s1 + "," + s2);
			}
		};

		concat2.makeString("hello", "world");
	}
}
```

우리가 람다식으로 구현하지만, 내부에는 익명 객체가 생성이 됩니다.<br/>
즉 익병 내부클래스가 내부적으로 람다식에서 사용됩니다.<br/>
(위의 코드에서 concat2)<br/>

## 4. 함수를 변수처럼 사용하는 람다식

프로그램에서 변수는<br/>

- 자료형에 기반하여 선언 ex)int a;
- 매개변수로 전달 ex) int add(int x, int y);
- 메서드의 반환값으로 사용 ex) return num;

입니다.<br/>

람다식은 프로그램 내에서 변수처럼 사용이 가능합니다.<br/>
위의 예시처럼 함수의 구현부분이 변수에 대입됩니다.<br/>

다른 예시를 보겠습니다.<br/>

```java
interface PrintString{
	void showString(String str);
}

public class TestLamda {

	public static void main(String[] args) {

		PrintString lambdaStr = s -> System.out.println(s);
		lambdaStr.showString("Test");

		showMyString(lambdaStr);

		PrintString test = returnString();
		test.showString("Test3");
	}

	public static void showMyString(PrintString p) {
		p.showString("Test2");
	}

	public static PrintString returnString() {
		return s->System.out.println(s + "!!!");
	}
	//함수의 구현부가 마치 변수처럼 반환돼서 변수값에 대입.
}
```
