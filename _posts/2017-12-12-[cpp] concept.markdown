---
layout:     post
title:      "[c++] concept"
subtitle:   "c++ concept"
date:       2017-12-12 00:00:00
author:     "gwonhyeong"
header-img: "img/home-bg.jpg"
---

http://en.cppreference.com/w/cpp/language/constraints

template type 의 named set of requirements

선언형식은 다음과 같으며 

{% highlight c++ %}

template < template-parameter-list >
concept concept-name = constrainte-expression;

{% endhighlight %}

사용방법은 다음과 같다

{% highlight c++ %}

template < typename T > requires concept-name
R function_name( param );

or

template < typename T >
R function_name( param ) requires concept-name;

// constraned c++20 function template
template < concept-name T >
R function_name( param );

{% endhighlight %}

cppreference 의 예제 코드는 이 포스팅을 작성할 시점에 다음과 같이 수정해야 작동한다.


{% highlight c++ %}

#include <string>
#include <locale>
using namespace std::literals;
 
// Declaration of the concept "EqualityComparable", which is satisfied by

// any type T such that for values a and b of type T,

// the expression a==b compiles and 

// its result is convertible to bool

template<typename T>
concept bool EqualityComparable = requires(T a, T b) { a == b; };
   
// template<EqualityComparable T> void f(T&&); 

// constrained C++20 function template

template<typename T>
void f(T&&) requires EqualityComparable<T> {}// long form of the same

// void f(EqualityComparable&&); 

// short form of the same (Concepts TS only, not C++20)

int main() {
  f("abc"s); // OK, std::string is EqualityComparable

  //f(std::use_facet<std::ctype<char>>(std::locale{})); // Error: not EqualityComparable 

}

{% endhighlight %}

c++11 부터 반영예정 이었던것 같은데 결국 c++20 에 포함예정인듯

g++ 에서 빌드 가능하며 -std=c++17 -fconcepts 옵션을 추가해야한다.

명확히 사용할 시점에 추가 정리할 것
