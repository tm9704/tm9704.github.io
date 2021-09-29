---
title: "Java(22) - 배열 리스트"
excerpt: "Java Array List"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
last_modified_at: 2021-09-28T17:00:00
---

이번 포스트에서는 java.util 패키지에서 제공하는 ArrayList에 대해 공부하겠습니다.<br/>

## 1. ArrayList 클래스

ArrayList 클래스는 Java에서 제공되는 객체 배열이 구현된 클래스입니다.<br/>
(JDK에서 제공)<br/>

객체 배열을 사용하는데 필요한 여러 메서드들이 구현되어 있습니다.<br/>

## 2. 주요 메서드

1. boolean add(E, e)<br/>
   요소 하나를 배열에 추가합니다. (E는 요소의 자료형)
2. int size()<br/>
   배열에 추가된 요소 전체 개수를 반환합니다.<br/>
   (length와의 차이점은, size는 요소의 개수만을 반환합니다.)
3. E get(int index)<br/>
   배열의 index 위치에 있는 요소의 값을 반환합니다.
4. E remove(int index)<br/>
   배열의 index 위치에 있는 요소 값을 제거하고 그 값을 반환합니다.
5. boolean isEmpty()<br/>
   배열이 비어있는지 확인합니다.

여러 operation들이 구현되어 있기 때문에 배열의 size, 요소 위치와 상관 없이 메서드만 이용하면 됩니다.<br/>

## \* ArrayList<> list = new ArrayList<>();

<>를 제네릭이라고 부릅니다. (나중에 더 깊게 다루겠습니다.)<br/>
어레이 리스트를 사용할 때 어떤 객체를 사용할 것인지 적어주는 곳입니다.<br/>
(ex)String)<br/>

아무것도 안적어도 괜찮지만, 요소를 꺼내올 때 형변환이 필요합니다.<br/>
즉 꺽쇠 안에 어떤 클래스를 배열의 요소로 쓸 것인지 지정해주는 것이 좋습니다.<br/>

```java
import java.util.ArrayList;

public class ArrayListTest {

	public static void main(String[] args) {

		ArrayList<String> list = new ArrayList<String>();

		list.add("aaa");
		list.add("bbb");
		list.add("ccc");

		for(int i = 0; i<list.size(); i++) {
			String str = list.get(i);
			System.out.println(str);
		}

		for(String s : list) {
			System.out.println(s);
		}
	}

}
```

ArrayList 역시<br/>

```java
ArrayList<Subject> list = new ArrayList <Subject>();
```

한다고 해서 Subject 객체가 10개 생성되는 것은 아닙니다.<br/>
Subject 객체를 필요시 마다 생성해서 add 해주셔야합니다.<br/>
(String은 생성하지않고 바로 사용함, 문자열 자체는 상수 풀 안에 있기 때문)
