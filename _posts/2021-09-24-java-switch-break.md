---
title: "Java 조건문(switch, break)"
excerpt: "Java Conditional Switch, Break"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
last_modified_at: 2021-09-24T17:49:00
---

이번 포스트에서는 switch문에 대해 공부하겠습니다.<br/>
if, if~else문과 유사하지만 조건에 따른 실행의 경우 수가 많다면 switch문이 좋은 선택이 될 수 있습니다.<br/>

## 1. switch문의 기본 구성

switch문의 기본 구성을 먼저 보겠습니다.

```java
switch(n)
{
    case 1:
    ...
    ...
    case 2:
    ...
    ...
    case 3:
    ...
    ...
    default:
    ...
}
```

n이 1이면 case 1, 2면 case 2, 3이면 case 3, 다 아닐 경우 default 부터 시작합니다.<br/>

우선 switch문을 이해하기 위해서는 case와 default의 의미를 이해해야 합니다.<br/>
이 두 키워드를 lable이라고 합니다. switch 소괄호 내부의 조건에 해당하는 lable을 찾아 해당 위치부터 실행하는 것입니다.<br/>
default의 경우 case에 해당하는 조건이 없을시 실행되며, 생략이 가능합니다.<br/>

## 2. switch문과 break

switch문의 경우 특정 lable(case) 위치부터 실행이 되는데, 해당 위치부터 switch문의 마지막 까지 전부 실행이 됩니다.<br/>
이것을 해결하기 위해 break 키워드가 사용됩니다.<br/>

```java
class SwitchTest{
    public static void main(String[] args){
        int n = 3;

        switch(n){
            case 1:
                System.out.println("Simple Java");
                break;
            case 2:
                System.out.println("Funny Java");
                break;
            case 3:
                System.out.println("Fantastic Java");
                break;
            default:
                System.out.println("The best programming language");
        }
    }
}
```

위 코드는 case 3 위치에서 시작해 Funny Java를 출력하고 switch문을 빠져나옵니다.<br/>
