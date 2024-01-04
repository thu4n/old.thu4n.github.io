---
title: Ôn tập An toàn mạng máy tính - Cấp cứu 2 tờ A4
date: 2023-12-30 13:00:00 +0700
authors: 
- thu4n
- ahamonuser
- howtodie
categories: [Learning]
image: /assets/img/other/tet-cuu.png
tags: [computer network, network security, sysadmin]
---

Xin chào các bạn, trong bài viết lần này mình sẽ cố gắng tóm tắt về môn học NT101 - An toàn mạng máy tính theo chương trình giảng dạy cho lớp Mạng tại UIT. Nội dung khá là dài nhưng thầy đã rộng lượng cho tận **2 tờ A4** thì có vẻ sẽ đủ, thời gian làm bài nếu mình nhớ không lầm thì đâu đó khoảng **75 phút**. Nôm na là thế, chúng ta sẽ có **8 chương** tổng cộng cần phải ôn tập nên mình vô thẳng nội dung chính luôn thôi.

Note: Cần bổ sung các hình ảnh minh họa.
## I. Tổng quan

### A. Một số khái niệm

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

### B. Các kỹ thuật tấn công phổ biến và cơ chế phòng thủ

#### 1. Eavesdropping (Nghe trộm)

- Cách thức tấn công: Sử dụng một thiết bị mạng (router, card mạng…) và một ứng dụng (Tcpdump, Ethereal, Wireshark…) để giám sát lưu lượng mạng, bắt các gói tin đi qua thiết bị này. Từ đó, "nghe trộm" được thông tin từ các phiên trao đổi dữ liệu.
- Nhận xét: Thực hiện dễ dàng hơn với mạng không dây. **Không** có cách nào ngăn chận việc nghe trộm trong một mạng công cộng.
- Cách phòng chống: Mã hoá dữ liệu trước khi truyền chúng trên mạng.

#### 2. Cryptanalysis

- Cách thức tấn công: Sử dụng một hoặc nhiều phương pháp khác nhau để tìm kiếm thông tin hữu ích từ dữ liệu đã mã hoá mà không cần biết khoá giải mã. Điển hình là phương pháp giải mã thông qua thống kê **tần suất xuất hiện** của các ký tự.
- Nhận xét: Khi sử dụng phương pháp phân tích thông kê tần suất, cần phải sử dụng các công cụ toán học và máy tính có hiệu suất cao khi cipher text có kích thước lớn dần.
- Cách phòng chống: Sử dụng những giải thuật mã hoá không thể hiện cấu trúc thống kê trong chuỗi mật mã; Sử dụng khoá có độ dài lớn để chống Brute-force attacks.

#### 3. Password Pilfering

Cơ chế chứng thực được sử dụng rộng rãi nhất là dùng username - tên người dùng và password - mật khẩu.

Password Pilfering là một hình thức tấn công nhằm đánh cắp mật khẩu của người dùng, có nhiều phương pháp khác nhau để thực hiện điều này.

**3.1. Guessing**

- Đúng như tên của nó, đây là đoán mật khẩu, hiệu quả khi đối phương có mật khẩu ngắn hoặc đang sử dụng mật khẩu mặc định.
- 10 mật khẩu phổ biến nhất trên Internet (bảng xếp hạng này tương đối cũ, không phải của hiện nay):
    1. Password
    2. 123456
    3. qwerty
    4. abc123
    5. letmein
    6. monkey
    7. myspace1
    8. password1
    9. blink182
    10. <"Tên của người dùng"> 

**3.2. Social engineering**

- Là phương pháp sử dụng các kỹ năng xã hội để ăn cắp thông tin mật của người khác thông qua các thủ thuật như Mạo danh, Lừa đảo qua email, Thu thập thông tin từ giấy tờ bị loại bỏ, Tạo trang web đăng nhập giả hoặc chỉ đơn giản là Kết bạn làm quen rồi lấy cắp thông tin cá nhân của mục tiêu.
- Ý kiến cá nhân: Đây có thể được xem là phương pháp nguy hiểm nhất khi nó tấn công vào yếu tố con người nhiều hơn là hệ thống.

