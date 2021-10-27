---
title: "Java(51) - 자바 Set 인터페이스 보충"
excerpt: "Java Set Interface"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-27 14:35:00"
last_modified_at: "2021-10-27 15:30:00"
---

## 1. Set 인터페이스의 특성과 HashSet 클래스

우선 List 인터페이스를 구현하는 클래스와 Set 인터페이스를 구현하는 클래스들의 차이점을 보겠습니다.<br/>

- List 인터페이스를 구현하는 클래스들과 달리 Set 인터페이스를 구현하는 클래스들은<br/>
  데이터의 저장순서를 유지하지 않는다
- List 인터페이스를 구현하는 클래스들과 달리 Set 인터페이스를 구현하는 클래스들은<br/>
  데이터의 중복저장을 허용하지 않는다.

즉 Set 인터페이스의 특성은 수학에서 말하는 집합과 동일한 특성을 지닙니다.<br/>
Set인터페이스를 구현하는 대표적인 클래스 HashSet을 이용한 예제를 보겠습니다.<br/>

```java
import java.util.Iterator;
import java.util.HashSet;

class SetInterfaceFeature{
    public static void main(String[] args){
        HashSet<String> hSet = new HashSet<String>();
        hSet.add("Frist");
        hSet.add("Second");
        hSet.add("Third");
        hSet.add("First");

        System.out.println("저장된 데이터 수: " + hSet.size());

        Iterator<String> itr = hSet.iterator();
        while(itr.hashNext()){
            System.out.println(itr.next());
        }
    }
}
```

위 예제의 실행 결과는<br/>

**저장된 데이터 수: 3**<br/>
**Third**<br/>
**Second**<br/>
**First**<br/>

입니다.<br/>

실행결과를 보면 First가 반복되서 저장되지 않음을 알 수 있습니다.<br/>
저장 순서 역시 저장된 순서에 관계없이 출력되는 것을 알 수 있습니다.<br/>

그렇다면 어떤 기준으로 동일한 데이터인지 아닌지 구분하는 것일까요?<br/>
예제를 하나 보겠습니다.<br/>

```java
import java.util.Iterator;
import java.util.HashSet;

class SimpleNumber{
    int num;
    public SimpleNumber(int n){
        num = n;
    }
    public String toString(){
        return String.valueOf(num);
    }
}

class HashSetEqulityOne{
    public static void main(String[] args){
        HashSet<SimpleNumber> hSet = new HashSet<SimpleNumber>();
        hSet.add(new SimpleNumber(10));
        hSet.add(new SimpleNumber(20));
        hSet.add(new SimpleNumber(20));

        System.out.println("저장된 데이터 수: " + hSet.size());

        Iterator<SimpleNumber> itr = hSet.iterator();
        while(itr.hasNext()){
            System.out.println(itr.next());
        }
    }
}
```

실행 결과는<br/>

**저장된 데이터 수: 3**<br/>
**20**<br/>
**10**<br/>
**20**<br/>

입니다.<br/>

해당 결과를 보면, hSet.add(new SimpleNumber(20)) 코드 두 줄을 다른 인스턴스로 간주하고 있습니다.<br/>
이는 HashSet 클래스가 equals 메서드의 호출결과와 hashCode 메서드의 호출 결과를 가지고 비교하기 때문입니다.<br/>

## 2. 해시 알고리즘과 hashcCode 메서드

우리가 HashSet 클래스를 제대로 활용하기 위해서는 해시 알고리즘의 약간의 이해가 필요합니다.<br/>

간단한 해시 알고리즘을 보겠습니다.<br/>
**_num%3_**<br/>
위 코드는 나머지 연산의 결과(0,1,2)에 따라 숫자를 묶습니다.<br/>

만약 3, 5, 7, 12, 25, 31이라는 숫자가 있고 해시 알고리즘을 통해 3가지 부류로 나눈 상태에서,<br/>
정수 5의 존재를 확인하는 효율적인 방법은 나머지의 연산 결과가 2인 집합을 찾는 것입니다.<br/>

이런식으로 했을 경우, 연산의 결과가 0,1인 부류들은 검색의 대상에서 제외되며<br/>
검색의 대상이 줄어들게 되고, 검색속도가 매우 빨라집니다.<br/>

이것이 해시 알고리즘의 장점이며, 해시 연산의 결과인 0, 1, 2를 **_해시 값_** 이라고 하며<br/>
이 값을 기준으로 데이터를 구분합니다.<br/>

예제를 하나 보겠습니다.<br/>

