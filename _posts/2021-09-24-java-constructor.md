---
title: "Java 생성자"
excerpt: "Java Constructor"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
last_modified_at: 2021-09-26T15:12:00
---

앞선 포스트에서 new 키워드 뒤에 Student()가 있었는데 이것을 생성자(constructor)이라고 합니다.<br/>
이번 포스트에서는 이 생성자에 대해 공부하겠습니다.<br/>

## 1. 생성자(constructor)

생성자는 객체 생성 시 new 키워드와 함께 호출됩니다.<br/>
(객체 생성 외에는 호출할 수 없습니다.)<br/>

주로 인스턴스를 초기화 하는 코드가 구현됩니다.(멤버 변수 초기화)<br/>
반환값이 없으며 상속되지도 않습니다.<br/>
생성자의 이름은 클래스 이름과 동일해야합니다.<br/>

예시를 보겠습니다.

```java
class Number{
    int num;

    public Number(){ //생성자
        num = 10;
    }

    public int getNumber(){
        return num;
    }
}

public class Constructor{
    public static void main(String[] args){
        Number num1 = new Number() //생성자 호출
        System.out.println(num1.getNumber());

        Number num2 = new Number() //생성자 호출
        System.out.println(num2.getNumber());
    }
}
```

위의 코드에서 Number 클래스에 있는 public Number()이 생성자입니다.<br/>

## 2. 기본 생성자(default constructor)

위의 예시에서는 생성자를 클래스 내부에 명시해주었습니다.<br/>
그러나 다른 포스트에서 우리는 생성자를 따로 생성하지 않고 객체를 생성했습니다.<br/>
이것이 가능한 이유는 바로 기본 생성자가 있기 때문입니다.<br/>

하나의 클래스에는 반드시 하나 이상의 생성자가 존재해야 합니다.<br/>
위의 코드처럼 프로그래머가 생성자를 구현해도 되지만, 따로 생성자를 구현하지 않는 경우 컴파일러가 생성자 코드를 넣어줍니다.<br/>
이것이 바로 기본 생성자(default constructor)입니다.<br/>

```java
class Student{
   int studnetNum;
   String name;

   /*
   public Student(){} => 컴파일러가 따로 생성해줌! 기본 생성자
   */

   public void showStudentInfo(){
       System.out.println(studnetNum + " " +name);
   }
}

public class StudentTest{
    public static void main(String[] args){
        Student studentLee = new Student();

        studentLee.studentNum = 1;
        studentLee.name = "Lee";

        studentLee.showStudentInfo();
    }
}
```

기본 생성자는 매개 변수가 없고 구현부가 없습니다.<br/>
만약 클래스에 다른 생성자가 있는 경우, 기본 생성자는 제공되지 않습니다.<br/>

## 3. 생성자 오버로딩(constructor overloading)

생성자는 한개만 생성할 수 있는 것이 아닙니다.<br/>
생성자를 두 개 이상 구현할 경우 생성자 오버로딩을 사용합니다.<br/>
사용하는 코드에서 여러 생성자 중 선택해서 사용할 수 있습니다.<br/>
private 변수도 생성자를 이용해서 초기화가 가능합니다.<br/>
(private 키워드는 나중 포스트에서 공부하겠습니다.)<br/>

```java
class Student{
   int studentNum;
   String studentName;


   public Student(){}

   public Student(int num, String name){
       studentNum = num;
       studentName = name;
   }


   public void showStudentInfo(){
       System.out.println(studentNum + " " + studentName);
   }
}

public class StudentTest{
    public static void main(String[] args){
        Student studentLee = new Student();
        Student studentKim = new Student(2, "Kim");

        studentLee.studentNum = 1;
        studentLee.studentName = "Lee";

        studentLee.showStudentInfo();
        studentKim.showStudentInfo();
    }
}
```

## 4. Java의 이름 규칙(Naming Rule)

생성자와 별개로 프로그래머들이 일반적으로 적용하는 이름 규칙에 대해 알아보겠습니다.<br/>

### 클래스 이름 규칙

1. 첫 문자는 대문자로 시작한다
2. 둘 이상의 단어가 묶여서 하나의 이름을 구성할 때, 새로 시작하는 단어는 대문자로 한다. (Camel notation)

### 메소드와 변수의 이름 규칙

1. 첫 문자는 소문자로 시작한다.
2. 둘 이상의 단어가 묶여서 하나의 이름을 구성할 때, 새로 시작하는 단어는 대문자로 한다.

### 상수의 이름 규칙

1. 모든 문자를 대문자로 구성한다.
