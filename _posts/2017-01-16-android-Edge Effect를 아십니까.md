---
layout: post
title: Android - Edge Effect를 아십니까
tag: Android, Edge Effect
---

# Android - Edge Effect를 아십니까

---

2017년이 되어 올리는 첫 포스팅이다.  
도대체 뭐했는지 모르겠는데 첫 달인 1월도 반이 가버렸다.  

자, 오늘은 간단하게 알게된 Edge Effect를 알아보겠다.  

![스크린샷](https://yands11.github.io/assets/images/20170116_0.png)  
설명을 돕기 위한 짤방.png  
Edge Effect는 po스크롤wer을 하면 생기는 녀석인데 맨 끝으로 갔을 때 생기는 Effect이다.  

이거는 Material Design 곧 v21 이상에서 적용되는 녀석이다.  
보통은 적용된 AppTheme의 ColorPrimary값으로 컬러가 나타난다.  

### 하.지.만.  

뭔가 이녀석의 컬러만 바꾸고 싶은 그런 충동이 든다면?  


![스크린샷](https://yands11.github.io/assets/images/20170116_1.png)   
다음과 같이 v21 이상 버전에 대응할 수 있는 styles.xml을 만들어 주고 적용하고있던 style에
아래 코드를 살포시 추가 하면 된다.  

{% highlight xml %}
<item name="android:colorEdgeEffect">@color/color_blabla</item>
{% endhighlight %}


별 유용하지않지만 언젠가 누군가에게 유용하게 사용되길 바라는 마음에 포스팅을 마친다.  