**3.3. Dictionary Attacks**

- Tên tiếng Việt là tấn công từ điển, thực hiện bằng cách duyệt tìm từ một từ điển (thu được từ các file SAM của Window, /etc/passwd trong Linux) các username và password đã được mã hoá.

**3.4. Password Sniffing**

- Là một phần mềm dùng để bắt các thông tin đăng nhập từ xa như username và password đối với các ứng dụng mạng phổ biến như Telnet, FTP, SMTP, POP3.
- Cách phòng chống Password Sniffing là sử dụng các giao thức như HTTPS, SSH để mã hóa toàn bộ thông điệp truyền đi.

**3.5. Một số quy tắc bảo vệ mật khẩu**

1. Sử dụng mật khẩu dài kết hợp giữa chữ thường, chữ hoa, số và các ký tự đặc biệt. Không dùng các từ có trong từ điển, các tên và mật khẩu thông dụng => gây khó khăn cho việc đoán mật khẩu và tấn công sử dụng từ điển.
2. Không tiết lộ mật khẩu với những người không có thẩm quyền hoặc qua điện thoại, thư điện tử hoặc bất kỳ phương tiện truyền thông nào => chống lại social engineering.
3. Thay đổi mật khẩu định kỳ và không sử dụng trở lại những mật khẩu cũ => Chống tấn công sử dụng từ điển.
4. Không sử dụng cùng một mật khẩu cho các tài khoản khác nhau.
5. Không sử dụng những phần mềm đăng nhập từ xa mà không có cơ chế mã hoá như là Telnet.
6. Huỷ hoàn toàn các tài liệu có lưu các thông tin quan trọng sau khi sử dụng xong mục đích ban đầu.
7. Tránh nhập các thông tin trong các cửa sổ popup và Không click vào các đường liên kết lạ, không rõ nguồn gốc.

#### 4. Identity Spoofing

- Là phương pháp tấn công cho phép kẻ tấn công mạo nhận nạn nhân mà không cần sử dụng mật khẩu của nạn nhân.
- Các phương pháp phổ biến bao gồm:
    - Man-in-the-middle attacks
    - Message replays attacks
    - Network spoofing attacks
    - Software exploitation attacks

**4.1. Man-in-the-middle**

– Kẻ tấn công cố gắng dàn xếp với thiết bị mạng (hoặc cài đặt một thiết bị của riêng mình) giữa hai hoặc nhiều người sử dụng, sau đó chặn và sửa đổi hay làm giả dữ liệu truyền giữa những người sử dụng rồi tiếp tục truyền chúng tới địa chỉ đích như chưa từng bị tác động bởi kẻ tấn công.
- Mã hoá và chứng thực các gói IP là biện pháp chính để ngăn chận các cuộc tấn công Man-in-the-midle.

**4.2. Messeage Replays**

- Trong một số giao thức xác thực, sau khi người dùng A chứng thực mình với hệ thống là một người dùng hợp pháp, A sẽ được cấp một chứng thực (giấy phép) thông qua. Với giấy phép này, A sẽ nhận được những dịch vụ cung cấp bởi hệ thống.
- Những kẻ tấn công có thể ngăn chặn gói tin chứa chứng thực đó, giữ một bản sao, và sử dụng nó sau này để mạo nhận (đóng vai) người dùng A.

**4.3. Network spoofing**
- **SYN flooding** là khi kẻ tấn công lấp đầy bộ đệm TCP của máy tính mục tiêu với một khối lượng lớn các gói SYN, làm cho máy tính mục tiêu không thể thiết lập các thông tin liên lạc với các máy tính khác.
- **TCP hijacking** là một kỹ thuật sử dụng các gói tin giả mạo để chiếm đoạt một kết nối giữa máy tính nạn nhân và máy đích. Để ngăn chặn kỹ thuật tấn công này, có thể sử dụng phần mềm như TCP Wrappers để kiểm tra địa chỉ IP tại tầng Transport.
- **ARP spoofing** hoặc ARP Poisoning diễn ra khi  kẻ tấn công thay đổi địa chỉ MAC đích hợp pháp của một địa chỉ IP đến một địa chỉ MAC khác được lựa chọn bởi những kẻ tấn công. Nói ngắn gọn, là thay đổi địa chỉ MAC đích thành địa chỉ mong muốn của kẻ tấn công. Để ngăn chặn các cuộc tấn công ARP spoofing, cần phải tăng cường kiểm tra các tên miền, và chắc chắn rằng địa chỉ nguồn và địa chỉ đích trong một gói tin không được thay đổi trong khi truyền.

