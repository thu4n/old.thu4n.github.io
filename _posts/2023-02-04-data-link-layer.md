---
title: Ôn tập Nhập môn Mạng - Tầng Data link
date: 2023-02-04 23:50:00 +0700
author: thu4n
categories: [Learning]
tags: [computer network, data link layer]
mermaid: true
math: true
img: '/assets/img/other/thuanOnThi.png'
---
## I. Lời mở đầu
![img](/assets/img/other/thuanOnThi.png){: width="400" height="400" }
{: .nolineno}
Xin chào các bạn, mình viết bài này là để tự ôn tập lại kiến thức về tầng Data link trong NMM. Do đó, bài viết sẽ không đi chuyên sâu mà chỉ bao gồm *những nội dung chính* có thể ra trong bài thi **cuối kỳ** của môn này tại UIT.

>Các kiến thức được mình chọn lọc từ tài liệu của trường và một số nguồn khác, mình sẽ trích dẫn nguồn ở cuối bài.
{: .prompt-info }

## II. Các dịch vụ tầng Data link
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

## III. Phát hiện và sửa lỗi trong tầng Data link

**EDC** = **E**rror **D**etection and **C**orrection bits.

**D** = **D**ata (dữ liệu được bảo vệ bởi kiểm tra lỗi, có thể chứa các trường header).

> Chức năng phát hiện lỗi không hoàn toàn đáng tin cậy 100%.
{: .prompt-warning }
Trường EDC càng **lớn** thì càng dễ phát hiện và sửa lỗi hơn.
### Parity checking
Là kỹ thuật dùng thêm một số bit để đánh dấu tính chẵn lẻ:

- Parity **1** chiều: Thể hiện dưới dạng 1 ma trận có kích thước **1xM**
- Parity **2** chiều: Thể hiện dưới dạng 1 ma trận có kích thước **MxN**

**Parity bit 1 chiều**:

- Số bit parity: 1 bit
- Có 2 mô hình:
    - Mô hình chẫn: Số bit 1 trong dữ liệu gửi đi là một số chẵn.
    - Mô hình lẻ: Số bit 1 trong dữ liệu gửi đi là một số lẻ.

|<center>Bên gửi</center> | <center>Bên nhận</center>|
|:-------|:--------|
|1. Xác định mô hình parity|1. Nhận D' có (D+1) bits
|2. Thêm 1 bit parity vào dữ liệu| 2. Đếm số bits 1 trong D'
|3. Tiến hành gửi dữ liệu đi| 3. Đưa ra kết luận cho dữ liệu

**Parity bit 2 chiều**:

- Số bit parity: (N+M+1) bit.
- Có 2 mô hình chẵn lẻ tương tự parity bit 1 chiều.

|<center>Bên gửi</center> | <center>Bên nhận</center>|
|:-------|:--------|
|1. Xác định mô hình parity|1. Biểu diễn dữ liệu nhận được thành ma trận (M+1)(N+1)
|2. Biểu diễn dữ liệu thành ma trận  MxN| 2. Kiểm tra từng dòng và cột. Đánh dấu các dòng, cột bị lỗi
|3. Tính giá trị bit parity trên từng dòng, từng cột.<br>Sau đó gửi dữ liệu| 3. Đưa ra kết luận cho dữ liệu

Ví dụ minh họa bit parity 2 chiều:
![img](/assets/img/other/data-link-1.png){: width="550" height="200" }
{: .nolineno}

### Internet checksum
**Mục tiêu:** Phát hiện "các lỗi" (VD: Các bit bị lộn) trong packet được truyền.

> Chỉ được dùng trong tầng Transport
{: .prompt-warning }

