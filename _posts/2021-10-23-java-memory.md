---
title: "Java(47) - 자바 메모리 모델"
excerpt: "Java Memory"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-23 15:20:00"
last_modified_at: "2021-10-20 16:00:00"
---

자바 가상머신(JVM)은 운영체제 위에서 동작한다는 것은 예전 포스트에서 공부했습니다.<br/>
JVM은 OS 위에서 실행되는 하나의 프로그램이며, 자바 프로그램은 JVM 위에서 실행되는 프로그램입니다.<br/>

그렇다면 JVM 실행에 필요한 메모리는 어떻게 제공되는 것일까요.<br/>
우리는 프로그램의 실행에 필요한 메모리를 가리켜 메인 메모리라고 부르며, 이것을 램(RAM)입니다<br/>

해당 메모리의 효율적인 관리를 위해 운영체제가 메인 메모리를 관리합니다.<br/>

JVM은 운영체제로부터 할당받은 메모리 공간의 효율적 사용을 위해 세가지의 메모리 영역으로 나눕니다.<br/>

1. 메서드 영역(Method Area) : 메서드의 바이트코드, static변수
2. 스택 영역(Stack Area) : 지역변수, 매개변수
3. 힙 영역(Heap Area) : 인스턴스

## 1. 메서드 영역

소스파일을 컴파일 할 때 생성되는, 자바 가상머신에 의해 실행이 가능한 코드를 가리켜 우리는<br/>
자바 바이트코드(bytecode) 즉 .class 파일 이라고 합니다.<br/>
이러한 바이트코드들도 메모리 공간에 저장되어 있어야 실행이 가능합니다.<br/>

실행의 흐름을 형성하는 메서드의 바이트코드들은 메서드 영역에 저장됩니다.<br/>
그리고 static으로 선언되는 클래스 변수도 이 영역에 할당이 됩니다.<br/>

바이트코드와 static 변수가 메서드 영역에 저장되는 시점은<br/>
클래스가 메모리에 올려지는 시점입니다.<br/>

즉 메서드 영역은 클래스 정보를 메모리 공간에 올릴 때 초기화 되는 대상을 저장하기 위한 메모리 공간입니다.<br/>

메서드의 바이트코드는 프로그램의 흐름을 구성하는 바이트코드이며, 사실상 컴파일 된 바이트코드의 대부분이므로<br/>
다른 바이트 코드 즉 전체 바이트 코드가 올라간다고 보면 됩니다.<br/>

그리고 메서드 영역에는 static 변수뿐 만 아니라, 메서드의 바이트코드가 올라가기 때문에<br/>
static 메서드 역시 올라갑니다.<br/>

## 2. 스택 영역

스택 영역은 지역변수와 매개변수가 저장되는 공간입니다.<br/>

즉 스택은 프로그램의 실행과정에서 임시로 할당되었다가, 메서드를 빠져나가면 바로 소멸되는<br/>
특성의 데이터 저장을 위한 영역입니다.<br/>

## 3. 힙 영역

인스턴스는 힙 영역에 할당이 됩니다. 인스턴스를 스택이 아닌 힙 영역에 할당하는 이유가 무엇일까요?<br/>
인스턴스의 소멸방법과 소멸시점이 지역변수와는 다르기 때문에 스택이 아닌 힙 영역에 할당을 받는 것입니다.<br/>

힙 영역에 생성된 인스턴스들은 JVM이 그 소멸시기를 결정합니다.<br/>

소멸시기를 결정하는 방법은 어떠한 참조변수로도 참조가 이루어지지 않는 인스턴스가 있을 경우,<br/>
즉 어떠한 형태로건 참조되지 않는 인스턴스가 소멸의 대상이 되며,<br/>
이러한 조건을 충족시킬 때 JVM은 해당 인스턴스를 소멸시킵니다.<br/>

JVM의 이러한 소멸기능을 GC(Garbage Collection)이라고 합니다.<br/>