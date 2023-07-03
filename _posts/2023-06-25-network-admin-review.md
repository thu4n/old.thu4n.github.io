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
        _**Hình 1. Topology minh họa**_

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

**VLAN - Virtual Local Area Network** tức là một LAN ảo được sử dụng cho việc phân đoạn các mạng chuyển mạch ở Layer 2. Nói cách khác, với VLAN, từ một LAN vật lý ta có thể phân chia ra thêm nhiều nhóm nhỏ tách biệt nhau và có traffic riêng. Từ đó, câu hỏi đặt ra là định tuyến giữa các VLAN này (inter-VLAN routing) như thế nào?

Cách định tuyến ban đầu là sử dụng các Ethernet interface của Router, mỗi interface đó được kết nối với một VLAN khác nhau. Dễ dàng nhận thấy rằng cách thức này không đáp ứng được về mặt **scaling** do số lượng interface vật lý trên một Router bị giới hạn đáng kể. Thay vào đó, người ta sử dụng 2 phương pháp khác là RoS và Multi-layer Switch để đáp ứng các nhu cầu về scaling.

- **RoS** (Router On a Stick)
    + Phương pháp này chỉ cần một Ethernet interface vật lý để phân luồng cho nhiều VLAN bằng cách sử dụng các **subinterface**. Đây là những interface ảo được tạo ra bởi phần mềm và nhờ vào đó, một interface vật lý có thể có nhiều subinterface. Các subinterface này có thể mang địa chỉ IP và được phân VLAN độc lập với interface của chúng.
    + Khi cấu hình RoS, cần đảm bảo interface của Router được để ở chế độ `802.1q trunk` và được kết nối đến một trunk port của Switch. Giải thích thêm, trunk là đường liên kết giữa Switch với một Switch khác hoặc một Router, cho phép mang nhiều tín hiệu cùng một lúc mà qua đó, cho phép nhiều VLAN sử dụng chung một interface vật lý để truyền thông.

    > Phương pháp này hiện tại chỉ hoạt động ổn định với **50 VLAN** trở xuống. Thường dùng trong các mạng có kích thước nhỏ và trung bình.
    {: .prompt-warning }

    + Để minh họa cách cấu hình, ta sẽ sử dụng topology kèm bảng thông tin về VLAN như hình sau:

    <img src="/assets/img/other/network-admin-2.png" alt="drawing" width="450"/>
    _**Hình 2. Topology mẫu có 3 VLAN**_
    
    Với cấu hình mặc định của các thiết bị, 2 PC trong hình sẽ không thể giao tiếp được với nhau mà chỉ có 2 Switch là giao tiếp được. Ta sẽ cấu hình định tuyến inter-VLAN theo kiểu Router On a Stick để khắc phục điều này.

    Đầu tiên, ta sẽ cấu hình cho switch S1 trước. Các bước thực hiện bao gồm tạo và đặt tên VLAN, tạo interface VLAN quản lý (VLAN 99) và cuối cùng là cấu hình switchport:
    ```
    S1(config)# vlan 10
    S1(config-vlan)# name LAN10
    S1(config-vlan)# exit
    S1(config)# vlan 20
    S1(config-vlan)# name LAN20
    S1(config-vlan)# exit
    S1(config)# vlan 99
    S1(config-vlan)# name Management
    S1(config-vlan)# exit
    S1(config)# interface vlan 99
    S1(config-if)# ip add 192.168.99.2 255.255.255.0
    S1(config-if)# no shutdown
    S1(config-if)# exit
    S1(config)# interface fa0/6
    S1(config-if)# switchport mode access
    S1(config-if)# switchport access vlan 10
    S1(config-if)# no shutdown
    S1(config-if)# exit
    S1(config)# interface fa0/5
    S1(config-if)# switchport mode trunk
    S1(config-if)# no shutdown
    S1(config-if)# exit
    S1(config)# interface fa0/1
    S1(config-if)# switchport mode trunk
    S1(config-if)# no shutdown
    S1(config-if)# exit
    ```
    Sở dĩ tạo VLAN quản lý (Management VLAN) như trên là để cho phép người quản trị có thể truy cập đến Switch thông qua kết nối IP (như là Telnet) và đồng thời cho phép các Switch truyền thông IP với nhau. Ở Switch S2 sẽ cấu hình tương tự như S1 (khác interface).

    Tiếp theo, Router On a Stick thì đương nhiên là sẽ cần cấu hình cho Router. Như bảng thông tin trong hình, ta sẽ chia interface `G0/0/1` của Router R1 ra 3 subinterface tương ứng với 3 VLAN đã tạo ở các Switch S1 và S2.

    ```
    R1(config)# interface G0/0/1.10
    R1(config-subif)# encapsulation dot1q 10
    R1(config-subif)# ip add 192.168.10.1 255.255.255.0
    R1(config-subif)# exit
    R1(config)# interface G0/0/1.20
    R1(config-subif)# encapsulation dot1q 20
    R1(config-subif)# ip add 192.168.20.1 255.255.255.0
    R1(config-subif)# exit
    R1(config)# interface G0/0/1.99
    R1(config-subif)# encapsulation dot1q 99
    R1(config-subif)# ip add 192.168.99.1 255.255.255.0
    R1(config-subif)# exit
    R1(config)# interface G0/0/1
    R1(config-if)# no shutdown
    R1(config-if)# exit
    ```

    Các dòng lệnh `encapsulation dot1q` kèm theo VLAN id là để cấu hình cho subinterface phản hồi lại các traffic được đóng gói theo chuẩn `802.1q` từ VLAN đó (10, 20 hoặc 99). Các địa chỉ được gán cho các subinterface của R1 chính là các default gateway cho mỗi VLAN tương ứng.

