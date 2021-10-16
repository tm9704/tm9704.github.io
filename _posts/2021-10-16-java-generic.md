---
title: "Java(39) - 제네릭 프로래밍"
excerpt: "Java Generic"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-16 10:02:00"
last_modified_at: "2021-10-16 10:30:00"
---

## 1. 제네릭 프로그래밍

제네릭(Generic) 프로그래밍이란 변수의 선언이나 메서드의 매개변수를 하나의 참조 자료형이<br/>
아닌 여러 자료형으로 변환될 수 있도록 프로그래밍 하는 방식입니다.<br/>

실제 사용되는 참조 자료형으로의 변환은 컴파일러가 검증하므로, 안정적인 프로그래밍 방식입니다.<br/>

## 2. 자료형 매개변수 T(typeparameter)

여러 참조 자료형으로 대체될 수 있는 부분을 하나의 문자로 표현한 것이 T입니다.<br/>
(or E(element), V (value))<br/>

예시를 하나 보겠습니다.<br/>

```java
public class GenericPrinter<T>{
    private T material;

    public void setMeterial(T material){
        this.material = material;
    }

    public T getMaterial(){
        return materail;
    }
}
```

위 코드에서 GenerciPrinter\<T>는 제네릭 클래스를 의미합니다.<br/>
\<T>에서 T는 type의 약자, 자료형 매개변수입니다.<br/>
T에 어떤 자료형이 들어갈 수 있느냐는 해당 클래스를 사용할 때 결정됩니다.<br/>

T는 변수의 자료형, 메소드 매개변수 타입, 반환형 타입 등에 사용됩니다.<br/>

실제 코딩 예시를 보겠습니다.<br/>

```java
class Plastic{
    public String toString(){
        return "재료는 Plastic 입니다.";
    }
}

class Powder{
    public String toString(){
        return "재료는 Powder 입니다.";
    }
}

class GenericPrinter<T>{
    private T material;

    public T getMaterial(){
        return material;
    }

    public T setMaterial(T material){
        this.material = material;
    }

    public String toString(){
        return material.toString();
    }
}

public class GenericPrinterTest{

    GenericPrinter<Powder> powderPrinter = new GenericPrinter<Powder>();
    Powder powder = new Powder();
    powderPrinter.setMaterial(powder);
    System.out.println(powderPrinter);

    GenericPrinter<Plastic> plasticPrinter = new GenericPrinter<Plastic>();
    Prastic plastic = new Plastic();
    prasitcPrinter.setMaterial(prlstic);
    System.out.println(plasticPrinter);
}
```

위 코드에서 GenericPrinter\<Powder> powderPrinter = new GenericPrinter\<Powder>();<br/>
은 뒤에 있는 <>연산자 내부의 자료형은 생략이 가능합니다.<br/>

위에서 제네릭 타입만 Powder로 선언했다고 해서, Powder라는 클래스의 객체가 생성되는 것은 아닙니다.<br/>
코드처럼 Powder 객체를 생성하고 대입해줘야 합니다.<br/>

그리고 PowderPrinter가 아닌 PlasticPrinter를 만들고자할 때, Printer를 새로 만드는게 아니라,<br/>
코드처럼 <>연산자 내부의 자료형만 Plastic으로 변경해주면 됩니다.<br/>

## 3. <T extends> 클래스

위 코드에서 새로운 재료가 추가된다고 가정합시다.<br/>

GenericPrinter\<Water> waterPrinter = new GenericPrintr<>();<br/>
이런식으로 말이 안되는 재료가 들어갈 때 우리는 이것을 방지할 필요가 있습니다.<br/>

이를 방지하기 위해 우선 상위클래스를 하나 만듭니다.<br/>

```java
class Material {
}
```

상위 클래스를 만들고 제네릭 클래스에 아래처럼 extends 키워드를 추가해줍니다.<br/>

```java
class GenericPrinter<T extends Material>{
    private T material;

    public T getMaterial(){
        return material;
    }

    public T setMaterial(T material){
        this.material = material;
    }

    public String toString(){
        return material.toString();
    }
}
```

이렇게 하고 우리가 T에서 사용할 클래스들만 Material이라는 상위클래스를 상속받게 합니다.<br/>

```java
class Plastic extends Material{
    public String toString(){
        return "재료는 Plastic 입니다.";
    }
}

class Powder extends Material{
    public String toString(){
        return "재료는 Powder 입니다.";
    }
}
```

이렇게 Material을 상속받은 클래스만 GenericPrinter의 재료로(T) 사용이 가능합니다.<br/>
즉 \<T extends>클래스는 T 대신에 사용될 자료형을 제한하기 위해 사용합니다.<br/>
(무작위 클래스가 들어갈 수 없게 제한)<br/>

추가적으로 상속하는 상위클래스를 추상클래스로 만들고 추상메서드(or 메서드)를 삽입한다면,<br/>

```java
abstract class Material {
    public abstract void doPrinting();
}
```

제네릭 클래스에서 T 자료형의 변수가 할 수 있는 기능이 Object의 메서드 외에 호출할 수 있는 메서드가 더 많아집니다.<br>
즉 제네릭 클래스에서 extends한 클래스에 정의된 메서드를 공유할 수 있습니다.<br/>

