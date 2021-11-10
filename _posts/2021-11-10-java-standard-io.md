---
title: "Java(57) - 자바 입출력 스트림"
excerpt: "Java IO Stream"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-11-10 16:59:00"
last_modified_at: "2021-11-10 17:30:00"
---

## 1. 표준 입출력

- System 클래스의 표준 입출력 멤버

```java
public class System{
    public static PrintStream out; // 표준 출력 스트림
    public static InputStream in; // 표준 입력 스트림
    public static PrintStream err; // 표준 에러 스트림
}
// 전부 static 즉, 생성하지 않고 가져다 사용
```

## 2. System.in 사용하여 입력받기

System.in은 InputSteam

- 한 바이트 씩 읽어들임
- 한줄과 같은 여러 바이트로 된 문자를 읽기 위해서는,<br/>
  InputStreamReader와 같은 보조 스트림을 사용해야함.<br/>
  (멀티 바이트를 읽는 애들)

## 3. Scanner 클래스

- java.util 패키지에 있는 입력 클래스
- 문자 뿐 아니라 정수, 실수 등 다양한 자료형을 읽을 수 있음
- 생성자가 다양하여 여러 소스로 부터 자료를 읽을 수 있음

## 4. Console 클래스

- System.in을 사용하지 않고 콘솔에서 표준 입출력이 가능
- 이클립스와는 연동되지 않는다
- Console 클래스의 메서드
  1. String readLine() : 문자열을 읽습니다.
  2. char[] readPassward() : 사용자에게 문자열을 보여주지 않고 읽습니다.
  3. Reader reader() : Reader 클래스를 반환합니다.
  4. PrintWriter writer() : PrintWriter 클래스를 반환합니다.
