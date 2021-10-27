---
title: "Java(50) - 자바 Collection 인터페이스 보충"
excerpt: "Java Collection Interface"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-26 14:35:00"
last_modified_at: "2021-10-26 15:30:00"
---

Collecton\<E\> 인터페이스를 구현하는 제네릭 클래스들은 모두 인스턴스를<br/>
저장의 대상으로 삼습니다.<br/>
(인스턴스의 참조 값이 저장의 대상)<br/>

## 1. List<E> 인터페이스 (ArrayList<E>, LinkedList<E>)

List\<E\> 인터페이스를 구현하는 제네릭 클래스들은<br/>

- 동일한 인스턴스의 중복 저장을 허용
- 인스턴스의 저장 순서가 유지

위 두개의 특성을 공통으로 지닙니다.<br/>

List\<E\> 인터페이스를 구현하는 대표적인 제네릭 클래스는<br/>
ArrayList\<E\>와 LinkedList\<E\>입니다.<br/>

사용방법은 거의 동일하나, 데이터 저장방식에 차이가 있습니다.<br/>
우선 ArrayList\<E\>의 예제를 보겠습니다.<br/>

```java
import java.util.ArrayList; //제네릭 클래스라도 클래스의 명만 명시

class IntroArrayList{
    public static void main(String[] args){
        ArrayList<Integer> list = new ArrayList<Integer>();

        //데이터 저장
        list.add(new Integer(11));
        list.add(new Integer(22));
        list.add(new Integer(33));

        //데이터 참조
        System.out.println("1차 참조");
        for(int i = 0; i<list.size(); i++){
            System.out.println(list.get(i));
        }

        //데이터 삭제
        list.remove(0);
        System.out.println("2차 참조");
        for(int i = 0; i<list.size(); i++){
            System.out.println(list.get(i));
        }
    }
}
```

해당 코드에서는 Integer를 기반으로 ArrayList의 인스턴스를 생성하고 있습니다.<br/>
즉 생성된 인스턴스는 Integer 인스턴스의 저장소로 사용됩니다.<br/>

예제에서 보듯이 ArrayList 클래스는 배열과 상당히 유사합니다.<br/>

그러나 데이터의 저장을 위해서 인덱스 정보를 별도로 관리할 필요가 없으며,<br/>
데이터의 삭제를 위한 추가적인 코드의 작성이 필요가 없습니다.<br/>
또한 저장되는 인스턴스의 수에 따라 그 크기도 자동으로 증가합니다.<br/>

다음은 LinkedList\<E\> 클래스의 예제를 보겠습니다.<br/>

```java
import java.util.LinkedList; //제네릭 클래스라도 클래스의 명만 명시

class IntroLinkedList{
    public static void main(String[] args){
        LinkedList<Integer> list = new LinkedList<Integer>();

        //데이터 저장
        list.add(new Integer(11));
        list.add(new Integer(22));
        list.add(new Integer(33));

        //데이터 참조
        System.out.println("1차 참조");
        for(int i = 0; i<list.size(); i++){
            System.out.println(list.get(i));
        }

        //데이터 삭제
        list.remove(0);
        System.out.println("2차 참조");
        for(int i = 0; i<list.size(); i++){
            System.out.println(list.get(i));
        }
    }
}
```

해당 코드를 보면 인스턴스 생성부분을 제외하면 ArrayList 클래스의 예제와 동일합니다.<br/>
즉 두 클래스의 차이는 사용방법이 아닌 내부적으로 인스턴스를 저장하는 것입니다.<br/>

## 2. ArrayList, LinkedList 차이점

두 클래스의 가장 큰 차이점은 내부적으로 인스턴스를 저장하는 방식에 있습니다.<br/>

ArrayList의 경우 배열을 기반으로 합니다.<br/>
즉 내부적으로 배열을 이용해서 인스턴스의 참조 값을 저장합니다.<br/>

ArrayList 클래스의 특징으로는<br/>

- 저장소의 용량을 늘리는 과정에서 많은 시간이 소요된다.
- 데이터의 삭제에 필요한 연산과정이 매우 길다.
- 데이터의 참조가 용이해서 빠른 참조가 가능하다.

