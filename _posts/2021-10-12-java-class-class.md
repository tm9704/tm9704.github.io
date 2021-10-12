---
title: "Java(34) - Class 클래스"
excerpt: "Java Class Class"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-12 13:20:00"
last_modified_at: "2021-10-12 13:50:00"
---

## 1. Class 클래스

자바의 모든 클래스와 인터페이스는 컴파일 후 class 파일로 생성됩니다.<br/>
(.class => byte코드 상태)<br/>

.class 파일에는 객체의 정보(컴파일된 코드) 즉, 멤버변수와 메서드, 생성자등이 포함되어 있습니다.<br/>

Class 클래스는 컴파일된 class 파일에서 객체의 정보를 가져올 수 있습니다.<br/>
(객체의 정보를 가져올 수 있는 여러 메서드들이 제공됨)<br/>

즉 Class 클래스는 컴파일된 class 파일을 로드하여 객체를 동적 로드하고,<br/>
정보를 가져오는 메서드를 제공합니다.<br/>

그러나 우리가 Class 클래스를 이용해서 코딩할 일은 그렇게 많지 않습니다.<br/>
이미 우리 local PC에는 우리가 사용하고자 하는 라이브러리들이 있고, 그 라이브러리들의 자료형을<br/>
대부분을 알고 있는 상태입니다.<br/>

예를 들면 String을 쓸 때 String을 쓰지 String클래스의 Class 클래스를 가져다 쓰진 않습니다.<br/>

그래서 쓰이는 경우는 local에 자료형이 없는경우, 해당 모듈이 없는 경우나<br/>
동적 로딩할 때 많이 사용합니다.<br/>

## 2. Class 클래스 가져오기

간단한 예시를 통해 보겠습니다.<br/>

1. String s = new String();<br/>
   Class c = s.getClass();<br/>
   (.getClass() 메서드는 Object클래스의 메서드)<br/>
2. Class c = String.Class;<br/>
3. Class c = Class.forName("java.lang.String");<br/>
   (동적 로딩)<br/>

위의 3번 코드의 경우 "java.lang.String"이 클래스의 fullname입니다.<br/>
해당 fullname을 가진 클래스가 로딩이 됩니다.<br/>
해당 statement가 수행될 때 로딩을 합니다.<br/>

우리가 대부분의 컴파일을 할 때, 우리가 지금 이 코드에서(모듈이든, 프로젝트이든)<br/>
이 시스템에서 어떤 애들을 가져다 쓸건지가 컴파일 파일에 거의 바인딩이 됩니다.<br/>
(정적로딩 static으로 바인딩이 됨)<br/>

Class.forName은 run time때 바인딩이 됩니다.<br/>
(동적로딩)<br/>
해당 이름을 가진 클래스가 local에 있면 그 때 그 이름의 클래스를 가져다 사용이 가능합니다.<br/>
fullname은 변경이 가능합니다.<br/>
(Inteager, Double..)<br/>

우리가 java에서 JDBC같은 걸 쓴다라고 할 때,<br/>
JDBC를 사용하려면 여러 JDBC 관련 DB 라이브러리가 있을 수 있습니다.<br/>
(Oracle, MSSQL, MySQL..)<br/>

이런 라이브러리들을 모두 static하게 링크해서 컴파일할 수는 없습니다.<br/>
만약 모두 static하게 링크를 하게 된다면 너무 많은 라이브러리를 링크한 상태에서 컴파일을 해야합니다.<br/>

이럴 필요없이 라이브러리들을 install해 놓은 상태에서 우리가 필요할 때 부를 수 있게끔<br/>
fullname을 바꿔주면 됩니다.<br/>
(이 프로그램이 oracle하고 연결이 될거다 하면 java.lang.oracle라이브러리 이름 이렇게 해주면 된다)<br/>

즉 동적로딩의 장점은 run time 때 그때그때 상황에 맞게<br/>
우리가 원하는 라이브러리, 클래스를 매칭시킬 수 있습니다.<br/>

단점은 오타가 날 경우 로딩되다가 런타임 때 프로그램이 죽을 수 있습니다.<br/>

## 3. reflection 프로그래밍

Class 클래스로부터 객체의 정보를 가져와 프로그래밍 하는 방식입니다.<br/>
즉 Class 클래스를 사용하여 클래스의 정보(생성자, 변수, 메서드)를 알 수 있고,<br/>
인스턴스를 생성하고, 메서드를 호출하는 방식의 프로그램입니다.<br/>

로컬(같은 메모리공간, process가 같은 것)에 객체가 없고, 자료형을 알 수 없는 경우<br/>
유용한 프로그래밍입니다.<br/>
자신의 로컬에 그 코드, 데이터 타입이 있다고 하면 굳이 사용하지 않습니다.<br/>

로컬에 객체가 없거나, 자료형을 알 수 없는 어떤 객체가 있다고 했을 때<br/>
그 객체의 정보를 꺼내와서 프로그래밍 하는게 reflection 프로그래밍 입니다.<br/>

java.lang.reflect 패키지 안에 여러 클래스들하고 메서드들과 같이 연동해서 많이 사용합니다.<br/>

## 4. 예시

우선 Class 클래스를 가져오는 방법을 보겠습니다.<br/>

```java
public class ClassTest {

	public static void main(String[] args) throws ClassNotFoundException {
        Class c1 = String.class;

        String str = new String();
        Class c2 = str.getClass();

		Class c3 = Class.forName("java.lang.String");
	}
}
```

