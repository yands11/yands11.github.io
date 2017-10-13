---
layout: post
title: "[Android] Gradle Update in Android Studio"
description: Android Studio에서 gradle 버전을 업데이트 하는 방법
image: 'https://i0.wp.com/www.starkreads.com/wp-content/uploads/2016/12/android-studio-logo.png?fit=1217%2C520'
category: 'blog'
introduction: Android Studio에서 gradle 버전을 업데이트 하는 방법
tags:
- android
---

> 닦고, 조이고, 기름칠.  

Android Studio 3.0(이하 AS) 이 발표되고 정식버전이 아직 나오기 전이지만, 많은 안드로이드 개발자들이 [Android Studio 3.0](https://developer.android.com/studio/preview/index.html?hl=ko)을 시도해보고 있다.  
AS 3.0의 새로운 점을 소개하고자하는 포스팅은 아니다.  

AS 3.0으로 개발하다가 `[Build] -> [Build APK(s)]`로 로컬에서 APK파일을 생성하려는 도중 다음과 같은 [이슈](https://stackoverflow.com/questions/44206433/gradle-version-3-3-does-not-support-fortask-method-on-buildactionexecuter/44254745)가 생겼다.  

gradle 버전을 올리면 해결될 것 같다.

그래서, gradle 버전을 올리기 위한 두 가지 방법을 소개하고자 한다.

## 1. Project Structure를 이용
1. AS 메뉴에서 `File` -> `Project Structure...` (혹은 `command + ;`)
2. Project -> Gradle Version 입력 칸 수정

## 2. gradle-wrapper.properties를 이용
1. gradle-wrapper.properties 파일 접근
2. `distributionUrl=https\://services.gradle.org/distributions/gradle-3.0-all.zip`에서 `3.0(현재 버전)` 수정 후
3. `Sync Now` 클릭

Reference Site : [https://developer.android.com/studio/build/gradle-plugin-3-0-0-migration.html#apply_plugin](https://developer.android.com/studio/build/gradle-plugin-3-0-0-migration.html#apply_plugin)
