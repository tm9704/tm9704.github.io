---
title: "Java(37) - 클래스 패스, 패키지"
excerpt: "Java Class Path, Package"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-13 13:35:00"
last_modified_at: "2021-10-13 14:00:00"
---

## 1. 클래스 패스

패스(path)는 경로의 의미를 지닙니다.<br/>
클래스 패스(class path)란 클래스의 경로(클래스가 존재하는 경로)를 뜻합니다.<br/>

우선 예시를 하나 보겠습니다.<br/>
해당 코드는 C드라이브 myclass라는 디렉토리에 저장되어있습니다.<br/>

```java
class AAA{
    public void printName(){
        System.out.println("AAA");
    }
}

class BBB{
    public void printName(){
        System.out.println("BBB");
    }
}

class ClassPath{
    public static void main(String[] args){
        AAA aaa = new AAA();
        aaa.printName();

        BBB bbb = new BBB();
        bbb.printName();
    }
}
```

디렉토리 구성은 myclass 파일안에<br/>
ClassPath.java, AAA.class, BBB.class, ClassPath.class가 있습니다.<br/>

해당 상태에서는 코드가 그대로 실행됩니다.<br/>

그러나 myclass파일에 서브 디렉토리로 mysubclass를 만들고 그 안에 AAA.class, BBB.class를<br/>
넣게 되면 클래스를 찾을 수 없다는 에러가 발생합니다.<br/>

그렇다면 우리는 mysubclass 디렉토리에서 클래스를 찾아라는 메세지를 전달해야합니다.<br/>

## 2. 환경변수(Environment Variable)

위 형태의 메세지를 전달하기 위해서는 우리는 환경변수를 이용해야합니다.<br/>

우리가 명령 프롬포트상에서 계산기 프로그램을 실행하기 위해서는<br/>
C:\myclass>calc.exe 라는 명령어를 입력하면 됩니다.<br/>

우리는 myclass 디렉토리 안에 계산기 프로그램이 들어있지 않다는 것을 알고있습니다.<br/>

명령 프롬포트 역시 일종의 프로그램입니다.(cmd.exe)<br/>
이렇게 다른 프로그램의 실행 능력을 지니는 명령 프롬포트는 환경변수 path를 참조하여<br/>
실행파일의 위치를 탐색합니다.<br/>

환경변수 path에 저장된 정보(문자열 형태의 정보)는<br/>
C:\myclass> echo %path%로 확인할 수 있습니다.<br/>

echo 명령어는 출력을 명하는 명령어 이며,<br/>
%path%는 환경변수 path의 내용을 출력하기 위한 구성입니다.<br/>

.;C:\WINDOWS\system32;C:\Program Files\Java\jdk버전\bin;<br/>
라는 정보가 담겨 있습니다.<br/>

역시서 .(dot)은 명령 프롬포트가 위치하는 현재 디렉토리를 의미합니다.<br/>
C:\WINDOWS\system32는 calc.exe가 저장되어 있는 위치입니다.<br/>
마지막 bin 디렉토리는 javac.exe, java.exe가 저장되어 있습니다.<br/>

그렇기 때문에 우리는 명령 프롬포트가 어디에 있든 자바 프로그램을 컴파일 및 실행 시킬 수 있습니다.<br/>

## 3. 환경변수에 classpath 설정하기

환경변수라는 것은 path하나만 존재하는 것이 아닙니다.<br/>
필요에 따라서 얼마든지 추가, 변경이 가능합니다.<br/>

그렇기 때문에 자바에서는 클래스의 검색 경로를 지정할 수 있도록 classpath라는 환경변수를 정의하고 있습니다.<br/>

클래스 패스를 확인하기 위해서는<br/>
C:\myclass> echo %classpath%를 사용하면 됩니다.<br/>

현재 디렉토리의 정보를 추가하기 위해서는<br/>
C:\myclass>set classpath=.;를 해주면 됩니다.<br/>
이처럼 set 명령어를 사용해 classpath를 설정합니다.<br/>

위 예제에서 mysubclass 경로를 추가하는 방법은 두가지가 있습니다.<br/>

1. 드라이브 명을 포함하는 완전 경로를 지정하는 방법<br/>
   C:\myclass> set classpath=.;C:\myclass\mysubclass;<br/>
   이 방법은 해당 위치와 완전히 동일한 위치에 저장해야 실행이 가능합니다<br/>
2. 상대경로(현재 디렉토리를 기준으로 경로 지정)<br/>
   C:\myclass> set classpath=.;.\mysubclass;<br/>
   명령 프롬포트의 현재 디렉토리와 현재 디렉토리의 서브 디렉토리이 mysubclass를 동시에<br/>
   classpath에 지정한 방법입니다.<br/>

위의 방법대로 환경변수를 설정하면 명령 프롬포트 창에서만 유효한 환경변수입니다.<br/>
즉 명령 프롬포트를 새로 실행할 때마다 다시 설정해야합니다.<br/>

매번 이렇게 해주는 방법말고도 제어판에서 설정하는 방법이 있지만,<br/>
이렇게 설정해줘야하는 컴퓨터가 많을 경우 매우 피로한 작업이 됩니다.<br/>

