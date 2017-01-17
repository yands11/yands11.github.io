---
layout: post
title: Android - Keypad 너란 녀석
tag: Android, Keypad
---

# Android - Keypad 너란 녀석

---

안드로이드 개발을 하면서 왜 이거는 기본 라이브러리로 제공되지 않는 것인가?에 대한 질문을 가끔 갖게 될 때가 있다.  
그 중에 전부터 제일 별것도 아닌것 같으면서 짜증나게 했던 놈을 소개한다.  
바로 키패드다.  

## 키. 패. 드.  

이 망할 녀석은 안드로이드에서 키패드가 올라와있는지 체크하는 것도 없다.  
우회하는 방법으로 뷰 높이 계산으로 키패드가 올라와있는지 체크하는 방법들이 있더라.  

하 여튼 보통 사람들이 필요한 건   

> 내가 올라오라할 때 올라오고, 내려가라할 때 내려갔으면 좋겠다.    

라는 것인데...  


그래서 이쪽 저쪽 돌아다니면서 답을 찾아보고 이래저래 해봤지만 상황에 따라서 애매애매했던 적이 많아서 방법과 화면 적용하는 Tip을 가져왔다.  


## 키패드를 올려보자  

{% highlight Java %}  
@Override
public void onCreate() {
    super.onCreate();
    // ... 생략

    edtText.requestFocus();
    View focusView = getActivity().getCurrentFocus();
    if (focusView instanceof EditText) {
        showKeypad((EditText) focusView);
    }
}

public void showKeypad(EditText edt) {
    if (edt == null) { return; }
    InputMethodManager imm = (InputMethodManager) edt.getContext().getSystemService(Context.INPUT_METHOD_SERVICE);
    imm.showSoftInput(edt, InputMethodManager.SHOW_FORCED);
}
{% endhighlight %}


## 키패드를 내려보자  

{% highlight Java %}  
@Override
public void onPause() {
    super.onPause();
    // ... 생략

    View focusView = getActivity().getCurrentFocus();
    if (focusView instanceof EditText) {
        hideKeypad((EditText) focusView);
    }
}

public void hideKeypad(EditText edt) {
    if (edt == null) { return; }
    InputMethodManager imm = (InputMethodManager) edt.getContext().getSystemService(Context.INPUT_METHOD_SERVICE);
    imm.hideSoftInputFromWindow(edt.getWindowToken(), 0);
}
{% endhighlight %}


위와 같은 방법을 사용할 때 제일 주의해야 할 부분인데, 원하는 화면이 아닐때는 당연히 내려가야 하는 것이다.  
**강제로 올렸으니 강제로 내려줘야 한다.**  

## 그럼 어디서 올려야 하나  
보통 화면 진입 시 많이 올리기 때문에 Activity라면 onCreate()에 올리는 것이 일반적이다.  
(background상태에서 foreground상태로 돌아올 때 마다 올리고 싶다면 onResume()에 다가 넣어주면 될 것 같다.)  
개인적으로는 onCreate()에서 초기에 View들을 Initialize시키고 난 다음에만 한번 띄우는 게 제일 깔끔한 것 같다.  
아 맞다. 가끔 가다가 여기서 위에 showKeypad()를 호출 해도 먹통인 경우가 있는데 그럴 때에는 약간의 딜레이를 주면 된다.   

{% highlight Java %}
public void showKeypad(EditText edt, int delay) {
    if (edt == null) { return; }
    edt.postDelayed(new Runnable() {
           @Override
           public void run() {
               InputMethodManager imm = (InputMethodManager) edt.getContext().getSystemService(Context.INPUT_METHOD_SERVICE);
               imm.showSoftInput(edt, InputMethodManager.SHOW_FORCED);
           }
       }, delay);
}
{% endhighlight %}

## 그럼 어디서 내려야 하나    
음 보통 foreground로 넘어가는 onPause()에서 키패드를 내려주어야 한다. 이거는 웬만하면 여기서 하기 때문에 다른 부분은 생각해보지 않았다.  

하 그래도 매번 걸리적거렸던 녀석인 키패드를 정리하고 나니 목욕탕 갔다온 느낌이다.(?)  

참고사이트 되시겠다.  
[https://realm.io/kr/news/tmi-dismissing-keyboard-ios-android/](https://realm.io/kr/news/tmi-dismissing-keyboard-ios-android/)  
