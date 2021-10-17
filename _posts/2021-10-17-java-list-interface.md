---
title: "Java(41) - List 인터페이스"
excerpt: "Java List Interface"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-17 12:55:00"
last_modified_at: "2021-10-17 13:30:00"
---

## 1. List 인터페이스

List 인터페이스란,

1. Collection의 하위 인터페이스
2. 객체를 순서에 따라 저장하고 관리하는데 필요한 메서드가 선언된 인터페이스
3. 배열의 기능을 구현하기 위한 메서드가 선언됨
4. ArrayList, Vector, LinkedList가 포함되어 있음

입니다.<br/>

즉 List 인터페이스는 순서, 중복을 허용하며 리스트(배열, 연결리스트)의 구현을 위한 인터페이스입니다.<br/>

## 2. ArrayList와 Vector

ArrayList와 Vector는 객체 배열 클래스입니다.<br/>
둘 다 내부적으로는 배열입니다.<br/>

일반적으로는 ArrayList를 더 많이 사용하며,<br/>
Vector는 멀티 쓰레드 프로그램에서 동기화(Synchronize)를 지원합니다.<br/>

동기화(Synchronize)란, 두 개의 쓰레드가 동시에 하나의 리소스에 접근할 때<br/>
순서를 맞추어서 데이터의 오류를 방지하도록 합니다.<br/>

ArrayList와 Vector는 default capacity가 0입니다.<br/>
(initial capacity = 10)<br/>

![size&capacity](/images/size_capacity.png)<br/>

capacity와 size는 다른 의미입니다.<br/>
capacity는 용량, size는 몇 개의 요소가 있는지를 뜻합니다.<br/>

추가적으로 ArrayList 순회 시에는 get(i)라는 메서드를 활용합니다.<br/>

## 3. ArrayList와 LinkedList

ArrayList와 LinkedList 모두 자료의 순차적 구조를 구현한 클래스입니다.<br/>

ArrayList는 배열을 구현한 클래스로 논리적 순서와 물리적 순서가 동일합니다.<br/>
LinkedList는 논리적으로는 순차적 구조이지만, 물리적으로는 순차적이지 않을 수 있습니다.<br/>

LinkedList의 구조는<br/>

![linkedlist](/images/linkedlist2.png)<br/>

이며, 자료의 추가와 삭제를 할 때에는<br/>

![add&delete](/images/linkedlist_add_delete.png)<br/>

이런 식으로 추가 시 해당 위치의 링크를 새로운 노드를 향하게 합니다.<br/>
삭제 시에는 삭제 노드로의 링크를 해제하고 다음 노드로 연결시켜줍니다.<br/>

또 다른 차이점은<br/>
ArrayList는 i번째 요소의 접근이 매우 빠르지만 추가 및 삭제시 요소들을 밀고 당기는<br/>
추가적인 연산이 필요하므로 이 때 속도가 느려집니다.<br/>

LinkedList는 추가 및 삭제가 위의 그림처럼 유연합니다.<br/>
(용량 역시 유연)<br/>
그러나 i번째 요소 접근 시 앞이나 뒤부터 순회를 해줘야하기 때문에 접근 속도가 느립니다.<br/>

## 4. 예시

```java
class Member {

	private int memberId;
	private String memberName;

	public Member() {}
	public Member(int memberId, String memberName) {
		this.memberId = memberId;
		this.memberName = memberName;
	}
	public int getMemberId() {
		return memberId;
	}
	public void setMemberId(int memberId) {
		this.memberId = memberId;
	}
	public String getMemberName() {
		return memberName;
	}
	public void setMemberName(String memberName) {
		this.memberName = memberName;
	}

	public String toString() {
		return memberName + "회원님의 아이디는 " + memberId + "입니다.";
	}
}
```

LinkedList<br/>

```java
public class LinkedListTest {

	public static void main(String[] args) {

		LinkedList<String> myList = new LinkedList<String>();

		myList.add("A");
		myList.add("B");
		myList.add("C");

		System.out.println(myList);
		myList.add(1, "D");
		System.out.println(myList);
		myList.removeLast();
		System.out.println(myList);

		for(int i = 0; i<myList.size(); i++) {
			String s = myList.get(i);
			System.out.println(s);
		}
	}

}
```

add() 메서드는 Collection 인터페이스에서 기본적으로 제공하는 메서드 입니다.<br/>
요소를 하나 추가합니다.<br/>

대부분의 Collection은 toString이 제공됩니다.<br/>
LinkedList의 경우 요소들을 보여줍니다.<br/>

add()메서드는 인자로 인덱스 번호와 값을 받습니다.<br/>
(인덱스 번호 지정을 안해주면 순서대로 값이 대입됨)<br/>

remove()메서드는 가장 마지막의 요소가 삭제됩니다.<br/>
(외에 여러 메서드가 존재합니다.)<br/>

get()메서드는 중복을 허용하며 순서에 따라 저장하는 List 인터페이스에만 존재합니다.<br/>