#### 5. Buffer-Overflow Exploitations

- Buffer-Overflow là lỗi xảy ra khi quá trình ghi dữ liệu vào bộ đệm nhiều hơn kích thước khả dụng của nó.
- Các hàm `strcat()`, `strcpy()`, `sprintf()`, `vsprintf()`, `bcopy()`, `get()`, `scanf()`… trong ngôn ngữ C có thể bị khai thác bằng lỗi này vì không kiểm tra xem liệu bộ đệm có đủ lớn để dữ liệu được sao chép vào mà không gây ra tràn bộ đệm hay không.

#### 6. Repudiation

- Trong một số trường hợp chủ sở hữu của dữ liệu có thể không thừa nhận quyền sở hữu của dữ liệu để tránh hậu quả pháp lý. Người này có thể cho rằng chưa bao giờ gửi hoặc nhận các dữ liệu đó. Ngay cả khi dữ liệu đã được chứng thực, chủ sở hữu của dữ liệu xác thực có thể thuyết phục quan tòa rằng vì những sơ hở, bất cứ ai cũng có thể dễ dàng chế tạo tin nhắn và làm cho nó trông giống như thật.

> "Cháu nó ở nhà vô phá máy tính em, lỡ leak hết thông tin công ty cho bên đối thủ chứ em có biết gì đâu"

- Sử dụng các thuật toán mã hóa và xác thực có thể 
giúp ngăn ngừa các cuộc tấn công bác bỏ.

#### 7. Intrusion

- Tấn công xâm nhập bất hợp pháp vào một mạng với mục đích truy cập vào hệ thống máy tính của người khác, đánh cắp thông tin và tài nguyên máy tính hoặc băng thông của nạn nhân.
- Phòng chống bằng cách đóng các cổng UDP hoặc TCP không cần thiết, sử dụng IP scan và Port scan để kiểm tra các lỗ hỏng trong hệ thống.

#### 8. Denial of Service Attacks

- Tấn công từ chối dịch vụ nhằm ngăn chặn người dùng hợp pháp sử dụng những dịch vụ mà họ thường nhận được từ các máy chủ. Thường buộc máy tính mục tiêu phải xử lý một số lượng lớn những thứ vô dụng nhằm tiêu hao tài nguyên máy, dẫn đến không hoạt động được nữa. Nếu cuộc tấn công được phát sinh từ một nhóm các máy tính phân bố khắp nơi thì sẽ trở thành **DDoS - Distributed Denial of Serivce Attack**.
- Công cụ để thực hiện tấn công DoS có thể là Jolt2, Bubonic.c, Land and LaTierra, Targa, Blast20, Nemesy, Panther2,Crazy Pinger, Some Trouble, UDP Flood, FSMax.
- DoS có các hình thức cơ bản sau:
    - Smurf
    - Buffer Overflow Attack
    - Ping of death
    - Teardrop
    - SYN Attack
- Trong Smurf, máy của attacker sẽ gởi rất nhiều lệnh ping đến một số lượng lớn máy tính trong một thời gian ngắn trong đó địa chỉ IP nguồn của gói ICMP echo sẽ được thay thế bởi địa chỉ IP của nạn nhân. Các máy tính này sẽ trả lại các gói ICMP reply đến máy nạn nhân, buộc máy nạn nhân phải xử lý một lượng lớn các gói tin trong thời gian ngắn đó.
- Trong DDoS, Attackers thường sử dụng Trojan để kiểm soát cùng lúc nhiều máy tính nối mạng. Attacker cài đặt một phần mềm đặc biệt (phần mềm zombie) lên các máy tính này (máy tính zombie) để tạo ra một đội quân zombie (botnet) nhằm tấn công DoS sau này trên máy nạn nhân

