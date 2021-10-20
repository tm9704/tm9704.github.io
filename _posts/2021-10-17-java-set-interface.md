---
title: "Java(43) - Set 인터페이스"
excerpt: "Java Set Interface"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-17 14:00:00"
last_modified_at: "2021-10-17 14:30:00"
---

## 1. Iterator로 순회하기

Iterator로 순회하기는 Collection에 저장되어 있는 요소를 읽어오는 방법입니다.<br/>

Iterator는 Collection의 개체를 순회하는 인터페이스입니다.<br/>
ArrayList 같은 경우는 순서가 있어서 get()메서드 같은 걸로 순회가 가능하지만,<br/>
Set 인터페이스 같은 경우는 순서대로 저장되지 않기 때문에 Iterator를 사용합니다.<br/>
(중복 방지를 위해 String 외에는 사용해야 합니다.)<br/>

iterator() 메서드를 호출하는 방법은,<br/>
Iterator ir = memberArrayList.iterator();<br/>
로 호출합니다.<br/>

Iterator 인터페이스에 선언된 메서드는<br/>

1. boolean hasNext()<br/>
   이후에 요소가 더 있는지를 체크하는 메서드<br/>
   요소가 있다면 true 반환
2. E next()<br/>
   다음에 있는 요소를 반환

예시를 하나 보겠습니다.<br/>

```java
public class HashSetTest{
    public static void main(String[] args){
        HashSet<String> set = new HashSet<String>();
        set.add("Kim");
        set.add("Lee");
        set.add("Park");
        set.add("Kim");

        Iterator<String> ir = set.iterator(); //<>안에 순회할 요소를 지정합니다.
        //모든 Collection에 대고 iterator()메서드를 부를 수 있습니다.

        while(ir.hasNext()){
            String str = ir.next();
            System.out.println(str);
        }
    }
}
```

그냥 출력하면 순서대로 출력되는게 아닙니다.<br/>
코드에서 중복된 값을 넣어줬는데, 중복된 값은 출력이 되지 않습니다.<br/>

다른 예시를 보겠습니다.<br/>

```java
import java.util.*;

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

	@Override
	public int hashCode() {
		return memberId;
	}
	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Member) {
			Member member = (Member)obj;
			return (this.memberId == member.memberId);
		}

		return false;
	}
}

class MemberHashSet {

	private HashSet<Member> hashSet;

	public MemberHashSet() {
		hashSet = new HashSet<Member>();
	}

	public void addMember(Member member) {
		hashSet.add(member);
	}

	public boolean removeMember(int memberId) {
		Iterator<Member> ir = hashSet.iterator();
		while(ir.hasNext()) {
			Member member = ir.next();
			if(member.getMemberId() == memberId) {
				hashSet.remove(member);
				return true;
			}
		}

		System.out.println(memberId + "번호가 존재하지 않습니다.");
		return false;
	}

	public void showAllMember() {
		for(Member member : hashSet) {
			System.out.println(member);
		}
		System.out.println();
	}
}

public class MemberHashSetTest {

	public static void main(String[] args) {

		MemberHashSet manager = new MemberHashSet();

		Member memberLee = new Member(100, "Lee");
		Member memberKim = new Member(200, "Kim");
		Member memberPark = new Member(300, "Park");
		Member memberPark2 = new Member(300, "Park2");

		manager.addMember(memberLee);
		manager.addMember(memberKim);
		manager.addMember(memberPark);
		manager.addMember(memberPark2);

		manager.showAllMember();

		manager.removeMember(100);

		manager.showAllMember();
	}
}
```

## 2. Set 인터페이스

Set 인터페이스는 Collection 하위의 인터페이스입니다.<br/>

- 중복을 허용하지 않음
- List는 순서 기반의 인터페이스이지만, Set은 순서가 없음
- get(i)메서드가 제공되지 않는다 (Iterator 이용)
- 저장된 순서와 출력 순서는 다를 수 있다
- 아이디, 주민번호, 사번 등 유일한 값이나 객체를 관리할 때 사용
- HashSet, TreeSet 클래스가 있다

즉 Set 인터페이스는 순서와 관계없이 중복을 허용하지 않고 유일한 값을<br/>
관리하는데 필요한 메서드가 선언되어있습니다.<br/>

## 3. HashSet 클래스

HashSet 클래스는 Set 인터페이스를 구현한 클래스입니다.<br/>

중복을 허용하지 않으므로 저장되는 객체의 동일함 여부를 알기 위해<br/>
equals()와 hashCode() 메서드를 재정의 해야합니다.<br/>
(String클래스는 JDK 내부에 이미 구현, String외에는 사용되는 클래스 쪽에 지정)<br/>

## 4. TreeSet 클래스

Tree Set 클래스는,<br/>

- 객체의 정렬에 사용되는 클래스
- 중복을 허용하지 않으면서 오름차순이나 내림차순으로 객체를 정의함
- 내부적으로 이진 검색 트리(BST)로 구현되어있음
- 이진 검색 트리에 자료가 저장될 때 비교하여 저장될 위치를 정함
- 객체 비교를 위해 Comparable이나 Comparator 인터페이스를 구현해야함<br/>
  (String은 이미 구현)

## 5. Comparable 인터페이스와 Comparator 인터페이스

두 인터페이스는 정렬 대상이 되는 클래스가 구현해야 하는 인터페이스입니다.<br/>

