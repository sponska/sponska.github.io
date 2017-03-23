---
layout: post
title:  "Fisher–Yates shuffle"
categories: JavaScript
tags:  JavaScript
author: sponska
---

* content
{:toc}


![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/7c/Riffle_shuffle.jpg/320px-Riffle_shuffle.jpg)




## Fisher and Yates 

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Biologist_and_statistician_Ronald_Fisher.jpg/189px-Biologist_and_statistician_Ronald_Fisher.jpg)


```
-- To shuffle an array a of n elements (indices 0..n-1):
for i from n−1 downto 1 do
     j ← random integer such that 0 ≤ j ≤ i
     exchange a[j] and a[i]
```

## JavaScript 

```js
/**
 * Fisher–Yates shuffle
 */
Array.prototype.shuffle = function() {
    var input = this;

    for (var i = input.length-1; i >=0; i--) {

        var randomIndex = Math.floor(Math.random()*(i+1));
        var itemAtIndex = input[randomIndex];

        input[randomIndex] = input[i];
        input[i] = itemAtIndex;
    }
    return input;
}
```

- [Fisher–Yates shuffle From Wikipedia](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)
