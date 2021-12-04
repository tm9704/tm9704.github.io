---
title: "Kotlin(1) - 코틀린 시작하기"
excerpt: "Kotlin Start"
toc: true
toc_sticky: true
categories:
  - Kotlin
tags:
  - Kotlin
date: "2021-11-30 13:52:00"
last_modified_at: "2021-11-30 14:30:00"
---

## 1. 코틀린이란

코틀린은 JVM, JS, Natvie상에서 실행될 수 있는 멀티플랫폼 언어입니다.<br/>

1. JVM(Java Virtual Machine) 기반의 코틀린<br/>
   자바 어플리케이션이나 안드로이드 어플리케이션을 만들 수 있습니다.<br/>
2. JS(Javascript) 기반의 코틀린<br/>
   데이터베이스부터 서버, 클라이언트까지 다루는 풀스택 웹 개발이 가능합니다.<br/>
3. Native 기반의 코틀린<br/>
   코드를 한 번만 작성해도 안드로이드와 IOS에서 모두 구동하는 어플리케이션을 만들 수 있으며,<br/>
   임베디드, IoT등을 타깃으로 한 어플리케이션도 만들 수 있습니다.<br/>

## 2. 코틀린의 장점

1. 자료형 오류를 미리 잡을 수 있는 정적 언어<br/>
   코틀린은 프로그램이 컴파일 될 때 자료형을 검사하여 확정하는 정적 언어입니다.<br/>
   즉, 자료형 오류를 초기에 발견할 수 있어 프로그램의 안전성이 높아집니다.<br/>

2. 널 포인터 예외로 인한 프로그램 중단 예방<br/>
   널 포인터 예외(NPE)는 변수나 객체의 초기화가 이루어지지 않은 상태에서 그 변수에 접근할 때 발생하는 예외 오류입니다.<br/>
   코틀린은 이 NPE를 예방할 수 있습니다.<br/>

3. 간결하고 효율적<br/>
   코틀린은 여러 가지 생략된 표현이 가능합니다.<br/>

4. 함수형 프로그래밍과 객체 지향 프로그래밍이 모두 가능<br/>
   함수를 변수에 저장하거나 함수를 다른 함수의 매개변수로 넘길 수 있는 함수형 프로그래밍과<br/>
   클래스를 사용하는 객체 지향 프로그래밍을 모두 할 수 있습니다.<br/>

5. 세미콜론 생략 가능<br/>
   Java에서 코드를 작성할 때마다 마지막에 사용하던 세미콜론(;)을 생략할 수 있습니다.<br/>

## 3. main() 함수

Java와 같은 객체 지향 언어에서 프로그램을 실행하려면, 최소한 하나의 클래스 안에 main()함수가 있어야 합니다.<br/>
그러나 코틀린의 경우 선언된 클래스 없이 main()함수를 이용해 프로그램을 실행할 수 있습니다.<br/>

```
fun main(args: Array<String>) {
    println("Hello Kotlin")
}
```

코틀린 코드는 JVM 위에서 실행되며, main() 함수가 있는 파일을 기준으로 자바 클래스가 자동으로 생성됩니다.<br/>

```
public final class HelloKotlinKt{
    public static void main(){
        String var0 = "Hello Kotlin";
        System.out.println(var0);
    }

    public static void main(String[] var0){
        main();
    }
}
```
