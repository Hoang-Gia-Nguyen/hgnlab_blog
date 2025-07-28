+++
title = "Xây Dựng Homelab Đầu Tiên Của Bạn: Chặn Quảng Cáo Với AdGuard Home"
date = 2025-07-15
draft = false
description = "Chuyển từ lý thuyết sang thực hành với hướng dẫn xây dựng homelab đầu tiên. Tôi sẽ chỉ cho bạn cách mình dùng Docker để chạy AdGuard Home, một trình chặn quảng cáo mạnh mẽ cho toàn mạng, và cách bạn có thể bắt đầu với phần cứng của riêng mình."
[taxonomies]
categories = ["Technology"]
tags = ["self-hosting", "homelab", "docker"]

[extra]
cover.image = "images/self-hosting-adguard.png"
cover.alt = "Homelab setup with AdGuard Home"
+++

Có thể bạn đã đọc [bài viết trước của mình về "self-hosting là gì và tại sao"](@/what-why-self-hosting.vi.md). Nhưng có một sự khác biệt lớn giữa việc hiểu một khái niệm và thực sự thử nó. Vậy, bạn nên bắt đầu từ đâu?

Câu trả lời là bắt đầu với việc xây dựng một homelab.

Đừng để khái niệm này làm bạn sợ. Một "homelab" không phải là một tủ rack đầy máy chủ và cần được vận hành trong phòng lạnh. Nó chỉ đơn giản là một chiếc máy tính tại nhà của bạn, luôn bật, sẵn sàng để chạy các dịch vụ bạn cần.

Khi đọc xong bài viết này, bạn sẽ có một homelab đơn giản, hoạt động với ứng dụng self-hosted đầu tiên của bạn. Không cần kiến thức chuyên môn. Hãy cùng nhau bắt đầu nào.

---

### Chương 1: Lựa Chọn Phần Cứng (Mà Không Tốn Kém)

Quy tắc đầu tiên của việc xây dựng homelab là bắt đầu nhỏ và rẻ. Mục tiêu là để học hỏi và thử nghiệm, không phải để xây dựng một trung tâm dữ liệu. Có lẽ bạn đã có sẵn thứ gì đó hoàn toàn phù hợp.

#### Lựa chọn A: Raspberry Pi
Đây là điểm khởi đầu kinh điển vì một lý do.
*   **Ưu điểm:** Tiêu thụ ít điện, hoàn toàn im lặng, có một cộng đồng hỗ trợ khổng lồ, và kích thước hoàn hảo cho nhu cầu của người mới bắt đầu.
*   **Nhược điểm:** Sức mạnh xử lý hạn chế.
*   **Khuyến nghị:** Lý tưởng để chạy một vài dịch vụ nhẹ.

#### Lựa chọn B: Laptop & Máy tính để bàn cũ
Trước khi bạn vứt bỏ laptop hoặc bộ máy tính cũ đó, hãy xem xét tiềm năng của nó.
*   **Ưu điểm:** Nó miễn phí! Và gần như chắc chắn mạnh hơn Raspberry Pi. Một chiếc laptop cũ đặc biệt tuyệt vời.
*   **Nhược điểm:** Sẽ tốn nhiều điện hơn (mà thật ra cũng không quá đáng kể) và chiếm nhiều không gian hơn Pi.
*   **Khuyến nghị:** Cho bất cứ ai có sẵn một cái máy cũ.

#### Lựa chọn C: Mini PC
Nếu bạn sẵn sàng chi một ít tiền, bạn có thể có một chiếc máy chuyên dụng.
*   **Ưu điểm:** Hiệu suất tuyệt vời, nhỏ gọn và hiện đại. Các thương hiệu như Beelink, Minisforum, hoặc dòng Intel NUC là những lựa chọn phổ biến.
*   **Nhược điểm:** Giá có thể dao động từ 150 đến 400 đô la cho một chiếc máy đủ khả năng.
*   **Khuyến nghị:** Lựa chọn tốt nhất nếu bạn biết mình muốn nghiêm túc với việc self-hosting và muốn có một nền tảng vững chắc để xây dựng lên một data center với nhiều ứng dụng.