- **Multi-layer Swtich** (Switch Layer 3)
    + Đây là thiết bị Switch có thể vận hành được ở cả Layer 2 (Data-link) và Layer 3 (Network). Phương pháp này có một số lợi thế như sau:
        + Tốc độ nhanh hơn RoS do mọi thứ được xử lý ở phần cứng từ chuyển mạch cho tới định tuyến.
        + Không cần phải sử dụng thêm liên kết tới Router do anh Switch Layer 3 này lo hết rồi.
        + Các đường trunk không nhất thiết sử dụng chung 1 liên kết vật lý nữa do các liên kết EtherChannels ở Layer 2 có thể được dùng để làm liên kết trunk giữa các Switch với nhau => Tăng băng thông.
    + Dù mang nhiều ưu điểm và được sử dụng trong các mạng có kích thước từ trung bình đến lớn, nhược điểm lớn nhất của Switch Layer 3 đó là **chi phí cao** (Đồ xịn thì nó phải mắc thôi).
    + Cấu hình định tuyến cho Switch Layer 3:
    ```
    SwitchLayer3(config)# ip routing
    SwitchLayer3(config)# interface <interface-name>
    SwitchLayer3(config-if)# no switchport
    SwitchLayer3(config-if)# ip add <ip-address> <subnet-mask>
    SwitchLayer3(config-if)# no shutdown
    SwitchLayer3(config-if)# exit
    ```

        Lệnh `ip routing` là để kích hoạt định tuyến trên Switch. Sau đó, vào interface cần cấu hình làm cổng định tuyến (routed port) và dùng lệnh `no switchport` để interface này hoạt động ở Layer 3. Cuối cùng, gán địa chỉ IP cho interface đó.

        Các bước tạo và gán VLAN vào interace thực hiện tương tự như cấu hình cho Switch trong RoS.

## III. Dịch vụ mạng

Định tuyến chỉ mới giúp các node giữa các LAN hoặc VLAN giao tiếp được với nhau. Để đáp ứng các nhu cầu khác khi sử dụng mạng của người dùng lẫn người quản trị thì sẽ cần tới các dịch vụ mạng.

### NAT
- NAT - Network Address Translation là một dịch vụ mạng giúp chuyển đổi một địa chỉ IP private thành public và ngược lại. NAT thường được dùng để cho phép các thiết bị ở trong LAN đi ra Internet được.

![nat-table](/assets/img/other/network-admin-3.png)
_**Hình 3. Topology mẫu sử dụng NAT**_

- Để hiểu rõ hơn cách thức hoạt động tổng quan của NAT, ta xét hình bên trên (cắt từ tài liệu ra nên hơi bể). Giả sử trường hợp là PC muốn giao tiếp với web server, trong đó Router R2 đang chạy dịch vụ NAT. Các bước diễn ra như sau:
    1. PC gửi gói tin với địa chỉ IP đích là `209.165.201.1` (đây là địa chỉ IP public)
    2. Router R1 nhận được và chuyển tiếp đến R2
    3. Router R2 nhận được gói tin và tiến hành kiểm tra xem địa chỉ IP nguồn có cần được chuyển đổi không (trong trường hợp này là có).
    4. Router R2 thêm ánh xạ địa chỉ nguồn local tới địa chỉ nguồn global vào bảng NAT (`192.168.10.10` --> `209.165.200.226`)
    5. Router R2 chuyển tiếp gói tin với địa chỉ nguồn đã được chuyển đổi tới đích
    6. Web server phản hồi với gói tin gửi tới địa chỉ đích là địa chỉ inside global `209.165.200.226`.
    7. Router R2 nhận được, check địa chỉ đích và tìm trong bảng NAT với entry tương ứng. Sau đó, đổi nó thành địa chỉ inside local, tức địa chỉ IP của PC trong LAN.
- Trong NAT sẽ bao gồm 4 loại địa chỉ:
    + **Inside local address** - Địa chỉ nguồn của thiết bị gửi ở phần mạng cục bộ (Thường là địa chỉ IP private).
    + **Inside global address** - Địa chỉ nguồn của thiết bị gửi được chuyển đổi qua NAT và được sử dụng ở phần mạng toàn cục
    + **Outside local address** - Địa chỉ đích ở phần mạng cục bộ. Thông thường, địa chỉ này sẽ trùng với địa chỉ outside global.
    + **Outside global address** - Địa chỉ đích ở phần mạng toàn cục.
