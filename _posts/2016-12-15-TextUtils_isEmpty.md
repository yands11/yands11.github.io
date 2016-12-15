---
layout: post
title: Java - TextUtils의 isEmpty()
---

# Java - TextUtils의 isEmpty()

---

안드로이드 개발을 하면 Java를 쓰다보니 API를 통해 받게 되는 String 값들, 뭐 네트워크를 떠나서도 String을 받는 부분에서 null체크를 하기 마련이다. 또한 ""으로 공백으로 값을 받는 경우도 고려해야한다.  

기존에는 아래와 같은 방식을 많이 썼다. `TextUtils.isEmpty()` 를 몰랐던 건 아니었는데 코드 스타일을 따라가다보니 저렇게 썼던 것 같기도하고 말그대로 그냥 썼다.  

## 기존 사용해 보았던 방식  
{% highlight Java %}  
public void checkString(String stringValue) {
    if (stringValue != null && stringValue.trim().lenght > 0) {
        Log.d("TAG", "Value is exist.");
    } else {
        Log.d("TAG", "Value is not exist.");
    }
}
{% endhighlight %}

지금은 isEmpty를 쓰고 있다.  

## 현재 사용하는 방식  
{% highlight Java %}  
public void checkString(String stringValue) {
    if (!TextUtils.isEmpty(stringValue)) {
        Log.d("TAG", "Value is exist.");
    } else {
        Log.d("TAG", "Value is not exist.");
    }
}
{% endhighlight %}

별 건 아닌데 `isEmpty()` 는 뭐라고 정의 되어있는지 궁금해서 안드로이드 문서를 뒤져보았다.  
**Description에 Return true if the string is null or 0-length.**  
말 그대로 null이랑 길이가 0인지 boolean으로 알려주는 참 고마운 녀석이었다.  

-끗-  


cf) [안드로이드 참조문서 - TextUtils](https://developer.android.com/reference/android/text/TextUtils.html)  
