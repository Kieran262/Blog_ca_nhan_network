---
title: "Xây dựng mô hình Client–Server đơn giản bằng Java: Góc nhìn người mới học"
subtitle: ""
description: "Hướng dẫn từng bước xây dựng ứng dụng Client-Server đơn giản bằng Java Socket, dành cho người mới bắt đầu học lập trình mạng."
date: 2025-12-23
author: "Vũ Trường Thịnh"
image: "img/contact-bg.jpg"
tags: ["java", "client-server", "socket", "network"]
categories: ["Java"]
---

> Khi học về Client-Server, mình đã từng thắc mắc: "Tại sao lại cần mô hình này? Tại sao không phải là cách khác?" Bài viết này là câu trả lời mà mình tự tìm ra thông qua việc thực hành.

## Hiểu về Client-Server theo cách của mình

Trước khi học lập trình mạng, mình nghĩ rằng mọi chương trình đều hoạt động độc lập. Nhưng khi được giới thiệu về Client-Server, mình mới nhận ra rằng trong thực tế, các ứng dụng thường chia thành hai phần: một phần cung cấp dịch vụ (server) và một phần sử dụng dịch vụ đó (client).

Mình ví dụ như một nhà hàng: server giống như nhà bếp, nơi chuẩn bị thức ăn. Client giống như khách hàng, người đặt món và nhận thức ăn. Nhà bếp không tự động mang thức ăn ra, mà phải đợi khách hàng yêu cầu. Tương tự, server không tự động gửi dữ liệu, mà phải đợi client yêu cầu.

## Xây dựng server đầu tiên của mình

Mình quyết định xây dựng một server đơn giản có thể nhận yêu cầu từ client và trả lời. Đây là cách mình hiểu về `ServerSocket`:

```java
public class MyFirstServer {
    private static final int PORT = 8888;
    
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("Server đang chờ kết nối tại port " + PORT);
            
            while (true) {
                Socket clientSocket = serverSocket.accept();
                System.out.println("Có client kết nối: " + 
                    clientSocket.getRemoteSocketAddress());
                
                handleClient(clientSocket);
            }
        } catch (IOException e) {
            System.err.println("Lỗi server: " + e.getMessage());
        }
    }
    
    private static void handleClient(Socket socket) {
        try (socket;
             BufferedReader in = new BufferedReader(
                 new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8));
             PrintWriter out = new PrintWriter(
                 new OutputStreamWriter(socket.getOutputStream(), StandardCharsets.UTF_8), true)) {
            
            String request = in.readLine();
            System.out.println("Nhận được yêu cầu: " + request);
            
            String response = processRequest(request);
            out.println(response);
            
        } catch (IOException e) {
            System.err.println("Lỗi xử lý client: " + e.getMessage());
        }
    }
    
    private static String processRequest(String request) {
        if (request == null) {
            return "Không nhận được yêu cầu";
        }
        
        if (request.equalsIgnoreCase("HELLO")) {
            return "Xin chào! Server đang hoạt động tốt.";
        } else if (request.equalsIgnoreCase("TIME")) {
            return "Thời gian hiện tại: " + LocalDateTime.now();
        } else {
            return "Không hiểu yêu cầu: " + request;
        }
    }
}
```

Khi viết code này, mình đã học được nhiều điều. Đầu tiên là về `accept()`. Hàm này sẽ "chặn" chương trình lại, đợi cho đến khi có client kết nối. Ban đầu mình không hiểu tại sao lại như vậy, nhưng sau đó mình nhận ra rằng đây là cách để server luôn sẵn sàng phục vụ.

## Viết client để test server

Sau khi có server, mình cần một client để test. Mình viết một client đơn giản có thể gửi yêu cầu và nhận phản hồi:

```java
public class MyFirstClient {
    private static final String SERVER_HOST = "localhost";
    private static final int SERVER_PORT = 8888;
    
    public static void main(String[] args) {
        try (Socket socket = new Socket(SERVER_HOST, SERVER_PORT);
             BufferedReader in = new BufferedReader(
                 new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8));
             PrintWriter out = new PrintWriter(
                 new OutputStreamWriter(socket.getOutputStream(), StandardCharsets.UTF_8), true);
             Scanner scanner = new Scanner(System.in)) {
            
            System.out.println("Đã kết nối với server!");
            System.out.println("Nhập lệnh (HELLO, TIME, hoặc QUIT để thoát):");
            
            while (true) {
                System.out.print("> ");
                String command = scanner.nextLine();
                
                if (command.equalsIgnoreCase("QUIT")) {
                    break;
                }
                
                out.println(command);
                String response = in.readLine();
                System.out.println("Server trả lời: " + response);
            }
            
        } catch (IOException e) {
            System.err.println("Lỗi kết nối: " + e.getMessage());
        }
    }
}
```

Khi chạy cả hai chương trình, mình cảm thấy rất thú vị. Mình có thể gõ lệnh trên client và nhận được phản hồi từ server. Đây là lần đầu tiên mình thực sự hiểu được cách hai chương trình có thể giao tiếp với nhau.

## Những điều mình học được

Qua việc xây dựng ứng dụng này, mình đã học được nhiều điều quan trọng:

**1. Server phải luôn sẵn sàng:** Server không thể "ngủ" hay tắt đi. Nó phải chạy liên tục trong vòng lặp `while(true)` để luôn sẵn sàng nhận kết nối mới.

**2. Mỗi client là một kết nối riêng:** Mỗi lần `accept()` trả về một `Socket` mới, đại diện cho một client riêng biệt. Mình cần xử lý mỗi client một cách độc lập.

**3. Giao thức phải rõ ràng:** Server và client phải thống nhất về cách giao tiếp. Trong ví dụ của mình, mình dùng dòng text đơn giản, nhưng trong thực tế có thể phức tạp hơn nhiều.

**4. Xử lý lỗi là quan trọng:** Mình đã học được cách sử dụng try-with-resources để đảm bảo socket được đóng đúng cách, và cách xử lý các exception có thể xảy ra.

## Thắc mắc và câu trả lời

Trong quá trình học, mình có nhiều thắc mắc. Ví dụ như: "Tại sao không thể có nhiều client cùng lúc?" Câu trả lời là: với code hiện tại, server chỉ xử lý một client tại một thời điểm. Nếu muốn xử lý nhiều client cùng lúc, mình cần dùng thread (đó là chủ đề của bài viết tiếp theo).

Một thắc mắc khác: "Làm sao để client biết khi nào server đã gửi xong dữ liệu?" Đây là vấn đề về giao thức. Trong ví dụ của mình, mình dùng `readLine()` nên client sẽ đọc đến khi gặp ký tự xuống dòng. Nhưng trong các ứng dụng phức tạp hơn, có thể cần gửi độ dài dữ liệu trước, hoặc dùng một ký tự đặc biệt để đánh dấu kết thúc.

## Kết luận

Xây dựng mô hình Client-Server đơn giản này đã giúp mình hiểu được nền tảng của lập trình mạng. Mình nhận ra rằng mọi ứng dụng mạng phức tạp đều bắt đầu từ những khái niệm cơ bản này. Bây giờ mình đã sẵn sàng để học về thread và cách xử lý nhiều client cùng lúc.

Nếu bạn cũng đang học như mình, mình khuyên bạn hãy tự tay viết code và chạy thử. Đừng chỉ đọc lý thuyết. Chỉ khi tự làm, bạn mới thực sự hiểu được cách mọi thứ hoạt động.