- Ta có 3 kiểu NAT là static NAT, dynamic NAT và PAT - Port Address Translation (còn được gọi là NAT overload). Bảng so sánh bên dưới sẽ bao gồm các thông tin về 3 anh này.

    |<center>Static NAT</center> | <center>Dynamic NAT</center>| <center>PAT</center>|
    |:-------|:--------|:--------|
    | Ánh xạ 1:1 giữa địa chỉ inside local <br>và inside global | Tương tự static NAT | Một địa chỉ inside global có thể <br> được ánh xạ tới nhiều địa chỉ <br> inside local|
    | Các ánh xạ được cấu hình tĩnh <br> bởi người quản trị | Tự ánh xạ theo cơ chế **First Come <br> First Serve** với một dãy các <br> địa chỉ public được tạo sẵn | Tự ánh xạ theo cơ chế **Next <br> Available Port** (*)|
    |Chỉ dùng IPv4|Tương tự static NAT|Dùng IPv4 kèm số port nguồn

    (*) - PAT sử dụng số port để phân biệt giữa các địa chỉ nguồn đang dùng chung một địa chỉ inside global. Do đó, nếu số port hiện tại đã được chiếm dụng, nó sẽ tìm số port thích hợp tiếp theo. Trong trường hợp không còn số port nào trống cho địa chỉ đó, PAT sẽ chuyển qua địa chỉ inside global kế tiếp.
- Cấu hình các kiểu dịch vụ NAT (topology và địa chỉ IP sẽ sử dụng lại của hình 3):
    + Static NAT
    ```
    R2(config)# ip nat inside source static 192.168.10.10 209.165.200.226
    R2(config)# interface s0/1/0
    R2(config-if)# ip nat inside
    R2(config-if)# exit
    R2(config)# interface s0/1/1
    R2(config-if)# ip nat outside
    R2(config-if)# exit
    ```
    Câu lệnh đầu tiên chính là việc ánh xạ tĩnh địa chỉ inside local tới địa chỉ inside global với `192.168.10.10` là địa chỉ IP của PC. Sau đó, ta cấu hình interface liên kết với LAN ở chế độ `nat inside` còn interface đi ra Internet ở chế độ `nat outside`.
    + Dynamic NAT
    ```
    R2(config)# ip nat pool NAT-POOL1 209.165.200.226 209.165.200.240 netmask 255.255.255.224
    R2(config)# access-list 1 permit 192.168.0.0 0.0.255.255
    R2(config)# ip nat inside source list 1 pool NAT-POOL1
    R2(config)# interface s0/1/0
    R2(config-if)# ip nat inside
    R2(config-if)# exit
    R2(config)# interface s0/1/1
    R2(config-if)# ip nat outside
    R2(config-if)# exit
    ```
    Câu lệnh đầu tiên dùng để định nghĩa một dãy các địa chỉ IP dùng cho việc chuyển đổi. Sau đó, tạo thêm một ACL để xác định những địa chỉ có dạng `192.168.X.X` mới cần được chuyển đổi (Khái niệm ACL sẽ nói ở phần nội dung của nó). Tạo xong ACL, ta ràng buộc nó vào danh sách địa chỉ IP nguồn inside - `ip nat inside source list`. Cuối cùng, cấu hình cho các interface tương tự như static NAT.

    + PAT

        Với PAT sử dụng một địa chỉ IPv4 duy nhất, ta chỉ việc thêm từ khóa `overload` sau khi ràng buộc ACL vào interface liên kết với LAN.
    ```
    R2(config)# ip nat inside source list 1 interface serial 0/1/0 overload
    R2(config)# access-list 1 permit 192.168.0.0 0.0.255.255
    R2(config)# interface serial0/1/0
    R2(config-if)# ip nat inside
    R2(config-if)# exit
    R2(config)# interface Serial0/1/1
    R2(config-if)# ip nat outside
    R2(config-if)# exit
    ```
        Với PAT sử dụng dãy địa chỉ IPv4, ta cấu hình tương tự dynamic NAT nhưng sẽ thêm từ khóa `overload` khi ràng buộc ACL.
    ```
    R2(config)# ip nat pool NAT-POOL1 209.165.200.226 209.165.200.240 netmask 255.255.255.224
    R2(config)# access-list 1 permit 192.168.0.0 0.0.255.255
    R2(config)# ip nat inside source list 1 pool NAT-POOL1 overload
    R2(config)# interface serial0/1/0
    R2(config-if)# ip nat inside
    R2(config-if)# exit
    R2(config)# interface Serial0/1/1
    R2(config-if)# ip nat outside
    R2(config-if)# exit
    ```
- Để kiểm tra cấu hình cho cả 3 kiểu NAT trên, ta sử dụng lệnh `show ip nat translation` và `show ip nat statistics`. 
    
### ACL

**Khái niệm về ACL**

