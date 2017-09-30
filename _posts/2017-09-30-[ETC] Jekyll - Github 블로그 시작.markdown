---
layout:     post
title:      "[ETC] Jekyll Blog 시작"
subtitle:   "블로그 시작"
date:       2017-09-30 00:00:00
author:     "gwonhyeong"
header-img: "img/home-bg.jpg"
---

작업물 기록및 정리용 블로그

<p>코드 예제</p>

<hr>
{% highlight html %}

{% raw %}
{% highlight c++ %}
{% endraw %}

#include <iostream>

template < typename T >
class handler {
public:
  handler( T&& h ) : _handler(std::move(h)){
  }
  ~handler( void ) {}
private:
  T _handler;
}

{% raw %}
{% endhighlight %}
{% endraw %}

{% endhighlight %}



{% highlight c++ %}
#include <iostream>


template < typename T >
class handler {
public:
  handler( T&& h ) : _handler(std::move(h)){
  }
  ~handler( void ) {}
private:
  T _handler;
}

{% endhighlight %}

<hr>


<p>문단</p>


<hr>

{% highlight html %}
<p>문단</p>
{% endhighlight %}
<p>문단</p>

<hr>

{% highlight html %}
<h2 class="section-heading">소제목</h2>
{% endhighlight %}

<h2 class="section-heading">소제목</h2>

<hr>

{% highlight html %}

<h2 class="section-heading">소제목</h2>

<a href="#">
    <img src="{{ site.baseurl }}/img/home-bg.jpg" alt="Post Sample Image">
</a>
<span class="caption text-muted">이미지 설명</span>

<blockquote>인용문</blockquote>
{% endhighlight %}

<a href="#">
    <img src="{{ site.baseurl }}/img/home-bg.jpg" alt="Post Sample Image">
</a>
<span class="caption text-muted">이미지 설명</span>

<blockquote>인용문</blockquote>


<hr>
