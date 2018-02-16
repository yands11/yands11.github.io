---
layout: post
title: "[DesignPattern] 01 Strategy Pattern"
description:
image: 'https://esgyn.com/wp-content/uploads/avatar_images/patterns.jpg'
category: 'blog'
introduction:
tags:
- DesignPattern
---

# Head First Design Pattern

사내에서 시작한 `Head First Design Pattern`책으로 북클럽을 하고있는데, 보면서 읽고 지나가는 것 보다는 이렇게 읽으면서 중요한 부분도 정리하고 생각도 적어볼겸 조금씩 블로그 포스팅을 해볼 생각이다.

## 1. 디자인패턴소개

> 달라지지 않는 부분을 찾아내고, 달라지지 않는 부분으로부터 분리시킨다.

> 무작정 구현만 하지 말고 인터페이스를 만들어서 그게 맞춰서 프로그래밍 한다.

> 상속보다는 구성을 활용한다.

상속보다 구성(composition)을 활용해야 프로그램을 더 유연하게 만들 수 있다.


## 2. Kotlin으로
```Kotlin
abstract class Duck(var quackBehavior: QuackBehavior,
                    var flyBehavior: FlyBehavior) {

  abstract fun display()

  fun performQuack() {
    quackBehavior.quack()
  }

  fun performFly() {
    flyBehavior.fly()
  }

  fun swim() {
    System.out.println("swim and swim")
  }
}
```

```Kotlin
class RedHeadDuck : Duck(Quack(), FlyWithWings()) {

  override fun display() {
    System.out.println("My head is red.")
  }
}
```

```Kotlin
class RubberDuck : Duck(Squeak(), FlyNoWay()) {

  override fun display() {
    System.out.println("I'm Rubber.")
  }

}
```
----
```Kotlin
public interface QuackBehavior {
  fun quack()
}
```

```Kotlin
class Quack: QuackBehavior {
  override fun quack() {
    System.out.println("quack! quack!")
  }
}
```

```Kotlin
class Squeak : QuackBehavior {
  override fun quack() {
    System.out.println("squeak! squack!")
  }
}
```

```Kotlin
class MuteQuack : QuackBehavior {
  override fun quack() {
    System.out.println("Hush!")
  }
}
```
----
```Kotlin
public interface FlyBehavior {
  fun fly()
}
```

```Kotlin
class FlyWithWings: FlyBehavior {
  override fun fly() {
    System.out.println("Fly High!")
  }
}
```

```Kotlin
class FlyNoWay : FlyBehavior {
  override fun fly() {
    System.out.println("I can't fly")
  }
}
```
----

### 느낀점
OOP의 개념을 얼마나 잘 활용하면서 코딩을 하고 있었는지 되돌아 보게 되었다.

안드로이드에서는 기본적으로 Android에서 제공하는 Activity나 Fragment라는 화면단위의 클래스들이 존재한다. 화면에 따라서 제공되어야 하는 부분들이 다르고 시간이 지남에 따라서 수정되고 삭제되는 부분들이 생기면서 상당히 OOP를 잘 짜두어도 새로운 기획에 따라 새로운 화면을 만들어야 할 때가 생긴다. 또한 새로운 개발방식이 나오면서 그에 맞게 전체적인 구조를 바꿔야 하는 경우도 많고 예외처리를 해야하는 경우도 상당히 많아진다.

1장에서 오리(Duck) 클래스를 요구에 따라서 자꾸 수정하면서 개발자가 겪는 고민을 하듯, 유지보수로 많은 삽질을 거듭하는 일이 많다. 개발을 하면서 항상 우리의 코드는 사용자에 의해서 어떤 요구에 의해서 변화해야한다. 피할 수 없는 운명처럼 그 변화를 최대한 부담스럽지않게 받아들이면서 기대에 충족할 수있는 프로그램을 만들어야 한다.

어떻게 보면 개발자의 숙명이기도 하고 개발자의 존재 이유기도 한 변화인데, 이 변화를 싫어하거나 부정적으로만 생각한다면 개발자라는 업이 나에게 맞는 것인지 생각해봐야하지 않을까. 마치 어질러진 방을 치우고 새로운 가구가 들어오는 방의 배치를 바꾸고 정돈하는 일을 질색하는 사람처럼 말이다.

최근 제한적인 시간으로 개발을 하는 경우가 있었는데 개발을 하면서도 일단 구현이 끝난 뒤에 이런 것들을 다시 해야겠구나 라는 생각이 자꾸 들게되었다. 개발 전의 고민하는 시간보다 개발 후의 유지보수의 시간이 더 많은 좋지 않은 개발방법을 택한 것 같다. 앞으론 개발 전 단계에서 충분한 시간을 가지고 지금 현재의 상황뿐만 아니라 변화될 상황도 고려하면서 Design 해야겠다. 물론 그렇다고해도 유지보수는 하게 되겠지만.

주니어개발자로서 이곳 저곳에서 일하면서 느낀 점은 기존 개발자 짜놓은 프로그램을 그 규칙에 맞게 추가적으로 프로그래밍을 하면 되니까 편하게 개발을 하긴 했지만, 한번도 내가 처음부터 이걸 만든다면 어떻게 했을지에 대해서 잘 생각을 못했던 것 같다. 지금은 잠시 혼자 프로젝트를 개발하는 상황이 되었는데 다시 누군가와 협업하게 되어도 문제없을 코드를 작성해야겠다는 책임감이 마구마구 든다.

다 써놓고 보니 디자인패턴 개념보다 생각이 더 많은 포스팅같다. 이번 챕터에서 가장 눈에 들어온 구절로 책 리뷰 겸 포스팅을 마친다.
> 디자인 패턴이 바로 코드로 들어가는 것은 아닙니다. 디자인 패턴은 우선 여러분의 머리 속에 들어갑니다.
