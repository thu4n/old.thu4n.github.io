---
title: Ôn tập Nhập môn Mạng - Tầng Data link
date: 2023-02-04 23:50:00 +0700
author: thu4n
categories: [Learning]
tags: [computer network, data link layer]
mermaid: true
---
## Lời mở đầu
![img](/assets/img/other/thuanOnThi.png){: width="700" height="400" }
{: .nolineno}
Xin chào các bạn, mình viết bài này là để tự ôn tập lại kiến thức về tầng Data link trong NMM. Do đó, bài viết sẽ không đi chuyên sâu mà chỉ bao gồm *những nội dung chính* có thể ra trong bài thi **cuối kỳ** của môn này tại UIT.

Các kiến thức được mình chọn lọc từ tài liệu của trường và một số nguồn khác, mình sẽ để credit ở cuối bài.
## Các dịch vụ tầng Data link
### Đóng gói, truy cập liên kết (framing, link access)

- Đóng gói datagram vào trong frame, thêm header vào trailer.
- Truy cập kênh truyền nếu môi trường được chia sẻ.
- Các địa chỉ MAC được sử dụng trong các header để xác định nguồn và đích.

### Truyền tin cậy giữa các node lân cận (reliable delivery between adjacent nodes)

- Tương tự giao thức rdt (được đề cập trong tầng Transport).
- Ít khi được sử dụng trên các đường truyền ít lỗi bit (cáp quang, cáp xoắn đôi).

### Điều khiển luồng (flow control)

- Điều khiển tốc độ truyền giữa các node gửi và nhận liền kề nhau.

### Phát hiện và sửa lỗi (error detection and correction)

- Lỗi gây ra bởi suy giảm tín hiệu hoặc tiếng ồn.
- Bên nhận phát hiện sự xuất hiện lỗi -> Tín hiệu bên gửi cho việc truyền lại hoặc hủy bỏ frame bị lỗi.
- Bên nhận xác định và **sửa** các bit lỗi mà không cần phải truyền lại.

### Half-duplex và full-duplex

- Với half-duplex, các node tại đầu cuối của kết nối có thể truyền cho nhau, nhưng không thể cùng lúc.
- Với full-duplex, các node tại đầu cuối của kết nối có thể truyền cho nhau cùng lúc.

## Phát hiện và sửa lỗi trong tầng Data link

**EDC** = **E**rror **D**etection and **C**orrection bits.

**D** = **D**ata (dữ liệu được bảo vệ bởi kiểm tra lỗi, có thể chứa các trường header).

> Chức năng phát hiện lỗi không hoàn toàn đáng tin cậy 100%.
{: .prompt-warning }
Trường EDC càng **lớn** thì càng dễ phát hiện và sửa lỗi hơn.