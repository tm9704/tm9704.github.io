---
title: "Java(55) - 자바 예외와 예외처리"
excerpt: "Java Exception Handling"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-11-01 14:05:00"
last_modified_at: "2021-10-27 15:00:00"
---

## 1. 오류란?

- 컴파일 오류 : 프로그램 코드 작성 중 발생하는 문법적 오류
- 실행 오류 : 실행 중인 프로그램이 의도하지 않은 동작을 하거나(bug),<br/>
  프로그램이 중지되는 오류(runtime error)

Java는 예외처리를 통해 프로그램의 비정상 종료를 막고 (프로그램이 다운되는 것을 방지)<br/>
로그(log)를 남길 수 있습니다.<br/>

## 2. 오류와 예외 클래스

1. 시스템 오류 (error)<br/>
   JVM에서 발생하며 프로그래머가 처리할 수 없습니다.<br/>
   동적 메모리를 모두 사용한 경우나, stack over flow등이 있습니다.<br/>

2. 예외 (Exception)<br/>
   프로그램에서 제어할 수 있는 오류를 말합니다.<br/>
   읽으려는 파일이 없는 경우나, 네트워크나 소켓연결 오류등이 있습니다.<br/>
   자바 프로그램에서는 예외에 대한 처리를 수행합니다.<br/>

   즉 예외는 프로그램의 실행 도중에 발생하는 문제상황을 뜻합니다.<br/>
   (프로그램의 논리에 맞지 않는 상황)<br/>
   컴파일 시 발생하는 문법적인 에러는 예외의 범주에 속하지 않습니다.<br/>

![에외와 오류](/images/java_exception.png)<br/>

if문을 이용한 예외처리를 보겠습니다.<br/>

```java
import java.util.Scanner;

class ExceptionHandleUseIf{
  public static void main(String[] args){
    Scanner keyboard = new Scanner(System.in);
    int[] arr = new int[100];

    for(int i = 0; i<3; i++>{
      System.out.print("피제수 입력 : ");
      int num1 = keyboard.nextInt();

      System.out.print("제수 입력 : ");
      int num2 = keyboard.nextInt();

      if(num2==0){
        System.out.println("제수는 0이 될 수 없습니다.");
        i-=1;
        continue;
      }

    System.out.println("연산결과를 저장할 배열의 인덱스 입력 : ");
    int idx = keyboard.nextInt();

    if(idx<0 || idx>99){
      System.out.println("유효하지 않은 인덱스 값입니다.");
      i-=1;
      continue;
    }

    arr[idx] = num1/num2;
    System.out.println("나눗셈 결과는 " + arr[idx]);
    System.out.println("저장된 위치의 인덱스는 " + idx);
  }
}
```

위 예제는 나눗셈 연산이 진행되는 과정에서 제수로 0이 입력되는 예외상황을 처리한 경우입니다.<br/>

그러나 if의 경우 예외처리 이외의 용도로 보통 사용되기 때문에,<br/>
프로그램 코드상에서 예외처리 부분을 구분하기가 쉽지 않습니다.<br/>

즉 if는 프로그램의 기능 완성을 위해 자주 사용되는 문장이므로 if문으로 예외처리를 하는 경우<br/>
프로그램의 주 흐름을 구성하는 코드와 예외상황을 처리하는 코드의 구분이 어려워집니다.<br/>

## 3. 예외 클래스

모든 예외 클래스의 최상위 클래스는 Exception 클래스입니다.<br/>

![예외 클래스](/images/java_exception_class.png)<br/>

## 4. try-catch문으로 예외 처리하기

위에서 설명했듯 if문으로 예외를 처리하는 방법은 바람직하지 않습니다.<br/>
그래서 등장한것이 try-catch 기반의 예외처리 방식입니다.<br/>

간단한 sudo코드를 보겠습니다.<br/>

```java
try{
    예외가 발생할 수 있는 코드 부분
}catch(처리할 예외 타입 e){
    try 블록 안에서 예외가 발생했을 때 수행되는 부분
}
```

