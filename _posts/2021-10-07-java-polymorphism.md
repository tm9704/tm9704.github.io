---
title: "Java(26) - 다형성"
excerpt: "Java Polymorphism"
toc: true
toc_sticky: true
categories:
  - Java
tags:
  - Java
date: "2021-10-07 12:10:00"
last_modified_at: "2021-10-07 12:30:00"
---

## 1. 다형성이란

1. 하나의 코드가 여러 자료형으로 구현되어 실행되는 것
   (같은 코드에서 여러 실행 결과가 나옴)
2. 정보은닉, 상속과 더불어 객체지향 프로그래밍의 가장 큰 특징 중 하나
3. 객체지향 프로그래밍의 유연성, 재활용성, 유지보수성에 기본이 되는 특징

간단한 예시를 말로 하자면,<br/>
여러 클래스를 한 클래스에 상속시키고 사용시 참조변수 타입을 부모 클래스 타입으로<br/>
선언하고 인스턴스는 각 하위클래스로 생성, 메소드 오버라이딩을 활용해서 같은 메소드를<br/>
사용해 다양한 결과를 도출하는 것입니다.<br/>

다형성 예시

```java
class Animal{
	public void move() {
		System.out.println("동물이 움직입니다.");
	}
}

class Human extends Animal{
	public void move() {
		System.out.println("사람이 두 발로 걷습니다.");
	}
	public void readBook(){
		System.out.println("사람이 책을 읽습니다.");
	}
}

class Tiger extends Animal{
	public void move() {
		System.out.println("호랑이가 네 발로 뜁니다.");
	}
	public void hunting() {
		System.out.println("호랑이가 사냥을 합니다.");
	}
}

class Eagle extends Animal{
	public void move() {
		System.out.println("독수리가 하늘을 날아갑니다.");
	}
	public void flying() {
		System.out.println("독수리가 멀리 날아갑니다.");
	}
}

public class AnimalTest {
	public static void main(String[] args) {
		Animal hAnimal = new Human();
		Animal tAnimal = new Tiger();
		Animal eAnimal = new Eagle();

		ArrayList<Animal> animalList = new ArrayList<Animal>();
		animalList.add(hAnimal);
		animalList.add(tAnimal);
		animalList.add(eAnimal);

		for(Animal animal : animalList) {
			animal.move();
		}
	}
}
```

## 2. 다형성의 장점

1. 다양한 여러 클래스를 하나의 자료형(상위클래스)으로 선언하거나,<br/>
   형변환 하여 각 클래스가 동일한 메서드를 오버라이딩한 경우,<br/>
   하나의 코드가 다양하게 구현을 실행할 수 있다.
2. 유사한 클래스가 추가되는 경우 유지보수에 용이하고,<br/>
   각 자료형 마다 다른 메서드를 호출하지 않으므로 코드에서 많은 if문이 사라짐.

상속과 오버라이딩, 다형성을 한 맥락으로 이해하는 것이 중요합니다.<br/>

## 3. 상속은 언제 사용하는가

1. IS-A 관계(is a relationship: inheritance)
   - 일반적인(general) 개념과 구체적인(specific) 개념의 관계
   - 단순히 재사용 목적이 아닙니다.
2. HAS-A 관계(composition)
   - 한 클래스가 다른 클래스를 소유한 관계
   - 코드 재사용의 한 방법

IS-A 관계는 우리가 계속 공부해 온 상속 관계라고 생각하면 됩니다.<br/>
이 관계의 특징은

1. 상속은 클래스간의 결합도가 높다.
2. 상위 클래스의 수정이 많으면 하위 클래스에 영향이 미칠 수 있다.
3. 계층 구조가 복잡하거나 hierarchy(계층)가 높으면 좋지 않다.

입니다.<br/>

HAS-A 관계 역시 상속의 조건에는 부합합니다.<br/>
상속의 기본 문법이 하위클래스가 상위클래스를 지니고 있기 때문입니다.<br/>
그러나 우리는 이 관계를 복합 관계로 이해하는게 일반적입니다.<br/>
(소유의 관계, 코드 재사용)<br/>

에시를 보겠습니다.<br/>
우선 상속을 이용한 코드를 보겠습니다.<br/>

```java
class Gun{
    int bullet; //장전된 총알의 수

    public Gun(int bnum){bullet = bnum;}
    public void shot(){
        System.out.println("BBANG!");
        bullet--;
    }
}

class Police extends Gun{
    int handcuffs; //소유한 수갑의 수

    public Police(int bnum, int bcuff){
        super(bnum);
        handcuffs = bcuff;
    }

    public void putHandcuff(){
        System.out.println("Snap!");
        handcuffs--;
    }
}

class HASInheritance{
    public static void main(String[] args){
        Police pman = new Police(5, 3);
        pman.shot();
        pman.putHandcuff();
    }
}
```

위 코드는 HAS-A관계를 상속으로 표현한 것입니다.<br/>

다음은 HAS-A관게를 소유의 방식으로 표현한 코드를 보겠습니다.<br/>

```java
class Gun{
    int bullet; //장전된 총알의 수

    public Gun(int bnum){bullet = bnum;}
    public void shot(){
        System.out.println("BBANG!");
        bullet--;
    }
}

class Police{
    int handcuffs; //소유한 수갑의 수
    Gun pistol; //소유하고 있는 권총

    public Police(int bnum, int bcuff){
        handcuffs = bcuff;
        if(bnum != 0){
            pistol = new Gun(bnum);
        }else{
            pistol = null;
        }
    }

    public void putHandcuff(){
        System.out.println("SNAP!");
        handcuffs--;
    }

    public void shot(){
        if(pistol == null){
            System.out.println("Hut BBANG!");
        }else{
            pistol.shot();
        }
    }
}

class HasComposite{
    public static void main(String[] args){
        Police haveGun = new Police(5, 3);
        haveGun.shot();
        haveGun.putHandcuff();

        Police dontHaveGun = new Police(0, 3);
        dontHaveGun.shot();
    }
}
```

코드의 양으로 봤을 때 지금은 소유의 관계의 코드가 더 많아보입니다.<br/>
하지만 이런 상황의 코드의 경우 상속으로 나타냈을 때 문제가 생길 수 있습니다.<br/>

1. 권총을 소유하지 않은 경찰을 표현해야 할 때
2. 권총, 수갑말고 다른 장비를 소유할 때

이런 경우를 반영하기 쉽지 않습니다.<br/>

즉 상속으로 이런 관계를 표현한 경우 두 클래스는 강한 연관성을 갖기 때문에<br/>
Gun 클래스를 상속받는 Police 클래스로는 총을 소유한 경찰만 표현이 가능합니다.<br/>

결론은 상속은 IS-A 관계의 표현에 매우 적절합니다.<br/>
그리고 경우에 따라서는 HAS-A 관계의 표현(소유 관계)에도 사용될 수 있으나,<br/>
프로그램의 변경에 제약이 생길 수 있습니다.<br/>
