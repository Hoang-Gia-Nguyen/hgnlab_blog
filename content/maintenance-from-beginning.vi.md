+++
title = "Self-host không chỉ là deploy: Maintenance mới là cuộc chơi thật"
date = 2026-04-04
description = "Self-host không chỉ là chạy được dịch vụ. Maintenance mới là phần khó nhất — và cần được nghĩ tới ngay từ ngày đầu."
slug = "self-host-maintenance-tu-ngay-dau"
draft = false

[extra]
#cover.image = "images/maintenance-from-beginning-#cover.png"
#cover.alt = "Self-Hosting Isn’t Deployment — Maintenance Is the Real Game"

[taxonomies]
tags = ["self-host", "devops", "maintenance", "homelab"]
categories = ["engineering", "self-host"]
+++

# Khi cuộc sống “bận” hơn hệ thống của bạn

Dạo gần đây mình bắt đầu một công việc mới. Em bé mới sinh cũng cần rất nhiều sự chăm sóc. Góc phòng nơi đặt chiếc server quen thuộc — thứ từng được mình chăm chút từng chút một — giờ gần như bị lãng quên. Màn hình không còn sáng, quạt vẫn quay, nhưng mình thì không còn thời gian để “nghịch” nó như trước.

Rồi một ngày, mọi thứ bắt đầu đòi lại sự chú ý.

Ứng dụng ghi chép chi tiêu của mình sập.  
Server Plex — nơi vợ mình xem phim — cũng không truy cập được nữa.

Không phải là mình không biết sửa. Nhìn qua là mình có thể đoán được hướng xử lý. Nhưng vấn đề là: để sửa cho đàng hoàng, mình cần vài tiếng tập trung.

Và vài tiếng đó… mình không có.

---

# Khi bạn nhận ra: thời gian mới là tài nguyên khan hiếm nhất

Trước đây, mình có thể dành cả buổi tối để debug, thử nghiệm, thậm chí phá đi làm lại. Nhưng bây giờ, thực tế là:

- Mỗi ngày may mắn có khoảng 1 tiếng rảnh
- Thậm chí không phải ngày nào cũng có
- Và khoảng thời gian đó không liên tục

Lúc này mình mới nhận ra một điều khá đau:

> Có những service mình setup rất “có suy nghĩ” từ đầu — sửa cực kỳ nhanh.  
> Nhưng cũng có những cái mình làm qua loa — giờ trở thành cục nợ.

Cùng là self-host, nhưng trải nghiệm maintain khác nhau hoàn toàn.

---

# Maintenance không phải là “việc sau deploy”

Khi nói tới self-host, đa phần chúng ta nghĩ tới:
- chạy được service
- expose ra internet
- truy cập được là xong

Nhưng thực tế, đó chỉ là bước đầu.

Maintenance bao gồm:
- Update hệ thống và ứng dụng
- Backup & restore dữ liệu
- Theo dõi tình trạng hệ thống
- Debug khi có sự cố
- Đảm bảo an toàn (security)

Và quan trọng nhất:

> Maintenance không xảy ra *sau* khi deploy.  
> Nó phải được nghĩ tới *ngay từ lúc bắt đầu*.

---

# Những gì mình làm (và đã trả giá để học được)

## 1. Update: Đừng để “thích là upgrade”

Update luôn là phần hấp dẫn nhất:
- tính năng mới
- bug fix
- cảm giác “up-to-date”

Nhưng nếu update mà không có rollback, bạn đang đánh cược toàn bộ hệ thống.

Với các service chạy bằng Docker, cách mình đang áp dụng:

- Đọc changelog trước khi upgrade
- Backup config + data
- Giữ nguyên container version cũ
- Chạy container mới song song
- Test ổn → mới xóa cái cũ

Với ứng dụng tự viết cũng tương tự:
- Deploy song song 2 version
- Tránh “đập đi build lại” trực tiếp

