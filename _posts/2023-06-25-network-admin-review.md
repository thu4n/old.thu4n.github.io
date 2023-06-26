---
title: Ôn tập Quản trị mạng và hệ thống - Viết gì vào tờ A4?
date: 2023-06-25 23:56:00 +0700
author: thu4n
categories: [Learning]
tags: [computer network, network administration, sysadmin]
mermaid: true
math: true
image: /assets/img/other/network-cu-lao.png
description: Những nội dung chính nên ghi vào tờ tài liệu A4 cho bài thi cuối kỳ môn học Quản trị mạng và hệ thống của UIT
---
## I. Giới thiệu chung

Chào các bạn, mục tiêu của bài viết này là tóm tắt lại kiến thức của môn học Quản trị mạng và hệ thống của UIT. Qua đó, ta có thể phần nào hình dung được nên ghi những gì vào tài liệu **duy nhất** được phép sử dụng trong phòng thi - một tờ A4 viết tay (không được phép in hoặc photo). Các kiến thức được mình tổng hợp từ tài liệu của trường và một số nguồn khác, mình sẽ để trích dẫn ở cuối bài viết.

Thông tin về bài thi:

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

**Nhận xét:** 75 phút cho 25 câu trắc nghiệm và 3 câu tự luận, thật sự là mình thấy hơi ít và khoai. Đề có câu hỏi mở nên tờ A4 dù có soạn kĩ thì chắc là cũng sẽ không ăn thua với nó, trừ khi ăn hên. 

Với những nội dung trên, mình sẽ chia nội dung ôn tập thành 3 phần với 2 phần nội dung lý thuyết và một phần đoán mò mấy câu tự luận.

## II. Định tuyến
Định tuyến là quá trình chọn lựa các đường đi trên một mạng máy tính để gửi dữ liệu qua đó. Phần lý thuyết này sẽ bao gồm 2 trường hợp định tuyến đó là giữa các LAN với nhau và giữa các VLAN với nhau.
### Định tuyến giữa các LAN

Chúng ta được học 2 loại đó là định tuyến tĩnh và định tuyến động.
- **Định tuyến tĩnh** (static routing): Đây là cách định tuyến thủ công hay nói cách khác là người quản trị sẽ trực tiếp cấu hình từng đường đi cho các Router. Ta có 2 loại định tuyến tĩnh chính là static default route và static route đến một mạng cụ thể.

    + **Static default route** nghĩa là một đường đi mặc định cho Router, khi không biết phải chuyển gói tin đi đâu (không có thông tin trong bảng định tuyến) thì nó sẽ chọn đường này. Câu lệnh cấu hình như sau:
```
    Router(config)# ip route 0.0.0.0 0.0.0.0 { exit-intf | next-hop-ip }
```

        Trong đó, `exit-intf` là interface đi ra của gói tin trên Router đó còn `next-hop-ip` là địa chỉ IP của một Router lân cận. Khi cấu hình, chỉ cần điền một trong 2 trường là interface hoặc địa chỉ IP.
    + **Static route đến một mạng cụ thể** , cái tên đã nói lên tất cả, ta sẽ cấu hình cho Router để nó biết được nếu muốn chuyển tiếp gói tin đến một mạng cụ thể nào đó thì trước tiên, sẽ phải đi qua interface nào hoặc đi tới địa chỉ IP nào. Câu lệnh cấu hình như sau:
```
    Router(config)# ip route network mask {next-hop-ip | exit-intf }
```
        Trong đó, `network` là địa chỉ mạng còn `mask` là subnet mask của mạng đó. Xét topology minh họa bên dưới:
        ![topology](/assets/img/other/network-admin-1.png)
        Giả sử, Router R1 cần chuyển tiếp gói tin đến mạng `172.31.1.128/26`. Để làm được điều đó, Router R1 sẽ cần phải giao tiếp được với R3 và do 2 anh này **không** liên kết trực tiếp với nhau nên ta sẽ cần một anh Router trung gian - Router R2. Tóm lại, để đến được mạng đích, R1 sẽ cần phải đi qua R2 đầu tiên rồi ở R2, ta tiếp tục cấu hình cho đi đến R3. Câu lệnh cấu hình như sau:

        ```
        R1(config)# ip route 172.31.1.128 255.255.255.192 { Địa chỉ IP của R2 }
        ```

        ```
        R2(config)# ip route 172.31.1.128 255.255.255.192 { Địa chỉ IP của R3 }
        ```
        > Cả 2 địa chỉ IP đều phải là địa chỉ IP của interface đang kết nối trực tiếp với Router gửi
        {: .prompt-warning }

