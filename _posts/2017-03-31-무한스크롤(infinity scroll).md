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

```javascript
$(window).scroll(function () {
        if($(window).scrollTop() >= $(document).height() - window.innerHeight){
            getList()
        }
});
```

```javascript
function getMatchHistoryList() {
    $("#loading").html('목록을 불러오는중 입니다.');
       $.post("http://example.com",function (list) {
        if(list){
            $.each(list , function (index , item) {
                $("#listTable").append(item);
            });
        }
    })
}
```
