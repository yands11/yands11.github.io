---
layout: post
title: Java - 삼항연산자를 Android에 적용하기
tag: Android, 삼항연산자, Java
---

# Java - 삼항연산자를 Android에 적용하기

---

요즘들어 자주 쓰게되는 삼항연산자...  
간단한 코드들은 `if`와 `else`로 쓰기보단 삼항연산자를 이용한다.  
삼항연산자는 '문'이 아닌 연산자이다.  
뭐 사람들의 의견이 분분하지만 단순히 참거짓 판별하는 로직이니 크게 차이는 없어보인다.  

안드로이드에서 `if - else`문을 삼항연산자로 바꾸면 무슨일이 일어나는지 보자.    

{% highlight java %}  
// if-else문
// isVisible에 따라 textView를 보여줄지 말지 결정하는 부분
if (isVisible == true){
    textView.setVisibility(View.VISIBLE);
} else {
    textView.setVisibility(View.GONE);
}
{% endhighlight %}  


{% highlight java %}  
// 삼항연산자
// isVisible에 따라 textView를 보여줄지 말지 결정하는 부분
textView.setVisibility(isVisible ? View.VISIBLE : View.GONE);
{% endhighlight %}  

짠 이런식으로 한줄이면 끝난다.  
위와 같이 어떤 View에 이거 아니면 저거로 세팅해주는 경우 많이쓴다.   

이 포스팅을 하면서 더 찾아보니 또 신박한 사용 예들이 있었다.  

{% highlight java %}  
// if-else문
// text가 null이면 text2에 ""값을 넣어주는 부분
if (text == null){
    textView.setText("");
} else {
    textView.setText(text);
}
{% endhighlight %}  

{% highlight java %}   
// 삼항연산자
// text가 null이면 text2에 ""값을 넣어주는 부분
textView.setText(text == null ? "" : text);
{% endhighlight %}   

또 한줄로 끝났다.   

마지막으로 `if-else`를 중첩으로 쓰듯 삼항연산자도 중첩으로 쓸 수 있다.  

{% highlight java %}  
// if-else문
// 90점이상 A, 80점 이상 B, 나머지 C
if (score >= 90){
    grade = "A";
} else {
    if (score >= 80) {
        grade = "B";
    } else {
        grade = "C";
    }
}
{% endhighlight %}  

{% highlight java %}  
// 삼항연산자
// 90점이상 A, 80점 이상 B, 나머지 C
grade = score >= 90 ? "A" : (score >= 80 ? "B" : "C");    
{% endhighlight %}  

크흐... 또 한줄로 끝이 났다.  

앞으로도 간단한건 삼항연산자를 애용해야겠다.  

![크흐](https://yands11.github.io/assets/images/hk.jpeg)  