- **Định tuyến động** (dynamic routing): Đây là cách định tuyến tự động, người quản trị chỉ việc cấu hình chế độ định tuyến phù hợp cho các Router và sau đó, các Router này sẽ tự trao đổi thông tin với nhau. Tương tự như anh tĩnh, chúng ta cũng được học 2 chế độ định tuyến tự động đó là RIP và OSPF.

    + **RIP** (Routing Information Protocol): Theo trường phái Distance Vector với thuật toán sử dụng là Bellman-Ford, chi phí đường đi trong RIP sẽ được tính bằng **hop count** - số Router cần đi qua từ nguồn đến đích. RIP có 2 phiên bản là `v1` và `v2`, anh `v1` là dùng cho mạng Classful còn `v2` là dùng cho mạng Classless (Khái niệm Classful với Classless sẽ không giải thích ở đây). Sử dụng lại topology minh họa ở trên, ta tiến hành cấu hình RIPv2 cho Router R1 như sau:

    ```
        R1(config)# router rip 
        R1(config-router)# version 2 
        R1(config-router)# network 172.31.1.0
        R1(config-router)# network 172.31.1.192
        R1(config-router)# no auto-summary
    ```
   Như vậy, khi cấu hình, ta cần phải nhập các mạng mà Router đang kết nối trực tiếp tới như R1 là đang kết nối tới 2 mạng `172.31.1.0/25` và `172.31.1.192/30`. Kết thúc với câu lệnh `no auto-summary` để không phát sinh vấn đề Router tự gộp các mạng chung lớp lại.
   + **OSPF** (Open Shortest Path First): Theo trường phái Link State với thuật toán sử dụng là Dijkstra, chi phí đường đi trong OSPF phụ thuộc vào bandwidth của đường đi giữa các Router. OSPF có 2 phiên bản thường dùng là `v2` dành cho IPv4 và `v3` dành cho IPv6. Các lệnh cấu hình đầu tiên trong OSPF như sau:

   ```
    Router(config)# router ospf <process-id> 
    Router(config-router)# network <ip-address> <wildcard-mask> area <area-id>
   ```
   Trong đó, `process-id` là id cho tiến trình chạy OSPF trong Router (bạn nào học Hệ Điều Hành thì chắc sẽ không lạ) còn `area-id` là dạng như một mã vùng. Các router chạy OSPF chung một vùng với nhau thì phải có cùng `area-id` và luôn luôn có ít nhất 1 vùng với `area-id` là **0**. Area 0 này còn được gọi là backbone area, tất cả các area khác (nếu có) phải kết nối trực tiếp với nó. Với tính chất chia vùng như thế thì OSPF tiếp tục có 2 loại khi cấu hình là OSPF đơn vùng và OSPF đa vùng. Trong bài viết này sẽ chỉ đề cập đến việc cấu hình OSPF đơn vùng do bài thi chỉ yêu cầu như thế tức là sẽ sử dụng area 0, còn `process-id` khi thi thì đề cũng cho luôn (thường sẽ là 10).

   Còn một trường nữa cần giải thích đó là `wildcard-mask`. Đơn giản thì coi như anh này là bản anti của subnet mask còn vai trò cụ thể thì mình sẽ nói sau trong phần ACL. Để có được `wild-card mask`, ta chỉ cần lấy `255.255.255.255` trừ đi cho subnet mask. Ví dụ như subnet mask của mạng `172.31.1.128/26` là `255.255.255.192` thì `wild-card mask` tương ứng sẽ là `0.0.0.63`.

   Quay lại topology minh họa, ta sẽ cấu hình OSPF đơn vùng cho Router R1 với `process-id` là 10 như sau:

    ```
    R1(config)# router ospf 10
    R1(config-router)# network 172.31.1.0 0.0.0.127 area 0
    R1(config-router)# network 172.31.1.192 0.0.0.3 area 0
   ```

   Cách cấu hình trên là để Router quảng bá địa chỉ mạng với wildcard mask, còn một cách cấu hình khác nữa là quảng bá IP interface với wildcard mask `0.0.0.0` nhưng sẽ không nói lệnh chi tiết ở đây (Mình thấy nắm rõ một cách là đủ dùng khi thi rồi).

Trong mọi ví dụ ở trên từ định tuyến tĩnh cho tới định tuyến động, mình chỉ mới thực hiện ở 1 Router. Trên thực tế, để mọi node trong mạng giao tiếp được với nhau thì cần cấu hình định tuyến cho **toàn bộ** các Router trong mạng đó.

> Nếu bạn lười hoặc sợ tính toán sai subnet mask hoặc wildcard mask khi thi, bạn có thể tham khảo [bảng này](https://en.wikipedia.org/wiki/Wildcard_mask) và ghi hết nó vô tờ A4 cũng được.
{: .prompt-info }

### Định tuyến giữa các VLAN

**VLAN - Virtual Local Area Network** tức là một LAN ảo được sử dụng cho việc phân đoạn các mạng chuyển mạch ở Layer 2. Nói cách khác, với VLAN, từ một LAN vật lý ta có thể phân chia ra thêm nhiều nhóm nhỏ tách biệt nhau và có traffic riêng. Từ đó, câu hỏi đặt ra là định tuyến giữa các VLAN này như thế nào?

Cách định tuyến ban đầu là sử dụng các Ethernet interface của Router, mỗi interface đó được kết nối với một VLAN khác nhau. Dễ dàng nhận thấy rằng cách thức này không đáp ứng được về mặt **scaling** do số lượng interface vật lý trên một Router bị giới hạn đáng kể. Thay vào đó, người ta sử dụng 2 phương pháp khác là RoS và Multi-layer Switch để đáp ứng các nhu cầu về scaling.

- **RoS** (Router On a Stick)
- **Multi-layer Swtich** (Switch Layer 3)

## III. Dịch vụ mạng

### NAT

### ACL

### DHCP

## IV. Quản trị Windows

## V. Quản trị Linux

## VI. Basic Network Troubleshooting

## VII. Nguồn tham khảo