그래서 실무에서는 이러한 일련 반복작업을 배치파일, 스크립트 파일이라는 것을 구성해 해결합니다.<br/>

## 4. 배치 파일 생성

우선 배치파일안에서 해야할 일의 순서를 정리하겠습니다.<br/>

1. 예제 ClassPath.java를 컴파일
2. mysubclass 디렉토리 생성
3. AAA.class, BBB.class를 mysubclass로 이동 및 복사
4. mysubclass 디렉토리를 classpath로 추가
5. 실행

만들어진 배치파일의 예시는<br/>

- javac ClassPath.java
- md mysubclass
- copy AAA.class .\mysubclass\AAA.class
- copy BBB.class .\mysubclass\BBB.class
- del AAA.class
- del BBB.class
- set classpath=.;.\mysubclass
- java ClassPath
- pause

입니다.

## 5. 패키지(Packages)에 이해

어느 한 회사에서 A팀에서는 원의 둘레를 계산하는 기능을 개발,<br/>
B팀에서는 원의 넓이를 계산하는 기능을 개발한다고 가정합시다.<br/>

여기서 두 팀이 따로 개발을 했는데, 두 팀 모두 Circle이라는 이름의 클래스가 존재할 경우 문제가 발생합니다.<br/>

두 파일을 컴파일 하면, 총 네 개의 클래스 파일이 생성되는데, 두 개의 파일 이름이 중복됙 때문에,<br/>
하나의 디렉토리에서 컴파일 하는 것은 불가능합니다.<br/>

그러나 디렉토리 경로를 다르게하고, classpath도 적절히 설정한다고 해서 문제가 해결되는 것은 아닙니다.<br/>
main 메소드에서 Circle 클래스의 인스턴스를 각각 생성한다고 했을 때<br/>

```java
Circle c1 = new Circle(1.5); //넓이 구하기 위한 Circle 인스턴스
Circle c2 = new Circle(2.5); //둘레 구하기 위한 Circle 인스턴스
```

이렇게 하면 인스턴스가 제대로 생성되지 않습니다.<br/>

여기서 해결 방법은<br/>

1. 컴파일 완료된 동일한 이름의 클래스 파일을 서로 다른 디렉토리에 저장
2. 인스턴스 생성시, 저장되어 있는 디렉토리 정보를 표시해서 클래스를 구분

하는 것입니다.<br/>

이렇게 클래스 파일이 저장되어 있는 디렉토리 정보를 표시해서 인스턴스를 생성하기 위해 등장한 것이<br/>
패키지(Package)라는 문법적 요소입니다.<br/>

```java
orange.area.Circle c1 = new orange.area.Circle();
orange.perimeter.Circle c2 = new orange.perimeter.Circle();
```

여기서 orange를 패키지라고 합니다.<br/>
area와 perimeter는 서브 패키지라고 합니다.<br/>
(각각 디렉토리, 서브디렉토리 명이기도 합니다.)<br/>

즉 디렉토리도 나누고 패키지 선언이라는 것도 별도로 해야 위와 같은 방식으로 인스턴스 생성이 가능합니다.<br/>

## 6. 패키지 선언

패키지의 선언을 소스파일의 윗부분에 명시해서 컴파일이 가능합니다.<br/>
이렇게 컴파일을 하면, 해당 클래스를 어느 패키지에 묶겠다는 뜻이 됩니다.<br/>

Circle.java(orange.area 패키지 버전)<br/>

```java
package orange.area; // orange.area 패키지 선언

public class Circle{
    double rad;
    final double PI;

    public Circle(double r){
        rad = r;
        PI = 3.14;
    }
    public double getArea(){
        return (rad*rad)*PI;
    }
}
```

PackageCircle.java

```java
class PackageCircle{
    public static void main(String[] args){
        orange.area.Circle c1 = new orange.area.Circle(1.5);
        System.out.println(c1.getArea());
    }
}
```

이렇게 선언을 해주면 파일내에서 생성되는 모든 클래스를 orange.area패키지에 묶겠다는 선언이 됩니다.<br/>

## 7. 이름 없는 패키지

자바의 클래스는 반드시 특정 패키지에 포함되어야 합니다.<br/>

그러나 우리는 앞에서 대부분 패키지 선언을 해주지 않았습니다.<br/>
이렇게 별도의 패키지 선언이 없는 경우 이름 없는 패키지라는 것으로 묶입니다.<br/>

## 8. import 선언

6번의 예시에서 매번 저렇게 객체를 생성하는 것은 매우 귀찮아보입니다.<br/>
그래서 우리는 import선언을 사용할 수 있습니다.<br/>

```java
import orange.area.Circle;
```

이 코드의 의미는 orange.area 패키지의 Circle을 의미할 때에는 다 생략하고 Circle만 표시하겠다는 의미입니다.<br/>

import 선언이 되었다고 이전 방식으로 인스턴스 생성이 불가능한 것은 아닙니다.<br/>

만일 orange.area 패키지에 Circle 외에 여러 클래스가 묶여있다면,<br/>
import orange.area.\*;를 해주면 됩니다.<br/>

의미는 orange.area 패키지로 묶여있는 클래스의 인스턴스 생성에서는 패키지의 이름은 생략하고 클래스의 이름만<br/>
명시하겠다는 의미입니다.<br/>
