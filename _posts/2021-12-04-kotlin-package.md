---
title: "Kotlin(2) - 코틀린 패키지"
excerpt: "Kotlin Package"
toc: true
toc_sticky: true
categories:
  - Kotlin
tags:
  - Kotlin
date: "2021-12-04 15:10:00"
last_modified_at: "2021-12-04 15:30:00"
---

코틀린에서 프로젝트(Project)는 모듈(Module), 패키지(Package), 파일(File)로 구성되어 있습니다.<br/>
프로젝트를 구성하는 모듈, 패키지, 파일에 대해 공부하겠습니다.<br/>

## 1. 프로젝트, 모듈, 패키지, 파일의 관계 이해

코틀린에서 프로젝트에는 모듈이 있고 모듈은 다시 패키지로 구성되어 있습니다.<br/>
패키지는 파일(클래스)로 구성되어 있습니다.<br/>

![package1](/images/kotlin_package1.png){: width="300" height="350"}
![package2](/images/kotlin_package2.png){: width="300" height="350"}
<br/>
위의 사진에서 PackageTest 프로젝트에는 PackageTest 모듈과 OtherModule 모듈이 있고,<br/>
PackageTest모듈 안에는 com.example.edu패키지와 default 패키지가 있고,<br/>
그 안에는 파일들이 있습니다.<br/>

com.example.edu패키지는 우리가 눈으로 확인할 수 있지만 default 패키지는 눈에 보이지 않습니다.<br/>
default패키지는 src 폴더에 따로 패키지 이름이 지정되지 않은 파일이 됩니다.<br/>
즉, src폴더이 저장된 HelloKotlin.kt 파일은 default 패키지에 포함된 파일입니다.<br/>

코틀린 파일은 .kt 확장자를 가지며 파일의 맨위에는 해당 파일이 어떤 패키지에 포함된 것인지<br/>
코틀린 컴파일러가 이해할 수 있도록 패키지 이름을 선언해야 합니다.<br/>
패키지 이름을 선언하지 않으면 자동으로 default 패키지에 포함됩니다.<br/>

만일 파일에 1개의 클래스가 정의되어 있다면 Project 창 화면에서 .kt 확장자가 빠진<br/>
클래스 이름만 보이게 됩니다.(Person 처럼)<br/>

파일에 클래스를 여러 개 정의한다면 파일은 단순히 클래스를 묶는 역할을 하고 .kt 확장자가 붙게 됩니다.<br/>

## 2. 패키지를 만들어야 하는 이유

2명 이상의 프로그래머가 프로젝트를 진행한다고 가정할 때 우연히 같은 이름의 파일(클래스)를 만들었다고 가정합시다.<br/>
이렇게 되면 당연히 오류가 발생하게 됩니다.<br/>

그러나 패키지가 다르면 오류가 발생하지 않습니다.<br/>
