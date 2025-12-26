---
title: "Lập trình bất đồng bộ trong JavaScript – Khi mình không còn sợ callback"
subtitle: ""
description: "Hành trình của mình từ callback hell đến Promise và async/await, cùng những khó khăn và bài học rút ra."
date: 2025-12-23
author: "Vũ Trường Thịnh"
image: "img/contact-bg.jpg"
tags: ["javascript", "async", "promise", "callback"]
categories: ["JavaScript"]
---

> Lập trình bất đồng bộ là một khái niệm quan trọng trong JavaScript. Mình chia sẻ hành trình học tập của mình.

## Callback: Bắt đầu từ đây

Ban đầu mình dùng callback. Nhưng khi có nhiều callback lồng nhau, code trở nên rất khó đọc (callback hell).

## Promise: Giải pháp tốt hơn

Promise giúp mình viết code bất đồng bộ dễ đọc hơn. Mình học về `.then()`, `.catch()`, và `.finally()`.

## Async/Await: Cách viết tự nhiên nhất

Async/await giúp code bất đồng bộ trông giống code đồng bộ. Đây là cách mình thích nhất.

```javascript
async function fetchData() {
    try {
        const response = await fetch('/api/data');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error:', error);
    }
}
```

## Kết luận

Từ callback đến Promise và async/await, mình đã học được cách viết code bất đồng bộ ngày càng tốt hơn.



