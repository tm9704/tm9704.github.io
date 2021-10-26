---
title: "Java(48) - 자바 제네릭 보충"
excerpt: "Java Generics"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-26 12:20:00"
last_modified_at: "2021-10-26 13:00:00"
---

제네릭(generic)은 일반화의 뜻을 가지고 있습니다.<br/>
java에서 일반화의 대상은 자료형입니다.<br/>

## 1. 제네릭이 필요한 이유

우선 하나의 예시를 보겠습니다.<br/>

```java
class AppleBox{
    Apple item;
    public void store(Apple item){this.item = item;}
    public Apple pullOut(){return item;}
}

class OrangeBox{
    Orange item;
    public void store(Orange item){this.item = item;}
    public Orange pullOut(){return item;}
}
```

위 코드의 두개의 클래스는 클래스만 다를 뿐 변수와 메서드는 모두 동일합니다.<br/>
이를 해결한 하나의 코드를 보겠습니다.<br/>

```java
class Orange{
    int sugarContent;
    public Orange(int sugar){sugarContent = sugar;}
    public void showSugarContent(){System.out.println("당도 "+sugarContent);}
}

class FruitBox{
    Object item;
    public void store(Object item){this.item = item;}
    public Object pullOut(){return item;}
}

class ObjectBaseFruitBox{
    public static void main(String[] args){
        FruitBox fBox1 = new FruitBox();
        fBox1.store = (new Orange(10));
        Orange org1 = (Orange)fBox1.pullOut();
        org1.showSugarContent();

        FruitBox fBox2 = new FruitBox();
        fBox2.store("오렌지");
        Orange org2 = (Orange)fBox2.pullOut();
        org2.showSugarContent();
    }
}
```

해당 코드의 경우 fBox2를 선언한 부분부터 보기 시작하면,<br/>
우리는 과일이라는 오브젝트를 담기 위해 FruitBox라는 클래스를 정의했는데,<br/>
문자열을 삽입하고 있습니다.<br/>

그러나 해당 부분에서 컴파일 에러는 발생하지 않고,<br/>
마지막 줄 org2.showSugarContent()에서 예외가 발생합니다.<br/>

```java
class Orange{
    int sugarContent;
    public Orange(int sugar){sugarContent = sugar;}
    public void showSugarContent(){System.out.println("당도 "+sugarContent);}
}

class OrangeBox{
    Orange item;
    public void store(Orange item){this.item = item;}
    public Orange pullOut(){return item;}
}

class ObjectBaseFruitBox{
    public static void main(String[] args){
        FruitBox fBox1 = new FruitBox();
        fBox1.store = (new Orange(10));
        Orange org1 = fBox1.pullOut();
        org1.showSugarContent();

        FruitBox fBox2 = new FruitBox();
        fBox2.store("오렌지");
        Orange org2 = fBox2.pullOut();
        org2.showSugarContent();
    }
}
```

위의 코드는 첫번째 코드에서 OrangeBox를 기반으로 변경한 코드입니다.<br/>
해당 코드에서 fBox2.store("오렌지") 부분에서 컴파일 에러가 발생합니다.<br/>

이렇게 실행과정에서 발생하는 오류와 컴파일 단계에서 발생하는 오류의 성격은 매우 다릅니다.<br/>
컴파일 과정에서 오류가 발생한다면 실행 전 미리 오류가 발생하는 부분을 알 수 있기 때문입니다.<br/>

즉 두번째 코드의 경우 첫번째 코드보다 안전성이 보장되는 코드라고 할 수 있습니다.<br/>
그러나 두번째 처럼 할 경우 필요에 따라 둘 이상의 비슷한 클래스를 정의해야할 수 있습니다.<br/>
이런 단점을 없애기 위해 필요한게 제네릭입니다.<br/>

## 2. 제네릭 클래스 설계

앞에서 본 FruitBox를 기준으로 제네릭 클래스의 정의방법을 보겠습니다.<br/>

```java
class FruitBox{
    Object item;
    public void store(Object item){this.item = item;}
    public Object pullOut(){return item;}
}
```

해당 코드를 제네릭으로 변경하게 되면,<br/>

```java
class FruitBox<T>{
    T item;
    public void store(T item){this.item = item;}
    public T pullOut(){return item;}
}
```

해당 코드가 제네릭 클래스입니다.<br/>
(T는 type의 약자, 어떤 문자가 와도 크게 상관없음)<br/>

class FruitBox\<T\>의 의미는<br/>
해당 클래스의 인스턴스를 생성하려면 자료형 정보를 인자로 전달해야하고,<br/>
전달되는 인자는 클래스 내에 존자하는 T를 대체해서 인스턴스가 생성된다는 의미입니다.<br/>