try-catch는 하나의 문장이며, 둘 사이에 다른 문장이 삽입될 수 없습니다.<br/>

catch 문의 소괄호 안의 있는 예외 유형만 처리가 가능합니다.<br/>

try 블록 내에서 예외가 발생하면 예외가 발생한 부분의 아래는 수행하지 않고,<br/>
바로 catch 블록부분으로 넘어갑니다.<br/>

예시를 하나 더 보겠습니다.<br/>

```java
import java.util.Scanner;

class DividByZero{
  public static void main(String[] args){
    System.out.println("두 개의 정수 입력: ");
    Scanner keyboard = new Scanner(System.in);
    int num1 = keyboard.nextInt();
    int num2 = keyboard.nextInt();

    try{
      System.out.println("나눗셈 결과의 몫 : " + (num1/num2));
      System.out.println("나눗셈 결과의 나머지 : " + (num1%num2));
    }catch(ArithmeticException e){
      System.out.println("나눗셈 불가능");
      System.out.println(e.getMessage());
    }

    System.out.println("프로그램을 종료합니다");
  }
}
```

위 코드는 try-catch를 이용해서 나눗셈시 제수가 0이 들어가는 상황을 예외처리한 것입니다.<br/>

여기서 catch문으로 이동하는 상황을 설명하자면,<br/>

1. JVM이 0으로 나누는 예외상황이 발생했음을 인식
2. 해당 상황을 위해 정의된 ArithmeticException 클래스의 인스턴스를 생성
3. 이렇게 생성된 인스턴스의 참조 값을 catch영역에 선언된 매개변수에 전달

즉 catch는 메서드와 형태가 유사해, 인스턴스의 참조 값을 인자로 전달받을 수 있개 때문에,<br/>
JVM은 예외상황 발생시 참조 값의 전달을 통해 catch 영역으로 실행을 이동시킵니다.<br/>

위에서 나온 e.getMessage() 메서드는 예외상황을 알리기 위해 정의된 메서드이며,<br/>
모든 예외 클래스들이 상속하는 Throwable 클래스에 정의되어 있는 메서드입니다.<br/>

## 5. try-catch-finally 문으로 예외처리하기

어떤 리소스를 열었다거나 file을 열었다던가, socket을 열었다던가..<br/>
할 때 리소스 해제시 많이 사용합니다.<br/>

간단한 sudo코드를 보겠습니다.<br/>

```java
try{
    예외가 발생할 수 있는 코드 부분
}catch(처리할 예외 타입 e){
    try 블록 안에서 예외가 발생했을 때 수행되는 부분
}finally{
    에외 발생 여부와 상관 없이 항상 수행되는 부분 (return과는 관계가 없음)
    중간에 return을 하더라도 finally영역은 수행됨
    리소스를 정리하는 코드를 주로 사용
}
```

리소스 해제시 많이 사용하는 이유는,<br/>
try catch마다 리소스를 해제하는 것은 번거로우니까 finally에서 리소스를 해제합니다.<br/>

## 6. try-with-resource

- 리소스를 자동으로 해제하도록 제공해주는 구문
- 해당 리소스가 AutoCloseable을 구현한 경우 close()를 명시적으로 호출하지 않아도<br/>
  try 블록에서 오픈된 리소스는 정상적인 경우나 예외가 발생한 경우 모두 자동으로<br/>
  close()가 호출됨
- FileInputStream의 경우 AutoCloseable을 구현하고 있음<br/>

## 7. AutoClosealbe 인터페이스 사용하기

AutoCloseable 인터페이스를 구현한 클래스를 만들고 close()가 잘 호출되는지 확인해 보겠습니다.<br/>

```java
public class AutoCloseObj implements AutoCloseable{
    @Override
    public void close() throws Exception{
        System.out.println("리소스가 close() 되었습니다");
    } //close 메서드 구현
}
```

## 8. 향상된 try-with-resources 문