|<center>Bên gửi</center> | <center>Bên nhận</center>|
|:-------|:--------|
|Xử lý các nội dung của một segment như<br>một chuỗi các số nguyên 16-bit|Tính toán checksum của segment vừa nhận
|Checksum: thêm (tổng bù 1) vào các nội<br> dung của segment|Kiểm tra giá trị checksum vừa tính được có bằng<br> giá trị trong trường checksum:<br> - Không bằng: Phát hiện lỗi <br> - Bằng nhau: Không có lỗi được phát hiện
|Bên gửi đặt các giá trị checksum vào trong <br>trường checksum của UDP|

### CRC (Cyclic Redundancy Check)

- Ta có `d` bit data, ký hiệu là `D`, cần được gửi đi.
- Cả 2 bên gửi và nhận sẽ thống nhất trước với nhau một mẫu `r + 1` bit, được gọi là một **generator**, ta sẽ ký hiệu là `G`. Yêu cầu MSB của `G` phải là **1**.

|<center>Bên gửi</center> | <center>Bên nhận</center>|
|:-------|:--------|
|Chọn thêm `r` bit, ký hiệu là `R`, gắn thêm vào `D` sao cho<br> `d + r` chia hết cho `G` theo module 2| Tiến hành chia `d + r` bit nhận được cho `G`. <br> Đưa ra kết luận dựa trên kết quả chia

Ta có các công thức tính toán như sau (mình lười format nên crop từ tài liệu ra):
![img](/assets/img/other/data-link-2.png){: width="550" height="100" }
{: .nolineno}
![img](/assets/img/other/data-link-3.png){: width="550" height="100" }
{: .nolineno}
![img](/assets/img/other/data-link-4.png){: width="550" height="100" }
{: .nolineno}

## IV. Các giao thức đa truy cập
Có 2 kiểu kết nối:

- **Point-to-point**: Truy cập dial-up, kết nối giữa Ethernet switch và host.
- **Broadcast** (Dây hoặc đường truyền được chia sẻ): Ethernet mô hình cũ, upstream HFC, 802.11 wireless LAN.

Xét môi trường quảng bá/dùng chung:

- Hai hay nhiều việc truyền đồng thời từ các node: Tín hiệu từ các frame bị trộn lẫn, không thể tách rời => Xảy ra **đụng độ** (collision).
- Phải có quy tắc để điều phối sự truy cập từ nhiều node gửi và nhận đến một kênh broadcase chung => Sử dụng **giao thức đa truy cập**.

### Slotted ALOHA
**Giả thuyết**

- Tất cả các frame có cùng kích thước.
- Thời gian được chia thành các slot bằng nhau (thời gian để truyền 1 frame).
- Các node chỉ bắt đầu truyền tại lúc bắt đầu slot.
- Các node được đồng bộ hóa.
- Nếu có từ 2 node trở lên truyền trong slot, tất cả các node đều phát hiện đụng độ.

**Hoạt động**

- Khi node có được frame mới, nó sẽ truyền trong slot kế tiếp.
    - Nếu không có đụng độ: node có thể gửi frame mới trong slot kế tiếp.
    - Nếu có đụng độ: node truyền lại frame trong mỗi slot tiếp theo với xác suất `p` cho đến khi thành công.

**Mô hình giả định**

![img](/assets/img/other/data-link-5.png){: width="550" height="200" }
_C - Collision slot, E - Empty slot, S - Successful slot_

|<center>Ưu điểm</center> | <center>Nhược điểm</center>|
|:-------|:--------|
| - Node đơn kích hoạt có thể truyền liên tục <br> với tốc độ tối đa của kênh <br> - Phân cấp cao: Chỉ có các slot trong các node <br> cần được đồng bộ <br> - Đơn giản| - Đụng độ, lãng phí slot <br> - Các slot nhàn rỗi <br> - Node phải phát hiện ra đụng độ với thời gian <br> ngắn hơn thời gian truyền <br> - Cần phải đồng bộ hóa

**Hiệu suất**

- Độ hiệu quả: `%` slot thành công so với tổng số slot, khi có nhiều node cùng truyền một thời gian dài, mỗi node có một số lượng lớn các frame cần truyền
- Hiệu suất cực đại: 1/e, xấp xỉ `37%`

