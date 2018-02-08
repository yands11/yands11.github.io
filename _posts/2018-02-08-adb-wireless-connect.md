---
layout: post
title: "[Android] wireless connect with adb"
description: 선을 끊자.
image: 'http://cdn.mos.cms.futurecdn.net/n79ctQ9qxtdwu7NUNDfHgC-970-80.jpg'
category: 'blog'
introduction: 선을 끊자.
tags:
- android
---

안드로이드 개발을 하게되면 실행해보는 방법은 두가지중에 하나다.
* 에뮬레이터를 실행시켜서 테스트해보거나
* 직접 단말기에 연결해서 테스트해보거나

이 중에서 직접 단말기에 연결하지만 거추장스럽기만한 케이블을 사용하지 않으려고 한다.

오늘의 주인공
## ADB
Android Debug Bridge

ADB의 환경변수 설정 on Mac
[https://stackoverflow.com/questions/17901692/set-up-adb-on-mac-os-x](https://stackoverflow.com/questions/17901692/set-up-adb-on-mac-os-x)


### 무선 연결
준비과정이 필요하다.
우선 개발중인 컴퓨터와 실제 단말기가 같은 무선 네트워크 상에 있어야한다.

자 이제 터미널을 열어서 다음 과정을 수행하면 된다.
```
$ adb tcpip 5555
```
위에 5555는 포트 번호인데
포트 번호를 다르게해서 여러 단말을 연결할 수 있다.


단말기의 현재 IP주소를 확인해서 아래의 <device_ip_address>에 다음과 같이 같이 입력한다.
```
$ adb connect <device_ip_address>:5555
// example
$ adb connect 192.xxx.xx.x:5555
```

### 연결된 디바이스 확인
```
$ adb devices
```

### 연결 해제
```
$ adb disconnect
```

### 앱 APK 설치
컴퓨터에 apk 파일있는데 뭐 디바이스 연결 프로그램을 설치하기도
직접 옮기기도 귀찮지 않은가 adb로 바로 설치시킬수있다.
```
$adb install <path_to_apk>
```

### 파일 추출하기
반대로 안드로이드 단말에 있는 파일들을 가져올 수 있다.
```
$adb pull <path_of_file>
```
사실 이 방법을 통해서 설치된 앱의 APK파일을 추출할 수있다.
apk파일을 decompile해서 코드를 본다고 해도 난독화 되어있다.

### 끝
사실 위에 적은 것 말고도 adb로 할 수있는 것이 아주 많다.
아래의 링크에는 더 많은 것들을 설명하고 있다.


Reference : [https://developer.android.com/studio/command-line/adb.html](https://developer.android.com/studio/command-line/adb.html)