해당 코드에서 오류가 없다면 잘 가져왔다는 뜻입니다.<br/>

다음은 동적로딩에 대한 예시입니다.<br/>

```java
public class ClassTest {

	public static void main(String[] args) throws ClassNotFoundException {
        Class c3 = Class.forName("java.long.String");
        //클래스의 정보를 가져옴

        Constructor[] cons = c3.getConstructors();
        for(Constructor con : cons){
            System.out.println(con);
        }
	}
}
```

위 코드는 String 클래스의 생성자 정보를 출력합니다.<br/>
그러나 String은 String str = new Stirng()을 하면 Constructor나<br/>
String 클래스에 관한 여러 정보가 확인이 가능하기 때문에 굳이 사용하지 않습니다.<br/>

로컬에 해당 모듈이나 프로젝트이 없거나 정확한 데이터 타입을 모를 때 사용합니다.<br/>

```java
public class ClassTest {

	public static void main(String[] args) throws ClassNotFoundException {
        Class c3 = Class.forName("java.long.String");
        //클래스의 정보를 가져옴

        Constructor[] cons = c3.getConstructors();
        for(Constructor con : cons){
            System.out.println(con);
        }

        System.out.println();

        Method[] methods = c3.getMethods();
        for(Method method : methods){
            System.out.println(method);
        }
	}
}
```

위 코드는 메소드 정보를 가져오는 코드입니다.<br/>
Method나 Constructor는 java.lang.reflect 패키지 안에 존재합니다.<br/>

```java
package classex;

class Person {
	private String name;
	private int age;

	public Person() {}

	public Person(String name) {
		this.name = name;
	}

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}

	public String toString() {
		return name;
	}
}

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

public class ClassTest {

	public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, SecurityException, IllegalArgumentException, InvocationTargetException {

		Person person = new Person("james");
		System.out.println(person);

		Class c1 = Class.forName("classex.Person");
		Person person1 = (Person)c1.newInstance(); //newInstance는 객체 생성
        //newInstance가 호출해주는건 해당 클래스의 기본 생성자
		System.out.println(person1);
        //결과는 null peron1은 이름을 지정해주지않음

        //이름 넣는 생성자 생성하는 법
        //일단 생성자를 가져와야함.
        //생성자에 들어가는 매개변수의 타입을 알아야함.
		Class[] parameterTypes = {String.class};
		Constructor cons = c1.getConstructor(parameterTypes);

        //위에서 생성자를 가져왔으면, 그 생성자로부터 newInstance로 객체 생성이 가능
        //객체를 생성할때 줄 매개변수가 있어야함.
		Object[] initargs = {"KIMYOUSIN"};
		Person personLee = (Person)cons.newInstance(initargs);
		System.out.println(personLee);
        //로컬에 타입이 있다면 위에처럼 할 필요는 없음

//		Class c2 = person.getClass();
//		Person p = (Person)c2.newInstance();
//		System.out.println(p);
//
//		Constructor con3 = c2.getConstructor(parameterTypes);
	}

}
```

위 코드는 지금 정확히 알 필요는 없고 중요한건 동적로딩이 가능하다.

## 5. forName() 메서드와 동적로딩

Class 클래스는 static 메서드입니다.<br/>

동적로딩이란<br/>
컴파일시에 데이터 타입이 모두 binding되어 자료형이 로딩되는 것을 의미합니다.<br/>
정적로딩(static loading)이 아니라 실행 중에(runtime) 데이터 타입을 알고 binding 되는 방식입니다.<br/>

실행시에 로딩되므로 경우에 따라 다른 클래스가 사용될 수 있어 유용합니다.<br/>

컴파일 타임에 체크할 수 없으므로 해당 무자열에 대한 클래스가 없는 경우<br/>
예외(ClassNotFoundException)가 발생할 수 있습니다.<br/>

즉 만약 10개 중에 1개가 돌아가야 하는데, 처음부터 10개를 죄다 변수로 선언하는게 아니라,<br/>
필요시에 10개 중 1개만 부르고 또 필요할 때 부르는 것입니다.<br/>

## 6. 동적로딩 vs 정적로딩

1. 동적로딩<br/>
   프로그램을 실행할 때, 필요시 마다 동적으로 메모리를 생성하고, 필요없는 메모리는<br/>
   자동으로 메모리에서 해제시킨다.<br/>
2. 정적로딩<br/>
   프로그램을 실행할 때 모든 실행파일이 메모리에 로드된다.<br/>

추가 설명<br/>
클래스 로더란 '.class' 바이트 코드를 읽어 들여 class 객체를 생성하는 역할을 담당한다. 즉, 클래스 로더는 클래스가<br/> 요청될 때 파일로부터 읽어 메모리로 로딩하는 역할을 하며 자바 가상 머신의 중요한 요소 중 하나다.

- 로딩 : 클래스 파일을 바이트 코드로 읽어 메모리로 가져오는 과정
- 링크 : 가장 복잡한 과정으로, 읽어본 바이트 코드가 자바 규칙을 따르는지 검증하고,<br/>
  클래스에 정의된 필드, 메소드, 인터페이스들을 나타내는 데이터 구조를 준비하며<br/>
  그 클래스가 참조하는 다른 클래스를 로딩한다.<br/>
- 초기화 : 슈퍼 클래스 및 정적 필드를 초기화한다.
