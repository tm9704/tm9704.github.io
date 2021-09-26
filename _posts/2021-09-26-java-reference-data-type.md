---
title: "Java 참조 자료형"
excerpt: "Java Reference Data Type"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
last_modified_at: 2021-09-26T15:50:00
---

앞선 포스트들에서 int, long, float등 기본 자료형들을 사용해왔습니다.<br/>
그러나 문자열 앞에 String 등을 사용했는데 이런 자료형에 대해 알아보겠습니다.<br/>

## 1. 참조 자료형(reference data type)

변수의 자료형에는 크게 2종류가 있습니다.

1. 기본 자료형: int, long, float, double 등
2. 참조 자료형: String, Date, Student 등

기본 자료형은 사용하는 메모리(몇 바이트인지)가 정해져 있는 자료형입니다.<br/>

참조 자료형은 클래스형으로 변수를 선언하는 것입니다

```java
    String name; //JDK에서 제공해주는 문자열을 위한 클래스 (나중에 자세히 다루겠습니다.)
```

참조 자료형은 기본 자료형과는 다르게 클래스에 따라 사용하는 메모리가 다릅니다.<br/>

## 2. 참조 자료형 직접 만들어 사용하기

예시로 보겠습니다.

```java
    class Student {
        int studentID;
        String studentname;

        Subject korea;
        Subject english;
        Subject math;

        public Student(int id, String name) {
            studentID = id;
            studentname = name;

            korea = new Subject();
            math = new Subject();
        }

        public void setKoreaSubject(String name, int score) {
            korea.subjectName = name;
            korea.score = score;
        }

        public void setMathSubject(String name, int score){
            math.subjectName = name;
            math.score = score;
        }

        public void showStudentScore() {
            int total = korea.score + math.score;
            System.out.println(studentname + "학생의 총점은 :" + total + "점 입니다.");
        }
    }

    class Subject {

	String subjectName;
	int score;
	int subjectID;
    }

    public class StudentTest {

        public static void main(String[] args) {
            Student studentLee = new Student(100, "Lee");
            studentLee.setKoreaSubject("국어", 100);
            studentLee.setMathSubject("수학", 95);

            Student studentKim = new Student(101, "Kim");
            studentKim.setKoreaSubject("국어", 80);
            studentKim.setMathSubject("수학", 99);

            studentLee.showStudentScore();
            studentKim.showStudentScore();
        }

    }
```

이런 식으로 학생 클래스에 있는 과목 이름, 과목 성적 속성을 과목 클래스로 분리하고,<br/>
Subject 참조 자료형 멤버 변수를 Student에 정의해서 사용합니다.<br/>

String은 new없이 문자열의 특성상 상수값을 바로 가져다 사용할 수 있지만, 다른 객체들은 생성을 해서 사용해야 합니다.<br/>
