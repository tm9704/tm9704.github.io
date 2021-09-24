---
title: "Java 단항 연산자"
excerpt: "Java Unary Operator"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
last_modified_at: 2021-09-24T16:34:00
---

단항 연산자는 피연산자가 하나인 연산자로써 이항 연산자보다 개수가 적습니다.

## 1. 부호 연산자(+, -)

앞선 포스트에서 이항 연산자 +, -는 덧셈과 뺄셈의 기능을 제공합니다.<br/>
단항 연산자로서 사용될 때는 우리가 알고있는 양수, 음수의 표현을 제공합니다.<br/>

```java
int num1 = 5;
int num2 = -5;
System.out.println(-num1);
System.out.println(-num2);
```

위 코드의 실행결과는 각각 -5, 5입니다.<br/>
즉 단항 연산자로서 -는 부호를 바꾸는 역할, +는 특별한 기능이 없습니다.<br/>

## 2. 증감 연산자(++, --)

증감연산자인 ++과 --는 변수에 저장된 값을 하나 증가, 감소시키는 역할을 합니다.<br/>

```java
int num = 10;

System.out.println(++num);
System.out.println(--num);
```

위 코드의 결과는 각각 9, 10 입니다.<br/>

여기서 우리가 주의해야 할 점이 있습니다.<br/>
증감 연산자가 변수 앞쪽에 붙냐 뒤쪽에 붙냐에 따라 연산이 달라지기 때문입니다.<br/>

```java
int num1 = 10;
int num2, num3;

num2 = num1++;
num3 = num1--;

System.out.println(num1);
System.out.println(num2);
System.out.println(num3);
```

위의 결과는 10, 10, 11입니다.<br/>
즉 변수 뒤쪽에 증감 연산자가 붙을 경우, 해당 구문이 끝나고 나서 변수의 값을 증가 또는 감소시킵니다.<br/>
(앞에 붙을경우 증가 또는 감소된 후 대입합니다.)
