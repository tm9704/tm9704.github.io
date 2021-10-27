---
title: "Java(52) - 자바 Map 인터페이스 보충"
excerpt: "Java Map Interface"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-27 16:05:00"
last_modified_at: "2021-10-27 16:30:00"
---

Map\<K,V\> 인터페이스를 구현하는 컬렉션 클래스들의 데이터 저장방식은<br/>
key-value 방식을 이루고 있습니다.<br/>

## 1. key-value방식과 HashMap<K,V> 클래스

데이터 저장방식의 한 종류인 key-value 방식에서<br/>
key는 데이터를 찾는 열쇠 즉, 이름을 의미하며<br/>
value는 찾고자 하는 실질적인 데이터를 의미합니다.<br/>

앞선 포스트에서 봤던 Collection\<E\>를 구현하는 컬랙션 클래스들이 value만 저장하는 구조였다면<br/>
Map 인터페이스를 구현하는 컬랙션 클래스들은 value를 저장할 때,<br/>
이를 찾는데 사용하는 key를 함께 저장하는 구조입니다.<br/>

Map 인터페이스를 구현하는 대표적인 클래스로는 HashMap과 TreeMap이 있습니다.<br/>
우선 HashMap을 보겠습니다.<br/>

```java
import java.util.HashMap;

class IntroHashMap{
    public static void main(String[] args){
        HashMap<Integer, String> hMap = new HashMap<Integer, String>();

        hMap.put(new Integer(3), "삼번");
        hMap.put(5, "오번");
        hMap.put(8, "팔번");

        System.out.println("8번 학생 : " + hMap.get(new Integer(8)));
        System.out.println("5번 학생 : " + hMap.get(5));
        System.out.println("3번 학생 : " + hMap.get(3));

        hMap.remove(5);
        System.out.println("5번 학생 : " + hMap.get(5));
    }
}
```

해당 코드에서 HashMap 인스턴스를 key는 Integer, value는 String으로 생성하고 있습니다.<br/>

put 메서드는 데이터의 저장에 사용되며, 첫번째 인자로는 key, 두번째 인자로는 value가 전달됩니다.<br/>

key가 Integer이므로 첫번째 인자로 Integer형 인스턴스가 전달되어야 하는데,<br/>
AutoBoxing의 도움으로 정수만 전달되어도 됩니다.<br/>

get 메서드는 앞에서 봤듯이 데이터를 참조하는 메서드입니다.<br/>
참조하고자 하는 데이터의 key가 인자로 전달되어야 key에 해당하는 데이터가 반환됩니다.<br/>

remove 메서드는 데이터의 삭제에 사용됩니다.<br/>
삭제할 데이터의 key값이 인자로 전달되어야 합니다.<br/>

key에 해당하는 데이터가 존재하지 않을 경우, null이 반환됩니다.<br/>

위 예제에서 보이지 않는 특징을 정리하자면,<br/>

- value에 상관없이 중복된 key의 저장은 불가능
- value는 같더라도 key가 다르면 둘 이상의 데이터도 저장이 가능

위의 두가지가 의미하는 바는 유사합니다.<br/>
그리고 데이터를 구분하는 기준이 key이므로, key의 중복은 허용되지 않습니다.<br/>

## 2. TreeMap<K,V> 클래스

HashSet\<E\>가 해시 알고리즘을 기준으로 구현되어 있듯이, HashMap 역시<br/>
해시 알고리즘을 기반으로 구현되어 있습니다.<br/>
HashSet의 장점인 매우 빠른 검색속도 역시 HashMap에 그대로 반영됩니다.<br/>

TreeMap 역시 트리 자료구조를 기반으로 구현되어 있습니다.<br/>
따라서 데이터는 정렬된 상태로 저장됩니다.<br/>

```java
import java.util.TreeMap;
import java.util.Iterator;
import java.util.NavigableSet;

class IntroTreeMap{
    public static void main(String[] args){
        TreeMap<Integer, String> tMap = new TreeMap<Integer, String>();
        tMap.put(1, "data1");
        tMap.put(3, "data3");
        tMap.put(5, "data5");
        tMap.put(2, "data2");
        tMap.put(4, "data4");

        NavigableSet<Integer> navi = tMap.navigableKeySet();

        System.out.println("오름차순 출력..");
        Iterator<Integer> itr = navi.iterator();
        while(itr.hasNext()){
            System.out.println(tMap.get(itr.next()));
        }

        System.out.println("내림차순 출력..");
        itr = navi.descendingIterator();
        while(itr.hasNext()){
            System.out.println(tMap.get(itr.next()));
        }
    }
}
```

위의 예제는 총 5개의 데이터를 저장하고 있습니다.<br/>
HashMap과 달리 정렬되어 저장이 됩니다. 단, 정렬의 대상은 key지 value가 아닙니다.<br/>
데이터는 key를 기준으로 참조가 이루어지기 때문입니다.<br/>

navigableKeySet 메서드가 호출되면, 인터페이스 NavigableSet\<E\>를 구현하는 인스턴스가<br/>
(인스턴스의 참조 값) 반환됩니다.<br/>
이 때 E는 key 자료형인 Integer가 되며, 반환된 인스턴스에는 데이터들의 key 정보가 저장되어있습니다.<br/>

NavigableSet\<E\> 인터페이스는 Set\<E\> 인터페이스를 상속하기 때문에,<br/>
navigableKeySet 메서드가 반환하는 인스턴스를 대상으로 반복자를 얻기 위해서 iterator 호출이 가능합니다.<br/>
그리고 이렇게 얻은 반복자로 저장된 모든 key에 접근이 가능합니다.<br/>

## 추가. TreeMap<K,V>의 전체 데이터 검색

TreeMap은 Collection이 아닌 Map을 구현하는 컬렉션 클래스입니다.<br/>

즉, 저장되어 있는 전체 데이터를 검색하는 방식에는 차이가 있습니다.<br/>

그리고 TreeMap에 저장된 전체 데이터 참조 과정에서 호출한 navigableKeySet 메서드가 반환하는<br/>
인스턴스가 Set 인터페이스를 구현합니다.<br/>

key는 중복이 불가능하기 때문에 집합의 성격을 띄며,<br/>
이러한 key를 저장하는 인스턴스는 Set 인터페이스를 구현하고 있는 것입니다.<br/>

나중에 NavigableSet 인터페이스 추가
