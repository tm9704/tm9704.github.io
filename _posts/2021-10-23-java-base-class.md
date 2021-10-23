---
title: "Java(48) - 자바 기본 클래스 보충"
excerpt: "Java Base Class"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-23 16:00:00"
last_modified_at: "2021-10-20 16:30:00"
---

## 1. Wrapper Class

Wrapper 클래스는 기본 자료형의 데이터를 감싸는 클래스입니다.<br/>
앞선 포스트에서 Wrapper 클래스들의 종류와 기능을 봤기 때문에 추가로 설명하지는 않겠습니다.<br/>

이번 포스트에서 공부할 것은 Wrapper 클래스에 가장 기본이 되는 두 기능입니다.<br/>

1. Boxing
2. Unboxing

Boxing은 기본자료형 데이터를 인스턴스화 시키는 작업을 가리킵니다.<br/>
즉 Boxing은 인스턴스의 생성에 의해 이루어집니다.<br/>

Unboxing은 인스턴스에 저장되어 있는 기본 자료형 데이터를 꺼내는 작업을 가리킵니다.<br/>
이는 Wrapper 클래스에 정의된 메서드 호출에 의해 이루어집니다.<br/>

```java
class BoxingUnboxing{
    public static void main(String[] args){
        Integer iValue = new Integer(10);
        Double dValue = new Double(3.14);

        System.out.println(iValue);
        System.out.println(dValue);

        iValue = new Integer(iValue.intValue() + 10);
        dValue = new Double(dValue.doubleValue() + 1.2);

        System.out.println(iValue);
        System.out.println(dValue);
    }
}
```

위 코드에서 Integer, Double 클래스를 통해 인스턴스를 생성하는 과정이 Boxing에 해당합니다.<br/>
intValue(), doubleValue() 메서드를 호출하는 부분이 Unboxing에 해당합니다.<br/>

여기서 주의할 점은,<br/>
Wrapper 클래스는 산술연산을 위해 정의된 클래스가 아니라는 점입니다.<br/>
따라서 Wrapper 클래스는 String과 마찬가지로 인스턴스에 저장된 값의 변경은 불가능합니다.<br/>
다만 위 예제처럼 증가된 값을 저장하는 새로운 인스턴스의 생성 및 참조만 가능합니다.<br/>

## 2. Auto Boxing & Auto Unboxing

자바 5.0 부터는 Boxing과 Unboxing이 필요한 상황에서 이를 자동으로 처리하기 시작했습니다.<br/>

```java
class AutoBoxingUnboxing{
    public static void main(String[] args){
        Integer iValue = 10; //auto boxing
        Double dValue = 3.14; //auto boxing

        System.out.println(iValue);
        System.out.println(dValue);

        int num1 = iValue; //auto unboxing
        double num2 = dValue; //auto unboxing
        System.out.println(num1);
        System.out.println(num2);
    }
}
```

int num1 = iValue에서 iValue.intValue()가 자동으로 호출되며,<br/>
double num2 = dValue에서는 dValue.doubleValue()가 자동으로 호출됩니다.<br/>

```java
class AutoBoxingUnboxing2{
    public static void main(String[] args){
        Integer num1 = 10;
        Integer num2 = 20;

        num1++;
        System.out.println(num1);

        num2+=3;
        System.out.println(num2);

        int addResult = num1 + num2;
        System.out.println(addResult);

        int minResult = num1 - num2;
        System.out.println(minResult);
    }
}
```

num1++은 num1 = new Integer(num1.intValue()+1);<br/>
num2+=3은 num2 = new Integer(num2.intValue()+3);<br/>
로 Unboxing과 Boxing이 동시에 이루어집니다.<br/>

## 3. BigInteger 클래스와 BigDecimal 클래스

short 그리고 int와 같은 정수 자료형의 문제점은 매우 큰 수의 표현이 불가능하고,<br/>
float와 double의 경우 정밀한 값의 표현이 불가능해 항상 오차가 존재합니다.<br/>
이러한 문제점을 해결하기 위해 BigInteger과 BigDecimal 클래스를 제공합니다.<br/>

BigInteger 클래스는 매우 큰 정수의 표현을 위한 클래스입니다.<br/>

```java
import java.math.*;

class SoBigInteger{
    public static void main(String[] args){
        System.out.println("최대 정수 : " + Long.MAX_VALUE);
        System.out.println("최대 정수 : " + Long.MIN_VALUE);

        BigInteger bigValue1 = new BigInteger("100000000000000000000");
        BigInteger bigValue2 = new BigInteger("-99999999999999999999");

        BigInteger addResult = bigValue1.add(bigValue2);
        BigInteger mulResult = bigValue1.multiply(bigValue2);

        System.out.println("큰 수의 덧셈결과 : " + addResult);
        System.out.println("큰 수의 곱셈결과 : " + mulResult);
    }
}
```

해당 코드에서 생성자를 통해 문자열을 전달하고 있습니다.<br/>
문자열로 전달하는 이유는 큰 수를 전달받을 수 있는 매개변수 선언이 불가능하므로,<br/>
해당 생성자가 존재할 수 없기 때문입니다.<br/>

BigDecimal 클래스는 오차 없는 실수의 표현을 위한 클래스입니다.<br/>

