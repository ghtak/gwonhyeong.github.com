---
layout:     post
title:      "[ETC] vscode build setting"
subtitle:   ""
date:       2017-10-29 00:00:00
author:     "gwonhyeong"
header-img: "img/home-bg.jpg"
--- 

Windows 환경에서 Visual Studio Code 를 이용한 c++ Build 환경 설정

1. Visual Studio Code 설치

2. CMake 설치

3.  <a href="http://landinghub.visualstudio.com/visual-cpp-build-tools">Visual Studio Build Tools 설치</a>
     
4. Visual Studio Code - CMakeTools 확장 설치

5. VSCode 에서 Ctrl + , 를 이용하여 CMakeTools 의 다음 설정을 override 

{% highlight html %}
 {
    "cmake.generator": "Visual Studio 15 2017",
 }
{% endhighlight %}

Build!

