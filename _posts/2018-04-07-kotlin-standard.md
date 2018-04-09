---
layout: post
title: "[번역+] Kotlin standard function 마스터하기: run, with, let, also, apply"
description:
image: 'https://cdn-images-1.medium.com/max/1600/1*gZ9XF80M8yOasLiFUzL07g.png'
category: 'blog'
introduction:
tags:
- Android, Kotlin
---

# [번역+] Kotlin standard function 마스터하기: run, with, let, also, apply


읽기 전, 주의사항

> 원문은 [여기](https://bit.ly/2pfS00x)에서 확인이 가능합니다.
>
> 원문의 특정 부분이 누락된 부분이 있습니다. 그 이유는 해석을 완벽하게 못했거나 필요없다고 생각되어서 생략한 부분이니 문제 되는 부분은 yands11@gmail.com로 신고하세요. DISQUS 아직도 못붙였네요.ㅠ 아 참  제맘대로의 의역이 많습니다.


Kotlin의 [standard function](https://github.com/JetBrains/kotlin/blob/master/libraries/stdlib/src/kotlin/util/Standard.kt)들은 많이 비슷해서 확신을 가지고 사용할 수 없다. 사용할 때 어떤 걸 골라야하는지, 명확하게 구별할 수 있는 간단한 방법을 소개하겠다.


## Scoping functions

살펴볼 함수들은 `run`, `with`, `T.run`, `T.let`, `T.also`, `T.apply`이다. 난 이것들을 scoping 함수(function)라 부른다.

scoping라는 것을 설명하기 위해서 가장 간단히 run 을 소개한다.

```kotlin
fun test() {
    var mood = "I am sad"

    run {
        val mood = "I am happy"
        println(mood) // I am happy
    }
    println(mood) // I am sad
}
```

`test` 함수안에서 `mood`가 출력되기 전에 `I am happy`로 재정의된다. 분리된 scope를 가지게 되는 것이다. 완전히 `run` 이라는 scope 안에 둘러싸여있다.

위의 scoping 함수는 그냥 scope만 구분하는 것이라서 아주 유용해보이지는 않는다. 그렇지만 보다 더 nice하게 사용하는 또 다른 방법이 있는데, scope안에 있는 마지막 object를 return 한다는 것이다.

 따라서 아래와 같이 깔끔해질 수 있다. `show()`를 각각의 view에 두 번씩이 아닌 한 번만 적용하고 있다.

```kotlin
run {
    if (firstTimeView) introView else normalView
}.show()
```

```java
// 같은 Java 코드 (attached by @dot2line)
if (firstTimeView) {
	introView.show();  
} else {
    normalView.show();
}
```



## scoping 함수들의 3가지 특징

더 흥미로운 scoping function을 만들어 내기 위해서, 3가지 특징으로 행동을 구분하려고 한다. 나는 각각 다른 방법으로 구분해서 사용할 것이다.

### 1. Normal vs. extension function

만약 `with`와 `T.run`을 본적있다면 두 function들은 정말로 비슷하다. 아래는 똑같은 내용의 코드이다.

```kotlin
with(webview.settings) {
    javaScriptEnable = true
    databaseEnabled = true
}
```

```kotlin
webview.settings.run {
    javaScriptEnabled = true
    databaseEnabled = true
}
```

같은 코드지만 다른점은 `with`는 일반적인 함수이고, 반면에 `T.run`은 extension 함수이다.

그렇다면 여기서 질문, 뭐가 더 좋은걸까?

`webview.settings`가 nullable이라면, 아래와 같이 달라진다.

```kotlin
// 이런!
with(webview.settings) {
    this?.javaScriptEnable = true
    this?.databaseEnabled = true
}
```

```kotlin
// 좋군
webview.settings?.run {
    javaScriptEnabled = true
    databaseEnabled = true
}
```

위와 같은 경우라면 `T.run`을 사용하는 게 더 명확하다. nullable한 것을 미리 확인해서 사용하기 때문이다.

### 2. This vs. it argument

`T.run` and `T.let` 를 본적있다면 하나를 빼고 상당히 비슷해 보일 것이다. 아래와 같이 같은 로직이 있다.

```kotlin
stringVariable?.run {
      println("The length of this String is $length")
}
```

```kotlin
stringVariable?.let {
      println("The length of this String is ${it.length}")
}
```

`T.run` 함수 모양을 확인해보자면, `T.run`은 `block: T.()`를 호출하는 extension 함수로 만들어져 사용되고 있는 것을 확인할 수 있을 것이다. scope 내부 전체에 `T`는 this로 언급될 수 있다. 프로그래밍하면서 this는 대부분 생략하는 경우가 많다. 그러므로 위의 예제에서 그냥 `$this.length` 대신에 `$length`로 바로 사용할 수 있다.

그러나`T.let`function을 확인해보자면, `T.let`은 `block: (T)`로 자신을 전달하고 있다. lambda와 비슷한 모습이다. `it`으로 scope안에서 언급될 수 있다.

위와 같이 `T.run`은 더 암시적으로 사용되기 때문에 `T.let`보다 더 좋을 수도 있지만(* let 을 쓰면 it으로 명시해주는 것 보다는 편리하다는 의미 같다. @dot2line) 아래와 같이 `T.let`의 몇몇 미묘한 이점들이 존재한다.

* `T.let`은 주어진 변수의 function/member와 다른 클래스의 function/member를 사용하여 구분할 수 있도록 한다.

```kotlin
// attached by @dot2line

class User {
    var name: String
    var location: Location

    // User의 name
    name = "dot2line"
    location.let {
        // Location의 name
        it.name = "starbucks"
        it.address = "서울특별시 강남구 삼성동 xxxx"
    }
}
```

  ​

* 함수의 parameter로 보낼 때, `this`를 생략할 수 없는데, 이 때 `it`은 `this` 보다 짧으니 더 명확하다.(* 이건 좀4글자와 2글자의 차이는... @dot2line)

```kotlin
// attached by @dot2line

// bad
name?.run {
    print(this)
}

// cool
name?.let {
    print(it)
}
```

* `T.let`은 `it` 대신에 더 좋은 이름을 지어서 다른 이름으로 바꿀 수 있다. (* 필요할 때는 it 대신 알기 쉬운 이름을 붙여주는 것도 좋은 방법! @dot2line)

```kotlin
stringVariable?.let {
      nonNullString ->
      println("The non null string is $nonNullString")
}
```

  ​

### 3. this를 return하는 것과 그렇지 않은 것

자 이제 `T.let`과 `T.also`를 살펴보자. 또 똑같다. 살펴보자.

```kotlin
stringVariable?.let {
      println("The length of this String is ${it.length}")
}
```

```kotlin
stringVariable?.also {
      println("The length of this String is ${it.length}")
}
```

그러나 return(반환)하는 것에 미묘한 차이가 있다. `T.let`은 값의 다른 타입을 반환하게고, 반면에  `T.also`는 `T` 자신을 그대로 return한다.

함수를 chaining할 때 유용한데, `T.let`은 변화시켜 나갈 때, `T.also`는 `this`로 계속 나갈 때 쓰면 된다.

이해가 잘 안된다면 자, 간단한 설명이 아래에 있다.

```kotlin
val original = "abc"
```

```kotlin
// Evolve the value and send to the next chain
original.let {
    println("The original String is $it") // "abc"
    it.reversed() // 다음 let으로 전달할 파라미터로서의 it 이 된다.
}.let {
    println("The reverse String is $it") // "cba"
    it.length  // 또 다른 타입으로 바뀐다. (Int 겠지.)
}.let {
    println("The length of the String is $it") // 3
}
```

```kotlin
// 잘못된 Also 방법
original.also {
    println("The original String is $it") // "abc"
    it.reversed() // 해봤자 다음 also에 반영되지 않지.
}.also {
    println("The reverse String is ${it}") // "abc"
    it.length  // 해봤자 다음 also에 반영되지 않지.
}.also {
    println("The length of the String is ${it}") // "abc"
}
```

```kotlin
// 올바른 Also 방법 (즉, 위 let의 예제처럼 변경된 값을 받아 chaining할 수는 없다.)
original.also {
    println("The original String is $it") // "abc"
}.also {
    println("The reverse String is ${it.reversed()}") // "cba"
}.also {
    println("The length of the String is ${it.length}") // 3
}
```

`T.also` 는 위와 같이 의미없어보일지 모르겠지만, 신중히 생각해보면 조금 좋은 이점이 있다.

1. 더 잘게 함수의 section을 나누고 싶다면 같은 object가 전달되니 명확하게 나눠질 수 있다.
2. builder 패턴 같은 곳에 적용시킨다면 아주 powerful할 것이다.

변화시킨 값을 계속 chaining할 때, 같은 값을 유지시키면서 chaining할 때 아래와 같이 강려크해질 수 있다.

```kotlin
// 평범한 전근법
fun makeDir(path: String): File  {
    val result = File(path)
    result.mkdirs()
    return result
}
```

```kotlin
// 강려크한 접근법
fun makeDir(path: String) = path.let{ File(it) }.also{ it.mkdirs() }
```



## 모든 특징을 살펴보자

3가지 특징을 보면, 함수 행태를 더 잘 이해할 수 있다.

`T.apply`를 설명하자면 다음과 같다.

1. extension  함수
2. 변수는 this
3. 자신(this)을 return

사용하자면, 다음과 같이 사용할 수 있다.

```kotlin
// 노말노말한 접근법
fun createInstance(args: Bundle) : MyFragment {
    val fragment = MyFragment()
    fragment.arguments = args
    return fragment
}
```

```kotlin
// 와우 한줄로 끝이 나따!
fun createInstance(args: Bundle)
              = MyFragment().apply { arguments = args }
```

또는 chaining 도 가능하다.

```kotlin
// 노말노말한 접근법
fun createIntent(intentData: String, intentAction: String): Intent {
    val intent = Intent()
    intent.action = intentAction
    intent.data=Uri.parse(intentData)
    return intent
}
```

```kotlin
// 다 묶어버리겠다!!!
fun createIntent(intentData: String, intentAction: String) =
        Intent().apply { action = intentAction }
                .apply { data = Uri.parse(intentData) }
```



## 선택

적절히 함수를 분류할 수 있게 되었다.

![categorize](https://cdn-images-1.medium.com/max/800/1*pLNnrvgvmG6Mdi0Yw3mdPQ.png)

바라기는, 위의 결정 Tree는 더 함수들을 이해하기 쉽게하고 결정하는 데 쉬워 질 거다. 함수들을 적절하게 사용하는 방법을 잘 마스터할 수 있을 것이다.