> Deploy mới không nên phá hủy cái đang chạy.

---

## 2. Backup & Restore: Không có thì đừng chơi

Nếu phải chọn một thứ bắt buộc phải có ngay từ đầu, thì đó là backup.

Không phải:
> “Sau này rảnh mình sẽ setup backup”

Vì hệ thống có thể chết **trước khi bạn kịp làm điều đó**.

Một setup đơn giản:
- `rsync` mỗi tuần
- Copy config + data sang ổ khác hoặc cloud

Đã đủ để cứu bạn trong nhiều tình huống.

Nhưng backup không chỉ là copy file:

- Tốn storage → cần tính chi phí
- Phải chọn:
  - cái gì backup
  - cái gì chấp nhận mất
- Cần có:
  - retention strategy
  - tần suất hợp lý

Và quan trọng nhất:

> Test restore.  
> Test restore.  
> Test restore.

Có file backup không có nghĩa là bạn restore được.

---

## 3. Observability: Đừng để hệ thống chết trong im lặng

Nếu bạn không theo dõi hệ thống, bạn sẽ chỉ biết nó có vấn đề khi… nó chết.

Những thứ tối thiểu nên có:
- Disk usage
- RAM / CPU
- Nhiệt độ
- Network traffic
- Uptime service

Không cần gì quá phức tạp. Chỉ cần đủ để:

> Phát hiện vấn đề sớm → chủ động xử lý khi còn thời gian.

Thay vì:
> Đợi tới lúc cần dùng → mới biết nó hỏng.

---

## 4. Security: Không ai nhắm bạn — nhưng bot thì có

Một suy nghĩ rất phổ biến:

> “Server nhỏ của mình thì ai hack làm gì?”

Thực tế:
- Không ai target bạn
- Nhưng bot scan thì quét **toàn internet**

Chỉ cần:
- Một port mở
- Một service config sơ sài

Là đủ để trở thành entry point vào network nhà bạn.

Một vài nguyên tắc mình cố gắng giữ:
- Chỉ expose những gì thật sự cần
- Ưu tiên local / VPN
- Áp dụng principle of least privilege
- Không cho service quyền nhiều hơn mức cần thiết

Security là một lĩnh vực rất rộng. Không cần hoàn hảo, nhưng:

> Làm tốt những thứ cơ bản đã giúp bạn tránh được rất nhiều rủi ro “ngớ ngẩn”.

---

# Maintenance là một bài toán thiết kế

Sau tất cả, điều mình nhận ra là:

> Maintenance không phải là vấn đề vận hành.  
> Nó là vấn đề thiết kế.

Ngay từ đầu, bạn nên tự hỏi:
- Làm sao update an toàn?
- Làm sao rollback?
- Nếu service chết thì sao?
- Nếu mất data thì sao?

Nếu bạn không trả lời những câu hỏi này từ đầu, bạn sẽ phải trả lời chúng… trong lúc hệ thống đang hỏng.

---

# Một lời nhắn cho “bạn của tương lai”

Bạn của 3 tháng nữa:
- sẽ quên config hôm nay
- sẽ không nhớ vì sao setup như vậy
- có thể sẽ lỡ tay xóa nhầm một thứ rất quan trọng

Và lúc đó, bạn sẽ ước:

> “Giá mà lúc đầu mình làm cái này cẩn thận hơn một chút”

---

# Kết

Self-host không phải là một project có điểm kết thúc.  
Nó là một hệ thống bạn phải sống chung với nó.

Và trong hệ thống đó:

- Disk sẽ đầy  
- Service sẽ crash  
- Network sẽ fail  

Không phải *nếu*, mà là *khi nào*.

Vì vậy, ngay từ lúc bắt đầu:

> Hãy thiết kế cho maintenance.  
> Hãy design for failure.

Bạn không chỉ đang deploy một hệ thống.  
Bạn đang nhận trách nhiệm vận hành nó.