### Pure(unslotted) ALOHA

- Frame được truyền tại thời điểm `t0` đụng độ với các frame được gửi trong khoảng thời gian `[t0 - 1, t0 + 1]`
- Ưu điểm:
    - Đơn giản hơn Slotted ALOHA
    - Không cần đồng bộ hóa
    - Truyền đi ngay lần đầu frame đến
- Nhược điểm:
    - Khả năng đụng độ cao hơn so với Slotted ALOHA
    - Hiệu suất = 1/(2e), **thấp hơn** một nửa so với Slotted ALOHA

### CSMA (carrier sense multiple access)

- Lắng nghe trước khi truyền
    - Nếu kênh nhàn rỗi: truyền toàn bộ frame
    - Nếu kênh truyền bận: trì hoãn truyền
- Đụng độ có thể vẫn xảy ra:
    - **Trễ lan truyền** khiến cho 2 node có thể không nghe thấy quá trình truyền lẫn nhau.
    - Khi đụng độ xảy ra: Toàn bộ thời gian truyền packet bị lãng phí.

Xét hình minh họa bên dưới. Tại thời điểm `t0`, node **B** nhận thấy kênh truyền nhàn rỗi nên bắt đầu truyền đi các bit. Do sự trễ lan truyền nên tới thời điểm sau đó là `t1`, các bit do **B** truyền đi chưa đến được node **D**. Bấy giờ, node **D** nhận thấy kênh truyền nhàn rỗi nên cũng bắt đầu truyền đi các bit của mình. Từ đó, xảy ra đụng độ.

![img](/assets/img/other/data-link-6.png){: width="550" height="200" }

> Độ lớn về **khoảng cách** và **thời gian lan truyền** sẽ ảnh hưởng đến khả năng đụng độ.
{: .prompt-warning }

### CSMA/CD (collision detection)

- Lắng nghe và trì hoãn tương tự CSMA.
- Đụng độ được phát hiện trong thời gian ngắn, việc truyền đụng độ được **bỏ qua** giúp giảm lãng phí kênh truyền.
- Phát hiện đụng độ:
    - Dễ dàng trong mạng LAN hữu tuyến: Đo cường độ tín hiệu, so sánh với các tín hiệu đã được truyền và nhận.
    - Khó thực hiện trong mạng LAN vô tuyến: cường độ tín hiệu nhận được bị áp đảo bởi cường độ truyền cục bộ.
- Thuật toán Ethernet CSMA/CD
    1. NIC (Network Interface Card) nhận datagram từ tầng network, tạo frame
    2. Nếu NIC nhận thấy kênh nhàn rỗi, bắt đầu truyền frame. Ngược lại, đợi đến kênh rảnh rồi truyền.
    3. Nếu NIC truyền toàn bộ frame mà không phát hiện việc truyền khác, NIC được truyền toàn bộ frame đó.
    4. Nếu NIC phát hiện có sự truyền khác trong khi đang truyền, nó sẽ hủy bỏ truyền và phát tín hiệu tắt nghẽn.
    5. Sau khi hủy bỏ truyền, NIC thực hiện **binary (exponential) backoff**:
        - Sau lần đụng độ thứ `m`, NIC chọn ngẫu nhiên số `K` trong khoảng `{0, 1, 2, ..., (2^m)-1}`. NIC sẽ đợi `K * 512 bit times` (`K` nhân với thời gian để truyền 512 bit vào Ethernet), sau đó trở lại **bước 2**.
        - Đụng độ càng nhiều thì khoảng thời gian backoff càng dài (tăng theo cấp số nhân).
- **Độ hiệu quả** được tính theo công thức:

 ![img](/assets/img/other/data-link-7.png){: width="550" height="200" }

 Trong đó, d<sub>prop</sub>  là độ trễ lan truyền lớn nhất giữa 2 node trong mạng LAN và d<sub>trans</sub> là thời gian để truyền frame có kích thước lớn nhất.

 Hiệu suất tiến tới 1 khi d<sub>prop</sub> tiến tới 0 hoặc khi d<sub>trans</sub> tiến tới vô cùng.