Trong hướng dẫn này, mình sẽ sử dụng **Mac Mini M4** của mình. Tại sao? Bởi vì đó là chiếc máy mình đã có sẵn, và nó luôn bật 24/7. Máy chủ tốt nhất thường là máy chủ bạn không cần phải mua. Chip Apple M4 cũng cực kỳ tiết kiệm điện, đây là một điểm cộng lớn cho một thiết bị luôn bật.

macOS không phải là một hệ điều hành lý tưởn cho máy chủ. Tuy nhiên, vì gần như tất cả các dịch vụ tự host của mình đều chạy bên trong các container Docker, hệ điều hành cơ bản không còn quan trọng nữa. Docker cung cấp một môi trường Linux nhất quán, cô lập cho các ứng dụng của mình.

Phần còn lại của hướng dẫn này sẽ giả định bạn đang làm việc trong một môi trường Linux, trong trường hợp của mình là do Docker Desktop trên macOS cung cấp. Các lệnh sẽ giống nhau cho dù bạn đang dùng Mac với Docker, một máy Linux chuyên dụng, hay một chiếc Raspberry Pi.

---

### Chương 2: Bộ Não Của Hệ Thống - Lựa Chọn Hệ Điều Hành

Phần cứng của bạn cần một bộ não — một hệ điều hành. Mặc dù mình đang sử dụng macOS với Docker, con đường phổ biến và được khuyến nghị nhất cho một máy chủ homelab chuyên dụng là sử dụng một hệ điều hành dựa trên Linux.

