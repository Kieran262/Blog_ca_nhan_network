---
title: "TCP và UDP trong Java: So sánh qua trải nghiệm học tập"
subtitle: ""
description: "Mình chia sẻ những thử nghiệm của mình với TCP và UDP trong Java, cùng những nhận xét cá nhân về khi nào nên dùng giao thức nào."
date: 2025-12-23
author: "Vũ Trường Thịnh"
image: "img/contact-bg.jpg"
tags: ["java", "tcp", "udp", "network"]
categories: ["Java"]
---

> Khi học về TCP và UDP, mình đã từng thắc mắc: "Tại sao lại cần hai giao thức? Khi nào dùng cái nào?" Bài viết này là những ghi chép của mình sau khi thực hành với cả hai.

## TCP: Đáng tin cậy nhưng chậm hơn

TCP (Transmission Control Protocol) là giao thức hướng kết nối. Mình hiểu nó giống như gửi thư có xác nhận: bạn gửi thư và đợi người nhận xác nhận đã nhận được. Nếu không nhận được xác nhận, bạn sẽ gửi lại.

Trong Java, mình dùng `ServerSocket` và `Socket` cho TCP. Điểm mạnh của TCP là đảm bảo dữ liệu được gửi đúng thứ tự và không bị mất. Nhưng điểm yếu là chậm hơn UDP vì phải đợi xác nhận.

## UDP: Nhanh nhưng không đảm bảo

UDP (User Datagram Protocol) là giao thức không kết nối. Mình ví dụ nó giống như hét to trong đám đông: bạn hét và hy vọng người nghe được, nhưng không chắc chắn.

Trong Java, mình dùng `DatagramSocket` và `DatagramPacket` cho UDP. Điểm mạnh là nhanh, nhưng điểm yếu là không đảm bảo dữ liệu đến đích và có thể không đúng thứ tự.

## Khi nào dùng gì?

Qua thực hành, mình rút ra:
- **Dùng TCP khi:** Cần đảm bảo dữ liệu đến đích (như gửi file, chat, web)
- **Dùng UDP khi:** Tốc độ quan trọng hơn độ tin cậy (như game online, streaming video)

## Kết luận

Cả hai giao thức đều có vai trò riêng. Việc chọn giao thức nào phụ thuộc vào yêu cầu của ứng dụng. Mình khuyên bạn hãy thử cả hai để hiểu rõ sự khác biệt.