#### 9. Malicious Software

Các phần mềm độc hại bao gồm:
- **Virus** - Một phần mềm có thể sao chép chính nó để lây nhiễm sang máy khác trong hệ thống, không đứng một mình mà phải được gắn vào một tập tin khác.
- **Worms** - Cũng là một chương trình có thể tự sao chép chính nó nhưng có thể tự đứng một mình. Một Worm có thể tự thực thi tại bất kỳ thời điểm nào. 
- **Trojan horses** - Thường nguỵ trang mình kèm theo những chương trình ứng dụng thông thường và vô hại. Không tự sao chép bản thân và chỉ thực hiện khi người dùng chạy chương trình có đính kèm Trojan. Chức năng chính của Trojan là điểu khiển máy tính từ xa, ăn cắp thông tin của nạn nhân hoặc làm nhiệm vụ backdoor.
- **Logic bombs** - Chương trình con hoặc lệnh được nhúng trong một chương trình khác. Nó được kích hoạt bởi điều kiện được thiết lập bởi kẻ tấn công.
- **Backdoors** - Những đoạn chương trình bí mật thường được đính kèm vào những chương trình khác, Backdoors sẽ mở một hoặc nhiều port trên máy nạn nhân giúp kẻ tấn công xâm nhập thông qua các port đó.
- **Spyware** - Phần mềm tự cài đặt chính nó trên máy tính của người dùng. Spyware thường được sử dụng để theo dõi 
xem người dùng làm gì và quấy rối họ. Các loại phổ biến là Browser hijacking - Thay đổi trình duyệt của user và Zombieware - Biến máy của user thành zombie dùng trong DDoS.

### C. Lý lịch của những kẻ tấn công

Các attacker được chia thành các loại bao gồm:
- **Black-hat hackers** - Là những người có tri thức đặc biệt về hệ thống máy tính. Họ quan tâm đến những chi tiết tinh tế của phần mềm, giải thuật, mạng máy tính và cấu hình hệ thống. Ngoài ra còn các loại hacker khác như **White-hat** và **Grey-hat**.
- **Script kiddies** - Là những chú bé sử dụng các script hoặc các chương trình được phát triển bởi các hacker mũ đen để tấn công các máy tính và gây thiệt hại cho người khác. Mấy anh trai này chỉ biết dùng công cụ có sẵn chứ không hiểu cách hoạt động của chúng cũng như tự phát triển ra công cụ tương tự.
- **Cyber spies** - Thường hoạt động trong lĩnh vực quân sự, kinh tế với mục đích đánh chặn truyền thông trên mạng và phá mã các thông điệp.
- **Vicious employees** - Những người cố tình vi phạm an ninh để làm hại công ty của chính họ vì các lý do cá nhân.
- **Cyber terrorists** - Những kẻ khủng bố cực đoan sử dụng máy tính và công nghệ mạng làm công cụ.

### D. Mô hình bảo mật cơ bản

Một mô hình bảo mật cơ bản gồm 4 thành phần:
- Hệ thống mã hoá (Cryptosystem)
- Tường lửa (Firewalls)
- Hệ thống phần mềm chống độc hại(Anti-Malicious System software – AMS software)
- Hệ thống tìm kiếm xâm nhập (Intrusion Detection System – IDS)

Đối với phương thức phòng thủ, ta có phương thức phòng thủ theo lớp (layer):
- Application
- Host
- Internal Network
- Perimeter
- Physical
- Policies, Procedures and Awareness

## II. Malware

### A. Trojan

#### 1. Khái niệm

Cái tên Trojan được lấy ý tưởng từ Ngựa Trojan trong truyền thuyết Hy Lạp cổ đại thế kỷ 17 (cái này thi không hỏi đâu, một fun fact thôi). Phần mềm Trojan đầu tiên trên máy tính là **Back Orifice** với port xâm nhập là 31337.

