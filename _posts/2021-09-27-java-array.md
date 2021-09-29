---
title: "Java(20) - 배열"
excerpt: "Java Array"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-09-27 15:15:00"
last_modified_at: "2021-09-27 15:30:00"
---

이번 포스트에서는 1차원, 다차원 배열에 대해 공부하겠습니다.<br/>

## 1. 배열이란

배열은 동일한 자료형의 순차적 자료 구조를 의미합니다.<br/>
(동일한 자료형을 순차적으로 관리하는 가장 기본적인 자료구조)<br/>

저희는 이전 시간에 반복문에 대해 공부했습니다.<br/>
반복문을 이용한 한 예시를 보겠습니다.

```java
import java.util.Scanner;

class ArrayTest{
    public static void main(String[] args){
        int fstHighScore = 0;
        int sndHighScore = 0;

        Scanner sc = new Scanner (System.in);

        System.out.println("점수 입력: ");
        int score1 = sc.nextInt();

        if(score1 >= fstHighScore){
            sndHighScore = fstHighScore;
            fstHighScore = score1;
        }else if(score1 < fstHighScore && score1 > sndHighScore){
            sndHighScore = score1;
        }

        System.out.println("점수 입력: ");
        int score2 = sc.nextInt();

        if(score2 >= fstHighScore){
            sndHighScore = fstHighScore;
            fstHighScore = score2;
        }else if(score2 < fstHighScore && score2 > sndHighScore){
            sndHighScore = score2;
        }

        System.out.println("점수 입력: ");
        int score3 = sc.nextInt();

        if(score3 >= fstHighScore){
            sndHighScore = fstHighScore;
            fstHighScore = score3;
        }else if(score3 < fstHighScore && score3 > sndHighScore){
            sndHighScore = score3;
        }

        System.out.println("A학점은 %d점 이상입니다.\n", sndHighScore);
    }

}
```

위의 코드는 학생 세명의 점수를 입력 받아서 A 학점의 기준이 되는 점수를 출력하는 코드입니다.<br/>
해당 코드의 문제점은 반복되는 코드의 수가 많다는 것입니다.<br/>
그렇다고 저 코드를 반복문으로 나타내기도 어렵습니다.<br/>

위 코드의 score1~3이라는 변수 선언을 좀 더 간단하게 나타낼 수 있다면 코드는 더욱 간단해질 것입니다.<br/>
이럴 때 사용하는게 배열입니다.<br/>

## 2. 배열 선언, 생성, 접근

배열 역시 인스턴스 입니다. 인스턴스 생성방법과 동일하게 선언하고 생성해야합니다.<br/>

```java
int[] arr = new int[10];
//int arr[] = new int[10];
```

위의 코드는 길이가 10인 정수형 배열 arr을 선언한 것입니다.<br/>
[]의 위치는 자료형 뒤쪽이나 배열명 뒤쪽에 적어주면 됩니다.<br/>
(arr은 배열명이자 참조변수)<br/>

이렇게 배열을 선언하고 나중에 배열에 값을 넣어줘도 상관 없지만, 배열을 선언과 동시에 초기화하는 방법이 있습니다.<br/>

```java
int[] arr = new int[]{1,2,3};
```

우선 첫번째 방법입니다. 위의 코드처럼 배열의 길이를 적어주는 대신 뒤에 중괄호안에 배열에 넣어줄 값을 적어주시면 됩니다.<br/>
(값의 개수만큼 길이가 잡힘)<br/>
이 방법의 경우 중괄호 앞에 new int[]는 생략이 가능합니다.<br/>

```java
int[] arr;
arr = new int[]{1,2,3};
//arr = {1,2,3}; 이건 불가능
```

두번째 방법은 위의 코드처럼 하는 것입니다. 선언과 동시에 초기화하는 방법과 다르게 new int[]는 생략이 불가능 합니다.<br/>

배열의 접근 방법은 매우 간단합니다.

```java
public static void main(String[] args){
    int[] arr = new int[7];
    arr[0] = 10; //배열의 첫 번째 요소에 10 저장
    arr[1] = 20; //배열의 두 번째 요소에 20 저장
    ...
}

```

위의 코드와 같이 arr이 참조하는 배열의 n번째 요소에 접근을 하려면 arr[n-1]을 이용하면 됩니다.

## 3. 배열의 특징

- 배열의 특징 첫번째는 메모리 구조에 있습니다.<br/>

  ![메모리 구조](/images/array.png)<br/>

  위의 그림과 같이 배열은 논리적인 위치와 물리적 위치의 순서가 동일합니다.<br/>
  (위 같은 경우 딱 4byte씩 떨어져서 주소가 생성)<br/>

  이와 반대되는 자료구조가 있는데 그것이 바로 linked-list입니다.<br/>
  linked-list에 대해서는 나중 포스트에서 깊게 설명하겠습니다.<br/>

- 배열의 특징 두번째는 배열은 연속된 자료구조이며 고정길이를 갖습니다(fixed length).<br/>
  중간에 빈공간이 있으면 안됩니다. 중간이 빈공간이 있을 경우 그 위치를 끝이라고 인식합니다.<br/>
  추가할 요소나 뺄 요소가 있다면, 배열의 길이를 늘리고 뒤로 밀어주거나 앞으로 당겨줘야합니다.<br/>

  배열의 길이가 더 필요한 경우에는 보통 다른 큰 배열(2배정도)을 하나 생성하고 요소들을 이동시켜줘야 합니다.<br/>

- 배열의 특징 3번째는 먼저 예시를 보겠습니다

  ```java
  int total = 1;
  double[] dArr = new double[5];

  dArr[0] = 1.1;
  dArr[1] = 2.1;
  dArr[2] = 3.1;

  for(int i = 0; i < dArr.length; i++){
      mtotal *= dArr[i];
  }
  ```

  위 코드를 출력하면 0이 나옵니다. 이유는 배열의 남은 길이에 아무런 값이 들어가지 않으면, 자동으로 0으로 초기화되기 떄문입니다.<br/>
  이 경우 .length를 쓰는 것보다 count라는 변수를 추가해 반복시 count값을 증가시켜서 써주면 됩니다.<br/>
  (실수의 경우 0.0으로 초기화)

## 4. 다차원 배열

다차원 배열은 2차원 이상의 배열을 뜻합니다.<br/>
보통 지도, 게임, 평면이나 공간을 구현할 때 사용합니다.<br/>

```java
int[][] arr = new int[2][3];
// 행 열              행  열
```

선언과 초기화, 사용방법은 1차원 배열과 다르지 않습니다.<br/>
차원 수에 맞게 []를 추가해주시면 됩니다.<br/>

주의해야 할 점은 1차원 배열과 다르게 논리적 구조와 물리적 구조는 동일하지 않습니다.<br/>
2차원 배열의 경우 두 줄로 배열의 메모리가 잡히는게 아니라 한 줄로 쭉 메모리가 잡힙니다.<br/>

int[][] arr = {\{1,2,3\},\{4,5,6\}};

```java
int row = arr.length; //행의 개수
int col = arr[0].length; //요소의 개수
```

위의 코드처럼 행의 개수를 알고싶다면 arr.length,<br/>
요소의 개수를 알고싶다면 arr[n].length을 해주면 됩니다.<br/>