**ACL - Access Control List** là một chuỗi các câu lệnh dùng để lọc các gói tin dựa trên thông tin có được từ header của gói tin đó. Trong ACL là một danh sách có thứ tự của các phát biểu `permit` hoặc `deny` - cho phép hoặc từ chối, còn được gọi là các entry. Khi một interface có ACL của Router nhận được gói tin nào đó, nó sẽ tiến hành so khớp gói tin với từng entry trong ACL theo thứ tự từ trên xuống. Quá trình so khớp sẽ dừng lại ngay khi khớp với một entry, bỏ qua các entry còn lại chưa duyệt.

Khi tạo ACL, tùy theo ngữ cảnh và nhu cầu sử dụng mà chúng ta có nhiều loại ACL khác nhau.

Xét về khả năng lọc gói tin (packet filter), ta có
- **Standard ACL**: Chỉ lọc dựa trên địa chỉ nguồn IPv4 => Chỉ hoạt động ở Layer 3

- **Extended ACL**: Lọc dựa trên địa chỉ nguồn/đích IPv4, số port TCP/UDP và các thông tin về loại giao thức => Hoạt động ở cả Layer 3 và 4.

Xét về cách đặt tên, ta có
- **Numbered ACL** (dùng số): Các ACL được đánh số trong đoạn 1 - 99 hoặc 1300 - 1999 là **standard** ACL. Còn đánh số trong các đoạn 100 - 199 hoặc 2000 - 2699 là **extended** ACL. Các số còn lại là dùng cho các mục đích khác, sẽ không đề cập ở đây.
- **Named ACL** (dùng từ ngữ): Đây là cách thông dụng hơn khi đặt tên cho ACL do nó cho phép người quản trị ghi chú tác dụng của một ACL cụ thể.

Xét về hướng interface để đặt ACL trên Router, ta có

![acl](/assets/img/other/network-admin-4.png)
_**Hình 4. Inbound và Outbound ACL**_

- **Inbound ACL** - Hướng vào trong Router: Inbound ACL lọc gói tin trước khi nó được xử lý định tuyến bên trong Router.
- **Outbound ACL** - Hướng ra khỏi Router: Outbound ACL lọc gói tin sau khi nó đã được định tuyến bởi Router.

> Inbound ACL được xem là hiệu quả hơn vì nó giúp bỏ qua việc xử lý định tuyến nếu gói tin bị hủy bỏ từ đầu. Tuy nhiên, tùy vào mỗi trường hợp khác nhau mà sẽ có lúc buộc phải dùng outbound hoặc ngược lại.
{: .prompt-info }

Nói về vị trí thiết lập ACL, đối với **standard** ACL thì nên đặt gần **đích** nhất có thể còn **extended** ACL thì nên đặt gần **nguồn** nhất có thể. Tất nhiên, các vị trị khuyến khích đó là khi điều kiện cho phép chứ không bắt buộc.

**Cấu hình ACL**

Để định nghĩa cho các entry hay nói tổng quát hơn là để ACL lọc gói tin theo điều kiện mong muốn của người quản trị thì sẽ cần sử dụng tới **wildcard mask** khi cấu hình.

Wildcard mask giống với subnet mask ở chỗ nó cũng sẽ xét từng bit trong một địa chỉ IPv4 để so khớp. Tuy nhiên, quy luật so sánh sẽ ngược lại hoàn toàn, theo đó:
- Wildcard mask bit 0 - So khớp giá trị bit tương ứng trong địa chỉ
- Wildcard mask bit 1 - Mặc kệ giá trị bit tương ứng trong địa chỉ

Để dễ hình dung rõ hơn về ý nghĩa wildcard mask, hãy xem bảng bên dưới:

|<center>Wildcard mask</center> | <center>Dạng nhị phân</center>| <center>Ý nghĩa</center>|
|:-------|:--------|:--------|
| 0.0.0.0  | 00000000 00000000 00000000 00000000| So khớp từng bit|
| 255.255.255.255 |  11111111 11111111 11111111 11111111 | Mặc kệ mọi bit|
|0.0.0.255|00000000 00000000 00000000 11111111|Chỉ so khớp 3 octect đầu|
|0.0.0.248|00000000 00000000 00000000 11111100|So 3 octect đầu và 2 bit cuối của octet cuối|

Wildcard mask sử dụng kết hợp với địa chỉ IP sẽ giúp người quản trị đặc tả được các điều kiện mong muốn của mình. Ta xét một số trường hợp cấu hình ACL (numbered ACL) thường dùng như sau:

1. Cho phép một host cụ thể

    ```
    Router(config)# access-list 1 permit 192.168.1.254 0.0.0.0
    ```
    Khi sử dụng wildcard mask `0.0.0.0` để so khớp từng bit thì chỉ có host nào mang đúng địa chỉ IP `192.168.1.254` mới được cho phép đi qua. Ngoài ra, ta có thể dùng từ khóa `host` thay cho wildcard mask để cấu hình cho trường hợp này vì chúng mang ý nghĩa như nhau.

    ```
    Router(config)# access-list 1 permit host 192.168.1.254
    ```

