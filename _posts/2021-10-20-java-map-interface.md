---
title: "Java(44) - Map 인터페이스"
excerpt: "Java Map Interface"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-20 14:40:00"
last_modified_at: "2021-10-20 15:00:00"
---

## 1. Map 인터페이스

Map은 쌍으로 이루어진 자료를 관리하기 위한 자료구조입니다.<br/>

- key,value pair의 객체를 관리하는데 필요한 메서드가 정의된 인터페이스
- key는 중복 불가<br/>
  (key는 Set의 개념, value는 Collection 개념)
- 검색을 위한 자료구조
- key를 이용하여 값을 저장하거나 검색, 삭제할 때 사용하면 편리함<br/>
  내부적으로는 hash 방식으로 구현됨<br/>
  (index = hash(key) //index는 저장 위치)
- key가 되는 객체는 객체의 유일성의 여부를 알기 위해 equals()와<br/>
  hashCode() 메서드를 재정의함

## 2. HashMap 클래스

- Map 인터페이스를 구현한 클래스 중 가장 일반적으로 사용하는 클래스<br/>
  (가장 많이 사용합니다)
- HashTable 클래스는 자바2부터 제공된 클래스로 Vector처럼 동기화를 제공<br/>
  (예전에 많이 사용)
- pair자료를 쉽고 빠르게 관리 가능

private HashMap\<Integer, Member> hashMap;

## 3. TreeMap 클래스

- key 객체를 정렬하여 key-value를 pair로 관리하는 클래스
- key에 사용되는 클래스에 Comparable, Comparator 인터페이스를 구현
- java에 많은 클래스들은 이미 Comparable이 구현되어 있음
- 구현된 클래스를 key로 사용하는 경우 구현할 필요가 없다

```java
public final class Integer extends Number implements Comparable<integer>{
    ...
    public int compareTo(Integer anotherInteger){
        return compare(this.value, anotherInteger.value);
    }
}
```

위 코드는 Integer는 이미 구현되어 있다는 것을 보여주는 것입니다.<br/>

## 4. 예시

1. HashMap<br/>

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
    }

    class MemberHashMap {

        private HashMap<Integer, Member> hashMap;

        public MemberHashMap() {
            hashMap = new HashMap<Integer, Member>();
        }

        public void addMember(Member member) {
            hashMap.put(member.getMemberId(), member);
        }

        public boolean removeMember(int memberId) {
            if(hashMap.containsKey(memberId)) {
                hashMap.remove(memberId);
                return true;
            }
            System.out.println("회원 번호가 없습니다.");
            return false;
        }

        public  void showAllMember() {

            Iterator<Integer> ir = hashMap.keySet().iterator();
            while(ir.hasNext()) {
                int key = ir.next();
                Member member = hashMap.get(key);
                System.out.println(member);
            }
            System.out.println();
        }
    }

    public class MemberHashMapTest {

        public static void main(String[] args) {

            MemberHashMap manager = new MemberHashMap();

            Member memberLee = new Member(100, "Lee");
            Member memberKim = new Member(200, "Kim");
            Member memberPark = new Member(300, "Park");
            Member memberPark2 = new Member(300, "Park");


            manager.addMember(memberLee);
            manager.addMember(memberKim);
            manager.addMember(memberPark);
            manager.addMember(memberPark2);

            manager.showAllMember();

            manager.removeMember(200);
            manager.showAllMember();
        }
    }
   ```

2. TreeMap<br/>

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
    }

    class MemberTreeMap {

        private TreeMap<Integer, Member> treeMap;

        public MemberTreeMap() {
            treeMap = new TreeMap<Integer, Member>();
        }

        public void addMember(Member member) {
            treeMap.put(member.getMemberId(), member);
        }

        public boolean removeMember(int memberId) {
            if(treeMap.containsKey(memberId)) {
                treeMap.remove(memberId);
                return true;
            }
            System.out.println("회원 번호가 없습니다.");
            return false;
        }

        public  void showAllMember() {

            Iterator<Integer> ir = treeMap.keySet().iterator();
            while(ir.hasNext()) {
                int key = ir.next();
                Member member = treeMap.get(key);
                System.out.println(member);
            }
            System.out.println();
        }
    }

    public class MemberTreeMapTest {

        public static void main(String[] args) {

            MemberTreeMap manager = new MemberTreeMap();

            Member memberPark = new Member(300, "Park");
            Member memberLee = new Member(100, "Lee");
            Member memberKim = new Member(200, "Kim");
            Member memberPark2 = new Member(400, "Park");


            manager.addMember(memberLee);
            manager.addMember(memberKim);
            manager.addMember(memberPark);
            manager.addMember(memberPark2);

            manager.showAllMember();

            manager.removeMember(200);
            manager.showAllMember();
        }
    }
   ```

## 5. 해싱, 요소 순회

1. 해싱(Hashing)<br/>
   해싱은 자료를 검색하기 위한 구조입니다.<br/>

   - key에 대한 자료를 검색하기 위한 사전(dictionary) 개념의 자료구조
   - key는 유일하고 이에 대한 value의 쌍으로 저장
   - index = h(key); 해시 함수가 key에 대한 인덱스를 반환해줌,<br/>
     해당 인덱스 위치에 자료를 저장하거나 검색하게 됨
   - 해시 함수에 의해 인덱스 연산이 산술적으로 가능
   - 저장되는 메모리 구조를 해시 테이블이라고 함
   - jdk 클래스: HashMap, Properties

2. 요소의 순회란<br/>
   - 컬랙션 프레임워크에 저장된 요소들을 하나씩 차례로 참조하는 것
   - 순서가 있는 List 인터페이스의 경우 Iterator를 사용하지 않고<br/>
     get(i) 메서드 활용
   - Set 인터페이스의 경우 get(i) 메서드가 제공되지 않으므로 Iterator를 이용하여<br/>
     객체를 순회