인스턴스 생성과정에서 자료형 정보의 전달방법을 보겠습니다.<br/>

```java
FruitBox<Orange> orBox = new FruitBox<Orange>();
FruitBox<Apple> apBox = new FruitBox<Apple>();
```

다음은 제네릭을 이용한 전체 예제를 보겠습니다.<br/>

```java
class Orange{
    int sugarContent;
    public Orange(int sugar){sugarContent = sugar;}
    public void showSugarContent(){System.out.println("당도 "+sugarContent);}
}

class Apple{
    int weight;
    public Apple(int weight){this.weight = weight;}
    public void showAppleWeight(){
        System.out.println("무게 " + weight);
    }
}

class FruitBox<T>{
    T item;
    public void store(T item){this.item = item;}
    public T pullOut(){return item;}
}

class GenericFruitBox{
    public static void main(String[] args){
        FruitBox<Orange> orBox = new FruitBox<Orange>();
        orBox.store(new Orange(10));
        Orange org = orBox.pullOut();
        org.showSugarContent();

        FruitBox<Apple> apBox = new FruitBox<Apple>();
        apBox.store(new Apple(20));
        Orange org = orBox.pullOut();
        org.showAppleWeight();
    }
}
```

해당 코드에서 앞서 말한 단점들이 모두 해결됩니다.<br/>
\<Orange\>의 경우 Orange 또는 Orange를 상속하는 인스턴스의 참조 값만이 인자로 전달되고,<br/>
다른형태의 참조 값이 전달되면 컴파일 에러가 발생합니다.<br/>

## 3. 제네릭의 필요성

우리가 제네릭을 실무 환경에서 활용할 확률은 매우 적다고합니다.<br/>
제네릭을 고려한 클래스의 설계는 많은 시간과 노력, 비용이 필요하기 때문입니다.<br/>

우리가 제네릭을 공부하는 이유는<br/>
자바에서 제공하는 라이브러리 성격의 클래스를 활용하기 위함입니다.<br/>

자바에서 제공하는 클래스의 상당수는 제네릭 기반으로 설계되어 있으며,<br/>
이런 제네릭 기반의 라이브러리를 활용하려면 제네릭에 대한 이해가 필요합니다.<br/>

## 4. 제네릭 메서드의 정의와 호출

제네릭 메서드에 대한 첫번째 예제를 보겠습니다.<br/>

```java
class AAA{
    public String toString(){return "Class AAA";}
}

class BBB{
    public String toString(){return "Class BBB";}
}

class InstanceTypeShower{
    int showCnt = 0;

    public <T> void showInstType(T inst){
        System.out.println(inst);
        showCnt++;
    }

    void showPrintCnt(){System.out.println("Show count : "+showCnt);}
}

class IntroGenericMethod{
    public static void main(String[] args){
        AAA aaa = new AAA();
        BBB bbb = new BBB();

        InstanceTypeShower shower = new InstanceTypeShower();
        shower.<AAA>showInstType(aaa);
        shower.<BBB>showInstType(bbb);
        shower.showPrintCnt;
    }
}
```

위 예제에서 호출 방법을 간단하게 나타내는 법도 있는데,<br/>
shower.showInstType(aaa);<br/>
shower.showInstType(bbb);<br/>
처럼 자료형 전달에 사용되는 \<AAA\>,\<BBB\>가 사라졌습니다.<br/>

이는 컴파일러가 메서드 호출 시 전달되는 참조변수 aaa, bbb의 자료형을 근거로 판단이 가능하기 때문입니다.<br/>

제네릭 메서드에는 둘 이상의 자료형 매개변수를 선언하고 각각 다른 자료형 정보를 전달할 수 있습니다.<br/>
두번째 예제를 보겠습니다.<br/>

```java
class AAA{
    public String toString(){return "Class AAA";}
}

class BBB{
    public String toString(){return "Class BBB";}
}

class InstanceTypeShower2{

    public <T, U> void showInstType(T inst1, U inst2){
        System.out.println(inst1);
        System.out.println(inst2);
    }
}

class IntroGenericMethod2{
    public static void main(String[] args){
        AAA aaa = new AAA();
        BBB bbb = new BBB();

        InstanceTypeShower2 shower = new InstanceTypeShower2();
        shower.<AAA,BBB>showInstType(aaa,bbb);
        shower.showInstType(aaa,bbb); //일반적 호출 방식
    }
}
```

위 예제에서는 제네릭 메서드에 대해서만 봤지만, 제네릭 클래스에서도 동일하게 적용됩니다.<br/>

