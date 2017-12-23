---
layout: post
title: "[Android] Intent"
description: 두가지 Intent
image: 'https://www.ithaka.travel/blog/wp-content/uploads/2017/07/Surfing-in-Bali-740x416.jpg'
category: 'blog'
introduction: 두가지 Intent
tags:
- android
---

아래와 같은 것들이 서로 통신할 때 Data를 전달해준다  

* `Activity` `Service` `BroadcastReceiver`
* `Application`  

두가지 성격의 Intent가 존재하는데 예를 들어 간단하게 설명해보겠다.

## 명시적 Intent  

Ex. NewActivity를 띄울 때  

```java
Intent intent = new Intent(this, NewActivity.class);
startActivity(intent);
```

## 암시적 Intent  

Ex. text message 를 보낼 때 (이 기능이 가능한 앱을 선택할 수 있음)  

```java
// 간단하게 text 공유
Intent sendIntent = new Intent(Intent.ACTION_SEND);
sendIntent.setType("text/plain");
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
// Verify that the intent will resolve to an activity
if (sendIntent.resolveActivity(getPackageManager()) != null) {
    startActivity(sendIntent);
}
```

참고로 최근 프로젝트를 하던 중 갤럭시S4에서만 테스트하다보니 몰랐던 사실인데,  
공유 intent를 띄울 때 **이번만** 이나 **항상** 버튼이 있었는데 최근 버전에서는 한번 앱선택을 하면 계속 그 앱으로만 Intent가 넘어가게 되어서 아래와 같이 코드에서 수정을 해주어야 한다.

```java
Intent sendIntent = new Intent(Intent.ACTION_SEND);
sendIntent.setType("text/plain");
sendIntent.putExtra(Intent.EXTRA_TEXT, text);
startActivity(Intent.createChooser(sendIntent, "Dialog Name"));
```
