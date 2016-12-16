---
layout: post
title: Google Static Maps API의 진실
---

# Google Static Maps API의 진실

---

안드로이드에서 MapView 대신에 관련 작업을 하다가 그냥 해당 위치 지도 이미지만 가져와서 ImageView에 넣어주도록 하려고 했다. 그렇게 해서 웹 API중 구글에서 제공하는 [Google Static Maps 개발자 가이드](https://developers.google.com/maps/documentation/static-maps/intro?hl=ko) 를 발견하였다.  

먼저 테스트로 웹에서  

> https://maps.googleapis.com/maps/api/staticmap?center=37.506452,127.054358&zoom=5&size=400x400&scale=1  

위와 같이 경/위도와 zoomLevel 그리고 이미지 사이즈, 스케일 등을 설정해주었다.  

## 그.런.데.  

![map1](https://yands11.github.io/assets/images/map1.png)  

일본해가 있는 것이 아닌가... 이런!! 혹시나 해서 language설정도 있길래 다음과 같이 수정해보았다.  

> https://maps.googleapis.com/maps/api/staticmap?center=37.506452,127.054358&zoom=5&size=400x400&scale=1&alanguage=ko  

자 이제 다시 호출

![map2](https://yands11.github.io/assets/images/map2.png)  

## ? ? ?  

영어로 해볼까?  

> https://maps.googleapis.com/maps/api/staticmap?center=37.506452,127.054358&zoom=5&size=400x400&scale=1&language=en

![map3](https://yands11.github.io/assets/images/map3.png)  

## Sea of Japan ? What the F...  

난 개발 전에 듣기로 분명 이제 구글에서는 동해로 표시되고 있겠거니했다... 좀 더 문서를 찬찬히 읽어보던 중~~(사실 공부를 했다기보다 동해가 나오는 방법을 찾았다)~~  
지역 설정을 할 수 있었다.

> https://maps.googleapis.com/maps/api/staticmap?center=37.506452,127.054358&zoom=5&size=400x400&scale=1&region=KR  

짜잔  

![map4](https://yands11.github.io/assets/images/map4.png)  

> https://maps.googleapis.com/maps/api/staticmap?center=37.506452,127.054358&zoom=5&size=400x400&scale=1&region=KR&&language=en  

짜잔 2(English)

![map5](https://yands11.github.io/assets/images/map5.png)  

일단 지역설정으로 동해가 표시되게하고 언어는 바꾸어도 동해가 계속 표시되고 있다.  
흐음... 하지만 지도의 **default** 가 동해가 아닌 일본해라는 것에 굉장히 놀랐고 분노했다. 다른 지역사람은 sea of japan으로 알고 있을 것이 아닌가...안타깝다  
오늘은 분노의 포스팅이었다.  
이러한 부분들은 우리가 더 노력해야 하는 부분인 것 같다.  

-끗-