```java
class GenericTwoParam<T,U>{
    T item1;
    U item2;

    public void setItem1(T item){
        item1 = item;
    }

    public void setItem2(U item){
        item2 = item;
    }
}
```

## 5. 매개변수의 자료형 제한

기본적으로 제네릭 메서드 내에서는 제네릭으로 선언된 참조변수를 통해서 Object 클래스에<br/>
정의된 메서드만 호출이 가능합니다.<br/>

이는 모든 자료형을 기반으로 실행이 가능하도록 하기 위함인데,<br/>
때때로 이러한 제한이 불편할 수 있습니다.<br/>
이에 대한 예제를 보겠습니다.<br/>

```java
interface SimpleInterface{
    public void showYourName();
}

class UpperClass{
    public void showYourAncestor(){
        System.out.println("UpperClass");
    }
}

class AAA extends UpperClass implements SimpleInterface{
    public void showYourName(){
        System.out.println("Class AAA");
    }
}

class BBB extends UpperClass implements SimpleInterface{
    public void showYourName(){
        System.out.println("Class BBB");
    }
}

class BoundedTypeParam{
    public static <T> void showInstanceAncestor(T param){
        ((SimpleInterface)param).showYourName();
    }

    public static <T> void showInstanceName(T param){
        ((UpperClass)param).showYourAncestor();
    }

    public static void main(String[] args){
        AAA aaa = new AAA();
        BBB bbb = new BBB();

        showInstanceAncestor(aaa);
        showInstanceName(aaa);
        showInstanceAncestor(bbb);
        showInstanceName(bbb);
    }
}
```

제네릭 매개변수로는 Object 클래스에 정의된 메서드만 호출이 가능하기 때문에,<br/>
위 예제에서는 param을 강제 형번환하고 있습니다.<br/>

이렇게 하게되면, 안전하지 않은 코드가 됩니다.<br/>
SimpleInterface 인터페이스를 구현하지 않은 인스턴스, 또는 UpperClass를<br/>
상속하지 않은 인스턴스의 참조 값이 메서드에 전달되어도 컴파일 및 실행이 되기 때문에<br/>
앞에서 말한 제네릭의 장점이 모두 사라집니다.<br/>

그래서 Java에서는 제네릭 매개변수의 자료형에 제한을 둘 수 있는 문법적 요소를 제공합니다.<br/>

```java
interface SimpleInterface{
    public void showYourName();
}

class UpperClass{
    public void showYourAncestor(){
        System.out.println("UpperClass");
    }
}

class AAA extends UpperClass implements SimpleInterface{
    public void showYourName(){
        System.out.println("Class AAA");
    }
}

class BBB extends UpperClass implements SimpleInterface{
    public void showYourName(){
        System.out.println("Class BBB");
    }
}

class BoundedTypeParam2{
    public static <T extends SimpleInterface> void showInstanceAncestor(T param){
        param.showYourName();
    }

    public static <T extends UpperClass> void showInstanceName(T param){
        param.showYourAncestor();
    }

    public static void main(String[] args){
        AAA aaa = new AAA();
        BBB bbb = new BBB();

        showInstanceAncestor(aaa);
        showInstanceName(aaa);
        showInstanceAncestor(bbb);
        showInstanceName(bbb);
    }
}
```

실행 결과는 BoundedTypeParam과 동일합니다.<br/>

해당 코드에서는 showInstanceAncestor 메서드의 인자로 SimpleInterface를 구현하지 않는<br/>
인스턴스의 참조 값이 전달되거나,<br/>
showInstanceName 메서드의 인자로 UpperClass를 상속하지 않는 인스턴스의 참조 값이<br/>
전달될 경우 컴파일 에러가 발생하므로 자료형에 안전한 구조입니다.<br/>

## 6. 제네릭 메서드와 배열

Java에서는 배열도 인스턴스이므로 제네릭 매개변수 전달이 가능합니다.<br/>

```java
class IntroGenenricArray{
    public static <T> void showArrayData(T[] arr){
        for(int i = 0; i<arr.length; i++){
            System.out.println(arr[i]);
        }
    }

    public static void main(String[] args){
        String[] stArr = new String[]{
            "Hi!",
            "I'm so happy",
            "Java Generic Programming"
        };

        showArrayData(stArr);
    }
}
```

배열의 경우 위의 예제처럼 처리해야 배열의 특성을 잘 활용할 수 있습니다.<br/>

즉 배열의 제네릭 매개변수 선언을 다음과 같이 한 것만으로도 매개변수에 전달되는 대상을<br/>
배열의 인스턴스로 제한할 수 있습니다.<br/>

```java
T[] arr
```

그리고 이렇게 제한을 함으로 인해서 참조변수 arr을 통한 인스턴스 멤버 length의 접근 및,<br/>
[] 연산이 가능해집니다.<br/>

