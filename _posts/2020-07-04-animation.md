---
layout: post
title: "안드로이드 Animation 구현하기"
author: "DAEUN"
---

# Contents
* 뷰 클릭 이벤트 만들기
* 애니메이션 구현하기
	* transitionName & ActivityOptions
	* window
* 클릭한 이미지 보여주기
	* SharedPreference

안드로이드에서 애니메이션을 구현하는 방법에 대해 공부했다. 우선적으로 현재 프로젝트에서 필요로 하는 기능 중 하나로 이미지를 클릭했을 때 이미지가 부드럽게 확장되면서 새로운 창이 뜨도록 하는 것을 목표로 공부했다. 아직 전부 공부하지는 못했지만 우선적으로 구현한 내용에 대해 정리해보려한다. 다음은 오늘 구현한 애니메이션 작동 화면이다.

<p align="center" width="300">
	<img src="/assets/images/animation_demo.gif"/>
</p>

## 뷰 클릭 이벤트 만들기
setOnClickListener로 클릭 이벤트를 작성할 수 있지만, 별도의 함수를 만들고 xml에서 속성값으로 적어줘도 간단히 만들 수 있다. 우선, xml에서 클릭 이벤트를 지정해주고 싶은 ImageView, TextView 등에 clickable="true"와 onClick="_(클릭 이벤트 함수명)_"을 작성해준다. 그 다음 해당 뷰의 액티비티 또는 프레그먼트에서 함수를 작성한다.

```xml
<ImageView
	...
	android:clickable="true"
	android:onClick="animation" />
```

```kotlin
class MainActivity : AppCompatActivity() {
	override fun onCreate(savedInstanceState: Bundle?) { ... }

	fun animation(v: View) { 
		//클릭 이벤트 코드 작성
	}
}
```

[목차로 돌아가기](#Contents)

## 애니메이션 구현하기
애니메이션을 구현하는 방법은 Scene과 Transition을 이용하는 등 다양하지만 오늘은 이미지를 클릭했을 때 새로운 액티비티를 띄우는 애니메이션을 만들었다. 액티비티는 2개를 만들었고 xml에서 transitionName을, 액티비티에서 ActivityOptions를 지정해 만들었다.

### transitionName & ActivityOptions
뷰의 속성 중 하나로 '태그'라고 생각하면 된다. 만약 사진A의 위치에서 사진B로의 위치로 애니메이션을 실행하고 싶다면, 동일한 값을 주어 동작을 처리할 수 있다.

```xml
//애니메이션을 시작하는 액티비티의 xml과
//애니메이션이 끝나는 액티비티의 xml의 ImageView 모두 같은 transitionName을 지정한다.
<ImageView
	...
	android:id="@+id/img"
	android:transitionName="image" />
```

다음으로 만들어준 클릭 리스너 animation()에서 startActivity()로 액티비티를 호출한다. 이 때 기존에 호출할 액티비티를 지정한 Intent를 넘겨준 것에 추가로 ActivityOptions을 Bundle로 전달한다. ActivityOptions에는 makeSceneTransitionAnimation()이라는 함수가 있는데 순서대로 시작 액티비티, 애니메이션을 시작할 뷰, 그리고 앞서 설정한 sharedElementName, 즉 transitionName으로 지정한 이름을 전달한다. 이를 toBundle()로 묶어 startActivity의 두 번째 인자로 전달하면 이 글의 시작 부분에 보여지는 화면처럼 이미지를 클릭했을 때 액티비티가 전환되면서 자연스럽게 커지는 것을 확인할 수 있다. 추가로, makeSceneTransitionAnimation을 사용하기 위해서 API 레벨이 21 이상이어야한다. 그 이하 레벨로 설정해 개발한다면 @RequiresApi(Build.VERSION_CODES.LOLLIPOP)을 적어주거나 if문으로 사용자 스마트폰의 API 레벨을 확인한 후 각각 다른 동작을 하도록 작성할 수도 있다.

```kotlin
fun animation(v: View) {
	...
	val intent = Intent(this, SecondActivity::class.java)

	//두 번째 인자를 v로 전달하면
	//findViewById()로 각자 지정하지 않아도
	//선택된 뷰에서 시작해 목표 지점까지의 애니메이션이 실행된다.
	val option = ActivityOptions.makeSceneTransitionAnimation(this, v, "image").toBundle()

	startActivity(intent, option)
}
```

반대로 두 번째 액티비티에서 다시 첫 번재 액티비티로 돌아갈 때도 startActivity()를 실행해봤지만, 첫 화면의 4개의 ImageView 모두 transitionName이 image로 동일해 하나의 ImageView로 사진이 축소됐다. SharedPreferences로 뷰의 id값을 받아와 전달하려 했지만 쉽지 않았다. 화면에서 뒤로가기 버튼을 눌렀을 때는 원하던대로 애니메이션이 실행되었기에 뒤로가기 버튼을 눌렀을 때 호출되는 onBackPressed()를 실행해 해결했다.

[목차로 돌아가기](#Contents)

### window
앞서 본 방법 외에도 window를 이용해 액티비티 실행 시 애니메이션을 적용시킬 수 있다. onCreate()에서 다음과 같이 코드를 적어주면 된다. enterTransition과 exitTransition에 원하는 애니메이션을 적어주면 되고 Fade(), Slide() 등을 제공하며 원하는 애니메이션을 만들어 사용할 수도 있다. duration은 애니메이션을 실행하는 시간을 의미한다. 여기서도 enter/exitTransition을 설정하기 위해서는 API 레벨이 21 이상이어야한다.

```kotlin
override fun onCreate(savedInstances: Bundle?) {
	super.onCreate(savedInstances)

	with(window) {
		requestFeature(Window.FEATURE_CONTENT_TRANSITIONS)
		enterTransition = Fade()
		exitTransition = Fade()
		exitTransition.duration = 2000
	}

	setContentView(R.layout.end)
}
```

## 클릭한 이미지 보여주기
이제 클릭한 이미지를 넘겨주는 일만 남았다. 이 부분은 SharedPreferences를 사용했다. 이미지를 4개만 사용했으므로 사진 순서를 그 값으로 넘겨주어 해당 번호의 사진을 보여주도록했다.

```kotlin
//첫 번째 액티비티
fun animation(v: View) {
        val sharedPref = getSharedPreferences("IMAGE", Context.MODE_PRIVATE)
        val editor = sharedPref.edit()
        when(findViewById<ImageView>(v.id)) {
            img -> editor.putInt("IMG", 1)
            img2 -> editor.putInt("IMG", 2)
            img3 -> editor.putInt("IMG", 3)
            else -> editor.putInt("IMG", 4)
        }
        editor.apply()

        ...
}
```

```kotlin
//두 번재 액티비티
override fun onCreate(savedInstances: Bundle?) {
        super.onCreate(savedInstances)
        setContentView(R.layout.end)

        val sharedPref = getSharedPreferences("IMAGE", Context.MODE_PRIVATE)

        when(sharedPref.getInt("IMG", 4)) {
            1 -> img2.setImageResource(R.drawable.cheetah)
            2 -> img2.setImageResource(R.drawable.fox)
            3 -> img2.setImageResource(R.drawable.lion)
            else -> img2.setImageResource(R.drawable.raccoon)
        }

        ...
}
```

[목차로 돌아가기](#Contents)

Reference: [Start an activity using an animation](https://developer.android.com/training/transitions/start-activity)