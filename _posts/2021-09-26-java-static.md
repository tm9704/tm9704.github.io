---
title: "Java(18) - Static"
excerpt: "Java Static"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-09-26 17:00:00"
last_modified_at: "2021-09-26 17:10:00"
---

인스턴스 변수는 인스턴스가 생성되었을 때 접근이 가능한 변수입니다.<br/>
클래스가 저의만 되어도 접근이 가능한 변수가 있는데, 클래스 변수 즉 static 변수에 대해 공부하겠습니다.<br/>

## 1. static 변수

클래스 변수, static 변수는 클래스가 정의만 되면 접근이 가능한 변수입니다.<br/>
즉 여러 인스턴스가 하나의 값을 공유할 필요가 있을 때 사용하는 변수입니다.<br/>

static 변수는 처음 프로그램이 로드 될 때 데이터 영역(상수 영역)에 생성됩니다.<br/>
인스턴스의 생성과 관계 없이 사용할 수 있으므로 클래스 이름으로도 참조가 가능합니다.<br/>
(인스턴스 이름으로도 참조가 가능합니다.)<br/>
프로그램이 끝나고 메모리를 해제할 때 소멸합니다.

```java
class Student{
    public static int serialNum = 1000;

    ...
}

public class StudentTest{
    Student.serialNum = 100;
}
```

static 변수는 클래스 변수, 정적 변수라고도 합니다.<br/>

static 변수와 인스턴스 변수의 차이점은 <br/>
인스턴스 변수는 각 인스턴스마다 참조하는 위치가 다르지만, static 변수는 데이터 영역에 위치한
동일한 메모리를 참조합니다.<br/>
(접근 제어자는 동일하게 작용합니다.)

## 2. static 메서드

static 메서드는 static 변수를 위한 기능을 제공하는 메서드 입니다.<br/>

static 메서드 내부에서는 인스턴스 변수를 사용할 수 없습니다.<br/>
static 메서드는 인스턴스의 생성과 상관없이 호출될 수 있는 메서드이므로,
아직 생성되지 않은 인스턴스 변수를 사용할 시 문제가 생길 수 있기 때문입니다.<br/>

static 메서드 역시 클래스 이름으로 참조가 가능합니다.

```java

class Student{
    public static int serialNum = 1000;

    ...
    public static void getSerialNum(){
        int i = 0; //지역 변수는 사용이 가능합니다.
        return serialNum;
    }
}

public class StudentTest{
    Student.serialNum = 100;
    Student.getSerialNum();
}

```

static 메서드는 클래스 메서드, 정적 메서드라고도 합니다.

- 주의할 점은 static은 큰 메모리를 사용하면 안됩니다. 프로그램이 로드되는 순간에 데이터 영역에 많은 메모리를 차지하기 떄문입니다.<br/>

## \* final 키워드

final키워드는 변수 앞에 붙어 변수를 상수화 시키는 키워드입니다.

```java
class Circle{
    final int PI = 3.14;
}
```

위의 코드에서는 선언과 동시에 초기화를 시켰지만, 초기화를 안시켰다면 딱 한번만 초기화가 가능합니다.
(나중에 더 추가)