다음은 오차확인을 위한 예제입니다.<br/>

```java
class DoubleError{
    public static void main(String[] args){
        double e1 = 1.6;
        double e2 = 0.1;

        System.out.println(e1+e2);
        System.out.println(e1*e2);
    }
}
```

위 코드의 결과는 1.7000000000000002, 0.16000000000000003 입니다.<br/>
즉 일반적인 실수의 표현에는 오차가 존재합니다.<br/>

아래 예제는 BigDecimal 클래스를 활용한 예제입니다.<br/>

```java
import java.math.*;

class BigDoubleError{
    public static void main(String[] args){
        BigDecimal e1 = new BigDecimal(1.6);
        BigDecimal e2 = new BigDecimal(0.1);

        System.out.println(e1.add(e2));
        System.out.println(e1.multiply(e2));
    }
}
```

해당 결과에서도 오차가 발생합니다.<br/>
이유는 인스턴스 생성부분에서 있는데, 생성자에 실수를 매개변수를 저장하는 동시에<br/>
오차가 발생하기 때문입니다.<br/>

```java
import java.math.*;

class BigDoubleError{
    public static void main(String[] args){
        BigDecimal e1 = new BigDecimal("1.6");
        BigDecimal e2 = new BigDecimal("0.1");

        System.out.println(e1.add(e2));
        System.out.println(e1.multiply(e2));
    }
}
```

위 예제는 오차가 발생하지 않습니다.<br/>

## 4. Math 클래스와 난수의 생성

Math 클래스는 모든 멤버가 static으로 선언되어 있는,<br/>
수학 관련 기능을 제공하기 위해 정의된 클래스입니다.<br/>

기본적인 사용방법은 API문서를 활용합시다.<br/>

다음으로 난수 생성방법을 보겠습니다.<br/>

난수(Random Number)는 예측이 불가능한 수를 의미합니다.<br/>
자바에서는 이러한 난수의 생성을 위한 클래스를 별도로 제공하고 있습니다.<br/>

우선 java.util 패키지에 묶여있는 Random 클래스의 인스턴스를 생성합니다.<br/>

- boolean nextBoolean() : boolean형 난수 반환
- int nextInt() : int형 난수 반환
- long nextLong() : long형 난수 반환
- int nextInt(int n) : 0이상 n미만 범위의 int형 난수 반환
- float nextFloat() : 0.0이상 1.0미만 float형 난수 반환
- double nextDouble() : 0.0이상 1.0미만 double형 난수 반환

```java
import java.util.Random;

class RandomNumberGenerator{
    public static void main(String[] args){
        Random rand = new Random();

        for(int i = 0; i<100; i++){
            System.out.println(rand.nextInt(1000));
        }
    }
}
```

위 코드는 0이상 1000미만의 난수를 100개 생성하는 코드입니다.<br/>

다음은 씨드(Seed) 기반의 난수 생성 방법을 보겠습니다.<br/>

컴퓨터는 완벽한 난수를 생성하지 못합니다.<br/>
그래서 컴퓨터가 생성하는 난수를 'Pseudo-random number'라고 하고 이것은 가짜 난수입니다.<br/>

예제를 보겠습니다.<br/>

```java
import java.util.Random;

class PseudoRandom{
    public static void main(String[] args){
        Random rand = new Random(12);

        for(int i=0; i<100; i++){
            System.out.println(rand.nextInt(1000));
        }
    }
}
```

위 예제해서는 해당 예제의 위 예제와 다르게 Random생성자에 인자로 12가 들어가있습니다.<br/>
해당 인자를 씨드 값(seed number)라고 합니다.<br/>
이 값을 기반으로 컴퓨터는 난수를 생성합니다.<br/>

즉 이 값이 동일할 경우 생성되는 난수는 100% 동일하게 됩니다.<br/>
이걸 방지한 예제를 보겠습니다.<br/>

```java
import java.util.Random;

class PseudoRandom{
    public static void main(String[] args){
        Random rand = new Random(12);
        rand.setSeed(System.currentTimeMillis());

        for(int i=0; i<100; i++){
            System.out.println(rand.nextInt(1000));
        }
    }
}
```

여기서 setSeed 메서드는 씨드를 변경하는 메서드입니다.<br/>
씨드를 변경하기 위해 또 호출한 메서드가 바로<br/>
System.currentTimeMillis() 메서드입니다.<br/>

해당 메서드는 컴퓨터의 현재시간을 기준으로 1970년 1월 1일 자정 이후로 지나온 시간을<br/>
밀리 초(1/1000초)단위로 계산해서 반환하는 메서드입니다.<br/>

따라서 위 예제가 실행될 때 마다 해당 메서드가 반환하는 값은 달라지므로,<br/>
setSeed()메서드에 매번 다른 값이 전달됩니다.<br/>

맨 처음 봤던 예제에서는 생성자에 아무런 값도 전달하지 않았는데 매번 다른 난수가 생성된 이유는<br/>
생성자 내에서 씨드 값을 생성하여 설정하기 때문입니다.<br/>

```java
public Random(){
    this(System.currentTimeMillis());
}
```

위 코드가 씨드값을 생성하여 설정하는 방법입니다.<br/>
