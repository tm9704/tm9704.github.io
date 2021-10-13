---
title: "Java(38) - String Builder, Buffer 보충"
excerpt: "Java StringBuilder, Buffer"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-13 15:00:00"
last_modified_at: "2021-10-13 15:30:00"
---

앞선 String 클래스 포스트에서 봤던 StringBuilder, Buffer 클래스에 대한<br/>
추가적인 설명을 한 포스트입니다.<br/>

## 1. StringBuilder 클래스

StringBuilder는 문자열의 저장 및 변경을 위한 메모리 공간(버퍼)을 내부에 지니는데,<br/>
해당 메모리 공간은 그 크기가 자동으로 조절된다는 특징이 있습니다.<br/>

해당 클래스에서 중요한 메서드가 두 개 있는데, 그걸 먼저 알아보겠습니다.<br/>

```java
class BuilderString{
    public static void main(String[] args){
        StringBuilder strBuf = new StringBuilder("ABC");
        strBuf.append(25);
        strBuf.append('Y').append(true);

        System.out.println(strBuf);
        strBuf.insert(2, false);
        strBuf.insert(strBuf.length(), 'Z');
        System.out.println(strBuf);
    }
}
```

각 행에 대한 설명을 하겠습니다.<br/>

1. StringBuilder strBuf = new StringBuilder("ABC");<br/>
   는 문자열 "AB"를 인자로 StringBuilder의 인스턴스를 생성하고 있습니다.<br/>
   StringBuilder 클래스는 java.lang 패키지로 묶여있기 때문에 원하는 순간에<br/>
   특별한 추가 선언 없이 인스턴스 생성이 가능합니다.<br/>
2. strBuf.append(25);<br/>
   append메소드를 호출하고 있습니다. append 메소드는 전달된 값을, StringBuilder의<br/>
   인스턴스가 저장하고 있는 문자열 데이터의 끝에 문자의 형태로 추가합니다.<br/>
   즉 'A', 'B'만 저장되어 있는 메모리 공간 끝에 문자 '2', '5'를 추가로 저장합니다.<br/>
3. strBuf.append('Y').append(true);<br/>
   문자 'Y'와 boolean형 데이터 true를 저장하고 있습니다.<br/>
   여기서 중요한 것은 append 메소드가 두 번 이어서 호출되었고, 그 아래 출력문 결과로<br/>
   append 메소드 호출 결과가 참조변수 strBuf에 그대로 반영됩니다.<br/>
   (AB25Ytrue)<br/>
4. strBuf.insert(2, flase);<br/>
   insert 메서드가 호출되고 있습니다.<br/>
   첫 번째 인자가 2인데, 이는 위치가 2인 지점에(실질적으로 3번째 위치),<br/>
   두 번째 인자를 문자형태로 저장하겠다는 의미입니다.<br/>
5. strBuf.insert(strBuf.length(), 'Z');<br/>
   StringBuilder의 length 메소드는 버퍼의 길이가 아닌, 저장된 문자의 개수 정보를 반환합니다.<br/>
   즉 이 문장은 문자 'Z'를 인자로 append 메소드를 호출하는 것과 같습니다.<br/>

마지막 출력문의 실행 결과는 ABflase25YtrueZ입니다.<br/>

여기서 중요한 것은 3번째 append메소드가 두 번 이어서 호출된 것과,<br/>
두 번째 append메소드의 호출 결과가 참조변수 strBuff에 반영됐다는 사실입니다.<br/>

이렇게 되기 위해서는 strBuff.append('Y')의 실행결과로 strBuff가 반환되어야 합니다.<br/>
이에 대한 예제를 보겠습니다.<br/>

```java
class SimpleAdder{
    private int num;
    public SimpleAdder(){num = 0;}

    public SimpleAdder add(int num){
        this.num += num;
        return this;
    }

    public void showResult(){
        System.out.println("add result : " + num);
    }
}

class SelfReference{
    public static void main(String[] args){
        SimpleAdder adder = new SimpleAdder();
        adder.add(1).add(3).add(5).showResult();
    }
}
```

위 코드의 경우 adder.add(1)메소드가 호출되면, add 메소드가 호출된 인스턴스의 참조 값이 반환됩니다.<br/>
사실상 참조변수 adder가 그대로 반환된 셈입니다.<br/>
따라서 adder.add(1)의 호출결과로 반환된 참조 값을 이용하여 add 메소드 호출이 다시 가능합니다.<br/>

위의 예시처럼 StringBuilder의 append 메소드 역시 add 메소드와 반환의 형태가 동일합니다.<br/>

## 2. StringBuilder 버퍼

StringBuilder는 버퍼의 크기를 스스로 확장합니다.<br/>
내부에 존재하는 버퍼는 자동으로 크기가 증가하도록 설계되어 있습니다.<br/>

그러나 필요에 따라서 그 크기 조절도 가능합니다.<br/>

```java
public StringBuilder(); // 16개의 문자 저장 버퍼 생성
public StringBuilder(int capacity) // capacity개의 문자 저장 버퍼 생성
public StringBuilder(String str) // str.length() + 16개의 문자 저장 버퍼 생성
```

첫번째 생성자는 빈 버퍼 상태의 StringBuilder 인스턴스를 생성할 때 사용됩니다.<br/>
생성자에 아무런 값을 전달하지 않으면, 16개의 문자를 저장할 수 있는 버퍼가 생성됩니다.<br/>
이는 초기의 버퍼 크기이고, 문자가 저장됨에 따라 자동으로 증가합니다.<br/>

두번째 생성자는 프로그래머가 인스턴스의 초기 버퍼 크기를 지정할 때 사용됩니다.<br/>

세번째 생성자는 문자열 정보를 저장하는 인스턴스 생성에 사용이 되는데, 이 때에도 문자열의 길이보다<br/>
16개의 문자를 더 저장할 수 있는 크기의 버퍼가 생성되어 문자열이 저장됩니다.<br/>

버퍼의 크기를 확장하는 작업은 많은 연산이 요구되는 작업이므로, 가급적이면 버퍼의 크기를<br/>
미리 할당하는 것이 성능에 도움이 됩니다.<br/>

## 3. StringBuffer

StringBuffer 클래스와 StringBuilder 클래스가 제공하는 메소드들은 세가지의 같은 점이 있습니다.<br/>

1. 메소드의 수(생성자 포함)
2. 메소드의 기능
3. 메소드의 이름과 매개변수 형

따라서 StringBuilder를 StringBuffer로 바꿔도 컴파일 및 실행이 가능합니다.<br/>

차이점은<br/>
StringBuffer는 쓰레드에 안전하지만, StringBuilder은 안전하지 않다는 것입니다.<br/>

이부분은 나중 쓰레드 포스트에서 자세히 다루겠습니다.<br/>
