---
title: "Android(7) - 뷰를 이용한 화면 구성(3)"
excerpt: "Android Kotlin View(3)"
toc: true
toc_sticky: true
categories:
  - Android
tags:
  - Android
date: "2023-01-09 14:30:00"
last_modified_at: "2023-01-09 15:35:00"
---

## 1. 뷰 바인딩

**뷰 바인딩** 은 레이아웃 XML 파일에 선언한 뷰 객체를 코드에서 쉽게 이용하는 방법이다.<br/>
레이아웃 XML 파일에 등록한 뷰는 findViewById() 함수로 얻어서 사용해야 한다.<br/>
이렇게 하게 되면 수많은 뷰를 findViewById()로 하나하나 가져와야 하기 때문에 매우 반복적이고 귀찮은
작업이 될 수 밖에 없다.<br/>

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <Button
        android:id="@+id/visibleBtn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="visible"/>
    <TextView
        android:id="@+id/targetView"
        android:layout_width="match_parent"
        android:layout_height="wrap_parent"
        android:text="hello world"
        android:background="#FF0000"
        android:textColor="#FFFFFF"/>
    <Button
        android:id="@+id/invisibleBtn"
        andorid:layout_widht="match_parent"
        android:layout_height="wrap_content"
        android:text="invisible"/>
```

위처럼 작성한 레이아웃 XML이 있다고 가정하고 뷰바인딩 기법을 사용해보겠다.<br/>
우선 뷰 바인딩을 사용하려면 그래들 파일에 아래와 같이 선언해야 한다.<br/>

```gradle
android{
    viewBinding{
        enabled=true
    }
}
```

build.gradle 파일을 열고 android 영역에 buildFeatures를 선언한다.<br/>
그리고 그 안에 뷰 바인딩을 적용하라는 의미로 viewBinding = true를 설정한다.<br/>
이렇게 하면 레이아웃 XML 파일에 등록된 뷰 객체를 포함하는 클래스가 자동으로 만들어지고 우리는
따로 코드에서 뷰를 선언하고 findViewById()를 호출하지 않아도 된다.<br/><br/>

자동으로 만들어지는 클래스의 이름은 레이아웃 XML파일명을 따른다.<br/>
첫 글자를 대문자로 하고 밑줄(\_)은 빼고 뒤에 오는 단어를 대문자로 만든 후 'Binding'을 추가한다.<br/>
(ex) activity_main.xml -> ActivityMainBinding)<br/><br/>

자동으로 만들어진 클래스의 inflate() 함수를 호출하면 바인딩 객체를 얻을 수 있다.<br/>
이때 인자로 layoutInflater를 전달한다. 그리고 바인딩 객체의 root 프로퍼티에는 XML의 루트 태그 객체가 자동으로
등록되므로 액티비티 화면 출력은 setContentView() 함수에 binding.root를 전달하면 된다.<br/>

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        //바인딩 객체 획득
        val biniding = ActivityMainBinding.inflate(layoutInflater)
        //액티비티 화면 출력
        setContentView(binding.root)

        //뷰 객체 이용
        binding.visibleBtn.setOnClickListener{
            binding.targetView.visibility = View.VISIBLE
        }
        binding.invisibleBtn.setOnClickListener{
            binding.targetView.visibility = View.INVISIBLE
        }
    }
}
```

바인딩 객체에 등록된 뷰 객체명은 XML 파일에 등록한 뷰의 id값을 따른다.<br/>
즉, XML에 뷰를 < Button android:id="@+id/visibleBtn" />처럼 등록했다면 바인딩 객체에
visibleBtn이라는 프로퍼티명으로 등록된다. 따라서 binding.visibleBtn으로 이용하면 됨.<br/><br/>

바인딩 클래스로 만들고 싶지 않은 XML파일이 있다면 루트 태그에 tools:viewBindingIgnore="true"라는 속성을
추가하면 된다.<br/>
