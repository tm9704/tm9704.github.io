---
title: "Java(56) - 자바 다양한 예외처리"
excerpt: "Java Exception Handling"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-11-01 14:45:00"
last_modified_at: "2021-10-27 15:30:00"
---

## 1. 예외처리 미루기

- throws를 사용해서 예외처리를 미룰 수 있습니다.
- try 블록으로 예외를 처리하지 않고, 메서드 선언부에 throws를 추가합니다.
- 예외가 발생한 메서드에서 예외처리를 하지 않고, 해당 메서드를 호출한 곳에서 예외 처리를 한다는 의미입니다.<br/>
  (책임 전가)
- main()에서 throws를 사용하면 가상머신에서 처리가 됩니다.(에러 발생)

```java
public class ThrowsException{
  public Class loadClass(String fileName, String className) throws FileNotFoundException, ClassNotFoundException{
    FileInputStream fis = new FileInputStream(fileName);
    Class c = Class.forName(className);
    return c;
  }

  public static void main(String[] args){
    ThrowsException test = new ThrowsException();
    try{
      test.loadClass("b.txt", "java.lang.string");
      }catch (FileNotFoundExcpetion e){
        System.out.println(e);
      }catch (ClassNotFoundException e){
        System.out.println(e);
      }catch (Exceptino e){
        System.out.println(e);
      }
      System.out.println("end");

      /*
      try{
        test.loadClass("a.txt", "java.lang.String");
      }catch(FileNotFoundException | ClassNotFoundException e){
        e.printStackTrace();
      }
      */
  }
}
```

## 2. 다중 예외처리하기

- 하나의 try 블록에서 여러 예외가 발생하는 경우 catch 블록 한 곳에서 처리하거나,<br/>
  여러 catch 블록으로 나누어 처리할 수 있습니다.
- 가장 최상위 클래스인 Exception 클래스는 가장 마지막 블럭에 위치해야합니다.<br/>

예시를 보겠습니다.<br/>

```java
public static void main(String[] args){
    ThrowsException test = new ThrowsException();
    try{
        test.loadClass("a.txt", "java.lang.String");
    }catch(FileNotFoundException e){
        e.printStackTrace();
    }catch(ClassNotFoundException e){
        e.printStackTrace();
    }catch(Exception e){ //위의 예외 외의 상황 처리
        e.printStackTrace();
    }
}
```

## 3. 사용자 정의 예외

- JDK에서 제공되는 예외 클래스 외에 사용자가 필요에 의해 예외 클래스를 정의해서 사용할 수 있습니다.<br/>
- 기존 JDK 클래스에서 상속받아 예외 클래스를 만듭니다.<br/>

예시를 하나 보겠습니다.<br/>

```java
class IDFormatException extends Exception{
    public IDFormatException(String message){
        super(message);
    }
}

public class IDFormatTest{
  private String userID;

  public String getUserID(){
    return userID;
  }

  public void setUser(String userID) throws IDFormatException{
    //사용하는곳에서 처리를 해라
    if(userID == null){
      throw new IDFormatException("아이디는 null일 수 없습니다.");
    }else if(userID.length() < 8 || userID.length() >20){
      throw new IDFormatException("아이디는 8자 이상 20자 이하로 쓰세요");
    }

    this.userID = userID;
  }

  public static void main(String[] args){
    IDFormatTest = idTest - new IDFormatTest();

    String myId = null;

    trys{
        idTest.setUserID(myId);
    }catch(IDForamat e{
      System.out.println(e);
    })
  }
}
```

throws 키워드로 예외를 발생시킵니다.<br/>
