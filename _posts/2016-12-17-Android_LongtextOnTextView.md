---
layout: post
title: Android - TextView 긴글 생략하기
tag: Android, TextView
---

# Android - TextView 긴글 생략하기

---

UI에서 발생하는 문제 중에 받은 텍스트 데이터가 길어지면 다른 뷰를 밀어내서 정상적인 뷰가 보이지 않는 등의 문제가 생긴다.  

그래서 이것들을 해결하기 위한 여러가지 방법이 있다.  



#### 1 보여주는 line 수를 한정하기  

#### 2 첫번째, 중간, 마지막을 ... 처리하기  

#### 3 전광판처럼 텍스트를 흐르게 하기  



## 나는 9가지 상황을 테스트해 보았다.

1. singleLine="true"   
결과 : **한줄**로 표시 되고 **마지막이 ...** 처리가 된다.  

2. maxLines="1"   
결과 : **한줄**로 표시 되고 그냥 텍스트가 **짤린다.**  

3. maxLines="1" / ellipsize="start"   
결과 : **한줄**로 표시 되고 **마지막이 ...** 처리가 된다.  

4. maxLines="1" / ellipsize="middle"   
결과 : **한줄**로 표시 되고 **중간이 ...** 처리가 된다.  

5. maxLines="1" / ellipsize="end"   
결과 : **한줄**로 표시 되고 **마지막이 ... 처리**가 된다.  

6. android:singleLine="true" / android:ellipsize="marquee" / android:marqueeRepeatLimit="marquee_forever"   
결과 : **한줄**로 표시가 되고 **텍스트가 흐르면서** 모든 텍스트를 볼 수 있다.  

7. maxLines="2" / ellipsize="start"   
결과 : **두줄**로 표시 되고 그냥 텍스트가 **짤린다.**  

8. maxLines="2" / ellipsize="middle"   
결과 : **두줄**로 표시 되고 그냥 텍스트가 **짤린다.**  

9. maxLines="2" / ellipsize="end"   
결과 : **두줄**로 표시 되고 **마지막이 ...** 처리가 된다.  

참고로 ellipsize의 start와 middle은 maxLines가 1일 때만 적용이 된다.  
지금은 xml 코드 상으로 했지만, Java 코드에서도 `textView.setEllipsize(TextUtils.TruncateAt.START);` 를 해도 적용이 안된다.  
아래는 TextView의 setEllipsize 메소드에 대한 공식문서이다.  

> Causes words in the text that are longer than the view's width to be ellipsized instead of broken in the middle.  
You may also want to setSingleLine() or setHorizontallyScrolling(boolean) to constrain the text to a single line.  
Use null to turn off ellipsizing. If setMaxLines(int) has been used to set two or more lines,  
only END and MARQUEE are supported (other ellipsizing types will not do anything).  

2줄 이상이면 END와 MARQUEE만 적용된다고 한다.  

[안드로이드 참조문서 - TextView](https://developer.android.com/reference/android/widget/TextView.html#attr_android:ellipsize)


{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_gravity="center"
            android:orientation="vertical">

            <TextView
                android:id="@+id/tv_one"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="@string/longtext"
                android:singleLine="true"/>

            <TextView
                android:id="@+id/tv_two"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="16dp"
                android:text="@string/longtext"
                android:maxLines="1"/>

            <TextView
                android:id="@+id/tv_three"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="16dp"
                android:text="@string/longtext"
                android:maxLines="1"
                android:ellipsize="start"/>

            <TextView
                android:id="@+id/tv_four"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="16dp"
                android:text="@string/longtext"
                android:maxLines="1"
                android:ellipsize="middle"/>

            <TextView
                android:id="@+id/tv_five"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="16dp"
                android:text="@string/longtext"
                android:maxLines="1"
                android:ellipsize="end"/>

            <TextView
                android:id="@+id/tv_six"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="16dp"
                android:text="@string/longtext"
                android:singleLine="true"
                android:ellipsize="marquee"
                android:marqueeRepeatLimit="marquee_forever"/>

            <TextView
                android:id="@+id/tv_seven"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="16dp"
                android:text="@string/longtext"
                android:maxLines="2"
                android:ellipsize="start"/>

            <TextView
                android:id="@+id/tv_eight"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="16dp"
                android:text="@string/longtext"
                android:maxLines="2"
                android:ellipsize="middle"/>

            <TextView
                android:id="@+id/tv_nine"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="16dp"
                android:text="@string/longtext"
                android:maxLines="2"
                android:ellipsize="end"/>
        </LinearLayout>
    </ScrollView>
</RelativeLayout>
{% endhighlight %}   

 ![textview](https://yands11.github.io/assets/images/textview.png)  
