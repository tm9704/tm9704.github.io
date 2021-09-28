---
title: "Java 객체 배열"
excerpt: "Java Object Array"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
last_modified_at: 2021-09-27T16:24:00
---

이전 포스트에서는 기본 자료형 배열에 대해 공부했습니다.<br/>
이번 포스테에서는 참조 자료형 배열에 대해 공부하겠습니다.<br/>

## 1. 참조 자료형 배열(객체 배열)

- 기본 자료형 배열
  int[] arr = new int[10]<br/>

  ![기본 자료형 배열](/images/array.png)<br/>

- 참조 자료형 배열(객체 배열)
  Book[] library = new Book[5]<br/>

  ![참조 자료형 배열](/images/objectarray.png)<br/>

참조 자료형 배열의 경우 data type을 참조 자료형으로 해주시고 기본 자료형 배열과 똑같이 해주시면 됩니다.<br/>

단 주의할 점이 있는데,<br/>
참조 자료형 배열에는 값 자체가 들어가는 것이 아닌, 생성할 객체의 주소가 들어갑니다.<br/>

그리고 객체 배열을 선언했다고 해서 객체들이 생성되는 것이 아닙니다.<br/>
Book book1 = new Book();<br/>
이런 식으로 인스턴스를 각각 생성한 후 배열에 대입해줘야합니다.<br/>

참조 자료형 배열은 초기화를 안해주면, 기본 자료형 배열과 달리 null로 초기화됩니다.<br/>

## 2. 배열 복사

한 array에서 다른 array로 배열을 복사할 때는<br/>

System.arraycopy(src, src position, destination, desPos, length);<br/>
을 이용하면 됩니다.

인자는 왼쪽부터 순서대로

1. 복사할 배열(source)
2. 복사할 배열의 몇번째 부터 인지 (source position)
3. 복사될 배열(destination)
4. 복사될 배열의 시작 위치(destination position)
5. 복사할 배열의 시작 위치부터 몇개인지(length)

```java
System.arraycopy(arr, 0, copyArr, 1, 3);
```

위 코드의 의미는 arr 배열의 0번째 인덱스부터 3개의 인덱스에 있는 값을,<br>
copyArr배열에 1번째 인덱스부터 복사한다는 의미입니다.<br/>

## 3. 객체 배열 복사

기본 자료형 배열의 경우 System.arraycopy를 사용해도 큰 문제가 없습니다.<br/>
그러나 객체 배열의 경우 값 자체를 저장하는 것이 아닌 주소를 저장하기 때문에 문제가 생깁니다.<br/>

- 얕은 복사

  System.arraycopy를 객체 배열에 사용했을 경우 얕은 복사가 일어납니다.<br/>
  얕은 복사란 실제로 인스턴스가 생성돼서 복사가 되는 것이 아닌, 주소만 복사하는 것입니다.<br/>
  즉 객체 배열들이 같은 인스턴스를 가리키는 것을 의미합니다.<br/>

  ![얕은 복사](/images/shallow_copy.png)

- 깊은 복사

  깊은 복사는 얕은 복사와 달리 서로 다른 인스턴스를 생성해 각각 배열에 넣어주는 방법입니다.<br/>

  ![깊은 복사](/images/deep_copy.png)

- 예시

  얕은 복사

  ```java
  public class ObjectCopy {

    public static void main(String[] args) {
      Book[] library = new Book[5];
      Book[] copyLibrary = new Book[5];

      library[0] = new Book("태백산맥1", "조정래");
      library[1] = new Book("태백산맥2", "조정래");
      library[2] = new Book("태백산맥3", "조정래");
      library[3] = new Book("태백산맥4", "조정래");
      library[4] = new Book("태백산맥5", "조정래");

      System.arraycopy(library, 0, copyLibrary, 0, 5);

      library[0].setTitle("나목");
      library[0].setAuthor("박완서");

      for(Book book : library) {
        book.showBookInfo();
      }

      System.out.println("============");

      for(Book book : copyLibrary) {
        book.showBookInfo();
      }
    }

  }
  ```

  깊은 복사

  ```java
  public class ObjectCopy2 {

    public static void main(String[] args) {
      Book[] library = new Book[5];
      Book[] copyLibrary = new Book[5];

      library[0] = new Book("태백산맥1", "조정래");
      library[1] = new Book("태백산맥2", "조정래");
      library[2] = new Book("태백산맥3", "조정래");
      library[3] = new Book("태백산맥4", "조정래");
      library[4] = new Book("태백산맥5", "조정래");

      copyLibrary[0] = new Book();
      copyLibrary[1] = new Book();
      copyLibrary[2] = new Book();
      copyLibrary[3] = new Book();
      copyLibrary[4] = new Book();

      for(int i = 0; i<library.length;i++) {
        copyLibrary[i].setTitle(library[i].getTitle());
        copyLibrary[i].setAuthor(library[i].getAuthor());
      }

      library[0].setTitle("나목");
      library[0].setAuthor("박완서");

      for(Book book : library) {
        book.showBookInfo();
      }

      System.out.println("============");

      for(Book book : copyLibrary) {
        book.showBookInfo();
      }
    }
  }
  ```

## 4. 향상된 for문

for-each문이라고도 하는 향상된 for문(enhanced for)은<br/>
배열 요소의 처음부터 끝까지 모든 요소를 참조할 때 편리한 for문 입니다.<br/>

```java
for(int e : arr){
  System.out.println(e + " ");
}
```

위의 코드를 보시면 arr은 반복의 대상이 되는 배열입니다.<br/>
배열의 모든 요소 각각을 e라 할 때 e가 의미하는 각각의 요소 값을 출력하라는 코드입니다.<br/>

즉 향상된 for문은 arr 배열에 있는 요소들을 하나하나 꺼내올 때 마다 변수 e에 넣으면서<br/>
arr의 전체를 다 돌 때까지 반복문을 실행합니다.<br/>
