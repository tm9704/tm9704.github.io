---
title: "Java(25) - 메소드 오버라이딩"
excerpt: "Java Method Overriding"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-03 14:35:00"
last_modified_at: "2021-10-03 15:00:00"
---

이번 포스트에서는 메소드 오버라이딩에 대해 공부하겠습니다.<br/>

## 1. 오버로딩(Overloading)

우선 메소드 오버로딩에 대해 공부하겠습니다.<br/>

우리는 생성자 포스트에서 생성자 오버로딩에 대해 공부했습니다.<br/>
메소드 오버로딩 역시 이것과 동일합니다.<br/>

즉 같은 메소드 명을 지니지만, 매개변수의 선언형태가 다른 경우를 말합니다.<br/>

```java
class AAA{
  void isYourFunc(int n){...}
  void isYourFunc(int n1, int n2){...}
  void isYourFunc(int n1, double n2){...}
  ...
}
```

오버로딩이 된 메소드들을 호출할 경우 애매한 상황을 만들지 않는 것이 중요합니다.<br/>

```java
AAA inst = new AAA();
inst.isYourFunc(10, 'a');
//isYourFunc(int n1, n2)인지 isYourFunc(int n1, double n2)인지
```

위의 코드가 애매한 이유는 메소드 인자 전달 과정에서 발생하게 되는 자동 형 변환 때문입니다.<br/>
int 형 정수 10과 char형 문자 'a'를 인자로 전달받는 메소드는 정의되어 있지 않습니다.<br/>

그러나 char형 변수는 자동 형 변환이 되는데,<br/>
int, double 모두 char형으로부터의 형 변환이 가능합니다.<br/>
에러는 발생하지 않는데,<br/>
가장 가까운 위치에 놓여있는 자료형으로 변환이 되므로 int, int로 변환이 됩니다.<br/>
하지만 이렇게 사용하는 것은 좋지 않습니다.<br/>

또 주의할 점은 반환형이 다른 것은 오버로딩이 성립되지 않습니다.<br/>

```java
class BBB{
  int isYourFunc(int n){...}
  boolean isYourFunc(int n){...}
}
```

이런 식으로 코드를 짠 경우,<br/>
무엇을 호출해야 하는지 구분할 수 없기 때문입니다.<br/>

## 2. 하위 클래스에서 메소드 재정의 하기

하위 클래스에서 상위 클래스의 메소드를 재정의 하는 것을 메소드 오버라이딩이라고 합니다.<br/>

오버라이딩(overriding)은 상위 클래스에 정의된 메소드의 구현 내용이 하위 클래스에서<br/>
구현할 내용과 맞지 않는 경우 하위 클래스에서 동일한 이름의 메소드를 재정의 할 수 있습니다.<br/>

즉, 이미 상위클래스에 어떤 메소드가 있는데<br/>
그 메소드가 하위클래스에서 구현할 내용과 맞지 않다면<br/>
하위 클래스에서 똑같은 이름의 메소드를 또 만들 수 있습니다.<br/>

## 3. 오버로딩 vs 오버라이딩

오버로딩

- 메소드 이름 : 동일
- 매개변수 이름, 타입 : 다름
- 리턴 타입 : 상관 없음

오버라이딩

- 메소드 이름 : 동일
- 매개변수 이름, 타입 : 동일
- 리턴 타입 : 동일

## 4. @override 애노테이션(Annotation)

@override는 재정의된 메소드라는 의미로,<br/>
선언부가 기존의 메소드와 다른 경우 에러를 발생시킵니다.<br/>

Customer 클래스에서의 calcPrice

```java
public int calcPrice(int price) {
		bonusPoint += price * bonusRatio;
		return price;
	}
```

Customer을 상속받은 VIPCustomer 클래스에서의 calcPrice

```java
@Override
public int calcPrice(int price) {
	bonusPoint += price * bonusRatio;
	return price - (int)(price * salesRatio);
}
```

애노테이션은 안써도 상관은 없지만,<br/>
컴파일 오류를 막아주고 컴파일러에게 정보를 전달합니다.<br/>

## 5. 형 변환과 오버라이딩 메소드 호출

우선 예시를 보겠습니다.<br/>
(위의 코드를 참고합니다)<br/>

```java
Customer vc = new VIPCustomer();
vc.calcPrice(10000);
```

위 코드에서 calcPrice() 메소드는 어느 클래스에 있는 메소드가 호출될까요?<br/>
Java에서는 항상 인스턴스(여기서는 VIPCustomer)의 메소드가 호출됩니다.<br/>

즉 Customer의 것들만 참조가 가능하지만, 인스턴스의 메소드가 호출됩니다.<br/>

## 6. 가상메소드(Virtual Method)

위에서 VIPCustomer의 메소드가 호출된 이유는 가상 메소드의 원리 때문입니다.<br/>

가상 메소드 원리는 메소드의 이름과 메소드 주소를 가진 가상 메소드 테이블에서<br/>
호출된 메소드의 주소를 참조합니다.<br/>
(가상 메소드 테이블에는 해당 메소드에 대한 주소를 가지고 있습니다.)<br/>

메소드의 이름이 같다는 것은 주소가 같다는 의미입니다.<br/>
오버로딩할 때든, 컴파일 할 때 매개변수의 타입에 따라 메소드의 이름이 조금씩 바뀝니다.<br/>

![가상 테이블](/images/virtual_table.png)<br/>

위의 사진을 보면, 각 메소드 마다 주소와 함께 매핑됩니다.<br/>
메서드가 수행되는 수행코드의 위치가 있는데, 그 수행코드로 매개변수가 들어가고<br/>
명령이 수행되면 제어가 넘어가야합니다.<br/>

즉 결론은 가상메서드 원리는 메소드에 대한 매핑되는 주소값이 따로 있는 것입니다.<br/>

## 7. 메소드 호출, 실행 원리

- 메소드(함수)의 이름은 주소값을 나타냄
- 메소드는 명령어의 집합(set)이고 프로그램이 로드되면 메소드 영역(코드 영역)에
  명령어 set이 위치함
- 해당 메소드가 호출되면 명령어 set이 있는 주소를 찾아 명령어가 실행됨
- 이때 메소드에서 사용하는 변수들은 스택 메모리에 위치
- 따라서 다른 인스턴스라도 같은 메소드의 코드는 같으므로 같은 메소드 호출<br/>
  (쓰는 변수값이 다른 것)
- 인스턴스가 생성되면 변수는 힙 메모리에 따로 생성되지만, 메소드 명령어 set은
  처음 한번만 로드됨

## 8. 재정의된 매소드의 호출 과정

```java
Customer vc = new VIPCustomer();
```

일 때 <br/>

![재정의 메소드 호출](/images/overriding_method.png)<br/>

즉 같은 메소드 이름이지만 매핑되는 주소가 다릅니다.<br/>
이럴 때 실질적으로 호출되는 것은 타입에 기반되어 호출되는 것이 아니라<br/>
생성된 인스턴스 기반으로 호출되는 것이 가상 메서드 기법입니다.<br/>
