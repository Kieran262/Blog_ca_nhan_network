---
title: "Hành trình làm quen với lập trình mạng bằng Java Socket"
subtitle: ""
description: "Câu chuyện của mình khi lần đầu tiên viết chương trình client-server bằng Java Socket, những khó khăn gặp phải và bài học rút ra."
date: 2025-12-23
author: "Vũ Trường Thịnh"
image: "img/contact-bg.jpg"
tags: ["java", "socket", "network", "học tập"]
categories: ["Java"]
---

> Đây là bài viết đầu tiên trong series về lập trình mạng của mình. Mình muốn ghi lại những cảm xúc và trải nghiệm khi lần đầu tiên làm cho hai chương trình Java có thể "nói chuyện" với nhau qua mạng.

## Tại sao mình lại học lập trình mạng?

Trước đây, mình chỉ biết viết những chương trình chạy trên một máy tính. Mỗi khi chạy code, mình thấy kết quả ngay trên màn hình console của chính mình. Nhưng một ngày, giảng viên hỏi: "Làm sao để chương trình trên máy tính này gửi dữ liệu cho máy tính khác?" Câu hỏi đó khiến mình tò mò.

Mình bắt đầu tìm hiểu về socket. Ban đầu, khái niệm này rất trừu tượng với mình. Socket là gì? Tại sao lại cần nó? Mình đọc tài liệu nhưng vẫn không hiểu rõ cho đến khi tự tay viết code.

## Lần đầu tiên viết ServerSocket

Mình nhớ rõ ngày đó. Mình mở IntelliJ IDEA và bắt đầu viết một class tên là `SimpleServer`. Code đầu tiên của mình trông như thế này:

```java
public class SimpleServer {
    public static void main(String[] args) {
        try {
            ServerSocket server = new ServerSocket(8080);
            System.out.println("Server đã khởi động!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

Mình chạy chương trình và thấy dòng "Server đã khởi động!" xuất hiện. Nhưng rồi… không có gì xảy ra tiếp theo. Chương trình cứ chạy mãi mà không làm gì cả. Mình nghĩ: "Có phải mình đã làm sai gì không?"

Sau đó mình mới hiểu ra rằng `ServerSocket` chỉ là việc mở một cửa để lắng nghe. Nó không tự động làm gì cả. Mình cần phải gọi `accept()` để chờ đợi client kết nối đến. Đó là bài học đầu tiên: server phải chủ động chờ đợi và xử lý kết nối.

## Viết client đầu tiên

Sau khi hiểu về server, mình quyết định viết một client đơn giản để kết nối với server. Đây là lúc mình gặp khó khăn thứ hai:

```java
public class SimpleClient {
    public static void main(String[] args) {
        try {
            Socket socket = new Socket("localhost", 8080);
            System.out.println("Đã kết nối!");
        } catch (IOException e) {
            System.out.println("Lỗi: " + e.getMessage());
        }
    }
}
```

Lần đầu chạy, mình gặp lỗi `Connection refused`. Mình nghĩ code của mình sai, nhưng thực ra vấn đề là mình đã tắt server trước khi chạy client. Bài học thứ hai: server phải chạy trước, và phải chạy liên tục để client có thể kết nối.

## Gửi và nhận dữ liệu đầu tiên

Khi cả server và client đã kết nối được với nhau, mình muốn thử gửi một tin nhắn đơn giản. Đây là lúc mình học về `InputStream` và `OutputStream`.

```java
// Trên server
Socket client = server.accept();
OutputStream out = client.getOutputStream();
out.write("Xin chào từ server!".getBytes());
out.flush();

// Trên client
InputStream in = socket.getInputStream();
byte[] buffer = new byte[1024];
int bytesRead = in.read(buffer);
String message = new String(buffer, 0, bytesRead);
System.out.println("Nhận được: " + message);
```

Lần đầu tiên thấy dòng "Xin chào từ server!" xuất hiện trên màn hình client, mình cảm thấy rất phấn khích. Đó là khoảnh khắc mình nhận ra rằng mình đã làm được điều gì đó thực sự thú vị: hai chương trình độc lập có thể giao tiếp với nhau!

## Những sai lầm mình từng mắc phải

Trong quá trình học, mình đã mắc rất nhiều lỗi. Một trong những lỗi phổ biến nhất là quên gọi `flush()`. Mình viết code gửi dữ liệu nhưng client không nhận được gì cả. Sau một hồi debug, mình mới phát hiện ra rằng dữ liệu vẫn còn trong buffer và chưa được gửi đi. Từ đó, mình luôn nhớ gọi `flush()` sau mỗi lần ghi dữ liệu.

Một lỗi khác là về encoding. Khi mình gửi tiếng Việt, client nhận được những ký tự lạ. Mình không hiểu tại sao cho đến khi một bạn trong lớp nhắc nhở về UTF-8. Mình đã học được rằng khi làm việc với text, luôn phải chỉ định encoding rõ ràng:

```java
BufferedReader reader = new BufferedReader(
    new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8)
);
```

## Bài học quan trọng nhất

Sau khi hoàn thành bài tập đầu tiên về socket, mình nhận ra một điều quan trọng: lập trình mạng không chỉ là về code. Nó còn về việc hiểu cách dữ liệu di chuyển từ điểm này sang điểm khác, cách các máy tính "nói chuyện" với nhau, và cách xử lý những tình huống không mong đợi (như mất kết nối, timeout, v.v.).

Mình cũng học được rằng việc tự tay viết code và gặp lỗi là cách học tốt nhất. Đọc tài liệu giúp mình hiểu khái niệm, nhưng chỉ khi tự code và debug, mình mới thực sự nắm được cách mọi thứ hoạt động.

## Hướng tiếp theo

Bây giờ mình đã hiểu cơ bản về socket, mình muốn thử những thứ phức tạp hơn. Mình đang nghĩ về việc xây dựng một ứng dụng chat đơn giản, nơi nhiều client có thể kết nối và gửi tin nhắn cho nhau. Đó sẽ là thử thách tiếp theo của mình.

Nếu bạn cũng đang bắt đầu học lập trình mạng như mình, mình khuyên bạn hãy bắt đầu từ những thứ đơn giản nhất. Đừng vội vàng, hãy để bản thân có thời gian hiểu từng khái niệm một. Và quan trọng nhất, đừng sợ mắc lỗi. Mỗi lỗi là một cơ hội để học hỏi.