Comparable은 <br/>
compareTo() 메서드를 구현합니다.<br/>
해당 메서드는 매개변수와 객체 자신(this)를 비교합니다.<br/>
(compareTo()는 콜백 함수로서 프로그래머가 호출하는 것이 아닌 시스템이 호출합니다.(add 시점))<br/>
java.lang 패키지 안에 존재합니다.<br/>

Comparator는 <br/>
compare() 메서드를 구현합니다.<br/>
두개의 매개변수를 비교합니다.<br/>
TreeSet 생성자에 Comparator가 구현될 객체를 매개변수로 전달합니다.<br/>
java.util 패키지 안에 존재합니다.<br/>

일반적으로 Comparable을 더 많이 사용합니다.<br/>
TreeSet\<Member> treeSet = new TreeSet\<Member>(new Member());<br/>
(comparator 사용 시 TreeSet 생성자 부분에 comparator 대상(compare이 구현된 객체)을 써야합니다.)<br/>

이미 Comparable이 구현된 경우 Comparator를 이용하여 다른 정렬 방식을 정의할 수 있습니다.<br/>
(String, Integer 등 JDK의 많은 클래스들은 이미 정의되어 있음)<br/>

## 6. TreeSet 예시

1번 예제<br/>

```java
import java.util.*;

public class TreeSetTest {

	public static void main(String[] args) {

		TreeSet<String> treeSet = new TreeSet<String>();
		treeSet.add("홍길동");
		treeSet.add("강감찬");
		treeSet.add("이순신");

		for(String str : treeSet) {
			System.out.println(str);
		}
	}
}
```

이 상황에서는 순서대로 정렬이 됩니다.<br/>
해당 이유는 String에는 이미 Comparable이 구현되어있기 때문입니다.<br/>
(기본적으로 오름차순)<br/>

2번 예제<br/>

```java
import java.util.*;

class Member implements Comparable<Member>{

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
	@Override
	public int hashCode() {
		return memberId;
	}
	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Member) {
			Member member = (Member)obj;
			return (this.memberId == member.memberId);
		}

		return false;
	}
	@Override
	public int compareTo(Member member) {

		return (this.memberId - member.memberId);
	}

	/*
	 * public int compareTo(Member member){
	 *
	 *		return this.memberName.compareTo(member.getMemberName());
	 *		String은 이미 구현되어있음.
	 * }
	 */

	/*
	 * public class Member implements Comparator<Member>{
	 * ...
	 *
	 * 	@Override
	 * 	public int compare(Member member1, Member member2){
	 *  	member1이 this member2가 매개변수.
	 *  	return (ember1.memberId - member2.memberId);
	 * 	}
	 * 	Comparator는  TreeSet 생성자에 명시 해줘야함.
	 * 	ex) treeSet = new TreeSet<Member>(new Member());
	 *
	 * }
	 */
}

class MemberTreeSet {

	private TreeSet<Member> treeSet;

	public MemberTreeSet() {
		treeSet = new TreeSet<Member>();
	}

	public void addMember(Member member) {
		treeSet.add(member);
	}

	public boolean removeMember(int memberId) {
		Iterator<Member> ir = treeSet.iterator();
		while(ir.hasNext()) {
			Member member = ir.next();
			if(member.getMemberId() == memberId) {
				treeSet.remove(member);
				return true;
			}
		}

		System.out.println(memberId + "번호가 존재하지 않습니다.");
		return false;
	}

	public void showAllMember() {
		for(Member member : treeSet) {
			System.out.println(member);
		}
		System.out.println();
	}
}

public class MemberTreeSetTest {

	public static void main(String[] args) {

		MemberTreeSet manager = new MemberTreeSet();

		Member memberLee = new Member(300, "Lee");
		Member memberKim = new Member(100, "Kim");
		Member memberPark = new Member(200, "Park");


		manager.addMember(memberLee);
		manager.addMember(memberKim);
		manager.addMember(memberPark);

		manager.showAllMember();
	}

	/*
	 * TreeSet<String> set = new TreeSet<String>(new MyCompare());
	 * set.add("Park");
	 * set.add("Kim");
	 * set.add("Lee");
	 *
	 * System.out.println(set);
	 */
}
```

Member 클래스에서 주석처리된 이름으로 정렬하기 위한 compareTo의 경우<br/>
이름은 String이기 때문에 이미 compareTo가 구현되어 있으므로,<br/>
comparTo(member.getMemberName())을 사용하면 됩니다.<br/>

또 주석처리된 부분을 보면, Comparator를 구현한 것입니다.<br/>
compare메서드를 구현해야하고, Comparator은 default constructor에<br/>
명시를 해줘야합니다.<br/>

이번에는 Comparator예시를 보겠습니다.<br/>

```java
import java.util.Comparator;
import java.util.TreeSet;

class MyCompare implements Comparator<String>{

	@Override
	public int compare(String s1, String s2) {

		return s1.compareTo(s2)*(-1);
	}

}

public class ComparatorTest {

	public static void main(String[] args) {

		TreeSet<String> treeSet = new TreeSet<String>(new MyCompare());
		treeSet.add("홍길동");
		treeSet.add("강감찬");
		treeSet.add("이순신");

		for(String str : treeSet) {
			System.out.println(str);
		}
	}
}
```

String의 경우 자동으로 오름차순으로 정렬되게 구현되어 있습니다.<br/>
내림차순으로 정렬하기 위해서는 위 코드처럼 MyCompare 클래스를 통해<br/>
Comparator를 구현해주면 됩니다.<br/>

Comparator은 보통 compare가 이미 구현되어있는 클래스가 있는 경우<br/>
해당 클래스에 대해 다른 방식의 정렬을 제공할 때 많이 사용합니다.<br/>