> Hiệu suất tốt hơn ALOHA do tính đơn giản, chi phí thấp và điều khiển phân tán
{: .prompt-tip }

### Các giao thức MAC xoay vòng

- **Polling**:
    - Master node mời các slave node truyền lần lượt
    - Thường được sử dụng với các thiết bị con "đần độn"
    - Quan tâm:
        - Polling overhead
        - Latency
        - Chỉ có 1 điểm chịu lỗi (master node)
- **Chuyển token**:
    - Điều hành token từ node này đến node kế tiếp một cách tuần tự
    - Token là một frame đặc biệt có kích thước nhỏ.
    - Quan tâm:
        - Polling overhead
        - Latency
        - Chỉ có 1 điểm chịu lỗi (bất kì node nào tham gia kênh truyền)

## V. Định địa chỉ và giao thức ARP
### Công dụng
**ARP** (address resolution protocol), còn gọi là giao thức phân giải địa chỉ, dùng để ánh xạ địa chỉ IP tới một địa chỉ vật lý cố định (địa chỉ MAC).

Mỗi node IP trên mạng LAN như host hay router đều có một bảng ARP chứa các địa chỉ IP/MAC ánh xạ cho các node trong mạng: <địa chỉ IP; địa chỉ MAC>. Mỗi địa chỉ như thế trong bảng sẽ bị lãng quên sau một khoảng thời gian nhất định gọi là **TTL** (Time To Live), thông thường là 20 phút.

### Cách thức hoạt động trong cùng mạng LAN
Ta có node A muốn gửi datagram tới B. Nếu địa chỉ MAC của B nằm trong bảng ARP của A thì sẽ lập tức gửi gói tin đi. Nếu không, thực hiện các bước sau:

- A sẽ broadcast ARP query packet có chứa địa chỉ IP của B, địa chỉ MAC đích lúc này là `FF-FF-FF-FF-FF-FF-FF`.
- B nhận được packet, phản hồi tới A với địa chỉ MAC của mình (unicast).
- A sẽ lưu lại cặp địa chỉ IP-MAC trong bảng ARP tới khi hết TTL. Sau đó, tiến hành gửi datagram tới B.

> ARP mang tính "plug and play". Các node tạo bảng ARP mà không cần sự can thiệp của người quản trị mạng.
{: .prompt-info }

### Cách thức hoạt động khi định tuyến tới mạng LAN khác
Xét hình bên dưới:

![img](/assets/img/other/data-link-8.png){: width="550" height="300" }

Giả sử node A muốn gửi datagram tới node B nhưng chỉ biết địa chỉ IP, không biết địa chỉ MAC của B. Biết rằng A cặp có địa chỉ IP/MAC của router R trong bảng ARP của mình. Quá tình định tuyến sẽ diễn ra như thế nào? 

1. A tạo IP datagram với IP nguồn A, đích B
2. A tiếp tục tạo frame ở tầng Data link với địa chỉ MAC của router R là địa chỉ đích. Frame này chứa IP datagram ở bước 1.
3. Frame được gửi từ A tới R.
4. Router R nhận được frame. Frame được chuyển lên tầng Network của R. Tại dây, R biết được địa chỉ IP đích là B.
5. R chuyển tiếp datagram với IP nguồn A, đích B
6. R tạo frame ở tầng Data link với địa chỉ mac của B là địa chỉ đích.
7. B nhận được frame vốn được gửi từ A.

## VI. Switch và VLAN
### Switch và tính năng của nó

