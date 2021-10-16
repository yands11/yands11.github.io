---
layout: post
title:  "jenv로 Java 버전을 관리해보자"
date:   2021-10-16 16:00:00 +0900
hash-tag: Java jenv
---

## 환경의 변화
개발자라면 새롭게 변화하는 모든 것에 대응해야 하는 것 같아요. 저도 안드로이드 개발을 하다보니 이제 최신 버전의 Android Gradle plugin을 사용하려면 Java 11 이상을 필요해졌습니다. 오늘 소개할 jenv은 Java를 사용하는 사용자라면 이런 어려움들을 충분히 해소할 수 있을 거라고 생각합니다.

## jenv
[공식 사이트](https://www.jenv.be/)의 설명을 참고해보니 'JAVA_HOME 환경 변수 세팅하는 방법을 깜박하지않는 커맨드 라인 툴'이라고 소개합니다.
이름에서 유추할 수 있듯 j(Java) + Env(environment)를 합쳐 만든 이름 같아요.

## 설치 및 설정

### 각 Java 버전 설치
저는 Java 8, 11을 때에 따라서 바꿔야 하는 상황이 종종 와서 아래처럼 brew로 [AdoptOpenJDK](https://adoptopenjdk.net/)를 설치해주었습니다.
{% highlight console %}
$ brew install --cask adoptopenjdk/openjdk/adoptopenjdk8
$ brew install --cask adoptopenjdk/openjdk/adoptopenjdk11
{% endhighlight %}

### jenv 설치
이제 jenv를 설치합니다.
{% highlight console %}
$ brew install jenv
{% endhighlight %}

### ~/.zshrc에 jenv를 셋업
{% highlight console %}
$ vi ~/.zshrc 

// 아래 두줄 추가
export PATH="$HOME/.jenv/bin:$PATH"
eval "$(jenv init -)"

$ source ~/.zshrc
{% endhighlight %}

### 설치해두었던 Java 버전들을 확인
저는 8, 11 두개가 표시되고 있습니다.
{% highlight console %}
$ ls /Library/Java/JavaVirtualMachines
adoptopenjdk-11.jdk adoptopenjdk-8.jdk
{% endhighlight %}

### jenv에 각 Java 버전을 추가
{% highlight console %}
$ jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
$ jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home
{% endhighlight %}

### jenv에 추가된 Java 확인
현재 설정된 버전 앞에 '*'표시가 있습니다.
{% highlight console %}
$ jenv versions
{% endhighlight %}

### Global 버전 설정
{% highlight console %}
$ jenv global openjdk64-11.0.11
// Java 버전 확인
$ java -version
{% endhighlight %}

### Local 버전 설정
특정 directory에서는 다른 버전을 사용해야하는 경우에는 아래와 같이 설정하면 됩니다.
{% highlight console %}
$ jenv local openjdk64-11.0.11
// Java 버전 확인
$ java -version
{% endhighlight %}

## 맺는 글
처음 사용할 버전만 jenv에 잘 추가해두면 언제든지 쉽게 jenv를 통해서 쉽게 Java 버전을 변경할 수 있으니 사용해보시면 좋을 것 같습니다.
