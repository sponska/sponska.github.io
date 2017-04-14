---
layout: post
title: 무한스크롤(infinity scroll)
category: Java Javascript
date:   2017-03-31
tags:  Java Javascript 무한스크롤 InfinityScroll Paging
author: SiHun
---

* content
{:toc}

무한스크롤(infinity scroll)
===========================
컨텐츠의 내용이 많아 스크롤시 ajax 로 불러오는 코드를 짯다.
```javascript
$(window).scroll(function () {
            if($(window).scrollTop() >= $(document).height() - window.innerHeight){
                getList()
            }
        });
```
첫줄 $(window).scroll()

>Description: Bind an event handler to the "scroll" JavaScript event, or trigger that event on an element.