## 7. 제네릭 변수의 참조와 상속의 관계

```java
public void hiMethod(Apple param){...}
```

해당 메서드 정의에서 매개변수로 전달될 수 있는 대상의 범위는,<br/>
Apple 인스턴스 또는 Apple을 상속하는 인스턴스의 참조 값 입니다.<br/>

```java
public void ohMethod(FruitBox<Fruit> para){...}
```

해당 메서드에서는 매개변수에는 FruitBox\<Fruit\> 인스턴스의 참조 값이 전달 대상이 됩니다.<br/>

만약 Fruit라는 클래스를 Apple이 상속하는 경우 FruitBox\<Apple\> 인스턴스의 참조 값이<br/>
매개변수로 전달될 수 있을까요?<br/>

전달될 수 없습니다. Fruit와 Apple이 상속관계에 놓여있다고 해서 FruitBox\<Fruit\>와<br/>
FruitBox\<Apple\>이 상속관계에 놓이는 것은 압니다.<br/>

상속관계에 놓일려면 extends 키워드가 사용되야 하는데, 해당 경우 extends 키워드를 통해<br/>
명시되어있지 않기 때문입니다.<br/>

## 8. 와일드카드와 제네릭 변수의 선언

위의 상속 관계상에서 FruitBox\<Fruit\> 인스턴스의 참조 값도, FruitBox\<Apple\> 인스턴스<br/>
참조 값도 인자로 전달받을 수 있는 매개변수의 선언을 하려면 어떻게 해야 할까요?<br/>

Java에서는 이를 위해 와일드 카드를 이용한 자료형의 명시를 허용합니다.<br/>

와일드카드란, 이름 또는 문자열에 제한을 가하지 않음을 명시하는 용도로 사용되는 특별한 기호입니다.<br/>

Java는 클래스의 이름을 명시하는데 있어서 와일드카드로 사용되는 기호 '?'를 정의하고 있습니다.<br/>
이를 기반으로 변수 또는 매개변수가 선언될 수 있도록 합니다.<br/>

```java
FruitBox<? extends Fruit> box1 = new FruitBox<Fruit>();
FruitBox<? extends Fruit> box2 = new FruitBox<Apple>();
```

위의 \<? extends Fruit\>가 의미하는 바는 Fruit를 상속하는 모든 클래스입니다.<br/>
즉 자료형을 결정짓는 제네릭 매개변수 T에 Fruit 클래스를 포함하여,<br/>
Fruit를 상속하는 클래스면 무엇이든 올 수 있음을 명시합니다.<br/>

즉 위 코드에서 참조변수 box1, box2는<br/>

```java
new FruitBox<'Fruit 클래스, 또는 Fruit를 상속하는 클래스의 이름'>()
```

의 형태로 생성되는 인스턴스면 무엇이든 참조가 가능합니다.<br/>
예제를 하나 보겠습니다.<br/>

```java
class Fruit{
    public void showYou(){
        System.out.println("과일");
    }
}

class Apple extends Fruit{
    public void showYou(){
        super.showYou();
        System.out.println("사과");
    }
}

class FruitBox<T>{
    T item;
    public void store(T item){this.item = item;}
    public T pullOut(){return item;}
}

class IntroWildCard{
    public static void openAndShowFruitBox(FruitBox<? extends Fruit> box){
        Fruit fruit = box.pullOut();
        fruit.showYou();
    }

    public static void main(String[] args){
        FruitBox<Fruit> box1 = new FruitBox<Fruit>();
        box1.store(new Fruit());

        FruitBox<Apple> box2 = new FruitBox<Apple>();
        box2.store(new Apple());

        openAndShowFruitBox(box1);
        openAndShowFruitBox(box1);
    }
}
```

추가로 전달되는 자료형에 상관없이 FruitBox\<T\>의 인스턴스를 참조하려면,<br/>
FruitBox<?> box;<br/>
처럼 참조변수를 선언하면 됩니다.<br/>

하위 클래스를 제한하는 용도의 와일드카드는<br/>

```java
FruitBox<? super Apple> boundedBox;
```

처럼 extends 대신 super를 사용하면 됩니다.<br/>

extends 키워드를 쓴 경우는 '~을 상속하는 모든 클래스'이고<br/>
super 키워드를 쓴 경우는 '~이 상속하는 모든 클래스'입니다.<br/>

즉 위에 선언된 참조변수 boundedBox는 FruitBox\<T\>의 인스턴스를 참조하되,<br/>
T가 Apple 클래스 또는 Apple 클래스가 직간접적으로 상속하는 클래스인 경우에만 참조가 가능합니다.<br/>
