---
layout:     post
title:      "[web] Spring Boot + Vue.js"
subtitle:   "Boot + Vue.js"
date:       2018-03-01 00:00:00
author:     "gwonhyeong"
header-img: "img/home-bg.jpg"
---

Spring Boot + Vue.js 연동

<h2 class="section-heading"><a href="https://start.spring.io/">Boot 프로젝트 생성</a></h2>
<hr>

다음 의존성 필요

{% highlight gradle %}

compile('org.springframework.boot:spring-boot-starter-thymeleaf')
compile('org.springframework.boot:spring-boot-starter-web')

{% endhighlight %}    

<hr>
<h2><a href="https://www.npmjs.com/"> npm 설치</a></h2>
<hr>

<h2>디렉토리 구조 설정</h2>
    - root/backend : spring
    - root/frontend : vue
<hr>

<h2>Vue.js 프로젝트 생성및 동작 확인</h2>

{% highlight shell %}

$ npm install --global vue-cli
$ vue init webpack frontend
$ cd frontend
$ npm install
$ npm run dev

{% endhighlight %}

<hr>

<h2>Spring Vue.js 연동</h2>

출력 디렉토리 지정

frontend/config/index.js 

{% highlight script %}
...
  build: {
    // Template for index.html
    index: path.resolve(__dirname, '../../backend/src/main/resources/templates/index.html'),

    // Paths
    assetsRoot: path.resolve(__dirname, '../../backend/src/main/resources/static/'),
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
...
{% endhighlight %}

Add Controller - RootController.java

{% highlight java %}

package org.codex.mobile.manager.controllers;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class RootController {

    @RequestMapping("/")
    public String viewIndex(Model model) {
        model.addAttribute("greeting", "Hello, world!");
        return "index";
    }
}

{% endhighlight %}

Build Vue

{% highlight shell %}

$ nbm run build

{% endhighlight %}

Run Spring And Test!

<a href="#">
    <img src="{{ site.baseurl }}/img/2018_03_02/hello_vue.png" alt="">
</a>

Vue.js 스터디후 이후 진행


<hr>

