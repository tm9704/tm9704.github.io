---
title: "Java(33) - Object 클래스"
excerpt: "Java Object Class"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-11 14:55:00"
last_modified_at: "2021-10-09 15:20:00"
---

## 1. Object 클래스

Object 클래스는 모든 클래스의 최상위 클래스입니다.<br/>

모든 클래스가 Object 클래스에서 상속을 받으며,<br/>
모든 클래스가 extends Object 하게 되어있습니다.<br/>

직접 코딩을 안해주어도 컴파일러가 자동으로 적용해줍니다.<br/>

Object 클래스는 java.lang 패키지 안에 있으며 굳이 import를 해주지 않아도됩니다.<br/>
java.lang 패키지는 우리가 많이 사용하는 기본클래스들이 속한 패키지입니다.<br/>
String, Integer, System, Object 클래스 등이 여기 속합니다.<br/>

모든 클래스는 Object 클래스의 메서드를 사용할 수 있습니다.<br/>
Object 클래스의 일부 메서드를 재정의하여 사용할 수 있는데,<br/>
Object 클래스에서 final로 정의된 메서드들은 재정의가 불가능합니다.<br/>

## 2. toString 메서드

toString 메서드는 어떤 객체의 정보를 String 형태로 표현해야할 때 호출되는 메서드입니다.<br/>
(Object클래스에 정의되어있는 메서드중 하나)<br/>

```java
String str = new String("토지");
System.out.pritnln(str);
```

위 코드의 경우 System.out.println(str)을 할 경우 str.toString()이 불립니다.<br/>
String 클래스의 경우 이미 toString()메서드가 오버라이딩 되어있기 때문입니다.<br/>

toString() 메서드의 원래 형태는<br/>
getClass().getName() + '@' + Integer.toMexString(hashCode())<br/>
입니다.<br/>

getClass().getName()이 class의 full name이고<br/>
Integer.toMexString(hashCode())가 메모리 주소입니다.<br/>
(JVM이 제공한 값, 실제 메모리 위치는 아님)

toString 메서드는 재정의해서 사용이 가능하며<br/>
(주로 재정의해서 사용합니다.)<br/>
객체의 정보를 String으로 바꾸어 사용할 때 유용합니다.<br/>

자바 클래스들 중에서는 이미 정의되어있는 클래스가 많습니다.<br/>
(ex) String, Integer, Calender...)<br/>

```java
class Book{
	String title;
	String author;

	public Book(String title, String author) {
		this.title = title;
		this.author = author;
	}

	@Override //toString overriding
	public String toString() {
		return author + "," +title;
	}
}

public class ToStringTest {

	public static void main(String[] args) throws CloneNotSupportedException {

		Book book = new Book("토지", "박경리");

		System.out.println(book);

//		String str = new String("토지");
//		System.out.println(str.toString());
//      위 코드는 System.out.println(str)과 동일
}
```

## 3. equals() 메서드

equals() 메서드는 두개의 인스턴스(객체)가 동일한지의 여부를 보여주는 메서드입니다.<br/>

두 인스턴스의 주소 값을 비교하여 true/false를 반환합니다.<br/>
보통 재정의해서 두 인스턴스가 논리적으로 동일한지의 여부를 구현합니다.<br/>

여기서 물리적 동일함과 논리적 동일함이 있는데,<br/>
물리적 동일함이란 같은 주소를 가지는 객체를 의미하고,<br/>
논리적 동일함이란 예를 들면 같은 학번의 학생, 같은 주문번호의 주문을 의미합니다.<br/>

물리적으로 다른 메모리에 위치한 객체라도 논리적으로 동일함을 구현하기 위해 사용합니다.<br/>

간단한 예제를 보면

```java
Student studentLee = new Student(100, "이상원");
Student studentLee2 = studentLee;
Student studentSang = new Student(100, "이상원");
```

![equals](/images/equals.png)<br/>

위의 코드와 사진을 봤을 때 studentLee와 studentLee2는 대입만 해서 같은 객체를 가리킵니다.<br/>
그러나 studentSang은 학번과 이름이 같아도 다른 객체이기 때문에 메모리의 위치가 다릅니다.<br/>

하지만 우리는 학번과 이름이 같으므로 같은 학생임을 인지하고 있습니다.<br/>
이것이 논리적 동일함이고, 우리는 이 논리적 동일함을 나타내기 위해 이 경우 보통 학번을 통해<br/>
같은 학생임을 나타냅니다.<br/>

```java
class Student{
	int studentNum;
	String studentName;

	public Student(int studentNum, String studentName) {
		this.studentNum = studentNum;
		this.studentName = studentName;
	}

	@Override
	public boolean equals(Object obj) { //Object로 자동 형변환
		if(obj instanceof Student) {
			Student std = (Student)obj; // 다운캐스팅
//			return(this.studentNum == std.studentNum);
			if(this.studentNum == std.studentNum)
				return true;
			else return false;
		}
		return false;
	}
}

public class EqualsTest {

	public static void main(String[] args) {

//		String str1 = new String("abc");
//		String str2 = new String("abc");
//
//		System.out.println(str1 == str2); 메모리 주소가 동일한가
//		System.out.println(str1.equals(str2)); 두개의 문자열이 같은가
//      String의 경우 equals()메서드가 문자열 비교로 재정의되어있습니다.

		Student Lee = new Student(100, "이순신");
		Student Shin = new Student(100, "이순신");

		System.out.println(Lee == Shin); //false (메모리 주소가 다르기 때문)
		System.out.println(Lee.equals(Shin)); //우리가 위에서 재정의했기 떄문에 true
	}

}
```