배열은 한번 생성 되면 길이를 변경시킬 수 없는 인스턴스이기 때문에, 용량을 늘리기 위해서는<br/>
새로운 배열 인스턴스를 생성하고 복사가 필요합니다.<br/>
데이터 삭제시 요소들을 앞으로 당기거나 뒤로 미는 추가적인 연산이 필요합니다.<br/>

즉 ArrayList 클래스의 경우 배열의 특성에 따라 용량을 늘리거나 삭제시 많은 연산이 수반될 수 있습니다.<br/>

다음은 LinkedList의 데이터 저장방식을 보기위한 예제입니다.<br/>

```java
class Box<T>{
    public Box<T> nextBox; //다른 Box<T>의 인스턴스 참조를 위한 변수
    T item;

    public void store(T item){this.item = item;}
    public T pullOut(){return item;}
}

class SoSimpleLinkedListTmp1{
    public static void main(String[] args){
        Box<String> boxHead = new Box<String>();
        boxHead.store("First String");

        boxHead.nextBox = new Box<String>();
        boxHead.nextBox.store("Second String");

        boxHead.nextBox.nextBox = new Box<String>();
        boxHead.nextBox.nextBox.store("Third String");

        Box<String> tempRef;

        //두 번째 박스에 담긴 문자열 출력 과정
        tempRef = boxHead.nextBox;
        System.out.println(tempRef.pullOut());

        //세 번째 박스에 담긴 문자열 출력 과정
        tempRef = boxHead.nextBox;
        tempRef = tempRef.nextBox;
        System.out.println(tempRef.pullOut());
    }
}
```

Box 클래스는 인스턴스를 담을 일종의 상자입니다.<br/>
해당 클래스를 이용해 인스턴스를 저장하고 배열을 대신합니다.<br/>

변수 nextBox는 상자를 연결하기 위한 참조변수입니다.<br/>
해당 변수를 이용해 Box의 인스턴스는 동일한 자료형의 인스턴스를 참조하게 됩니다.<br/>

item은 인스턴스의 저장을 위한 참조변수입니다.<br/>
즉 하나의 상자에는 하나의 인스턴스만 저장이 가능합니다.<br/>

해당 코드가 리스트 자료구조에 해당합니다.<br/>
LinkedList 클래스의 특징으로는<br/>

- 저장소의 용량을 늘리는 과정이 간단하다.
- 데이터의 삭제가 매우 간단하다.
- 데이터의 참조가 다소 불편하다.

가 있습니다.<br/>

## 3. Iterator를 이용한 인스턴스의 순차적 접근

Collection\<E\> 인터페이스에는 iterator라는 이름의 메서드가 정의되어 있습니다.<br/>

```java
Iterator<E> iterator()
```

해당 메서드는 반환형이 Iterator\<E\> 인데, 이것은 인터페이스의 이름입니다.<br/>
즉 iterator 메서드가 하는 일은,<br/>
**itertor 메서드가 호출되면 인스턴스가 하나 생성되는데, 해당 인스턴스는 Iterator\<E\> 인터페이스를**<br/>
**구현한 클래스의 인스턴스이며 iterator 메서드는 이 인스턴스의 참조값을 반환**<br/>
입니다.<br/>

메서드의 반환형이 Iterator\<E\>으로 선언되어 있는 상황에서, 우리는 Iterator\<E\> 인터페이스에<br/>
정의되어 있는 메서드 이상의 내용에 알 필요가 없습니다.<br/>
인터페이스에 정의된 메서드가 제공하는 기능만 활용하면 됩니다.<br/>

해당 인터페이스에 정의되어 있는 메서드로는<br/>

- boolean hasNext() : 참조할 다음 번 요소가 존재하면 true 반환
- E next() : 다음 번 요소를 반환
- void remove() : 현재 위치의 요소를 삭제

예제를 보겠습니다.<br/>

```java
import java.util.Iterator;
import java.util.LinkedList;

class IteratorUsage{
    public static void main(String[] args){
        LinkedList<String> list = new LinkedList<String>();
        list.add("Frist");
        list.add("Second");
        list.add("Third");
        list.add("Fourth");

        Iterator<String> itr = list.iterator();

        System.out.println("반복자를 이용한 1차 출력과 \"Third\" 삭제");
        while(itr.hasNext()){
            String curStr = itr.next();
            System.out.println(curStr);
            if(curStr.compareTo("Third") == 0){
                itr.remove();
            }
        }

        System.out.println("\n\"Third\" 삭제 후 반복자를 이용한 2차 출력 ");
        itr = list.iterator();
        while(itr.hasNext()){
            System.out.println(itr.next());
        }
    }
}
```

