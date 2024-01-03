---
title: Ôn tập An toàn mạng máy tính - Cấp cứu 2 tờ A4
date: 2023-12-30 13:00:00 +0700
author: thu4n
categories: [Learning]
image: /assets/img/other/tet-cuu.png
tags: [computer network, network security, sysadmin]
---

Xin chào các bạn, trong bài viết lần này mình sẽ cố gắng tóm tắt về môn học NT101 - An toàn mạng máy tính theo chương trình giảng dạy cho lớp Mạng tại UIT. Nội dung khá là dài nhưng thầy đã rộng lượng cho tận **2 tờ A4** thì có vẻ sẽ đủ, thời gian làm bài nếu mình nhớ không lầm thì đâu đó khoảng **75 phút**. Nôm na là thế, chúng ta sẽ có **8 chương** tổng cộng cần phải ôn tập nên mình vô thẳng nội dung chính luôn thôi.
## I. Tổng quan

### 1 - Một số khái niệm

#### Dữ liệu

Dữ liệu là một tập hợp các dữ kiện, chẳng hạn như số, từ, hình ảnh, nhằm đo lường, quan sát hoặc chỉ là mô tả về sự vật. Dữ liệu gồm có 2 trạng thái:
- Transmission state (Dữ liệu đang được trao đổi, truyền tải)
- Storage state (Dữ liệu đang được lưu trữ)

Khi trao đổi dữ liệu, có 4 yêu cầu như sau:
1. **Tính bí mật** (Confidentiality) - Khả năng chỉ cho phép những người có quyền truy cập vào thông tin đó
2. **Tính toàn vẹn** (Integrity) - Khả năng đảm bảo rằng thông tin không bị thay đổi hoặc sửa đổi mà không được phép. 
3. **Tính không thể từ chối** (Non-repudiation) - Khả năng xác định người gửi hoặc người nhận một thông điệp.
4. **Tính sẵn sàng (Availability)** - Khả năng đảm bảo rằng thông tin hoặc hệ thống có sẵn khi cần thiết.

#### Một số khái niệm khác

- Tam giác bảo mật bao gồm 3 thành phần là **Security** (Restrictions), **Functionality**(Features) và **Usability**(GUI). Khi ta thiên về một thành phần nào thì 2 thành phần còn lại trong tam giác sẽ bị yếu đi. VD: Tăng tính bảo mật thì sẽ hạn chế hơn về tính năng và giao diện ít thân thiện người dùng hơn, ngược lại, nếu thêm nhiều tính năng mới thì dễ có lỗ hỏng bảo mật và người dùng sẽ gặp khó khăn hơn khi mới sử dụng lần đầu.
### 2 - Các kỹ thuật tấn công phổ biến và cơ chế phòng thủ

#### 2.1. Eavesdropping (Nghe trộm)

- Cách thức tấn công: Sử dụng một thiết bị mạng (router, card mạng…) và một ứng dụng (Tcpdump, Ethereal, Wireshark…) để giám sát lưu lượng mạng, bắt các gói tin đi qua thiết bị này. Từ đó, "nghe trộm" được thông tin từ các phiên trao đổi dữ liệu.
- Nhận xét: Thực hiện dễ dàng hơn với mạng không dây. **Không** có cách nào ngăn chận việc nghe trộm trong một mạng công cộng.
- Cách phòng chống: Mã hoá dữ liệu trước khi truyền chúng trên mạng.

#### 2.2. Cryptanalysis

- Cách thức tấn công: Sử dụng một hoặc nhiều phương pháp khác nhau để tìm kiếm thông tin hữu ích từ dữ liệu đã mã hoá mà không cần biết khoá giải mã. Điển hình là phương pháp giải mã thông qua thống kê **tần suất xuất hiện** của các ký tự.
- Nhận xét: Khi sử dụng phương pháp phân tích thông kê tần suất, cần phải sử dụng các công cụ toán học và máy tính có hiệu suất cao khi cipher text có kích thước lớn dần.
- Cách phòng chống: Sử dụng những giải thuật mã hoá không thể hiện cấu trúc thống kê trong chuỗi mật mã; Sử dụng khoá có độ dài lớn để chống Brute-force attacks.

#### 2.3. Password Pilfering

Một hình thức tấn công mạng nhằm đánh cắp mật khẩu của người dùng, có nhiều phương pháp khác nhau để thực hiện điều này.

**2.3.1. Guessing**

**2.3.2. Social engineering**

**2.3.3. Dictionary Attacks**

**2.3.4. Password Sniffing**


### 3 - Lý lịch của những kẻ tấn công

### 4 - Mô hình bảo mật cơ bản

## II. Malware

### 1 - Trojan

### 2 - Phòng chống Trojan

### 3 - Virus và các kỹ thuật của Virus

### 4 - Phòng chống Virus

## III. Các giải thuật mã hóa

### 1 - Giới thiệu về mã hóa

### 2 - Giải thuật mã hóa cổ điển

### 3 - Giải thuật mã hóa hiện đại

### 5 - Bẻ gãy một hệ thống mật mã

## IV. Mã hóa công khai và quản lý khóa

### 1 - Hệ mã hoá khoá công khai

### 2 - Giao thức trao đổi khoá Diffie-Hellman

### 3 - Hệ RSA

### 4 - Quản lý khoá

## V. Chứng thực dữ liệu

### 1 - Mã chứng thực thông điệp

### 2 - Hàm băm

### 3 - Chữ ký số

## VI. Giao thức bảo mật mạng

### 1 - Vị trí của mật mã trong mạng máy tính

### 2 - Cơ sở hạ tầng khoá công khai

### 3 - IPsec

### 4 - SSL/TLS

### 5 - PGP và S/MIME

### 6 - Kerberos

### 7 - SSH

## VII. Bảo mật mạng không dây

### 1 - Tổng quan và phân loại

### 2 - Các kiểu chứng thực

### 3 - WEP, WPA và WPA2

### 4 - Các mối đe dọa

### 5 - Phương pháp và công cụ tấn công

### 6 - Tấn công mạng Bluetooth

### 7 - Biện pháp phòng chống

### 8 - Công cụ bảo mật Wi-Fi

### 9 - Kiểm thử hệ thống

## VIII. Bảo mật mạng ngoại vi

### 1 - Tổng quan
### 2 - Bộ lọc gói tin (Packet Filters)
### 3 - Cổng mạch (Circuit Gateways)
### 4 - Cổng ứng dụng (Application Gateways)
### 5 - Bastion Hosts
### 6 - Cấu hình tường lửa
### 7 - Chuyển dịch địa chỉ mạng (NAT)
### 8 - TMG – Threat Management Gateway