위 코드는 학번을 통해 두 객체의 논리적 동일함을 보기 위해 equals()메서드를 재정의한 예제입니다.<br/>
이렇게 같은 객체로 취급해줘야 프로그램 내에서 오류가 없습니다.<br/>

이렇게 객체가 같다는 의미는 같은 hash code값을 갖는다는 말과 동일합니다.<br/>

## 4. hashCode() 메서드

hash code란 Java에서 인스턴스가 생성이 됐을 때 JVM이 메모리 주소를 주는데, 이 주소값이 hash code 입니다.<br/>
즉 인스턴스마다 고유하게 지니는 값입니다. JVM이 힙메모리에 저장한 인스턴스의 주소값을 말합니다.<br/>
(나중에 더 자세히 다루겠습니다.)<br/>

hashCode() 메서드의 반환 값은 인스턴사가 저장된 가상머신의 주소를 10진수로 반환합니다.<br/>

두개의 서로 다른 메모리에 위치한 인스턴스가 동일하다는 것은,<br/>

1. 논리적으로 동일: equals()의 반환값이 true
2. 동일한 hashCode값을 가짐: hashCode()의 반환값이 동일

입니다.<br/>
(실제 메모리의 값이 바뀌는 것은 아닙니다.)<br/>
equals()를 오버라이딩 하면 보통 hashCode()도 같이 오버라이딩 합니다.<br/>
(논리적으로 두 인스턴스가 같다면 같은 해쉬 코드를 반환해야하기 때문)<br/>

```java
class Student{
	int studentNum;
	String studentName;

	public Student(int studentNum, String studentName) {
		this.studentNum = studentNum;
		this.studentName = studentName;
	}

	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Student) {
			Student std = (Student)obj;
			if(this.studentNum == std.studentNum)
				return true;
			else return false;
		}
		return false;
	}

	@Override
	public int hashCode() {
		return studentNum;
	}


}

public class EqualsTest {

	public static void main(String[] args) {

		Student Lee = new Student(100, "이순신");
		Student Lee2 = Lee;
		Student Shin = new Student(100, "이순신");

		System.out.println(Lee == Shin);
		System.out.println(Lee.equals(Shin));

		System.out.println(Lee.hashCode());
		System.out.println(Shin.hashCode());

		Integer i1 = 100;
		Integer i2 = 100;

		System.out.println(i1.equals(i2));
		System.out.println(i1.hashCode());
		System.out.println(i2.hashCode());
	}

}
```

위의 코드 처럼 보통 equals()에서 사용한 값을 hashCode()에서 이용합니다.<br/>
(학번 , 사번..)<br/>

Integer나 String의 경우 equals와 hashCode메서드는 알맞게 오버라이딩 되어있습니다.<br/>

실제 메모리 위치는(해쉬코드값)<br/>
System.out.println(System.identityHashCode(..))로 확인이 가능합니다.<br/>

## 5. clone() 메서드

clone() 메서드는 객체의 복사본을 만드는 메서드입니다.<br/>
기본틀(prototype)으로 부터 같은 속성 값을 가진 객체의 복사본을 만들 수 있습니다.<br/>
즉 같은 객체를 인스턴스 값을 동일하게 만드는 것입니다.<br/>

생성자와는 다른데, 생성자는 처음에 초기화를 하면서 만드는 것인데, clone()은 지금 인스턴스의 상태를<br/>
그대로 복제합니다.<br/>
즉 생성과정의 복잡한 과정을 반복하지 않고 복제합니다.<br/>

private 필드까지 다 복제하기 때문에, 객체지향 프로그래밍의 정보은닉에 위배될 가능성이 있습니다.<br/>
그래서 복제할 객체는 cloneable 인터페이스를 명시해줘야합니다.<br/>

```java
class Book implements Cloneable{
	String title;
	String author;

	public Book(String title, String author) {
		this.title = title;
		this.author = author;
	}

	@Override
	public String toString() {
		return author + "," +title;
	}

	@Override
	protected Object clone() throws CloneNotSupportedException {
		return super.clone();
	}

}

public class ToStringTest {

	public static void main(String[] args) throws CloneNotSupportedException {

		Book book = new Book("토지", "박경리");

		System.out.println(book);

		Book book2 = (Book)book.clone();
		System.out.println(book2);

	}
}
```

clone() 메서드의 경우 보통 그대로 사용합니다.<br/>

위 코드에서 Book boo2 = book.clone();을 하면 clone() 메서드의 원형이 object형으로<br/>
반환되기 때문에 (Book)으로 명시적으로 형변환을 해줘야 합니다.<br/>
(예외는 나중에 다루겠습니다.)<br/>

Book 클래스에서 보듯이 clonable 인터페이스를 명시해줘야합니다.<br/>
(mark interface)

## 6. finalize() 메서드

우리가 직접 위의 메서드들 처럼 부르는 메서드는 아닙니다.<br/>

해당 객체가 Heap 메모리에서 해제될 때 GC에서 호출되는 메서드입니다.<br/>

해당 메서드가 정의되어 있으면 GC에서 이 메서드를 수행하며,<br/>
주로 리소스의 해제, 소켓 닫기를 수행합니다.<br/>

해당 메서드는 나중에 더 깊게 다루겠습니다.<br/>
