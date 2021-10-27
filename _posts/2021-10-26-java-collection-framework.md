---
title: "Java(49) - 자바 컬렉션 기본 보충"
excerpt: "Java Collection Framework"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-26 14:10:00"
last_modified_at: "2021-10-26 14:30:00"
---

## 1. 컬렉션 프레임워크의 이해

프레임워크(Framework)는 기본적으로 잘 정의된, 약속된 구조나 골격이라는 의미를 지닙니다.<br/>

즉 Java에서 프레임워크는<br/>
'잘 정의된, 약속된 구조의 클래스들'이라는 의미입니다.<br/>

쉽게 말해서 여러 프로그래머들에 의해 사용되도록, 잘 정의된 클래스들의 모임이라고 할 수 있습니다.<br/>
그러나 이게 전부라면 라이브러리와 다를 바가 없습니다.<br/>

컬렉션 프레임워크라고 굳이 하는 이유는 컬렉션과 관련된 클래스들의 정의에 적용되는 설계의 원칙,<br/>
구조가 있기 때문입니다.<br/>

## 2. 컬렉션의 의미와 자료구조

컬렉션의 의미를 이해하기 위해서는 자료구조와 알고리즘에 대한 이해가 필요합니다.<br/>

자료구조는 데이터의 저장과 관련있는 학문으로<br/>
검색 등 다양한 측면을 고려하여 호율적인 데이터의 저장 방법을 연구하는 학문입니다.<br/>

알고리즘은 저장된 데이터의 일부 또는 전체를 대상으로 진행하는 각종 연산을 연구하는 학문입니다.<br/>

컬렉션은 데이터의 저장, 그리고 이와 관련 있는 알고리즘을 구조화 해 놓은 프레임워크입니다.<br/>
즉 자료구조와 알고리즘을 클래스로 구현해 놓은 것입니다.<br/>

이렇기 때문에 컬렉션 클래스들은 컨테이너 클래스라고도 합니다.<br/>
컬렉션 프레임워크를 구성하는 클래스들은 많은 양의 인스턴스를 다양한 형태로 저장하는 기능을 제공합니다.<br/>