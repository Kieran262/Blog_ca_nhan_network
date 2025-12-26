---
title: "Sử dụng Fetch API để giao tiếp với Server – Bài thực hành mình thích nhất"
subtitle: ""
description: "Mình chia sẻ về bài thực hành yêu thích của mình khi học Fetch API, cùng những điều mình học được và ý tưởng cho dự án tương lai."
date: 2025-12-23
author: "Vũ Trường Thịnh"
image: "img/contact-bg.jpg"
tags: ["javascript", "fetch-api", "http", "practice"]
categories: ["JavaScript"]
---

> Fetch API là cách hiện đại để gửi HTTP request trong JavaScript. Đây là bài thực hành mình thích nhất.

## Fetch API là gì?

Fetch API là một cách mới để gửi HTTP request trong JavaScript. Nó dựa trên Promise, nên dễ sử dụng hơn XMLHttpRequest.

## Ví dụ cơ bản

```javascript
fetch('/api/data')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error('Error:', error));
```

## Với async/await

```javascript
async function getData() {
    try {
        const response = await fetch('/api/data');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error:', error);
    }
}
```

## POST request

```javascript
fetch('/api/data', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify({ name: 'John' })
})
.then(response => response.json())
.then(data => console.log(data));
```

## Kết luận

Fetch API giúp mình viết code giao tiếp với server một cách dễ dàng và hiện đại. Đây là công cụ mình sử dụng nhiều nhất khi làm việc với API.