#### Lựa Chọn Hàng Đầu: Một Hệ Điều Hành Máy Chủ Tiêu Chuẩn
*   **Là gì:** **Ubuntu Server** hoặc **Debian**.
*   **Tại sao:** Chúng là nền tảng của thế giới máy chủ. Chúng cực kỳ ổn định, an toàn, và có nhiều hướng dẫn, bài viết và diễn đàn hơn bạn có thể đọc trong cả cuộc đời. Đối với mục đích của chúng ta, chúng cung cấp một nền tảng hoàn hảo, không rắc rối để chạy Docker trực tiếp.
*   **Cài đặt:** Quá trình cài đặt cho chúng được tài liệu hóa rất kỹ lưỡng. Mục tiêu của bạn là cài đặt hệ điều hành và đảm bảo bạn có thể kết nối với nó từ máy tính chính của mình bằng SSH.
    *   Đối với Raspberry Pi: Sử dụng [Raspberry Pi Imager](https://www.raspberrypi.com/software/) chính thức. Đó là một công cụ tuyệt vời giúp bạn làm tất cả công việc khó khăn.
    *   Đối với các máy tính khác: Làm theo [hướng dẫn cài đặt Ubuntu Server](https://ubuntu.com/tutorials/install-ubuntu-server) chính thức.

---

### Chương 3: Các Bước Đầu Tiên - Mạng và Docker

Khi máy chủ của bạn đã hoạt động, có hai khái niệm cần hiểu trước khi chúng ta có thể triển khai ứng dụng đầu tiên: Static IP và Docker container.

#### Tầm Quan Trọng Của Địa Chỉ IP Tĩnh
Router nhà bạn gán địa chỉ IP cục bộ (như `192.168.1.123`) cho các thiết bị khi chúng kết nối. Mặc định, điều này là động, có nghĩa là địa chỉ IP của một thiết bị có thể thay đổi mỗi khi nó khởi động lại. Đối với laptop hoặc điện thoại, điều này hoàn toàn ok.

Tuy nhiên, đối với một máy chủ, đó là một vấn đề. Nếu bạn muốn kết nối ổn định tới các dịch vụ của mình (như trình chặn quảng cáo của chúng ta), bạn cần biết địa chỉ của nó. Nếu địa chỉ liên tục thay đổi, bạn sẽ phải tìm nó mỗi khi khởi động lại máy.

Gán một **địa chỉ IP tĩnh** sẽ giải quyết vấn đề này. Nó giống như việc cấp cho máy chủ của bạn một chỗ đậu xe cố định, được đặt trước trên mạng của bạn. Bạn đang nói với router của mình rằng máy tính cụ thể này *luôn luôn* nên có địa chỉ cụ thể này. Điều này có thể được thực hiện trong cài đặt quản trị của router, thường nằm trong một mục gọi là "DHCP Reservations" hoặc "Static Leases." Hoặc được thực hiện trực tiếp từ giao diện của máy chủ.

#### Giới Thiệu Docker: Người Bạn Thân Trong Giới Self-Hosting
Docker là gì? Trong một câu: **Docker  là nền tẳng cho phép bạn chạy các ứng dụng trong các môi trường ảo, sạch sẽ, cô lập, để bạn không làm xáo trộn hệ điều hành của máy chủ.**

Nếu không dùng Docker, bạn sẽ cài đặt các ứng dụng trực tiếp lên máy chủ. Điều này có thể dẫn đến xung đột giữa các ứng dụng.

Docker giải quyết vấn đề này bằng cách gói mỗi ứng dụng và tất cả các dependency của nó vào một "container" khép kín. Bạn có thể chạy hàng chục container trên cùng một máy mà chúng không bao giờ can thiệp vào nhau hoặc vào hệ điều hành chủ. Nó đơn giản hóa việc cài đặt, cập nhật và gỡ bỏ ứng dụng chỉ bằng một vài lệnh đơn giản.

Cài đặt Docker là bước tiếp theo. Quá trình này thay đổi một chút tùy thuộc vào hệ điều hành của bạn, vì vậy tốt nhất là làm theo tài liệu chính thức.
*   **[Hướng Dẫn Cài Đặt Docker Chính Thức](https://docs.docker.com/engine/install/)**

---

### Chương 4: Ứng Dụng Đầu Tiên - Khoảnh Khắc "Hello World" với AdGuard Home

Chúng ta sẽ cài đặt **AdGuard Home**, một trình chặn quảng cáo và theo dõi trên toàn mạng.

*   **Tại sao lại là AdGuard Home?** Nó mang lại giá trị tức thì, hữu hình—một khi nó hoạt động, nó sẽ chặn quảng cáo trên *mọi thiết bị* trong mạng của bạn (điện thoại, laptop, TV thông minh). Nó nhẹ, có giao diện web tuyệt vời, và là một sự giới thiệu hoàn hảo về mô hình máy chủ/máy khách.

Chúng ta sẽ sử dụng `docker-compose` để định nghĩa ứng dụng. Đây là một tệp văn bản đơn giản mô tả dịch vụ chúng ta muốn chạy.

1.  Tạo một thư mục mới cho cấu hình AdGuard Home của bạn: `mkdir adguard && cd adguard`
2.  Tạo một tệp mới có tên `docker-compose.yml`: `nano docker-compose.yml`
3.  Dán nội dung sau vào tệp:

```yaml
version: "3.7"
services:
  adguardhome:
    image: adguard/adguardhome
    container_name: adguardhome
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "3000:3000/tcp"
    volumes:
      - ./workdir:/opt/adguardhome/work
      - ./confdir:/opt/adguardhome/conf
    restart: unless-stopped
```

Hãy phân tích nhanh:
*   `image`: Cho Docker biết ứng dụng nào cần tải về.
*   `ports`: Ánh xạ các cổng từ máy chủ đến container Docker. Chúng ta ánh xạ cổng `53` cho các truy vấn DNS và cổng `3000` cho giao diện web.
*   `volumes`: Điều này rất quan trọng. Nó ánh xạ các thư mục cấu hình từ bên trong container đến các thư mục `workdir` và `confdir` trên máy chủ của chúng ta. Điều này có nghĩa là cài đặt AdGuard của bạn sẽ được lưu lại ngay cả khi bạn cập nhật hoặc tạo lại container.

Bây giờ, đến lệnh thần kỳ. Từ bên trong thư mục `adguard` của bạn, chạy:

```bash
docker-compose up -d
```

Docker bây giờ sẽ tải image AdGuard Home và khởi động nó như một dịch vụ nền. Bạn có thể kiểm tra xem nó có đang chạy không bằng `docker ps`.

#### Cấu Hình & Thiết Lập Router

Khi container đang chạy, có hai giai đoạn cấu hình cuối cùng: thiết lập chính ứng dụng, và sau đó yêu cầu mạng của bạn sử dụng nó.

1.  **Thiết Lập AdGuard Home Lần Đầu:** Mở trình duyệt web và truy cập `http://<địa-chỉ-ip-máy-chủ>:3000`. Bạn sẽ được chào đón bởi trình hướng dẫn cài đặt AdGuard Home. Nó rất đơn giản; hãy làm theo hướng dẫn trên màn hình để đặt tên người dùng và mật khẩu quản trị.

2.  **Cấu Hình Router Của Bạn:** Đây là bước quan trọng nhất. Để có được trình chặn quảng cáo trên toàn mạng, bạn cần yêu cầu router của mình sử dụng máy chủ AdGuard Home mới làm trình phân giải DNS. Quá trình này hơi khác nhau đối với mỗi router, nhưng nói chung bạn cần:
    *   Đăng nhập vào trang quản trị của router (thường là `192.168.1.1`).
    *   Tìm cài đặt **DNS**. Thường nằm trong mục **DHCP** hoặc **LAN**.
    *   Thay đổi máy chủ DNS chính (và nếu có thể, cả phụ) thành địa chỉ IP tĩnh bạn đã gán cho máy chủ homelab của mình. Trong ví dụ của mình 192.168.1.10 là địa chỉ IP tĩnh của chiếc Mac Mini M4 mình đang sử dụng.
    *   Lưu cài đặt. Router của bạn có thể cần khởi động lại.

![Setting Primary DNS to your server IP](/images/screenshot_setdns.png)

Khi router của bạn đang sử dụng AdGuard Home cho DNS, mọi thiết bị kết nối vào mạng của bạn sẽ tự động bị chặn quảng cáo và trình theo dõi.

Và cứ như thế ... quảng cáo đã biến mất. Hãy thử lướt vài trang web trên điện thoại hoặc máy tính của bạn; bạn sẽ nhận thấy sự khác biệt ngay lập tức. Bạn không chỉ đang sử dụng internet, bạn đang bắt đầu *kiểm soát* nó.

![AdGuard Home Interface](/images/screenshot_adguardhome.png)

---

### Kết Luận: Bạn Đã Xây Dựng Một Homelab!

Hãy dành một chút thời gian để đánh giá cao những gì bạn đã đạt được. Bạn đã lấy một phần cứng, cài đặt một hệ điều hành máy chủ, cấp cho nó một địa chỉ ổn định trên mạng của bạn, và triển khai một ứng dụng thực tế giúp cải thiện trải nghiệm internet hàng ngày của bạn. Bạn đã có một homelab.

Hành trình không kết thúc ở đây. Đây là nền tảng của bạn. Bây giờ bạn đã có thiết lập này, bạn có thể khám phá việc tự host:
*   **Trình Quản Lý Mật Khẩu** với Vaultwarden
*   **Dịch Vụ Đồng Bộ Hóa Tệp** (giống như Dropbox cá nhân của bạn) với Syncthing
*   **Máy Chủ Đa Phương Tiện** với Jellyfin

Khả năng là vô tận. Vậy, bạn sẽ host gì đầu tiên? Hãy cho mình biết trong phần bình luận bên dưới!
