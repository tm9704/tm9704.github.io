---
title: "Java(19) - Singleton Pattern"
excerpt: "Java Singleton Pattern"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
last_modified_at: 2021-09-26T18:10:00
---

static의 응용인 singleton pattern에 대해 공부하겠습니다.<br/>

## 1. 디자인 패턴

이번 포스트에서는 가볍게 알아보고 가겠습니다.<br/>

디자인 패턴이란 객체지향 프로그램으로 프로그램을 설계할 효율적이고, 유지보수에 용이하게 설계할 수 있는 방식, 패턴입니다.<br/>
Java에만 국한되는 것이 아닌 객체지향언어면 사용이 가능합니다.<br/>

## 2. singleton pattern

singleton pattern은 앞서 말한 디자인 패턴의 일종입니다.<br/>

사용 시기는 어느 객체가 여러번 생성(new)됐을 때, 여러개의 인스턴스 변수를 가지면 문제가 될 때 사용합니다.<br/>
즉 인스턴스가 단 하나만 존재하는 패턴이 singleton pattern입니다.<br/>
(ex) 학교 자체, 회사 자체)

우선 사용 방법을 알아보겠습니다.<br/>

1. 생성자를 private로 생성
   - (생성자를 제공하지 않으면 컴파일러가 public한 디폴트 생성자를 제공해, 외부에서 누구나 생성이 가능하기 때문)
2. static으로 유일한 객체(인스턴스) 생성
3. 외부에서 유일한 객체를 참조할 수 있는 (public)static get 메서드 구현
   - (일반 메서드는 인스턴스가 생성되어야지만 호출이 가능하기 때문)

예시를 보겠습니다.

```java
class Company {

	//유일한 객체 생성
	private static Company instance = new Company();

	private Company() {}

	public static Company getInstance() {
		//혹시 모르는 상황대비.
		if(instance == null)
			instance = new Company();

		return instance;
	}
}

public class CompanyTest {

	public static void main(String[] args) {

		Company company1 = Company.getInstance();
		Company company2 = Company.getInstance();

		System.out.println(company1);
		System.out.println(company2);
	}

}
```

더 자세한건 나중 포스트에서 보겠습니다.
