---
title: "Java(30) - Interface"
excerpt: "Java Interface"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-09 11:47:00"
last_modified_at: "2021-10-09 12:10:00"
---

이번 포스트에서는 인터페이스에 대해 공부하겠습니다.<br/>

## 1. 인터페이스란?

우선 인터페이스(Interface)란 어떠한 객체에 대한 명세입니다.<br/>

"이 객체가 어떤 메서드들을 제공할 것이다"<br/>
"어떤 역할을 하는 객체다"<br/>

라는 것을 설명한 일종의 설명서입니다.<br/>
(프로젝트 설계 단계에서 주로 사용합니다.)<br/>

## 2. 인터페이스의 요소

1. 추상 메서드<br/>
   인터페이스는 모두 추상 메서드들로 이루어져 있습니다.<br/>
   (인스턴스화가 불가능)<br/>
   힙영역의 사용이 불가하므로 모든 변수들은 자동으로 상수가 됩니다.<br/>
2. 상수
3. 디폴트 메서드
4. 정적 메서드
5. private 메서드

3~5번째의 요소들은 자바8 버전 이후에 나온것들입니다.<br/>
기본적으로 인터페이스는 구현을 갖지 못하므로<br/>
여러 클래스들이 중복 구현을 하게 됩니다.<br/>
이러한 중복 구현을 막기 위해 기본적으로 제공되는 메서드들입니다.<br/>

## 3. 인터페이스 선언과 구현

예시를 보겠습니다.<br/>

```java
public interface Calc {
	double PI = 3.14;
	int ERROR = -999999999;

	int add(int num1, int num2);
	int substract(int num1, int num2);
	int times(int num1, int num2);
	int divide(int num1, int num2);
}
```

위의 코드에서 보듯이<br/>
인터페이스의 경우 class 키워드를 사용하는 대신 interface 키워드를 사용합니다.<br/>

double PI, int ERROR 처럼 인터페이스에서 선언한 변수는<br/>
컴파일 단계에서 상수로 변환됩니다.<br/>
(앞에 public static final 키워드가 붙음)<br/>

인터페이스에서 선언한 메서드는 컴파일 과정에서 추상 메서드로 변환됩니다.<br/>
(abstract키워드가 없어도 자동으로 변환됩니다.)<br/>

즉 인터페이스는 전체적으로 선언만 되어있습니다.<br/>

다음으로는 위의 인터페이스를 구현한 코드를 보겠습니다.<br/>

```java
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
}
```

위의 코드가 인터페이스를 '구현'한 코드입니다.<br/>

추상클래스는 상속을 의미하는 abstract를 사용해서 나타내고,<br/>
extends로 상속을 받는 클래스를 나타냈지만,<br/>

인터페이스는 interface를 사용하고<br/>
상속을 받는게 아닌 구현의 의미를 가진 implements 키워들르 사용합니다.<br/>

구현을 담당하는 클래스는 추상클래스가 될수 있고 다른 클래스가 상속을 받을 수 있습니다.<br/>

즉 인터페이스 선언 후 그 인터페이스를 가져다 쓰는 클래스가 해야하는 일은 인터페이스를 구현해야합니다.<br/>
구현하는 클래스는 extends대신 implements 키워드를 씁니다.<br/>
ex) public class Calculator implements Calc

## 4. 타입 상속과 형 변환

인터페이스를 구현한 클래스는 인터페이스 형으로 선언한 변수로 형 변환이 가능합니다.<br/>

```java
Calc calc = new CompleteCalc();
```

즉 인터페이스를 구현한 클래스는 인터페이스 타입으로 변수를 선언하여 인스턴스 생성이 가능합니다.<br/>
이런 것을 타입 상속이라고 합니다.<br/>
(인터페이스는 구현코드가 없기 떄문에 타입 상속이라고 함)<br/>

상속에서의 형 변환과 동일한 의미를 가집니다.<br/>
(상속에서는 구현 상속)<br/>

클래스 상속과 다릴 구현 코드가 없으므로 여러 인터페이스를 구현할 수 있습니다.<br/>
(상속은 다중 상속이 안됨)<br/>

형 변환 되는 경우 인터페이스에 선언된 메서드만 사용이 가능합니다.<br/>

![타입 상속](/images/interface1.png)
