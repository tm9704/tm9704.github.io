---
title: "Java(46) - 내부 클래스 보충"
excerpt: "Java Inner, Local, Anonymous"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-23 13:44:00"
last_modified_at: "2021-10-20 14:30:00"
---

## 1. Inner, Nested 클래스

자바의 클래스 안에는 다음과 같이 다른 클래스의 정의를 포함할 수 있습니다.<br/>

```java
class OuterClass{
    ...
    class InnerClass{
        ...
    }
}
```

위 코드에서 바깥쪽에 정의된 클래스가 Outer클래스, 안쪽에 정의된 클래스를 Inner클래스라고 합니다.<br/>
그리고 해당 Inner클래스를 static으로 선언할 수 있는데,<br/>
이렇게 선언되는 클래스를 Static Inner 클래스 또는, Nested 클래스라고 합니다.<br/>

```java
class OuterClass{
    ...
    static class NestedClass{
        ...
    }
}
```

예시를 하나 보겠습니다.<br/>

```java
class OuterClassOne{
    OuterClassOne(){
        NestedClass nst = new NestedClass();
        nst.simpleMethod();
    }

    static class NestedClass{
        public void simpleMethod(){
            System.out.println("Nested Instance Method One");
        }
    }
}

class OuterClassTwo{
    OuterClassTwo(){
        NestedClass nst = new NestedClass();
        nst.simpleMethod();
    }

    private static class NestedClass{
        public void simpleMethod(){
            System.out.println("Nested Instance Method Two");
        }
    }
}

class NestedClassTest{
    public static void main(String[] args){
        OuterClassOne one = new OuterClassOne();
        OuterClassTwo two = new OuterClassTwo();

        OuterClassOne.NestedClass nst1 = new OuterClassOne.NestedClass();
        nst1.simpleMethod();
        //OuterClassTwo.NestedClass nst2 = new OuterClassTwo.NestedClass();
        //nst2.simpleMethod();
    }
}
```

위 코드에서 OuterClassOne에 정의된 NestedClass의 경우 Outer의 클래스 입장에서 Nested 클래스의 인스턴스<br/>
생성이 외부에 정의된 클래스의 인스턴스 생성과 크게 다르지 않습니다.<br/>

그러나 OuterClassTwo에 정의된 NestedClass의 경우 private로 선언이 되어있기 때문에,<br/>
Outer 클래스 내에서만 인스턴스의 생성이 가능한 클래스가 됩니다.<br/>

즉 Nested 클래스는 클래스 내부에 정의되기 떄문에 private으로 선언되면, 외부에서 인스턴스<br/>
생성이 불가능합니다.<br/>

또 다른 특징으로는 Nested 클래스는 Outer 클래스의 static 멤버에 직접 접근이 가능합니다.<br/>

다음으로 Inner클래스의 특징을 보겠습니다.<br/>
Inner 클래스의 인스턴스는 반드시 Outer 클래스의 인스턴스에 종속되어야 하기 때문에<br/>
Outer 클래스의 인스턴스 생성없이는 생성이 불가능합니다.<br/>

주목해야할 점은<br/>

- Outer 클래스의 인스턴스 생성 후에야 Inner 클래스의 인스턴스 생성이 가능하다.
- Inner 클래스 내에서는 Outer 클래스의 멤버에 직접 접근이 가능하다.
- Inner 클래스의 인스턴스는 자신이 속할 Outer 클래스의 인스턴스 기반으로 생성된다.

## 2. Inner 클래스의 사용

Inner 클래스와 Nested 클래스는 다음 장점들을 가집니다.<br/>

- 클래스들을 논리적으로 묶는 수단
- 클래스들을 논리적으로 묶다 보니, 캡슐화가 증가되는 효과
- 결과적으로 코드의 가독성이 향상되고, 유지보수성이 향상

즉 관계가 매우 긴밀한 두 개의 클래스가 있을 때, 이중 하나는 다른 하나의 Inner 클래스<br/>
또는, Nested 클래스로 정의합니다.<br/>
따라서 이 둘은 논리적으로 묶이며, 캡슐화의 증가, 가독성의 향상으로 이어집니다.<br/>

## 3. Local(지역) 클래스

지금 설명할 Local 클래스와 다음으로 설명할 Anonymous 클래스는 앞에서 공부한 Inner 클래스와<br/>
유사한 성격의 클래스입니다.<br/>

Inner 클래스의 성격을 그대로 유지합니다.<br/>

- Outer 클래스의 인스턴스 생성 후에 Inner 클래스의 인스턴스 생성이 가능
- Inner 클래스 내에서는 Outer 클래스의 멤버에 직접 접근이 가능
- Inner 클래스의 인스턴스는 자신이 속할 Outer 클래스의 인스턴스를 기반으로 생성

