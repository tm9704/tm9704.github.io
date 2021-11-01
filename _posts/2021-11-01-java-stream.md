---
title: "Java(54) - 자바 스트림"
excerpt: "Java Stream"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-11-01 13:20:00"
last_modified_at: "2021-10-27 14:00:00"
---

스트림이란 자료의 연산을 위해 사용하는 객체입니다.<br/>
입출력을 위한 java.io의 stream과는 다릅니다.<br/>

## 1. 스트림(Stream)

- 자료의 대상과 관계 없이 동일한 연산을 수행할 수 있는 기능입니다.<br/>
  (자료의 추상화)
- 배열, 컬렉션에 동일한 연산이 수행되어 일관성 있는 처리가 가능합니다.
- 한번 생성하고 사용한 스트림은 재사용이 불가능합니다.
- 스트림 연산은 기존 자료를 변경하지 않습니다.
- 중간 연산과 최종 연산으로 구분됩니다.
- 최종 연산이 수행되어야 모든 연산이 적용되는 지연 연산입니다.<br/>

스트림을 사용하려면 배열이나 컬렉션에 대해서 stream 객체를 생성해야합니다.<br/>

즉, 배열이 있고 배열에 대한 stream 객체 생성을 하고 연산을 한다고 하면,<br/>
해당 배열은 건드리지 않고 다른 메모리에서 연산이 이루어집니다.<br/>
(원래의 자료를 건드리지않음)<br/>

## 2. 스트림 연산 - 중간 연산

- 중간 연산 : filter(), map()
- 조건에 맞는 요소를 추출 => filter()
- 조건에 맞는 요소를 반환 => map()

문자열의 길이가 5 이상인 요소만 출력하는 간단한 예제를 보겠습니다.<br/>

```java
sList.stream().filter(s->s.length()>=5).forEach(s->System.out.println(s));
//스트림 생성           중간연산                        최종연산
```

배열의 경우 stream()의 괄호 안에 배열의 종류를 적고, 컬렉션은 그냥 stream()만 작성합니다.<br/>

고객 클래스에서 고객의 이름만 가져오는 간단한 예제를 보겠습니다.<br/>

```java
customerList.stream().map(c->c.getName()).forEach(s->System.out.println(s));
//스트림 생성            중간연산                       최종연산
```

예제들에서 모두 람다식을 사용하고 있는데,<br/>
stream에서 연산에 대한 구현부는 람다식으로 쓰고있습니다.<br/>

## 3. 스트림 연산 - 최종 연산

- 스트림의 자료를 소모 하면서 연산을 수행합니다.<br/>
  (한번 생성해서 소모한 스트림은 재사용이 불가능, 즉 스트림을 생성해서 변수에 담아두고 사용하는건 안됨)
- 최종 연산 후에 스트림은 더 이상 다른 연산을 적용할 수 없습니다.<br/>

최종 연산의 대표적인 종류로는<br/>

1. forEach() : 요소를 하나씩 꺼내옴
2. count() : 요소의 개수
3. sum() : 요소의 합

등이 있고<br/>
이 외에도 여러가지가 있습니다.<br/>

예제를 보겠습니다.<br/>

배열 예제<br/>

```java
import java.util.Arrays;

public class IntArrayTest {

	public static void main(String[] args) {

		int[] arr = {1,2,3,4,5};

		int sum = Arrays.stream(arr).sum();
        //한번 사용하면 끝
		int count = (int)Arrays.stream(arr).count();
        //재생성해서 사용

		System.out.println(sum);
		System.out.println(count);
	}
}
```

컬렉션 예제<br/>

```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

public class ArrayListStreamTest {

	public static void main(String[] args) {

		List<String> sList = new ArrayList<String>();

		sList.add("Tomas");
		sList.add("Edward");
		sList.add("Jack");

		Stream<String> stream =  sList.stream();
		stream.forEach(s->System.out.print(s + " "));
		System.out.println();

		sList.stream().sorted().forEach(s->System.out.print(s+ " "));
		System.out.println();
		sList.stream().map(s->s.length()).forEach(n->System.out.println(n));
		System.out.println();
	}
}
```

## 4. reduce() 연산

- 정의된 연산이 아닌 프로그래머가 직접 지정하는 연산을 적용합니다.
- 최종 연산으로 스트림의 요소를 소모하여 연산을 수행합니다.

배열의 모든 요소의 합을 구하는 reduce() 연산을 보겠습니다.<br/>

```java
Arrays.stream(arr).reduce(0,(a,b)->a+b);
//0은 초기 값, (a,b)는 전달되는 요소, (a,b)->a+b는 각 요소가 수행해야 할 기능
```

두번째 요소로 전달되는 람다식에 따라 다양한 기능을 수행합니다.<br/>

예제를 보겠습니다.<br/>

```java
import java.util.*;
import java.util.function.BinaryOperator;

class CompareString implements BinaryOperator<String>{

	@Override
	public String apply(String s1, String s2) {
		if(s1.getBytes().length >= s2.getBytes().length)
			return s1;
		else return s2;
	}

}

public class ReduceTest {

	public static void main(String[] args) {
		String[] greetings = {"안녕하세요~~~~", "hello", "Good morning", "반갑습니다"};

		System.out.println(Arrays.stream(greetings).reduce("", (s1, s2) ->
				{if(s1.getBytes().length >= s2.getBytes().length)
					return s1;
				else return s2;
				}));

		System.out.println(Arrays.stream(greetings).reduce(new CompareString()).get());
	}
}
```
