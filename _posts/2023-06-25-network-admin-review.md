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
    R2(config-if) ip nat inside
    R2(config-if) exit
    R2(config) interface s0/1/1
    R2(config-if) ip nat outside
    R2(config-if) exit
    ```
    Câu lệnh đầu tiên chính là việc ánh xạ tĩnh địa chỉ inside local tới địa chỉ inside global với `192.168.10.10` là địa chỉ IP của PC. Sau đó, ta cấu hình interface liên kết với LAN ở chế độ `nat inside` còn interface đi ra Internet ở chế độ `nat outside`.
    + Dynamic NAT
    ```
    R2(config)# ip nat pool NAT-POOL1 209.165.200.226 209.165.200.240 netmask 255.255.255.224
    R2(config)# access-list 1 permit 192.168.0.0 0.0.255.255
    R2(config)# ip nat inside source list 1 pool NAT-POOL1
    R2(config)# interface s0/1/0
    R2(config-if) ip nat inside
    R2(config-if) exit
    R2(config) interface s0/1/1
    R2(config-if) ip nat outside
    R2(config-if) exit
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

## IV. Quản trị Windows

### Mô hình Workgroup

### Mô hình Domain với Active Directory

### Các dịch vụ trên Windows Server

## V. Quản trị Linux

### Tương tác với file và thư mục

### Các lệnh thường dùng trong Linux

## VI. Basic Network Troubleshooting

## VII. Nguồn tham khảo