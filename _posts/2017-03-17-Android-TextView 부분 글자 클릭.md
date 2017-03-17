---
layout: post
title: 'Android - TextView 부분 글자 클릭'
tag: 'Android'
---

# Android - TextView 부분 글자 클릭

--------------------------------------------------------------------------------

## TextView의 일정 부분만 클릭되도록 수정하기    

앱에서 주로 이용약관 동의 같은 설명 글에 '이용약관'라는 글자만 클릭해서 해당되는 내용을 보여줘야할 때가 있다.  

이럴 때, Spannable을 사용하면 간단하게 구현할 수 있다.  

{% highlight java %}  
TextView tv;
String totalStr = "xxx의 이용약관 동의에 동의하는 것으로로 간주됩니다.";
String targetStr = "이용약관 동의";
int startIndex = totalStr.indexOf(targetStr);
int endIndex = startIndex + targetStr.length();

SpannableStringBuilder sp = new SpannableStringBuilder(totalStr);
sp.setSpan(new ClickableSpan() {
    @Override
    public void onClick(View widget) {
        // TO DO SOMETHING
        Toast.makeText(getContext(), "이야호", Toast.LENGTH_SHORT).show();
    }
}, startIndex, endIndex, Spannable.SPAN_INCLUSIVE_INCLUSIVE);
tv.setMovementMethod(new LinkMovementMethod());   // 꼭 넣어줘야함!
tv.setText(sp, TextView.BufferType.SPANNABLE);
{% endhighlight %}  


자 됐다.

--------------------------------------------------------------------------------
