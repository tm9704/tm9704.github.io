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

![에외와 오류](/images/java_exception.png)<br/>

## 3. 예외 클래스

모든 예외 클래스의 최상위 클래스는 Exception 클래스입니다.<br/>

![예외 클래스](/images/java_exception_class.png)<br/>

## 4. try-catch문으로 예외 처리하기

간단한 sudo코드를 보겠습니다.<br/>

```java
try{
    예외가 발생할 수 있는 코드 부분
}catch(처리할 예외 타입 e){
    try 블록 안에서 예외가 발생했을 때 수행되는 부분
}
```

try 블록 내에서 예외가 발생하면 예외가 발생한 부분의 아래는 수행하지 않고,<br/>
바로 catch 블록부분으로 넘어갑니다.<br/>

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
