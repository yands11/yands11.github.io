---
layout: post
title: "[Android] 안드로이드 앱으로 글로벌서비스를 하려면"
description:
image: 'https://korpialttari.files.wordpress.com/2016/12/dsa.jpeg?w=500'
category: 'blog'
introduction:
tags:
- Android
---

# 안드로이드 앱으로 글로벌 서비스 하려면

최근 글로벌 서비스를 하게되면서 고려해야할 부분들에 대해 소개되어있는 [Android App Development: Localization and Internationalization](https://www.linkedin.com/learning/android-app-development-localization-and-internationalization) 강의를 보면서 '아 나도 글로벌 서비스 개발하고 있지?!' 라는 생각이 들면서 그 동안 겪어 온 글로벌 서비스에 대한 정리를 해보려고 한다.

## Playstore

[Playstore 콘솔](https://play.google.com/apps/publish)에 가면 내 앱을 어디에 배포할지 설정할 수 있다. 글로벌 시대라고는 하지만 언어는 해당 국가에서 비롯되었기 때문에, 어떤 언어를 선택하여 서비스 할 것인지는 가장 중요하다. 만약 영어로 서비스를 하게 된다면 한국어만 사용하는 유저보다는 훨씬 많은 지역의 사용자의 Pool을 얻을 수 있을 것이다. 당연한 이야기지만, 해당 지역에 서비스를 한다면 그에 맞는 쾌적한 서비스를 제공해야 한다. 특정 언어 혹은 나라에 대해서 다른 스크린샷 다른 앱 설명 문구 등을 적용하면서 사용자에게 도움을 줄 수 있다. 요즘은 [Playstore에서도 A/B 테스트](https://developer.android.com/distribute/best-practices/grow/store-listing-experiments.html?hl=ko)를 할 수 있어졌기 때문에 위와 같은 부분을 테스트하면서 각 상황에 맞는 적절한 그림/문구 등을 제공하면서 앱을 소개할 수 있을 것이다.

## 무슨 말인지 알아야지

앱을 새로 설치했는데 내가 모르는 언어로 되어있다면, 과연 앱 정상적으로 사용할 수 있을까? 기존에 사용하던 익숙한 앱이라면 모를까. 무슨 말인지 모른다면 사용자는 앱에서 action을 해보기 전까진 당최 알 수가 없다. 여러 나라에 지원한다면 여러 나라 언어를 사용자에게 제공해야한다. 

제목이나 설명문이나 버튼 글귀 등은 사실 은근히 자주 바뀌는 텍스트리소스들이다. 그래서 대부분 서비스를 하고 있는 사람들이 관리를 하기 위해서 strings.xml에 key-value형태로 리소스를 참조하여 사용하고 관리하게 된다. 

여러 언어를 관리하기 위해서 아래와 같이 언어별로 values 패스를 나누어서 strings.xml을 생성하면 된다. 이렇게하면 실제로 리소스를 가져다가 사용하지만 코드에서는 언어 분기처리를 하지 않아도 된다. 사용자가 안드로이드에서 시스템설정언어에 맞는 언어가 보여지게 된다. (해당 언어 리소스가 없는 경우에는 default strings.xml을 참조)

````
MyProject/
    res/
       values/
           strings.xml
       values-es/
           strings.xml
       values-fr/
           strings.xml
````

이게 각각 언어들마다 파일이 존재하다보니 관리하는 측면에서 불편할 수 있지만, 우리의 Android Studio는 [Translations Editor](https://developer.android.com/studio/write/translations-editor.html)를 제공하고 있다. Excel sheet처럼 관리하기 편하다.

하지만 위의 방법도 사실상 모바일 서비스의 큰 장점이자 단점인 배포라는 제약이 따른다. 배포가 되야만 변경된 언어를 제공할 수 있다는 것이다. (물론 Remote Config 등을 통해서 몇몇 부분들은 변경할 수 있겠지만) Flitto에서는 더 빈번하고 더 많은 언어의 서비스를 제공하기 위해서 위와같은 strings.xml를 대신해서 언어 Set들을 앱 로딩 시에 받아와서 Realm에 저장해두고 가져다 쓴다. 장점은 서비스되는 언어들이 Android/iOS/Web에서 실시간으로 변경될 수 있다는 점이 겠지만 변경될 때마다 앱 시작 시, 로딩 시간이 길어진다는 단점도 존재한다.

다국어를 지원하다보면 또 하나의 문제가 생긴다. 바로 Layout이다.

언어마다 같은 말을 해도 길이가 다 다르다. 경험상 러시아어가 다른 언어보다 긴 경우가 많은 것 같다. 그렇기에 고정된 Layout에서 별다른 조치를 취하지 않았다면 언어가 그냥 잘려 보일 것이다. 이 부분은 사실 UI/UX와 관련이 깊어서 디자인 파트와도 커뮤니케이션을 통해서 결정해야한다. 몇 줄로 제한해서 보여줄지 혹은 이부분은 가변적인 Layout을 채택할 것인지 등을 결정하면 된다.

## [RTL (Right to Left)](https://material.io/guidelines/usability/bidirectionality.html#bidirectionality-ui-mirroring-overview) 

오른쪽에서 왼쪽으로 쓰고 읽는 언어가 있다. 아랍어. 17 이상 버전 부터 RTL support가 제공된다고 한다.
[https://android-developers.googleblog.com/2013/03/native-rtl-support-in-android-42.html](https://android-developers.googleblog.com/2013/03/native-rtl-support-in-android-42.html)

## 언어만 다를까?

해외여행가면 꼭 해야하는 게 있다. 환전. 그 나라의 돈을 사용해야 한다. 서비스에서 실제 cash를 표현하는 부분에서 각 나라의 통화를 사용하여 표시하는 것도 중요한 부분이다. 

돈 말고도 사실 모든 단위들이 나라마다 다른 경우가 가끔씩 있다. 큰 문제는 아니겠지만 이부분이 서비스와 밀접한 관계가 있다면 충분히 중요하게 생각해야 할 부분이다. 

reference : 

[https://developer.android.com/training/basics/supporting-devices/languages.html?hl=ko](https://developer.android.com/training/basics/supporting-devices/languages.html?hl=ko)

[https://developer.android.com/develop/quality-guidelines/building-for-billions.html](https://developer.android.com/develop/quality-guidelines/building-for-billions.html)

