---
layout: post
title: Android - Components
---

# Component of Android

---

## 1. Activity  

사용자랑 상호작용할 수 있는 화면을 제공한다.  
(UI를 보여주고 사용할 수 있다)

### Activity의 Life Cycle  

* onCreate()
* onStart()
* onResume()  
[Activity Running...]
* onPause()
* onStop()
* onDestroy()

### Activity를 사용하려면 Manifest에 등록해라  

(AndroidManifest.xml) 안 그러면 오류난다

```
<manifest ... >
  <application ... >
      <activity android:name=".ExampleActivity" />
      ...
  </application ... >
  ...
</manifest >
```  

### 처음으로 뜨는 Activity에는 꼭 있어야 하는 Manifest element  

(AndroidManifest.xml)  

```
<activity android:name=".ExampleActivity" android:icon="@drawable/app_icon">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

### AppCompatActivity?  

Compat이 Compatible (사이좋게 지내는, 양립할 수 있는, 공존할 수 있는 이런 뜻.)  
이건 Android 버전이 다 다르니까 Activity의 호환성을 위한 Activity 이다.  
보통 그냥 새로운 Project생성해서 보면 default가 이 Activity를 extends하는 것으로 되어있다.

## 2.Service  

백그라운드에서 실행된다.
Activity와 별개로 작동한다.
쉽게 생각하면 앱을 종료해도 살아있을 수 있다.
예를 들어보자면 카카오톡에서 우리가 메시지를 받을 수 있는 것도 서비스에서 Push를 받았기 때문이다.

## 3.BroadcastReceiver  

Broadcast 말 그대로 방송이다.
주로 안드로이드 내에서 일어나는 일들에 대해서 귀를 열고 있으라고 하는 것이다.
예를 들면 단말기 전원이 켜졌을 때, 네트워크 연결이 되거나 끊어졌을 때 등과 같은 방송(?)이 나오면
그걸 받는 Receiver가 동작해서 특정 기능을 수행하는 것이다.

## 4.ContentProvider

이거는 안드로이드 단말내에 있는 것들을 접근하기 위한건데
그것들이 뭐냐면, 주소록, 앨범 등과 같은 것들이다.
DB에서 쿼리를 날려 데이터를 가져오듯
우리 단말기에 안에 있는 데이터들을 쿼리를 날려서 가져올 수 있는 것이다.
