---
title: "Java 정보 은닉"
excerpt: "Java Information Hiding"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
last_modified_at: 2021-09-26T16:30:00
---

## 1. 접근 제어자(access modifier)

우선 정보 은닉에 대해 공부하기 전에 필수적으로 알아야할 접근 제어자에 대해 공부하겠습니다.<br/>

접근 제어자는 변수, 메서드, 생성자에 대한 접근 권한을 지정해주는 키워드 입니다.<br/>
종류는 총 4가지가 있습니다.

1. public
   - 외부에 모두 접근 허용
2. private
   - 같은 클래스 내부에서만 접근 허용
3. protected
   - 상속 관계에서 상위 클래스가 가진 private 변수나 메서드를 하위 클래스에 public하게 접근을 허용하고 싶을 때
4. default(아무것도 안쓰는 경우와 동일)
   - 같은 패키지 내에서만 접근 허용

## 2. 정보 은닉

정보 은닉이란 클래스 내부에서 사용할 변수나 메서드를 외부에서 접근하지 못하게 private로 선언하는 것을 말합니다.<br/>

```java
    public class MyDate{
        public int day;
        public int month;
        public int year;
    }

```

위의 날짜에 대한 코드가 있습니다.<br>

날짜는 각 달에 대한 정해진 일 수가 있습니다. 그러나 누군가 이 코드에 접근해 day 값을 45 이런 식으로 바꾸게 되면<br/>
당연히 문제가 생길 수밖에 없죠.

```java
    public class MyDate{
        private int day;
        private int month;
        private int year;

        public void setDay(int day) {
            this.day = day;
        }

        public int getDay() {
            return day;
        }

        public int getMonth() {
            return month;
        }

        public void setMonth(int month) {
            this.month = month;
        }

        public int getYear() {
            return year;
        }

        public void setYear(int year) {
            this.year = year;
        }

        public void showDate() {
            System.out.println(year + "년 " + month + "월 " + day + "일 ");
        }
    }

```

위의 코드처럼 접근 제어자를 public을 private로 바꾸면 외부 클래스에서 변수에 접근할 방법이 없습니다.<br/>
안전성이 높아진 것이죠.<br/>
하지만 우리가 사용할 때도 접근 할 수 없기 때문에 보통 getter, setter 메소드로 접근을 가능하게 합니다.<br/>

get, set을 제공하는 것과 public으로 하는 것의 차이는

1. get을 허용하고 set을 허용하지 않는 경우 (읽기 전용)
2. get, set 내부에 if문을 넣어서 제한을 둘 수 있음
   - 즉 public 으로 허용할 때 보다 변수들을 핸들링하기 쉬워짐.

## 3. 캡슐화 (Encapsulation)

캡슐화는 객체 지향 프로그래밍에서 2가지 측면이 있습니다.

1. 객체의 속성(멤버 변수)과 행위(메서드)를 하나로 묶는다.
2. 실제 구현 내용 일부를 외부에 감추어 은닉한다.

캡슐화가 무너지면 인스턴스의 생성 및 활용이 매우 어려워지고,<br/>
클래스 상호간의 관계가 복잡해지기 때문에 프로그램의 복잡도가 높아집니다.<br/>
(앞선 포스트에서 참조 자료형 포스트와 연관이 있습니다.)<br/>

## 4. this

이번엔 this 키워드에 대해 공부하겠습니다.<br/>

this의 역할은 총 3개가 있습니다.

1. 자신의 메모리를 가리킴
2. 생성자에서 다른 생성자를 호출함
3. 인스턴스 자신의 주소를 반환

우선 자기 자신의 메모리를 가리키는 경우를 보겠습니다.

```java
class Person{
    String name;
    int age;

    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
}

public class PersonTest{
    public static void main(String[] args){
        Person personLee = new Person(21, "Lee");
    }
}
```

위 코드에서 this는 new Person으로 생성된 인스턴스의 주소를 가리킵니다.<br/>
즉 인스턴스(객체) 내부에서 자기자신이 생성된 메모리 주소를 가리키는게 this 입니다.<br>

두번째로 생성자에서 다른 생성자를 호출하는 경우를 보겠습니다.

```java
public class Person {
	String name;
	int age;

	public Person() {
		this("이름없음", 1);
	}

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

}
```

위 코드와 같이 자기자신에서 다른 생성자를 부를 땐 this를 사용합니다.<br/>

주의할 점은 생성자는 인스턴스 생성을 초기화합니다.<br/>
그런데 인스턴스 생성이 다 되지 않았는데, 어떤 일을 하면 문제가 생길 수 있기 때문에<br/>
this로 다른 생성자를 호출할 때 그 위에 다른 구문이 있으면 안됩니다.<br/>
