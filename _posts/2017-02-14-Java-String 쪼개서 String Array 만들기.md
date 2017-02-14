---
layout: post
title: 'Java - String 쪼개서 String Array 만들기'
tag: 'String 쪼개기'
---

# String 쪼개서 String Array 만들기

--------------------------------------------------------------------------------

## 단어장을 만들어야 했다.  
친구의 요청이었고 나의 아이디어가 BAMM하고 충돌해서 단어장 앱 하나를 만들어 보고자 했다.  
친구는 CVS파일을 불러와서 단어장을 사용했으면 좋겠다고 했다.  
CVS파일 ~ 이렇다.
그래서 `,`(Comma) 로 구분해서 텍스트를 쪼개서 하나는 Word(단어), 하나는 Meaning(뜻)으로 저장해야했다.  

File로 한줄씩 읽어와서 `,`(Comma) 로 구분해서 각각 String의 Array로 저장하기 위한 방법을 찾았다.  
String의 split이라는 method를 활용하는 것이다.  
parameter로 String값을 넣어주면 그 값으로 잘라서 Array에 담아서 돌려준다.  

{% highlight java %}  
String rawData = "Apple, 사과";
String[] array = rawData.split(", ");
{% endhighlight %}  

--------------------------------------------------------------------------------