Local(지역) 클래스는 Inner 클래스와 유사합니다.<br/>
다만 메서드 내에 정의가 되고, 정의된 메서드 내에서만 인스턴스의 생성과 참조변수의 선언이 가능합니다.<br/>

```java
class OuterClass{
    ...
    public LocalClass createLocalClassInst(){
        class LocalClass{
            ...
        }

        return new LocalClass();
    }
}
```

위의 코드는 잘못 정의된 Local 클래스 예시입니다.<br/>
해당 코드의 경우 메소드 createLocalClassInst() 내에 정의된 Local 클래스입니다.<br/>
따라서 이 클래스의 인스턴스 생성은 정의된 메서드 내에서만 가능합니다.<br/>
그렇기 때문에 클래스를 정의한 후에 다음과 같이 인스턴스 생성하는 것이 가능합니다.<br/>

```java
return new LocalClass();
```

그러나 해당 코드의 문제점은 반환형에서 문제가 있습니다.<br/>
LocalClass의 인스턴스를 생성해서 반환해야 하므로 반환형이 LocalClass인 것은 당연하지만,<br/>
이렇게 반환형으로 Locals 클래스의 이름이 올 수는 없습니다.<br/>
Local 클래스는 메서드 내에서만 의미가 통하기 떄문입니다.<br/>

해당 상황을 해결하기 위해서 우리는 인터페이스를 활용합니다.

```java
interface Readable{
    public void read();
}

class OuterClass{
    private String myName;

    OuterClass(String name){
        myName = name;
    }

    public Readable createLocalClassInst(){
        class LocalClass implements Readable{
            public void read(){
                System.out.println("Outer inst name : " + myName);
            }
        }
        return new LocalClass();
    }
}

class LocalClassTest{
    public static void main(String[] args){
        OuterClass out1 = new OuterClass("First");
        Readable localInst1 = out1.createLocalClassInst();
        localInst1.read();

        OuterClass out2 = new OuterClass("Second");
        Readable localInst2 = out2.createLocalClassInst();
        localInst2.read();
    }
}
```

위 예제 처럼 Local 클래스가 정의되면, Local 클래스의 인스턴스 접근을 위해서 인터페이스가 함께<br/>
정의되는 것이 보통입니다.<br/>
(LocalClass가 Readable을 구현하기 때문에 반환형을 Readable로 선언 가능)<br/>

## 4. Local(지역) 클래스와 매개변수

Local 클래스는 메서드 내에 존재하기 때문에 매개변수와 지역변수에 접근이 가능합니다.<br/>
단, final로 선언되는 매개변수와 지역변수에만 접근이 가능합니다.<br/>

그 이유는 매개변수와 지역변수는 메서드를 빠져나가면 소멸이 되기 때문에, 컴파일러는 Local 클래스에서<br/>
접근하는 지역변수와 매개변수의 복사본을 Local 클래스가 항상 접근 가능한 메모리 영역에 만들어 둡니다.<br/>

때문에 컴파일러는 이들 변수를 final로 선언할 것을 강요하는 것입니다.<br/>
만약 Local 클래스 내에서 이 값을 변경시킨다면, 원본과 복사본의 값이 일치하지 않는 문제가 발생하기 때문에<br/>
final로 선언을 강요합니다.<br/>

## 5. Anonymous(익명) 클래스

Anonymous 클래스는 Local 클래스와 유사합니다.(즉 Inner과도 유사)<br/>
단, 차이점은 이름이 없다는 것입니다.<br/>

```java
interface Readable{
    public void read();
}

class OuterClass{
    private String myName;

    OuterClass(String name){
        myName = name;
    }

    public Readable createLocalClassInst(final int instID){
        return new Readable(){
            public void read(){
                System.out.println("Outer inst name : " + myName);
                System.out.println("Local inst ID : " + instID);
            }
        };
    }
}

class LocalClassTest{
    public static void main(String[] args){
        OuterClass out1 = new OuterClass("My Outer Class");
        Readable localInst1 = out1.createLocalClassInst(111);
        localInst1.read();

        Readable localInst2 = out2.createLocalClassInst(222);
        localInst2.read();
    }
}
```

위 코드에서 인터페이스의 경우 인스턴스의 생성이 불가능하다고 배웠지만,<br/>
우리가 인터페이스의 인스턴스를 생성하지 못하는 경우는 메서드가 완전히 정의되지 않아서 입니다.<br/>

그래서 자바는 위의 코드와 같이 메서드의 몸체를 채워 넣는 방식의 인스턴스 생성을 허용하고 있습니다.<br/>