2. Cho phép các host của một subnet cụ thể
    ```
    Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
    ```
    Wildcard mask `0.0.0.255` sẽ chỉ so khớp 3 octet đầu của địa chỉ IP, còn lại mặc kệ. Do đó, tất cả các host thuộc subnet `192.168.1.0/24` sẽ được phép đi qua.
3. Cho phép mọi địa chỉ IP đi qua
    ```
    Router(config)# access-list 1 permit 255.255.255.255
    or
    Router(config)# access-list 1 permit any
    ```
    Câu lệnh thứ 2 là dùng từ khóa `any` sẽ mang cùng ý nghĩa với wildcard mask `255.255.255.255`. Thường đây sẽ là entry cuối cùng người quản trị nhập cho ACL để phòng trường hợp các mọi chỉ IP không khớp các entry trước đều bị chặn. Nguyên nhân là do khi tạo ACL, luôn luôn có một entry được tạo sẵn với nội dung `deny any` tức từ chối tất cả.

4. Từ chối mọi truy cập HTTP/HTTPS
    ```
    Router(config)# access-list 103 deny tcp any any eq 80
    Router(config)# access-list 103 deny tcp any any eq 443
    ```
    Do yêu cầu liên quan tới các giao thức nên cần dùng tới extended ACL được đánh số `103`. HTTP và HTTPS thì sẽ sử dụng TCP ở Layer 4 và số port lần lượt là 80 và 443. `any any` là chỉ bất kì địa chỉ nguồn và địa chỉ đích nào, `eq` tức là kiểm tra bằng (so sánh số port ở header).

    > Cấu hình extended ACL thì muôn vàn trường hợp nên là bạn có thể xem thêm các từ khóa [tại đây](https://www.cisco.com/c/en/us/td/docs/app_ntwk_services/waas/waas/v401_v403/command/reference/cmdref/ext_acl.html). Bạn nào sợ quên thì nên ghi luôn số port của các giao thức kèm theo dùng TCP hay UDP vào tờ A4.
    {: .prompt-info }

Với những trường hợp trên, nếu muốn dùng named ACL thì ta cấu hình như sau:
```
Router(config)# ip access-list standard PERMIT-HOST
Router(config-std-nacl)# permit 192.168.1.254 0.0.0.0
Router(config-std-nacl)# exit
Router(config)# ip access-list extended DENY-HTTP-HTTPS
Router(config-ext-nacl)# deny tcp any any eq 80
Router(config-ext-nacl)# deny tcp any any eq 443
Router(config-ext-nacl)# exit
```

Khi tạo xong ACL, ta cần đặt chúng vào các interface để đưa vào hoạt động (Sử dụng lại ACL 1 và 103).

```
    Router(config)# interface g0/0/0
    Router(config-if)# ip access-group 103 in
    Router(config-if)# ip access-group 1 out
    Router(config-if)# exit
```
Như trong đoạn lệnh minh họa là ta cho interface `g0/0/0` của Router chạy ACL 103 theo inbound còn ACL 1 là chạy theo outbound. Mỗi một interface của Router có thể chạy **tối đa** 4 ACL: inbound IPv4, inbound IPv6, outbound IPv4 và outbound IPv6.

Khi hoàn tất cấu hình, ta có thể kiểm tra lại với lệnh `show access-lists` để coi toàn bộ các ACL hiện có.

### DHCP

DHCP - Dynamic Host Configuration Protocol thực hiện quản lý và cấp phát tự động các địa chỉ IP đến các thiết bị mạng bên trong một mạng. Nói cách khác, thay vì phải đi gán IP thủ công cho từng thiết bị trong mạng thì người quản trị chỉ việc cấu hình DHCP cho Router là được.

Router được cấu hình DHCP sẽ coi như là DHCP server (cũng có thể là DHCP relay agent nhưng sẽ không nói chi tiết ở đây), còn các host trong mạng sẽ là các DHCP client. Việc trao đổi thông tin và cấp phát địa chỉ IP giữa 2 bên được chia thành 4 giai đoạn như sau:
1. DHCP DISCOVER - Client broadcats gói tin để tìm kiếm địa chỉ IP của DHCP server.
2. DHCP OFFER - Server nhận được và phản hồi với một "offer" gồm các địa chỉ IP khả dụng cho client.
3. DHCP REQUEST - Client broadcast thông điệp trong đó có bao gồm thông tin địa chỉ IP mong muốn
4. DHCP ACK/NAK - Server trả về ACK nếu địa chỉ IP đó vẫn còn khả dụng hoặc NAK nếu không khả dụng nữa.

Lý thuyết DHCP thì mình thấy không đề cập quá nhiều nên ta đi thẳng vào cách cấu hình luôn:
```
Router(config)# ip dhcp included-address 192.168.1.101 192.168.1.150
Router(config)# ip dhcp pool
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# domain-name cisco.com
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# exit
Router(config)# service dhcp vlan1
```
Câu lệnh đầu tiên là để khai báo dãy địa chỉ IP sẽ sử dụng trong DHCP. Các câu lệnh tiếp theo lần lượt là tạo pool DHCP, xác định subnet, tên miền, dns server và Router mặc định. Các DHCP client sẽ cần các thông tin này để yêu cầu cấp phát động địa chỉ IP. Cuối cùng, đưa DHCP vào hoạt động trên một interface mà ở đây là `vlan1`.

Ngoài ra, trong trường hợp mạng có một DHCP server riêng biệt liên kết trực tiếp tới Router, ta có thể cấu hình cho Router làm trung gian giúp các host ở subnet khác gửi được DHCP request của mình tới server. Đây gọi là cấu hình `ip helper-address`, còn Router được cấu hình sẽ trở thành DHCP Relay Agent. Xét hình mình họa bên dưới, được cắt ra từ đề ôn tập năm 2019.

![domain](/assets/img/other/network-admin-7.png)
_**Hình 5. Mô hình minh họa có một DHCP server riêng biệt**_

```
Router(config)# interface Gi0/0
Router(config-if)# ip helper-address 192.168.11.6
Router(config-if)# exit
```

Khi cấu hình như thế, interface phải là interface liên kết với subnet của các host cần gửi DHCP request và địa chỉ được IP thì sẽ là địa chỉ của DHCP server.

## IV. Quản trị Windows

### Mô hình Workgroup

<img src="/assets/img/other/network-admin-5.jpg" alt="drawing" width="450"/>
_**Hình 6. Setup mô hình workgroup**_

Workgroup là một môi trường dành cho các văn phòng loại nhỏ hoạt động theo hình thức mạng LAN **Peer-to-Peer**. Nó là một nhóm các máy tính chia sẻ tài nguyên và quyền quản lí với nhau. Do đó, workgroup có thể được tạo với các PC thông thường mà không cần phải có thêm một server.

Mình không nghĩ đây sẽ là nội dung được hỏi nhiều trong thi nên nguyên lý chi tiết và cách set up sẽ không được đề cập ở đây (Cầu nguyện thầy không đọc được cái này và quyết định cho nó vô thi).

### Mô hình Domain với Active Directory

Ngược lại với Workgroup, mô hình Domain hoạt động theo kiến trúc mạng **Client - Server**. Trong đó, một nhóm máy tính mạng cùng chia sẻ cơ sở dữ liệu thư mục tập trung.

![domain](/assets/img/other/network-admin-6.png)
_**Hình 7. Mô hình Domain minh họa**_

Việc quản lý và chứng thực người dùng mạng tập trung tại máy tính **Primary Domain Controller** (PDC). Domain controller (DC) là một Server quản lý tất cả các khía cạnh bảo mật của Domain. Các tài nguyên mạng cũng được quản lý tập trung và cấp quyền hạn cho từng người dùng. Lúc đó trong hệ thống có các máy tính chuyên dụng làm nhiệm vụ cung cấp các dịch vụ và quản lý các máy trạm. 

Ngoài ra, để có thể backup phòng trường hợp PDC không hoạt động được hoặc có lỗi xảy thì có thể thêm vào các Domain Controler ở các máy server khác. Các Domain Controller thêm vào được gọi là **Additional Domain Controller** (ADC), chúng còn có vai trò giúp cân bằng tải trong trường hợp traffic cao. 

Một loại DC khác nữa ta sẽ đề cập đó là **RODC - Read Only Domain Controller**. RODC cũng giống như các domain controller khác, ngoại trừ cơ sở dữ liệu Active Directory không thể ghi trực tiếp. RODC giúp làm giảm được một phần tải trọng của các máy chủ đầu cầu vì chỉ có lưu lượng bản sao gửi đến RODC là được cho phép, chứ các và tăng tính bảo mật vì người dùng kết nối với RODC không thể thay đổi bất cứ thứ gì trong cơ sở dữ liệu Active Directory. 

Các loại DC giới thiệu ở trên đều cần người quản trị cấu hình cho server của mình, nếu không, một Windows server mặc định sẽ hoạt động ở chế độ "stand-alone".

**Active Directory** là một hệ thống quản trị người dùng tập trung được tích hợp trong các Windows server sử dụng mô hình Domain, cụ thể hơn là được đặt trên các DC. Active Directory dùng để lưu trữ dữ liệu của domain như các đối tượng user, computer, group cung cấp những dịch vụ (directory services) tìm kiếm, kiểm soát truy cập, ủy quyền, và đặc biệt là dịch vụ xác thực người dùng.

> Mình tham khảo đề thi năm 2019 thì có hỏi một số lệnh liên quan tới Domain Controller. Nhưng trong quá trình học ở lớp thì mình không thấy nói gì đến các lệnh nào cả nên sẽ không viết về nội dung đó.
{: .prompt-warning }

### Các dịch vụ trên Windows Server

Windows Server cung cấp rất nhiều dịch vụ nhưng ở đây chúng ta sẽ chỉ nói một số dịch vụ tiêu biểu đó là DNS, HTTP và FTP.

**DNS**

Viết tắt của Domain Name System, được hiểu là hệ thống phân giải tên miền. Đây là một hệ thống chuyển đổi các tên miền website, chuyển từ dạng www.<tenmien>.com sang dạng địa chỉ IP tương ứng với tên miền và ngược lại. Bên cạnh đó, các thao tác này có DNS có vai trò lớn trong liên kết các thiết bị mạng với nhau trong việc định vị và gán địa chỉ cụ thể cho các thông tin trên internet.

Trong DNS, có các record (bản ghi) với các chức năng khác nhau như sau:
- NS - Name Server, chứa địa chỉ IP của DNS server cùng với một số thông tin về domain.
- A - Address, dùng để ánh xạ từ một domain thành địa chỉ IP cho phép có thể truy cập website.
- PTR - Pointer, chuyển đổi tền miền thành địa chỉ IP.
- CNAME - Canonical Name hay còn gọi là bí danh, cho phép người dùng truy cập tài nguyên thông qua các bí danh này. Một tên miền có thể có nhiều bí danh.
- MX - Chuyển tiếp mail đến domain.

Còn nhiều loại record khác nữa nhưng mình nghĩ biết 4 anh trên là đủ đi thi rồi.

Để cài đặt và cấu hình vụ DNS trên Windows server, ta thực hiện các bước như sau:
- Phía server
1. Thiết lập địa chỉ IP cho server và địa chỉ DNS server theo địa chỉ IP đó
2. Thiết lập DNS suffix (hậu tố, dạng như `thu4n.x.y` thì `x.y` chính là hậu tố)
3. Kích hoạt dịch vụ DNS trong Dashboard
4. Tạo Forward Lookup Zone để phân giải tên miền ra địa chỉ IP
5. Tạo Reverse Lookup Zone để dịch ngược từ địa chỉ IP ra tên miền

- Phía client
1. Thiết lập địa chỉ DNS server trong network setting
2. Dùng lệnh `nslookup` trong terminal để xem lại đã tham gia vào domain chưa

**Các dịch vụ khác**

~~Mình kiếm không ra để nói~~ . Hóa ra là mình bị mù quáng, có slide nói về quản trị Windows bao gồm chi tiết các dịch vụ mà nó cung cấp. Mình sẽ để link ở phần [Nguồn tham khảo](#viii-nguồn-tham-khảo) do bài viết này quá dài rồi.

## V. Quản trị Linux

Với Quản trị Linux, thì mình tham khảo chủ yếu là hỏi một số lệnh thiết yếu nhưng như vậy là cũng đủ nhiều rồi (nếu không muốn nói là quá nhiều). Bên dưới mình sẽ chỉ list các keyword, lệnh nà mình cảm thấy quan trọng thì mình sẽ đính kèm thêm link chứ không thể đặc tả chi tiết cách sử dụng từng lệnh được do quá dài. Ngoài ra, mình cũng sẽ để link tới tài liệu Quản trị Linux ở phần [Nguồn tham khảo](#viii-nguồn-tham-khảo) luôn.

### Tương tác với thư mục cơ bản

- `pwd` - Coi mình đang ở trong đường dẫn thư mục nào.
- `ls` - Liệt kê nội dung của thư mục hiện tại ra.
    + `ls -a` - Liệt kê toàn bộ nội dung, bao gồm các file bị ẩn
    + `ls -l` - Liệt kê thông tin chi tiết của các nội dung
- `mkdir` - Tạo thư mục kèm tên thư mục đó
- `cd` - Di chuyển đến một thư mục nào đó
    + `cd` hoặc `cd ~` hoặc `cd ~/` - Trở về thư mục `/home`
    + `cd ..` - Trở về thư mục cha
- `rmdir` - Xóa một thư mục **rỗng**

### Tương tác với file cơ bản
- `touch` - Tạo một file rỗng (Không phải công dụng chính của nó).
- `echo` - Viết đối số cho output tiêu chuẩn.
- `cat` - Hiển thị nội dung của file.
- `wc` - Đếm số dòng, số từ và số kí tự của một file văn bản.
- `cp` - Copy nội dung file qua một file khác.
- `mv` - Copy nội dung file nguồn đến một chỗ khác rồi xóa nội dung nguồn đó.
- `rm` - Xóa file (hoặc thư mục).
- `grep` - In ra các dòng trong file có nội dung khớp một pattern nào đó (nói cách khác là tìm kiếm).
- `chmod` - Thay đổi các quyền Read, Write và Execute của một file. Xem thêm cách sử dụng [tại đây](https://www.geeksforgeeks.org/chmod-command-linux/).

### Các lệnh liên quan đến network
- `ifconfig` - Xem các cấu hình interface của máy bao gồm địa chỉ IP, địa chỉ MAC và MTU.
- `iptables` - Thiết lập các network rule. Xem thêm cách sử dụng [tại đây](https://www.geeksforgeeks.org/iptables-command-in-linux-with-examples/).
- `ufw` - Cấu hình firewall trên Ubuntu. Xem thêm cách sử dụng [tại đây](https://learnubuntu.com/ufw-commands/).
- `nslookup` - Truy vấn DNS.
- `route` - Hiển thị routing table.
- `traceroute`- Xác định route đi đến một destination nào đó.

## VI. Basic Network Troubleshooting

Phần nội dung này cũng chính là phần nội dung chính trong phần thi tự luận. Những gì mình viết sẽ là góc nhìn cá nhân trong việc troubleshoot chứ không phải là cách hoàn hảo, làm theo là đúng.

Trước khi xem xét các vấn đề khác thì luôn kiểm tra xem cấu hình địa chỉ IP cho các thiết bị trong mạng đã đúng hết chưa, có cùng một subnet hay chưa và đảm bảo là các interface cần sử dụng đều đang ở trạng thái `on` đã cấu hình `no shutdown`.
### Vấn đề liên quan tới dịch vụ mạng

Đề thường sẽ nói như sau: "Mạng này đang chạy dịch vụ X, song PC A không sử dụng được dịch vụ X" => Khả năng cao là Router chạy dịch vụ đó được cấu hình sai, cần xem kĩ lại các lệnh.
- Host không nhận được địa chỉ IP -> **DHCP**: Xem lại dãy địa chỉ IP trong network pool đã khai báo đúng hoặc đủ chưa, cấu hình ip-helper đã đúng interface hay chưa
- Traffic của host bị chặn bất thường, các host không giao tiếp được -> **ACL**: Cấu hình `inbound` hoặc là `outbound` đã hợp lý chưa, xem lại vị trí đặt ACL trong mạng đã đúng chưa là 2 chuyện ưu tiên nhất. Nếu không có gì thì ta mới xét tới thứ tự các entry và tính hợp lệ của các entry đó.
- Host đi ra internet không được -> **NAT**: Kiểm tra lại interface `in` và interface `out` đã đúng chiều chưa. Còn lại là phụ thuộc vào việc đã cấp đủ số địa chỉ IP inside global để phục vụ số lượng thiết bị trong mạng hay chưa.

Trong cả 3 anh trên, ta đều phải đảm bảo interface của Router có đang chạy dịch vụ đó.

### Vấn đề liên quan tới định tuyến

Nếu đề không nói chạy dịch vụ mạng nào hết, chỉ nói các host không giao tiếp được với nhau như mong muốn thì ta chỉ xét về định tuyến. Điều đầu tiên cần là đảm bảo các thiết bị đang chạy cùng một chế độ định tuyến với nhau, một anh chạy RIP thì làm sao nói chuyện được với anh kia chỉ chạy OSPF.

- Đối với định tuyến tĩnh, ta chỉ cần nhớ là có đường đi thì phải có đường về. Bạn cấu hình route từ A đến B thì cũng phải cấu hình route từ B đến A mới nói chuyện được.
- Đối với định tuyến động và định tuyến các VLAN, lỗi mình thường thấy là cấu hình lộn interface với địa chỉ IP (không rõ có gì khác nữa không...)

## VII. Tổng kết và một số lưu ý

Trong bài viết này, có thể mình bỏ qua khá nhiều chi tiết nhưng do thời gian có hạn (còn ôn mấy môn khác nữa) nên mình chỉ viết tới đây. Một số kiến thức khác như cách **chia mạng con**, cách viết rule trong `iptables` hoặc cách đọc thông tin định tuyến qua lệnh `show ip route` với `show ip interface brief`, mình là nghĩ khá cần thiết và nên xem qua (ghi vô tờ A4 luôn cho chắc).

Nhìn độ dài của một bài viết còn thiếu nội dung này, ta kết luận rõ rệt là không thể nhét hết vô một tờ A4 được (hoặc chỉ là vấn đề kĩ năng). Nên mình khuyến khích là chỉ ghi những nội dung thực sự dài và khó nhớ, cái nào đã nắm chắc thì khỏi ghi để chừa chỗ trống cho mấy anh nào khoai hơn. Ngoài ra, có thể chia tờ A4 thành **nhiều cột** để tăng diện tích viết chữ lên một tí.

Và đây cũng là kết thúc cho bài viết chia sẻ nho nhỏ của mình, rất cảm ơn những ai đã đọc được đến đây và chúc các bạn có một kỳ thi cuối kỳ tốt đẹp 🎉.
## VIII. Nguồn tham khảo
1. Tài liệu lý thuyết và thực hành của môn học Quản trị mang và hệ thống, UIT.
2. [DHCP Configuration by Cisco](https://www.cisco.com/c/en/us/td/docs/routers/ir910/software/release/1_2/configuration/guide/ir910scg/swdhcp.pdf)
3. Tài liệu của Cisco nói chung.
4. Đề ôn tập môn học Quản trị mạng và hệ thống,
5. [Tài liệu Quản trị Windows](https://drive.google.com/file/d/1X1Sfj5IZqKlYxopSoK1vsD9-m-QpCtBp/view?usp=sharing)
6. [Tài liệu Quản trị Linux](https://drive.google.com/file/d/1jvCpfiJ4MPNSbmv5AJXtb2BG8PutOEp4/view?usp=sharing)
7. [Video bài giảng môn học của cô Trần Thị Dung](https://youtube.com/playlist?list=PLgN0LjU9JK-qEr52DVA6SIGSGM3mge7HF)