- Thiết bị tầng Data link
- Có chức năng lưu, chuyển tiếp các frame Ethernet và xem xét địa chỉ MAC của frame đến để chọn lựa chuyển tiếp frame tới một hay nhiều đường liên kết đi ra khi frame được chuyển tiếp vào segment.
- Dùng **CSMA/CD** để truy nhập segment
- Có tính transparent - các host không nhận thức được sự hiện diện của các switch.
- Có tính "plug-and-play", tự học. Các switch **không cần** được cấu hình.

### Switch - Cho phép nhiều đường truyền đồng thời

- Các host kết nối trực tiếp tới switch.
- Các switch có thể được kết nối với nhau.
- Switch lưu tạm (buffer) các packet.
- Giao thức Ethernet được sử dụng trên mỗi đường kết nối vào nhưng không có đụng độ. Mỗi đường kết nối là 1 **miền đụng độ riêng**.
- Hoạt động ở chế độ **full-duplex**, cho phép dữ liệu truyền theo 2 chiều đồng thời.

### Switch - Tự học

- Switch học các host có thể tới được thông qua các interface kết nối với các host đó
    - Khi frame được nhận, switch học vị trí của bên gửi (incoming LAN segment).
    - Switch ghi nhận lại cặp thông tin về host gửi (địa chỉ MAC) và nhánh mạng chứa host gửi (interface).
- Khi swich nhận được frame:
    - Ghi lại đường liên kết vào, địa chỉ MAC của host gửi.
    - Ghi vào mục lục bảng switch với địa chỉ MAC đích
    - Thực hiện các bước được mô tả trong đoạn **pseudocode** bên dưới

    ```python
    if(tìm thấy thông tin đích đến){
        if(đích đến nằm trên cùng phân đoạn mạng với frame)
            bỏ frame
        else chuyển tiếp frame trên interface được 
        chỉ định bởi thông tin trong bảng switch
    }
    else flood, tức chuyển tiếp trên tất cả interface 
    ngoại trừ interface nhận frame đó

    ```
### VLAN

- Các switch hỗ trợ khả năng VLAN có thể được cấu hình để định nghĩa nhiều mạng LAN ảo (multiple **virutal** LANs).
- Port-based VLAN: các port của switch được nhóm lại để trở thành một switch **vật lý** duy nhất hoạt động như là **nhiều** switch ảo. 
    - **Traffic isolation**: Các frame từ các port trong khoảng nào thì chỉ có thể tới được các port trong khoảng đó (Lấy ví dụ trong hình bên dưới: `[2-8]`, `[9-15]`).
    - **Dynamic membership**: các port có thể được gán động giữa các VLAN.
    - **Chuyển tiếp giữa các VLAN** được thực hiện thông qua định tuyến (tương tự như các switch riêng biệt).

![img](/assets/img/other/data-link-9.png){: width="550" height="300" }
_Một switch được cấu hình có 2 VLAN_

>Cũng có thể định nghĩa VLAN dựa trên địa chỉ MAC của thiết bị đầu cuối hơn là dựa trên port của switch.
{: .prompt-info }

- Trunk port: Mang các frame giữa các VLAN được định nghĩa trên nhiều switch vật lý
    - Các frame được chuyển tiếp bên trong VLAN giữa các switch không thể là các frame **802.1** (phải mang thông tin VLAN ID).
    - Giao thức **802.1q** thêm/ gỡ bỏ các trường header giúp frame được chuyển tiếp giữa các trunk port.

## VII. Nguồn tham khảo:

1. [Computer Networking A Top-Down Approach 6th Edition](https://eclass.teicrete.gr/modules/document/file.php/TP326/%CE%98%CE%B5%CF%89%CF%81%CE%AF%CE%B1%20(Lectures)/Computer_Networking_A_Top-Down_Approach.pdf)
2. [Tài liệu ôn tập cuối kì NMM 2023 - BHT khoa CNPM, UIT](https://drive.google.com/file/d/1RepeMkgpAAPj1T39q6vIigz8U0HHFckD/view)
3. Tài liệu Lý thuyết Chương 5, môn Nhập môn Mạng máy tính, UIT