전체 코드를 보겠습니다.<br/>

```java
abstract class Material {

	public abstract void doPrinting();
}

class Plastic extends Material{

	public String toString() {
		return "재료는 Prastic 입니다.";
	}

	@Override
	public void doPrinting() {
		// TODO Auto-generated method stub

	}
}

class Powder extends Material{

	public String toString() {
		return "재료는 Powder 입니다.";
	}


	@Override
	public void doPrinting() {
		// TODO Auto-generated method stub

	}
}

class GenericPrinter<T extends Material> {

	private T material;

	public T getMaterial() {
		return material;
	}

	public void setMaterial(T material) {
		this.material = material;
	}

	public String toString() {
		return material.toString();
	}

	public void printing() {
		material.doPrinting();
	}
}

public class GenericPrinterTest {

	public static void main(String[] args) {

		//뒤에 꺽쇠에는 자료형 안적어줘도됨.
		GenericPrinter<Powder> powderPrinter = new GenericPrinter<Powder>();
		Powder powder = new Powder();
		powderPrinter.setMaterial(powder);
		System.out.println(powderPrinter);

		GenericPrinter<Plastic> plasticPrinter = new GenericPrinter<Plastic>();
		Plastic plastic= new Plastic();
		plasticPrinter.setMaterial(plastic);
		System.out.println(plasticPrinter);
	}

}
```

## 4. 자료형 매개변수가 두 개 이상일 때

예시를 먼저 보겠습니다.<br/>

```java
public class Point<T, V>{
    T x;
    V y;

    Point(T x, V y){
        this.x = x;
        this.y = y;
    }

    public T getX(){
        return x;
    }

    public V getY(){
        return y;
    }
}
```

위의 코드 처럼 두개의 서로 다른 타입의 자료형 매개변수 사용이 가능합니다.<br/>
그리고 getX(), getY()처럼 반환값이나 메서드의 매개변수를 자료형 매개변수로 사용하는 것을<br/>
제네릭 메서드라고 합니다.<br/>

## 5. 제네릭 메서드

위에서 설명한 것과 같이 메서드의 매개변수 또는 반환값을 자료형 매개 변수로 사용하는 메서드를<br/>
제네릭 메서드라고 합니다.<br/>

```java
public class GenericMethod {

	public static <T, V> double makeRectangle(Point<T, V> p1, Point<T, V> p2) {
		double left = ((Number)p1.getX()).doubleValue();
		double right =((Number)p2.getX()).doubleValue();
		double top = ((Number)p1.getY()).doubleValue();
		double bottom = ((Number)p2.getY()).doubleValue();

		double width = right - left;
		double height = bottom - top;

		return width * height;
	}

	public static void main(String[] args) {

		Point<Integer, Double> p1 = new Point<Integer, Double>(0, 0.0);
		Point<Integer, Double> p2 = new Point<>(10, 10.0);

		double rect = GenericMethod.<Integer, Double>makeRectangle(p1, p2);
		System.out.println("두 점으로 만들어진 사각형의 넓이는 " + rect + "입니다.");
	}
}
```

위 코드는 사각형의 넓이를 구하는 코드입니다.<br/>

해당 코드처럼 일반 클래스에서도 제네릭 메서드 사용이 가능합니다.<br/>

GenerciMethod클래스에서 사용된 public static \<T,V> double makeRectangle 메서드에서<br/>
제네릭 자료형인 T, V는 지역변수와 동일한 성질을 지닙니다.<br/>

즉 해당 메서드 내에서만 의미가 있으며, 매개변수 몇 개를 쓸 것이다를 나타냅니다.<br/>

```java
class Shape<T>{
    public static <T,V> double makeRectangle(Point<T,V> p1, Point<T,V> p2){
        ....
    }
}
```

위 코드에서 Shape 옆의 T와 메서드에서의 T는 완전히 다른 것입니다.<br/>
Shape에서의 T는 해당 클래스의 매개변수, 반환 값등으로 사용이 가능하지만,<br/>
makeRectangle에서의 T는 오직 Point를 위한 T입니다.<br/>
(즉 지역변수와 같은 개념)<br/>

제네릭 메서드 정리를 하자면<br/>

- 자료형 매개변수를 메서드의 매개변수나, 반환값으로 가지는 메서드
- 자료형 매개변수가 하나 이상인 경우도 있다
- 제네릭 클래스가 아니어도 내부에 제네릭 메서드 구현 가능
- public<자료형 매개변수> 반환형 메서드이름(자료형 매개변수 ...){} 이런식으로 쓰임

## 추가. 다이아몬드 연산자

제네릭 타입을 쓸 때 사용했던 <>연산자를 다이아몬드 연산자라고 합니다.<br/>

ArrayList lis <Object> = new ArrayList<>();<br/>
위에서 설명했듯이 다이아몬드 연산자 내부에서 자료형 생략이 가능합니다.<br/>

만약 전부 다 생략할 경우<br/>
GenericPrinter printer = new GenericPrinter();<br/>
이런식으로 제네릭 타입을 선언해주지 않으면 Object로 취급합니다.<br/>