```java
import java.util.Iterator;
import java.util.HashSet;

class SimpleNumber{
    int num;

    public SimpleNumber(int n){
        num = n;
    }
    public String toString(){
        return String.valueOf(num);
    }
    public int hashCode(){
        return num%3;
    }
    public boolean equals(Object obj){
        SimpleNumber comp = (SimpleNumber)obj;
        if(comp.num==num){
            return true;
        }else{
            return false;
        }
    }
}

class HashSetEqualityTwo{
    public static void main(String[] args){
        HashSet<SimpleNumber> hSet = new HashSet<SimpleNumber>();
        hSet.add(new SimpleNumber(10));
        hSet.add(new SimpleNumber(20));
        hSet.add(new SimpleNumber(20));

        Iterator<SimpleNumber> itr = hSet.iterator();
        while(itr.hasNext()){
            System.out.println(itr.next());
        }
    }
}
```

해당 예제의 단계는<br/>

- Object 클래스의 hashCode 메서드의 반환 값을 해시 값으로 활용
- Object 클래스의 equals 메서드의 반환 값을 이용해 내용 비교

입니다.<br/>
(각각 public int hashCode(), public boolean equals(Object obj)를 오버라이딩)<br/>

이렇게 하게되면 앞서 본 예제의 결과와 달리 중복을 허용하지 않는 결과가 도출됩니다.<br/>

## 3. TreeSet 클래스의 이해와 활용

TreeSet 클래스는 **_트리(Tree)_**라는 자료구조를 기반으로 구현되어 있습니다.<br/>

**_트리_**는 데이터를 정렬된 상태로 저장하는 자료구조입니다.<br/>
즉, 이를 기반으로 구현된 TreeSet 클래스 역시 데이터를 정렬된 상태로 유지합니다.<br/>
(저장순서를 유지하는 것과는 의미가 다릅니다.)<br/>

예제를 보겠습니다.<br/>

```java
import java.util.Iterator;
import java.util.TreeSet;

class SortTreeSet{
    public static void main(String[] args){
        TreeSet<Integer> sTree = new TreeSet<Integer>();
        sTree.add(1);//AutoBoxing에 의해 생성되는 인스턴스
        sTree.add(2);
        sTree.add(4);
        sTree.add(3);
        sTree.add(2);

        System.out.println("저장된 데이터 수: " + sTree.size());

        Iterator<Integer> itr = sTree.iterator();
        while(itr.hasNext()){
            System.out.println(itr.next());
        }
    }
}
```

해당 코드의 결과는 1 2 3 4 입니다.<br/>
중복된 데이터의 저장을 허용하지 않으며, 오름차순으로 정렬되고 있습니다.<br/>

iterator 메서드가 반환하는 반복자는 오름차순의 참조를 보장하지만,<br/>
여기서 우리가 중요하게 봐야할 것은 정렬의 방식이 아닌, **_정렬의 기준_**입니다.<br/>

위 예제처럼 숫자라면 정렬의 기준에 대해 논란이 없습니다.<br/>
숫자의 크기 자체가 정렬의 유일한 기준이기 때문입니다.<br/>

```java
class Person{
    String name;
    int age;
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
}
```

그러나 위의 클래스의 인스턴스가 저장대상이라면 이야기는 달라집니다.<br/>

```java
Person man1 = new Person("Lee", 24);
Person man2 = new Person("Kim", 15);
Person man3 = new Person("Park", 30);
```

위 처럼 인스턴스가 생성됐다고 가정합시다.<br/>
이 상황에서는 무엇을 정렬의 기준으로 삼아야하는지 명확하지 않습니다.<br/>

이런 상황 때문에 Java에서는 인터페이스의 구현을 통해 정렬의 기준을<br/>
프로그래머가 직접 정의할 것을 요구합니다.<br/>

```java
interface Comparable<T>{
    int compareTo(T obj);
}
```

위의 Comparable\<T\> 인터페이스의 compareTo 메서드는 다음의 내용을 근거로 정의되어야 합니다.<br/>

- 인자로 전달된 obj가 작다면 양의 정수를 반환
- 인자로 전달된 obj가 크다면 음의 정수를 반환
- 인자로 전달된 obj가 같다면 0을 반환

여기서 말하는 크고 작음에 대한 기준이 세워지면, 정렬의 기준이 세워지는 것입니다.<br/>

TreeSet\<E\> 역시 comparTo 메서드의 호출결과를 참조하여 정렬을 하기 때문에,<br/>
TreeSet\<E\>에 저장되는 인스턴스는 반드시 Comparable\<T\> 인터페이스를 구현해야 합니다.<br/>

Integer 클래스 같은 Java에 정의되어 있는 클래스들은 이미 해당 인스턴스를 구현하고 있습니다.<br/>

## 4. Comparable<T> 인터페이스 구현

위에서 본 Person 클래스의 정렬기준을 나이로 하고 Comparable 인터페이스를<br/>
구현한 예제를 보겠습니다.<br/>

