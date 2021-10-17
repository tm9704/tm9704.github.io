---
title: "Java(42) - Stack & Queue"
excerpt: "Java Stack Queue"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-17 13:30:00"
last_modified_at: "2021-10-17 14:00:00"
---

이번 포스트에서는 Stack과 Queue를 구현하는 법을 알아보겠습니다.<br/>

## 1. Stack 구현하기

스택은 Last In First Out(LIFO), 즉 후입 선출의 구조를 지닙니다.<br/>
(맨 마지막에 추가된 요소가 가장 먼저 꺼내지는 구조)<br/>

이미 구현된 클래스가 제공되지만, ArrayList나 LinkedList로도 구현이 가능합니다.<br/>

![stack](/images/stack2.png)<br/>

## 2. Queue 구현하기

큐는 First In First Out(FIFO), 즉 선입 선출의 구조를 지닙니다.<br/>
(먼저 저장된 자료가 먼저 꺼내지는 구조)<br/>

보통 선착순, 대기열 구현시 사용하며,<br/>
ArrayList나 LinkedList로 구현할 수 있습니다.<br/>

![queue](/images/queue2.png)<br/>

## 3. 예시

Stack<br/>
(ArrayList로 구현)<br/>

```java
import java.util.ArrayList;

class MyStack {

	private ArrayList<String> arrayStack = new ArrayList<String>();

	public void push(String data) {
		arrayStack.add(data);
	}

	public String pop() {
		int len = arrayStack.size();

		if(len == 0) {
			System.out.println("스택이 비었습니다.");
			return null;
		}

		return arrayStack.remove(len-1);
	}

}

public class StackTest{

	public static void main(String[] args) {
		MyStack stack = new MyStack();

		stack.push("A");
		stack.push("B");
		stack.push("C");

		System.out.println(stack.pop());
		System.out.println(stack.pop());
		System.out.println(stack.pop());
		System.out.println(stack.pop());
	}
}
```

Queue<br/>
