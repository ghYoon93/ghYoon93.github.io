---
title: JavaScript의 탄생 배경과 async vs defer
categories: [JavaScript]
tags: [async, defer]
---



## JavaScript

#### 탄생배경

1993 Mosic Web Browser



#### 어떤 문제를 해결하기 위해 만들어졌는지?

HTML (Hyper Text Markup Language) 텍스트에 링크를 걸어 페이지와 페이지 사이의 이동

Scripting 언어로 동적인 웹사이트를 만들자

#### 어떤 분야에 활용할 수 있는지?

node.js (back-end)

react (mobile)

electron(desktop)



#### 뜨고 있는 다른 기술?

WA (Web Assembly)

JavaScript != Web APIs



#### js를 html에 포함할 때 async vs defer

**head**

parsing HTML blocked(fetching js, executing js) parsing HTML

js를 읽을 때까지 HTML을 모두 parsing할 수 없다.



**body**

parsing HTML  blocked(fetching js, executing js))

기본적인 컨텐츠는 빠르게 볼 수 있지만 js에 의존적이라면 완전한 페이지를 볼 수 없다.



**head + async**

parsing HTML(fetcing js) blocked(executing js) parsing HTML

병렬적으로 js를 다운 받을 수 있어서 빠르다.

다수의 js를 다운 받을 때 순서가 정해지지 않는다.



**head + defer**

parsing HTML(fetcing js) executing js