```java
class Person implements Comparable<Person>{
    String name;
    int age;

    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
    public void showData(){
        System.out.println("%s %d \n",name, age);
    }
    public int compareTo(Person p){
        if(age>p.age){ //인자로 전달된 p가 작음 -> 양수 반환
            return 1;
        }else if(age<p.age){ //음수 반환
            return -1;
        }else{ //0 반환
            return 0;
        }
    }
}

class ComparablePerson{
    public static void main(String[] args){
        TreeSet<Person> sTree = new TreeSet<Person>();
        sTree.add(new Person("Lee", 24));
        sTree.add(new Person("Kim", 15));
        sTree.add(new Person("Park", 30));

        Iterator<Person> itr = sTree.iterator();
        while(itr.hasNext()){
            itr.next().showData();
        }
    }
}
```

다음은 또 다른 정렬의 기준에 대해 보겠습니다.<br/>
String 클래스의 경우 사전편찬 순으로 정렬되도록 Comparable 인터페이스가 구현되어 있습니다.<br/>

즉 문자열을 TreeSet\<E\>에 저장할 경우 사전편찬 순으로 정렬이 이루어집니다.<br/>
여기서 사전편찬 순이 아닌 길이 순으로 저장하고 싶을 때를 보겠습니다.<br/>

우선 String 클래스의 Wrapper 클래스를 다음과 같이 정의하겠습니다.<br/>

```java
class MyString{
    String str;
    public MyString(String str){this.str = str;}
    public int getLength(){return str.length();}
    public String toString(){return str;}
}
```

다음은 전체 예제를 보겠습니다.<br/>

```java
import java.util.TreeSet;
import java.util.Iterator;

class MyString implements Comparable<MyString>{
    String str;
    public MyString(String str){this.str = str;}
    public int getLength(){return str.length();}

    public int compareTo(MyString mStr){
        if (getLength()>mStr.getLength()){
            return 1;
        }else if(getLength()<mStr.getLength()){
            return -1;
        }else{
            return 0;
        }

        // return getLength() - mStr.getLength()
    }

    public String toString(){return str;}
}

class ComparableMyString{
    public static void main(String[] args){
        TreeSet<MyString> tSet = new TreeSet<MyString>();
        tSet.add(new MyString("Orange"));
        tSet.add(new MyString("Apple"));
        tSet.add(new MyString("Dog"));
        tSet.add(new MyString("Individual"));

        Iterator<MyString> itr = tSet.iterator();
        while(itr.hasNext()){
            System.out.println(itr.next());
        }
    }
}
```

해당 코드는<br/>
compareTo 메서드의 구현을 통해 정렬의 기준을 정합니다.<br/>

그리고 주석처리 되어있는 부분이 있는데,<br/>
해당 코드 위의 코드는 이해를 위한 코드이고 주석처리한 부분처럼 간단하게 나타내도 됩니다.<br/>
(내림차순의 반복자를 반환하는 메서드는 찾아보도록 합시다)<br/>

## 5. Comparator<T> 인테페이스를 기반으로 TreeSet<E>의 정렬 기준 제시하기

앞서 본 예제 ComparableMyString.java의 경우 정렬을 위해 MyString이라는<br/>
String의 Wrapper 클래스를 별도로 정의했습니다.<br/>

그렇다면 반드시 TreeSet의 정렬의 기준을 변경하기 위해서 별도의 클래스를 매번 정의해야하는 것일까요?<br/>

이런 것을 해결하기 위해 정의된 것이 Comparator\<T\> 인터페이스 입니다.<br/>

```java
interface Comparator<T>{
    int compare(T obj1, T obj2);
    boolean equals(Object obj);
}
```

위의 인터페이스 중에서 equals 메서드는 신경쓰지 않아도 됩니다.<br/>
해당 인터페이스를 구현하는 모든 클래스는 Object 클래스를 상속하기 때문에<br/>
Object 클래스의 equals 메서드 위의 equals 메서드를 구현하는 꼴이 됩니다.<br/>

즉 우리는 compare 메서드만 신경쓰면 됩니다.<br/>
compare 메서드의 구현 방법은 compareTo 메서드의 구현방법과 유사합니다.<br/>
obj1이 크면 양수, obj2가 크면 음수, 같으면 0을 반환합니다.<br/>
크고 작음의 기준은 프로그래머가 정의합니다.<br/>

예제를 보겠습니다.<br/>

```java
import java.util.TreeSet;
import java.util.Iterator;
import java.util.Comparator;

class StrLenComparator implements Comparator<String>{
    public int compare(String str1, String str2){
        if(str1.length()>str2.length()){
            return 1;
        }else if(str1.length()<str2.length()){
            return -1;
        }else{
            return 0;
        }

        // return str1.length()-str2.length();
    }
}

class IntroComparator{
    public static void main(String[] args){
        TreeSet<String> tSet = new TreeSet<String>(new StrLenComparator());

        tSet.add("Orange");
        tSet.add("Apple");
        tSet.add("Dog");
        tSet.add("Individual");

        Iterator<MyString> itr = tSet.iterator();
        while(itr.hasNext()){
            System.out.println(itr.next());
        }
    }
}
```
