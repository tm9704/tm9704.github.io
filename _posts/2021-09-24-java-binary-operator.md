---
title: "Java(8) - 이항 연산자"
excerpt: "Java Binary Operator"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
last_modified_at: 2021-09-24T15:30:00
---

우선 이항 연산자에 대해 공부하기 전에 항과 연산자의 개념에 대해 공부하겠습니다.

## 1. 항과 연산자

항(operand)는 연산에 사용되는 값으로 피연산자라고도 부릅니다.<br/>
연산자(operator)는 항을 이용하여 연산하는 기호를 뜻합니다.<br/>
항의 개수에 따라 단항 연산자, 이항 연산자, 삼항 연산자가 있습니다.<br/>
제일 개수가 많은 이항 연산자에 대해 공부하겠습니다.

## 2. 대입 연산자와 산술 연산자

가장 대표적인 이항 연산자로는 대입 연산자와 산술 연산자가 있습니다.<br/>

우선 대입 연산자(=)는 연산자 오른쪽에 있는 값을 연산자 왼쪽에 있는 변수에 대입합니다.

```java
var = 20;
```

우선순위가 가장 낮다는 특징이 있습니다. <br/>

다음으로 산술 연산자에 대해 알아보겠습니다. 산술 연산자는 우리가 잘 알고있는 사칙연산 연산자입니다.<br/>
종류로는 +, -, \*, /, %가 있습니다<br/>

우선 +, -, \*를 먼저 알아보겠습니다.<br/>
세 연산자의 결합방향은 모두 왼쪽에서 오른쪽이며 \*는 곱하기를 나타냅니다.<br/>

```java
int var, var2, var3;

var = 4 + 3;
var2 = 4 - 3;
var3 = 4 * 3;
```

그 다음으로 /, %가 있습니다.<br/>
/는 나눗셈 연산자로 왼쪽의 피연산자 값을 오른쪽의 피연산자 값으로 나눕니다.<br/>
%는 왼쪽의 피연산자 값을 오른쪽의 피연산자 값으로 나눴을 때 얻게 되는 나머지를 반환합니다.<br/>

```java
int var, var2;

var = 7 / 3;
var2 = 7 % 3;

```

주의할 점은 / 연산자의 경우 피연산자들이 정수면 정수형 나눗셈, 실수면 실수형 나눗셈을 합니다.<br/>
Java에서 % 연산자 역시 정수, 실수 모두 지원하지만 다른 언어에서 실수형 나머지 연산을 막는 경우가 있으니 주의해야 합니다.<br/>

## 3. ()

우리가 잘 알고 있는 소괄호(())는 2가지 형태가 있습니다.<br/>

1. 구분자
2. 형변환 연산자

1번의 경우 예시를 보면,

```java
int n1 = 3;
int n2 = 6;

System.out.println("곱셈 결과: " + (n1 * n2));
```

이 경우 소괄호의 역할은 우선순위를 높여준다.<br/>
모든 연산자들은 우선순위라는 연산 순서를 가지는데, 소괄호로 묶어주면 먼저 연산이 됩니다.<br/>

2번의 경우,

```java
System.out.println("형 변환 나눗셈 : " + (float)7/3);
```

명시적 형변환으로 형변환에 사용되는 소괄호는 연산자입니다.<br/>

## 4. 복합(Compound) 대입 연산자

복합 대입 연산자는 대입 연산자가 다른 연산자와 묶여서 정의된 형태의 연산자 입니다.<br/>

우선 예를 들겠습니다.

```java
int a = 3;
int b = 4;

a = a + b;
a = a += b;
```

위의 두 식은 a의 값은 다르지만, 동일한 연산을 합니다.<br/>
복합 대입 연산자는 변수의 중복 사용을 줄여줍니다.<br/>
종류로는 +=, -=, \*=, /=, %= 와 <br/>
&=, ^=, |=, <<=, =>>, >>>= 가 있습니다. 앞의 연산자들은 나중에 공부할 연산자들을 공부하고 나면 이해가 가능합니다.<br/>

## 5. 관계 연산자

관계 연산자는 크기 및 동등 관계를 따지는 연산자입니다.<br/>
즉 두 피연산자의 크기 관계를 따져주는 연산자입니다.<br/>
종류로는 <, >, <=, =>, ==, != 가 있습니다.<br/>

```java
int n1 = 1;
int n2 = 2;

System.out.println(n1 < n2);
System.out.println(n1 > n2);
System.out.println(n1 <= n2);
System.out.println(n1 >= n2);
System.out.println(n1 == n2);
System.out.println(n1 != n2);
```

위에 4개의 예시는 우리가 알고 있는 연산자와 동일하며, ==는 동일한지, !=는 다른지를 판단합니다.<br/>
연산의 결과에 따라 boolean 값을 반환합니다.<br/>

## 6. 논리 연산자

논리 연산자 역시 boolean 값을 반환하는 연산자로써 AND(논리곱), OR(논리합), NOT(논리부정)을 의미합니다.<br/>
종류로는 &&, ||, !가 있습니다.<br/>
&&(논리곱)은 A && B에서 A와 B가 모두 참일 경우 참을 반환합니다.<br/>
||(논리합)은 A || B에서 A와 B중 하나가 참일 경우 참을 반환합니다.<br/>
!(논리부정)은 !A 에서 A가 참일 경우 거짓, 거짓을 경우 참을 반환합니다.<br/>

```java
int num1 = 10;
int num2 = 20;

boolean result1 = (num1 == 10 && num2 == 20);
boolean result2 = (num1 <= 12 || num2 >= 30);
boolean result3 = (!(num1 == num2));

```

result 값들은 모두 true입니다.

## 7. 단락 회로 평가(short circuit evaluation)

논리 곱(&&)은 두 항이 모두 true 일 때만 결과가 true입니다.<br/>
즉 앞의 항이 flase이면 뒤 항의 결과를 평가하지 않고 false를 반환합니다.<br/>

논리 합(||)은 두 항이 모두 false일 때만 결과가 true입니다.<br/>
즉 앞의 항이 true이면 뒤 항의 결과를 평가하지 않고 true를 반환합니다.<br/>

단락 회로 평가는 연산을 빠르게 하기 위해 진행하는 방식인데요, 모든 항이 연산이 되지 않을 수도 있기 때문에
주의하면서 사용해야합니다.