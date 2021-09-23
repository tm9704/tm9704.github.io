---
title: "형 변환"
excerpt: "Java Type Conversion"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
last_modified_at: 2021-09-23T16:14:00
---

1과 1.0은 동일한 값입니다. 그러나 Java에서는 1.0과 1은 다른 값입니다.<br/>
표현방식이 다른 두 개의 값을 같은 값으로 쓸 수 있는 방법을 공부하겠습니다.<br/>

## 1. 형 변환

형 변환이란 서로 다른 자료형의 값이 대입되는 경우 자료형이 바뀌는 것을 의미합니다. <br/>
예를 들어

```java
int main(String[] args){
    short num1 = 10;
    short num2 = 20;
    short result = num1 + num2;
}
```

위의 코드에서 short형인 num1과 num2는 덧셈 연산 과정에서 int로 형 변환 됩니다.<br/>

## 2. 묵시적 형 변환 (implicit type conversion)

프로그래머가 별도의 형 변환 명령을 내리지 않아도 형 변환이 발생하는 경우 묵시적 형 변환이라고 합니다.<br/>
작은 수에서 큰 수로, 덜 정밀한 수에서 더 정밀한 수로 대입되는 경우 발생합니다.<br/>

```java
int main(String[] args){
    double dNum;
    float fNum = 2.0F;
    int iNum = 3;

    dNum = fNum + iNum;
}
```

위의 코드에서 iNum이 더 fNum의 자료형인 float으로 묵시적 형 변환이 된 후,
두개를 더한 float형 값이 dNum의 자료형인 double로 묵시적 형 변환이 됩니다.<br/>

묵시적 형 변환 규칙은<br/>
byte -> short -> int -> long -> float ->double<br/>
char -> int -> long -> float ->double 입니다.

## 3. 명시적 형 변환 (excelicit type conversion)

묵시적 형 변환 규칙에 위배되는 상황임에도 불구하고 형 변환이 필요한 경우 명시적 형 변환을 이용합니다.<br/>
변환 되는 자료 형을 명시해 주면 되는데, 이 상황에서 자료의 손실이 발생할 수 있습니다.<br/>

```java
int num = (int)3.15;
```

이런 식으로 작성해 주면 됩니다.<br/>

형 변환도 값을 반환하는 하나의 연산이므로

```java
long num1 = 21348595L;
int num2 = (int)num1;
```

의 경우 num1의 값이 변경되는 것이 아니고 새로운 값을 만들어 num2에 저장하는 것입니다.
