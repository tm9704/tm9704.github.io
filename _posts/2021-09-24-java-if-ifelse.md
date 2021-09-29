---
title: "Java(11) - 조건문(if, ifelse)"
excerpt: "Java Conditional If, Else"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-09-24 17:10:00"
last_modified_at: "2021-09-24 17:25:00"
---

특정 조건이 만족될 때에만 실행하게 하고 싶다면 if문을 사용하면 됩니다.

## 1. if문, if~else문

우선 예제를 하나 보겠습니다.

```java
class IfTest{
    public static void main(String[] args){
        if(true){
            System.out.println("if & ture");
        }

        if(false){
            System.out.println("if~else & true");
        }else{
            System.out.println("if~else & false");
        }
    }
}
```

위의 코드에서 if문의 경우 소괄호의 값이 참일 경우 중괄호 내부의 코드를 실행합니다. 거짓일 경우 if문을 빠져나옵니다.<br/>
if~else문의 경우 if의 소괄호 값이 참일 경우 if문 중괄호 내부의 코드를, 거짓일 경우 else문 중괄호 내부의 코드를 실행합니다.<br/>

if문과 if~else문 안에서 if, if~else문은 중첩이 가능하고, 코드가 한줄일 경우 중괄호 생략이 가능합니다.<br/>

또 다른 예시를 보겠습니다<br/>

```java
class IfTest{
    public static void main(String[] args){
        int num = 10;

        if(num < 0) {
            System.out.println("0 미만");
        }else if (num < 100){
            System.out.println("0이상 100미만");
        }else{
            System.out.println("100이상");
        }
    }
}
```

위의 코드의 경우 분기점을 하나 더 생성했습니다.<br/>
else문안에 중첩된 if~else문을 추가하지 않고 else if 라는 조건을 하나 더 추가해 준 것입니다.<br/>
else if문의 개수는 몇개가 되어도 상관없습니다.<br/>

## 2. 삼항 연산자(조건 연산자)

if~else와 유사한 연산자가 존재합니다.<br/>

예시를 먼저 보겠습니다.

```java
int num1 = 50, num2 = 100;
int num3;

num3 = (num1>num2) ? num1 : num2;
System.out.println(num3);

```

위 코드의 실행 결과는 num2 입니다.<br/>

조건 연산자의 기본구조는 true or false ? 숫자 1 : 숫자 2 입니다. <br/>
즉 앞 조건의 결과가 true일 경우 : 기호 왼편의 숫자를 반환하고,<br/>
false인 경우 : 기호 오른쪽의 숫자를 반환합니다.<br/>
