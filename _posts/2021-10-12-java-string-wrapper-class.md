---
title: "Java(35) - String, Wrapper 클래스"
excerpt: "Java String, Wrapper Class"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-12 14:45:00"
last_modified_at: "2021-10-12 15:30:00"
---

## 1. String 클래스

String 클래스 선언하는 방법을 보겠습니다.<br/>

1. String str1 = new String("abc");<br/>
   (인스턴스로 생성됨)
2. String str2 = "abc";<br/>
   (상수풀에 있는 문자열을 가리킴)

```java
public class StringTest {

	public static void main(String[] args) {
		String str1 = new String("abc");
		String str2 = "abc";
        String str3 = "abc";

		System.out.println(str1 == str2);
		System.out.println(str2 == str3);
	}

}
```

실행 결과는 false와 true 입니다.<br/>

![String Class1](/images/string_class1.png)<br/>

위의 그림처럼 str1은 힙 메모리에 있는 인스턴스를 참조하고,<br/>
str2,str3는 상수폴에 있는 값을 공유합니다.<br/>

## 2. 한 번 생성된 String은 immutable(불변)

한번 선언되거나 생성된 문자열은 변경할 수 없습니다.<br/>

String 클래스의 concat()메서드 혹은 "+"를 이용하여<br/>
String을 연결하는 경우 문자열은 새로 생성됩니다.<br/>
(계속 연결해서 결과를 만들게 되면 메모리의 낭비가 발생합니다.)<br/>

![String Class2](/images/string_class2.png)<br/>

```java
public class StringTest2 {

	public static void main(String[] args) {
		String java = new String("java");
		String android = new String("android");
		System.out.println(System.identityHashCode(java));

		java = java.concat(android);

		System.out.println(java);
		System.out.println(System.identityHashCode(java));
	}

}
```

## 3. StringBuffr 와 StringBuilder

가변적인 char[] 배열을 멤버 변수로 가지고 있는 클래스들입니다.<br/>
문자열을 변경하거나 연결할 경우 사용하면 편리한 클래스입니다.<br/>

StringBuffer는 멀티쓰레드 프로그래밍에서 동기화(Synchronization)이 보장됩니다.<br/>
단일 쓰레드 프로그래밍에서는 StringBuilder를 사용하는게 더 좋습니다.<br/>

toString()메서드를 통해 String을 반환합니다.<br/>

```java
public class StringBuilderTest {

	public static void main(String[] args) {
		String java = new String("java");
		String android = new String("android");
		System.out.println(System.identityHashCode(java));

		StringBuilder buffer = new StringBuilder(java);
		System.out.println(System.identityHashCode(buffer));
		buffer.append("android");
		System.out.println(System.identityHashCode(buffer));
		System.out.println(buffer);

		java = buffer.toString();
		System.out.println(System.identityHashCode(java));
	}

}
```

## 4. Wrapper 클래스

기본 자료형에 대한 클래스입니다.<br/>

![Wrapper Class](/images/wrapper_class.png)<br/>