iterator 메서드가 호출될 때 생성되는 인스턴스를 반복자라고 합니다.<br/>

추가적으로 Collection 인터페이스는 Interable 인터페이스를 상속합니다.<br/>
Iterable 인터페이스에 정의되어 있는 유일한 메서드가 iterator이며,<br/>
Collection 인터페이스에 정의되어 있는 iterator메서드는 인터페이스간 상속에 의한 것입니다.<br/>

## 4. 반복자 사용 이유

앞서 본 예제 IntroLinkedList, IteratorUsage의 데이터 참조방식만 보면<br/>
반복자의 사용에 큰 의미가 있는지 애매합니다.<br/>

반복자의 의미는 참조방식에 큰 의의가 있는것이 아닙니다.<br/>
반복자는 **컬렉션 클래스의 종류에 상관없이 동일한 형태의 데이터 참조방식을 유지**합니다.<br/>

예제의 LinkedList인터페이스의 경우 데이터 참조를 위해 정의된 메서드는 get() 메서드입니다.<br/>
List 인터페이스들은 저장순서가 유지되기 때문에 해당 메서드가 데이터참조에 어울리는 메서드입니다.<br/>

그러나 컬렉션 클래스들 중에는 데이터의 저장순서가 유지되지 않는 것도 있습니다.<br/>
이런 클래스들은 get이라는 메서드 자체가 정의되어 있지 않으며, 그렇기 때문에 반복자가 필요합니다.<br/>

또, 반복자는 저장된 데이터 전부를 참조할 때 매우 유용합니다.<br/>
예제를 하나 보겠습니다.<br/>

```java
import java.util.Iterator;
import java.util.HashSet;

class UsefulIterator{
    public static void main(String[] args){
        HashSet<String> set = new HashSet<String>();
        set.add("First");
        set.add("Second");
        set.add("Third");
        set.add("Fourth");

        Iterator<String> itr = set.iterator();

        System.out.println("반복자를 이용한 1차 출력과 \"Third\" 삭제");
        while(itr.hasNext()){
            String curStr = itr.next();
            System.out.println(curStr);
            if(curStr.compareTo("Third") == 0){
                itr.remove();
            }
        }

        System.out.println("\n\"Third\" 삭제 후 반복자를 이용한 2차 출력 ");
        itr = set.iterator();
        while(itr.hasNext()){
            System.out.println(itr.next());
        }
    }
}
```

위 예제는 IteratorUsage의 코드의 LinkedList\<String\>를 HashSet\<String\>으로 변경한<br/>
것입니다.<br/>

해당 코드에서는 get 메서드가 정의되어 있지 않습니다.<br/>
따라서 반복자를 기반으로 코드를 작성하지 않았다면, 코드의 상당 부분이 변경되어야 했을 것입니다.<br/>

## 5. 컬렉션 클래스를 이용한 int형 정수 여러개 저장

컬랙션 클래스를 이용해서 int형 정수를 열 개 저장하는 것은 생각보다 단순하지 않습니다.<br/>

기본 자료형을 기반으로 제네릭 인스턴스를 생성할 수 없기 때문입니다.<br/>

```java
ArrayList<int> arr = new ArrayList<int>();
LinkedList<int> link = new LinkedList<int>();
```

이런식으로 컬랙션 인스턴스의 생성이 불가능합니다.<br/>

이것을 해결하는 방법은 앞선 포스트에서 다룬 Wrapper 클래스와 Auto Boxing, Unboxing을 사용하면<br/>
가능합니다.<br/>

```java
import java.util.Iterator;
import java.util.LinkedList;

class PrimitiveCollection{
    public static void main(String[] args){
        LinkedList<Integer> list = new LinkedList<Integer>();
        list.add(10); //AutoBoxing
        list.add(20); //AutoBoxing
        list.add(30); //AutoBoxing

        Iterator<Integer> itr = list.iterator();

        while(itr.hasNext()){
            int num = itr.next(); //Auto Unboxing
            System.out.println(num);
        }
    }
}
```