Trojan là chương trình gây hại, thường hoạt động bí mật và có công dụng phổ biến là **thiết lập quyền điều khiển từ xa** cho hacker trên máy bị nhiễm Trojan. Các đặc điểm khác của Trojan có thể xem thêm lại ở mục [I.B.9. Malicious Software](#9-malicious-software)

Trojan có thể xâm nhập vào hệ thống qua rất nhiều con đường nhau như qua ứng dụng nhắn tin, file đính kèm trong email, phần mềm miễn phí trên mạng...

#### 2. Phân loại Trojan

Ta có các loại Trojan thường thấy như sau:
- **RAT** - Remote Access Trojan: Biến máy tính bị nhiễm thành server để máy của hacker có thể truy cập vào và nắm quyền điều khiển. RAT tự động kích hoạt mỗi khi máy tính hoạt động và thường vô hiệu hóa việc chỉnh sửa registry nên khóa xóa Trojan này. Phổ biến có Back Orifice, Girlfriend, Netbus…
- **Keyloggers**: Bao gồm 2 loại phần cứng và phần mềm, kích thước nhỏ gọn và sử dụng ít bộ nhớ nên khó phát hiện. Chúng có nhiệm vụ chủ yếu là ghi lại diễn biến của bàn phím rồi lưu lại trên máy hoặc gửi về cho hacker. Một keylogger thường gồm **3 thành phần** chính là **Chương trình điều khiển** để điều phối hoạt động và thiết lập, **Tập tin hook** để ghi nhận các thao tác bàn phím và **Tập tin nhật ký (log)** - Nơi chứa đựng toàn bộ những gì hook ghi nhận được.
- **Trojan lấy cắp password**: Đúng như tên gọi, chúng ăn cắp các mật khẩu lưu trên hệ thống rồi gửi về cho hacker. Các loại phổ biến là Barri, Kuang, Barok.
- **FTP Trojan**: Loại này mở cổng **21** trên máy bị nhiễm nên mọi người đều có thể truy cập máy này để tải dữ liệu.
- **Trojan phá hoại**: Có khả năng phá hủy đĩa cứng, mã hoá các file. Rất nguy hiểm và khó kiểm soát.
- **Trojan chiếm quyền kiểu leo thang**: Thường được gắn vào một ứng dụng hệ thống nào đó và sẽ cho hacker quyền cao hơn quyền đã có trong hệ thống khi ứng dụng này chạy.

#### 3. Một số Trojan phổ biến

- Loại Keyloggers
    - KGB Spy.
    - Blazing Tool Perfect Keylogger
    - Stealth Keylogger
- Loại RAT
    - DJI RAT
    - HackerzRAT
    - NET BUS
- Công cụ tạo Trojan
    - Trojan Horse Construction Kit
    - Dark Horse Trojan Virus Maker

### B. Phòng chống Trojan

Dưới đây là một số bước cần thực hiện nhằm phòng chống Trojan:
- Quét các port đang mở với các công cụ như Netstat, Fport, TCPView.
- Quét các tiến trình đang chạy với Process Viewer, What’s on my computer, Insider.
- Quét những thay đổi trong Registry với MsConfig, What’s running on my computer.
- Quét những hoạt động mạng với Ethereal, WireShark.
- Chạy các phần mềm diệt Trojan như Trojan Hunter, Trojan Guard.

Một số cổng đi cùng các Trojan thông dụng:

|<center>Trojan</center> | <center>Protocol</center>| <center>Port</center>|
|:-------|:--------|:--------|
| Back Orifice  |UDP| 31337 hoặc 31338|
| Deep Throat |  UDP | 2140 và 3150|
|NetBus|TCP|12345 và 12346|
|Whack-a-mole|TCP|20034|
|NetBus 2 Pro|TCP |21544|
|GirlFriend|TCP|21544|
|Masters Paradise|TCP|3129, 40421, 40422, 40423 và 40426|

> Trong slide của thầy mình, có thêm khoảng 3 bảng nữa chứa rất nhiều loại Trojan kèm số port, còn ở đây mình chỉ mới nêu tiêu biểu một phần thôi. Anh em nào muốn xem chi tiết có thể xem tại [link này, slide số 58.](https://uithcm-my.sharepoint.com/:b:/g/personal/21522652_ms_uit_edu_vn/EYc8QDF6C1hPhcIccrnen4UBvwMJf_4kHizX9Ej5kWcKkg?e=QyXEu0)
{: .prompt-info }


### C. Virus

Những thông tin cơ bản về virus, bạn có thể xem lại tại mục [I.B.9. Malicious Software](#9-malicious-software)

#### 1. Lịch sử của Virus

- Năm 1949: John von Neumann (1903-1957) phát triển nền tảng lý thuyết tự nhân bản của một chương trình cho máy tính.
- Năm 1981: Các virus đầu tiên xuất hiện trong hệ điều hành của máy tính Apple II.
- Năm 1983: Tại Đại Học miền Nam California, Fred Cohen lần đầu đưa ra khái niệm computer virus.
- Năm 1986: Virus "the Brain", virus cho máy tính cá nhân (PC) đầu tiên, được tạo ra tại Pakistan bởi Basit và Amjad.
- Năm 1987: Virus đầu tiên tấn công vào command.com là virus "Lehigh". 
- Năm 1988: Virus Jerusalem tấn công đồng loạt các đại học và các công ty trong các quốc gia vào ngày thứ Sáu 13.
- Tháng 11.1988, Robert Morris, 22 tuổi, chế ra worm chiếm cứ các máy tính của ARPANET, làm tê liệt khoảng 6.000 máy.
- Năm 1990: Norton giới thiệu chương trình thương mại chống virus đầu tiên.
- Năm 1991: Virus đa hình (polymorphic virus) ra đời đầu tiên là virus "Tequilla".
- Năm 1995: Virus văn bản (macro virus) đầu tiên xuất hiện.
- Năm 1998: Virus Melissa, tấn công hơn 1 triệu máy, lan truyền bởi một tệp đính kèm kiểu Word.
- Năm 2003: Virus Slammer, một loại worm lan truyền với vận tốc kỉ lục, truyền cho khoảng 75 ngàn máy trong 10 phút.
- Năm 2004: Xuất hiện worm Sasser. Với virus này thì người ta không cần phải mở đính kèm của điện thư mà chỉ cần mở lá thư là đủ cho nó xâm nhập vào máy.

#### 2. Tính chất của Virus

1. Tính lây lan.
2. Tính phá hoại.
3. Tính nhỏ gọn.
4. Tính tương thích.
5. Tính phát triển và kế thừa.

#### 3. Phân loại virus

**3.1. Phân loại theo phương pháp tìm đối tượng lây nhiễm**

1. Virus thường trú
2. Virus không thường trú

**3.2. Phân loại theo đối tượng nhiễm và môi trường hoạt động**

1. Boot Virus
2. File-System Virus
3. File-Format Virus
4. Macro Virus
5. Script Virus
6. Registry Virus

**3.3. Phân loại theo phương pháp lây nhiễm**

1. Ghi đè
2. Ghi đè bảo toàn
3. Dịch chuyển
4. Song hành
5. Nối thêm
6. Chèn giữa
7. Định hướng lại lệnh nhảy
8. Điền khoảng trống

**3.4. Cách phân loại khác theo tính năng và bản chất**

1. System Sector or Boot Virus: lây nhiễm trên cung boot của đĩa. 
2. File Virus: lây nhiễm trên các file thực thi.
3. Macro Virus: lây nhiễm trên các tập tin word, excel, access…
4. Source Code Virus: ghi đoạn code của Trojan đè hoặc nối tiếp vào đoạn code của tập tin chủ.
5. Network Virus: tự phát tán theo email bằng cách sử dụng lệnh và các giao thức của mạng máy tính.
6. Stealth Virus: có thể ẩn với các chương trình chống virus.
7. Polymorphic Virus: có thể thay đổi đặc điểm của nó với mỗi lần lây nhiễm.
8. Cavity Virus: duy trì kích thước file không thay đổi trong khi lây nhiễm.
9. Tunneling Virus: tự che giấu dưới những dạng anti-virus khi lây nhiễm.
10. Camouflage Virus: ngụy trang dưới dạng những ứng dụng chính hãng của người dùng.
11. Shell Virus: đoạn mã của virus sẽ tạo thành một shell xung quanh đoạn mã của chương trình bị lây nhiễm, tương tự như một chương trình con trên chương trình gốc nguyên thủy.
12. Add-on Virus: ghi đoạn mã của nó nối tiếp vào 
điểm bắt đầu của chương trình bị lây nhiễm và không tạo ra thêm bất kỳ thay đổi nào khác.
13. Intrusive Virus: viết đè đoạn code của nó lên một phần hoặc hoàn toàn đoạn code của file bị lây nhiễm.

#### 4. Một số công cụ tạo virus

- Terabit Virus Maker
- DELmE's Batch Virus Maker
- Sam's Virus Generator
- JPS Virus Maker
- Sonic Bat
- Poison Virus Maker

### D. Các kỹ thuật của Virus

#### 1. Kỹ thuật cơ bản
Các kỹ thuật cơ bản của Virus bao gồm 9 kỹ thuật như sau:

1. [Kỹ thuật lây nhiễm](#11-kỹ-thuật-lây-nhiễm)
2. [Kỹ thuật định vị trên vùng nhớ](#12-kỹ-thuật-định-vị-trên-vùng-nhớ)
3. [Kỹ thuật kiểm tra sự tồn tại](#13-kỹ-thuật-kiểm-tra-sự-tồn-tại)
4. [Kỹ thuật thường trú](#14-kỹ-thuật-thường-trú)
5. [Kỹ thuật mã hoá](#15-kỹ-thuật-mã-hoá)
6. [Kỹ thuật nguỵ trang](#16-kỹ-thuật-nguỵ-trang)
7. [Kỹ thuật phá hoại](#17-kỹ-thuật-phá-hoại)
8. [Kỹ thuật chống bẫy](#18-kỹ-thuật-chống-bẫy)
9. [Kỹ thuật tối ưu](#19-kỹ-thuật-tối-ưu)

##### 1.1. Kỹ thuật lây nhiễm

Là kỹ thuật cơ bản cần phải có của mỗi virus. Có thể đơn giản hoặc phức tạp tuỳ loại virus.

- Kỹ thuật lây nhiễm Boot Record / Master Boot của đĩa: thay thế BR hoặc MB trên phân vùng hoạt động với chương trình virus.
- Kỹ thuật lây nhiễm file thực thi: chương trình virus sẽ được ghép vào file chủ bằng cách nối thêm, chèn giữa, điền vào khoảng trống, ghi đè

Thuật toán thường dùng để lây nhiễm một file `.COM`:

- Mở file
- Ghi lại thời gian/ngày tháng/thuộc tính
- Lưu trữ các byte đầu tiên (thường là 3 byte)
- Tính toán lệnh nhảy mới
- Đặt lệnh nhảy
- Chèn thân virus chính vào
- Khôi phục thời gian/ngày tháng/thuộc tính
- Đóng file


##### 1.2. Kỹ thuật định vị trên vùng nhớ

Phân phối một vùng nhớ để thường trú, chuyển toàn bộ chương trình virus tới vùng nhớ này, sau đó chuyển quyền điều khiển cho đoạn mã tại vùng nhớ mới với địa chỉ `segment:offset` mới.

Virus macro và virus script thực chất là các lệnh của chương trình ứng dụng nên không cần tiến hành kỹ thuật này. Với  virus boot và virus file thì đây là một kỹ thuật quan trọng, cần phải có.

##### 1.3. Kỹ thuật kiểm tra sự tồn tại

Mỗi virus chỉ nên lây nhiễm/kiểm soát một lần để đảm bảo không làm ảnh hưởng đến tốc độ làm việc của máy tính => Cần có kỹ thuật kiểm tra sự tồn tại của mình trước khi lây nhiễm hoặc thường trú.

Kỹ thuật kiểm tra thường là:

- Dò tìm đoạn mã nhận diện trên file hoặc bộ 
nhớ.
- Kiểm tra theo kích thước hoặc nhãn thời 
gian của file

##### 1.4. Kỹ thuật thường trú

Cũng có thể gọi đây là kỹ thuật phân phối vùng nhớ, được thực hiện trước khi triển khai định vị tới vùng nhớ đã xác định đó.

- Các virus boot phải phân phối một vùng nhớ riêng để lưu giữ chương trình virus bao gồm mã lệnh, biến, vùng đệm.
- Các virus file cần phải kiểm tra xem chương trình đã thường trú chưa, nếu chưa sẽ định rõ vùng nhớ muốn sử dụng, copy phần virus vào bộ nhớ, sau đó khôi phục file chủ và trả quyền điều khiển về cho file chủ.


##### 1.5. Kỹ thuật mã hoá

Che giấu mã lệnh thực sự của chương trình virus. Thủ tục mã hoá cũng chính là thủ tục giải mã.

##### 1.6. Kỹ thuật nguỵ trang

Giấu giếm, nguỵ trang sự tồn tại của virus trên đối tượng chủ. Những virus sử dụng kỹ thuật này thường chậm bị phát hiện nên có khả năng lây lan mạnh.

Quy trình nén file để ngụy trang sự tồn tại của virus:
- Kiểm tra kích thước file chủ định lây nhiễm
- Nén file chủ
- Gắn đoạn mã cần lây nhiễm vào file chủ
- Có thể chèn thêm những đoạn ký tự vô nghĩa khi kích thước file chủ + virus vẫn nhỏ hơn kích thước file chủ ban đầu.
- Giải nén file chủ trước khi file này thực thi.

##### 1.7. Kỹ thuật phá hoại

Rất đa dạng trong phương thức phá hoại, có thể liên quan tới việ phá hoái dữ liệu trên máy tính hoặc phá hoại trực tiếp các chương trình hệ thống.

##### 1.8. Kỹ thuật chống bẫy

Chọn lọc file trước khi lây nhiễm theo một số tiêu chí nào đó nhằm tránh những file bẫy của chương trình Antivirus.

##### 1.9. Kỹ thuật tối ưu

Gồm các kỹ thuật viết mã và thiết kế nhằm tối ưu chương trình về tốc độ và kích thước

#### 2. Các kỹ thuật đặc biệt

### E. Phòng chống Virus

## III. Các giải thuật mã hóa

### A. Giới thiệu về mã hóa

### B. Giải thuật mã hóa cổ điển

### C. Giải thuật mã hóa hiện đại

### D. Bẻ gãy một hệ thống mật mã

## IV. Mã hóa công khai và quản lý khóa

### A. Hệ mã hoá khoá công khai

### B. Giao thức trao đổi khoá Diffie-Hellman

### C. Hệ RSA

### D. Quản lý khoá

## V. Chứng thực dữ liệu

### A. Mã chứng thực thông điệp

### B. Hàm băm

### C. Chữ ký số

## VI. Giao thức bảo mật mạng

### A. Vị trí của mật mã trong mạng máy tính

### B. Cơ sở hạ tầng khoá công khai

### C. IPsec

### D. SSL/TLS

### E. PGP và S/MIME

### F. Kerberos

### G. SSH

## VII. Bảo mật mạng không dây

### A. Tổng quan và phân loại

### B. Các kiểu chứng thực

### C. WEP, WPA và WPA2

### D. Các mối đe dọa

### E. Phương pháp và công cụ tấn công

### F. Tấn công mạng Bluetooth

### G. Biện pháp phòng chống

### H. Công cụ bảo mật Wi-Fi

### I. Kiểm thử hệ thống

## VIII. Bảo mật mạng ngoại vi

### A. Tổng quan
### B. Bộ lọc gói tin (Packet Filters)
### C. Cổng mạch (Circuit Gateways)
### D. Cổng ứng dụng (Application Gateways)
### E. Bastion Hosts
### E. Cấu hình tường lửa
### G. Chuyển dịch địa chỉ mạng (NAT)
### H. TMG – Threat Management Gateway
