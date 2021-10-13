---
title: "Java(36) - Main 메서드"
excerpt: "Java Main Method"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-13 12:55:00"
last_modified_at: "2021-10-13 13:20:00"
---

이번 포스트에서는 그동안 배운걸로 main 메서드에 대한 공부를 하겠습니다.<br/>

## 1. Main 메서드

우리는 예제를 작성할 때마다, main이라는 이름의 메서드를 작성하고 있습니다.<br/>

```java
public static void main(String[] args){
    int num1 = 5, num2 = 7;
    System.out.println("5+7=" + (num1 + num2));
}
```

여기서 알 수 있는 것은 메서드의 이름은 main이고, 중괄호 내부의 코드가<br/>
순차적으로 실행된다는 것입니다.<br/>

자바 프로그램의 시작은 main이라는 이름의 메서드를 실행하는데서 부터 시작합니다.<br/>
(제일 먼저 메인 메서드를 실행함)<br/>
즉 main은 프로그램의 시작과 끝을 구성하는 메서드이며, 인스턴스의 생성과는 아무런 상관이 없는 메서드이며<br/>
JVM에 의해서 딱 한번만 호출되는 메서드입니다.<br/>

그렇다면 main이라는 메서드는 어느 클래스에 정의해야 할까요?<br/>

main은 어느 클래스에 작성해도 크게 상관이 없습니다.<br/>
그래서 우리는 보통 main메서드를 담고 있는 클래스를 별도로 정의합니다.<br/>

그리고 실행방식에 따른 차이가 있는데,<br/>
main 메서드가 있는 클래스로부터 실행이됩니다.<br/>

- ex) Employer, Employee라는 두개의 클래스가 있을 때<br/>
  Employer에 있을 때 : C:\JavaStudy>java Employer<br/>
  Employee에 있을 때 : C:\JavaStudy>java Employee<br/>

실행 결과는 동일합니다.<br/>

## 2. public static void main

위에서 main이라는 메서드는 인스턴스의 생성과 관계없이 JVM에 의해 호출된다고 설명했습니다.<br/>

우리가 항상 main 메서드를 정의할 때 main 메서드 앞에 static 키워드를 붙여주는 것이<br/>
인스턴스의 생성과 관계가 없기 때문입니다.<br/>

void는 main이라는 메서드는 반환하는 값이 없기 때문입니다.<br/>
(어차피 main메서드가 종료되면 프로그램도 종료되기 때문에)<br/>

public은 앞선 포스트에서 배웠듯이, 접근제어자로 어디서든 참조가 가능하다는 의미입니다.<br/>
JVM이 접근하기 위해서는 반드시 public으로 작성되어야 합니다.<br/>

그리고 추가적으로 메서드는 자신이 속해있는 클래스의 인스턴스 생성이 가능합니다.<br/>

```java
class AAA{
    public static void makeAAA(){
        AAA a1 = new AAA();
        ...
    }
}
```

즉 자바에서는 메소드의 종류에 상관없이 메서드 내에서 자신이 속해있는 클래스의 인스턴스 생성을<br/>
허용합니다.<br/>

그렇기 때문에 main 메서드는 어디든 속할 수 있는 것입니다.<br/>

## 3. main 메서드 매개변수

main 메서드의 매개변수는<br/>
String[] args 입니다.<br/>

여기서 String[]은 인스턴스 배열의 참조 값을 전달받기 위한 매개변수 선언입니다.<br/>
따라서 다음과 같이 선언된 배열의 참조 값이 main 메서드에 전달될 수 있습니다.<br/>

String[] strArr1 = {"AAA", "BBB", "CCC"};<br/>
String[] strArr2 = {"public", "static", "void", "main"};<br/>

그렇다면 main 메서드에 전달되는 String 인스턴스 배열은 어떻게 형성되는 것일까요?<br/>
다음 예제를 보겠습니다.<br/>

```java
class MainParam{
    public static void main(String[] args){
        for(String e : args){
            System.out.println(e);
        }
    }
}
```

C:\JavaStudy>java MainParam으로 실행했을 때<br/>
실행 결과는 아무것도 출력되지 않습니다.<br/>

C:\JavaStudy>java MainParam AAA BBB CCC로 실행했을 때는<br/>
AAA BBB CCC가 출력됩니다.<br/>

즉 프로그램이 실행되면, 실행할 클래스 파일의 이름 뒤로 이어지는 문자열들이 (문자열들은 공백으로 구분)<br/>
하나의 배열로 묶여서 main의 인자로 전달됩니다.<br/>

사용하는 곳은 우리는 보통 이클립스나 인텔리제이에서 실행하다 보니 쓸 일은 거의 없지만,<br/>
별도의 모듈로 분리하여 사용할 경우 전달되는 값에 따라 다르게 동작될 필요가 있을 때 사용합니다.<br/>

조금 설명이 난잡한데, 나중에 추가로 설명하겠습니다.<br/>
