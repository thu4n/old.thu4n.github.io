---
title: Ôn tập Quản trị mạng và hệ thống - Viết gì vào tờ A4?
date: 2023-06-25 23:56:00 +0700
author: thu4n
categories: [Learning]
tags: [computer network, network administration, sysadmin]
mermaid: true
math: true
image: /assets/img/other/preview.png
description: Những nội dung chính nên ghi vào tờ tài liệu A4 cho bài thi cuối kỳ môn học Quản trị mạng và hệ thống của UIT
---
## I. Lời mở đầu

Chào các bạn, mục tiêu của bài viết này là tóm tắt lại kiến thức của môn học Quản trị mạng và hệ thống của UIT. Qua đó, ta có thể phần nào hình dung được nên ghi những gì vào tài liệu **duy nhất** được phép sử dụng trong phòng thi - một tờ A4 viết tay (không được phép in hoặc photo). Các kiến thức được mình tổng hợp từ tài liệu của trường và một số nguồn khác, mình sẽ để trích dẫn ở cuối bài viết.

## II. Thông tin về bài thi
1. Hình thức thi: Trắc nghiệm (5 điểm) và Tự luận (5 điểm)

    a) Trắc nghiệm: 25 câu

    b) Tự luận: 3 câu (Cấu hình, Xử lý sự cố, Câu hỏi mở)

2. Thời gian thi: 75 phút

3. Nội dung: Các nội dung đã học
+ Cơ sở hạ tầng mạng: Định tuyến, Chuyển mạch, Dịch vụ mạng (NAT, ACL, DHCP)
+ Hệ thống: Windows, Linux
+ Câu hỏi tình huống
+ Các kiến thức, kỹ năng xoay quanh các nội dung đã học. (Chắc ý nói cách troubleshoot)

4. Thí sinh được sử dụng 01 tờ A4 viết tay, lưu ý: Chỉ viết tay, không được in/photo.

**Nhận xét chung:** 75 phút cho 25 câu trắc nghiệm và 3 câu tự luận, thật sự là mình thấy hơi ít và khoai. Đề có câu hỏi mở nên tờ A4 dù có soạn kĩ thì chắc là cũng sẽ không ăn thua với nó, trừ khi ăn hên.

## III. Tới lúc cù lao rồi

Với những nội dung trên, mình sẽ chia nội dung ôn tập thành 3 phần với 2 phần nội dung lý thuyết và một phần đoán mò mấy câu tự luận.

### Cơ sở hạ tầng mạng

Nhắc đến hạ tầng mạng thì chắc chắn là sẽ nói đến 2 anh là **Router** và **Switch** kèm theo rất nhiều câu chuyện về cấu hình. Nên trước khi chúng ta đi sâu hơn thì bạn nên đảm bảo là mình hiểu rõ khái niệm và vai trò của 2 anh này trong các hệ thống mạng (Đã được học ở Nhập môn mạng).

**Định tuyến**

Hiểu đơn giản, định tuyến là xác định đường đi từ một node A đến một node B. Chúng ta được học 2 loại định tuyến đó là định tuyến tĩnh và định tuyến động.
- **Định tuyến tĩnh** (static routing): Đây là cách định tuyến thủ công hay nói cách khác là người quản trị sẽ trực tiếp cấu hình từng đường đi cho các Router. Ta có 2 loại định tuyến tĩnh chính là static default route và static route đến một mạng cụ thể.

    + **Static default route** nghĩa là một đường đi mặc định cho Router, khi không biết phải chuyển gói tin đi đâu (không có thông tin trong bảng định tuyến) thì nó sẽ chọn đường này. Câu lệnh cấu hình như sau:
```
    Router(config)# ip route 0.0.0.0 0.0.0.0 { exit-intf | next-hop-ip }
```

        Trong đó, `exit-intf` là interface đi ra của gói tin trên Router đó còn `next-hop-ip` là địa chỉ IP của một Router lân cận. Khi cấu hình, chỉ cần điền một trong 2 trường interface hoặc địa chỉ IP. Nên dùng interface
        
- **Định tuyến động** (dynamic routing)