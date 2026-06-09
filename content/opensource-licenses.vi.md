+++
title = "Open source license - Bình dân học vụ"
date = 2026-05-26
description = "Nói chuyện tếu táo về các open source license mà anh em hay gặp"

[taxonomies]
categories = ["Technology"]
tags = ["open-source", "licensing", "software-licenses", "intellectual-property"]
+++

# 📜 Bí Kíp Chọn Open Source License "Bình Dân Học Vụ" – Tránh Bẫy Bản Quyền Cho Doanh Nghiệp & Dev

Chào anh em, thế giới mã nguồn mở (Open Source) nghe thì đao to búa lớn, nhưng thực chất mấy cái License (giấy phép) giống như **"luật giang hồ"** khi anh em share code với nhau vậy. 

Nhiều ông cứ thấy code trên GitHub là hí hửng "bê" về xài, đến khi công ty bị kiện sấp mặt hoặc bị bắt **công khai sạch sành sanh mã nguồn** thì mới khóc ròng. 

Hôm nay, mình xin tóm gọn lại bản đồ License thế giới theo phong cách "bình dân học vụ" – đọc một lần là hiểu ngay để né bẫy nhé!

---

### 1. Phái "Làm Gì Thì Làm" (Permissive) – Đèn Xanh 🟢
Mấy ông này thoáng nhất hệ mặt trời, doanh nghiệp hay dev cứ tự tin xài thoải mái.

* **MIT & Apache 2.0:** *"Tớ cho cậu code này nè, cầm về muốn xài, muốn sửa, muốn đóng gói đem bán lấy tiền gì tùy ý. Miễn là đừng xóa tên (credit) của tớ ra khỏi nguồn và sau này app có sập, hệ thống có sập thì đừng đến bắt đền tớ là được."*

### 2. Phái "Bánh Ít Trao Đi, Bánh Quy Trao Lại" (Copyleft) – Đèn Vàng Khè 🟡
Mấy ông này sống theo chủ nghĩa "đại đồng", tôi tốt với bạn thì bạn phải tốt với xã hội. Doanh nghiệp làm app độc quyền thương mại phải cực kỳ né dòng này ra.

* **GNU GPL & GNU LGPL:** *"Tớ cho cậu xài code miễn phí, nhưng app nào dính tới code này thì cũng **PHẢI mở mã nguồn** (opensource) cho thiên hạ xài chung. Cấm giữ làm của riêng để đem bán bản quyền thương mại đấy nhé!"* *(Mẹo nhỏ: LGPL thì nhẹ hơn tí, nếu chỉ mượn làm cái 'móng nhà' (thư viện) thì được, còn GPL là dính vào đâu là 'lây' opensource tới đó).*

### 3. Phái "Ngụy Mở" (Chống Cạnh Tranh) – Đèn Đỏ Cảnh Báo 🔴
Đây là chiêu bài của các ông lớn công nghệ sau khi đã đủ lông đủ cánh và muốn bảo vệ nồi cơm của mình (Ví dụ điển hình: MongoDB, Redis, ElasticSearch...).

* **SSPL & BSL:** *"Nhìn thì giống Open Source đấy nhưng thực chất là **cấm doanh nghiệp lấy code này làm sản phẩm cạnh tranh trực tiếp** với chính chủ."* *(Nôm na: Bạn lấy Redis về chạy app cho công ty thì được, nhưng lấy Redis về rồi tự tạo ra một dịch vụ 'Cloud Redis' để thu tiền của người khác là ăn gậy ngay).*

### 4. Phái "Văn Nghệ Cho Vui" (Sáng Tạo Nội Dung) – Cẩn Thận Tuyệt Đối ⚠️
Mấy giấy phép này hay xuất hiện ở các kho tài nguyên như hình ảnh, font chữ, âm thanh, icon...

* **CC BY-NC (Non-Commercial):** *"Xài thoải mái, chỉnh sửa thoải mái nhưng **cấm dùng để kiếm tiền**."* (Local brand hay startup bê cái ảnh dính license này vào poster quảng cáo là ăn kiện liền).
* **CC BY-ND (No-Derivatives):** *"Xài thoải mái cho cả thương mại, nhưng **cấm chỉnh sửa, chế cháo** dưới mọi hình thức."* (Lỡ tay Photoshop đổi màu cái icon cho hợp phong thủy công ty là vi phạm luật).

### 5. Phái "Đi Tu / Buông Bỏ" – Đèn Xanh Sáng Ngời 🟢
Những con người cống hiến thuần túy, không màng danh lợi hay sự đời.

* **Unlicense / CC0:** *"Coi như tớ chưa từng viết ra đống code này đi, của thiên hạ tất cả đấy. Mang đi đâu thì mang, làm gì thì làm, tớ đi tu đây không màng sự đời nữa."*

### 6. Phái "Tâm Linh & Nhân Quả" – Luật Sư Né Gấp 🧘‍♂️
Nghe rất nhân văn nhưng các doanh nghiệp lớn cấm tiệt nhân viên đụng vào vì... rủi ro pháp lý không lường trước được.

* **JSON & Karma License:** *"Cậu dùng code thoải mái, nhưng **phải cam kết dùng code này làm điều thiện chứ không được làm điều ác**."* *(Khổ nỗi dưới góc độ pháp lý, đố ai định nghĩa được thế nào là thiện, thế nào là ác. Lỡ làm app tài chính bị tính là ác hay thiện? Nên tốt nhất là né!).*

---

### 💡 Tóm lại cho dễ nhớ:
* **Làm app kiếm tiền, đóng gói bán:** Chọn **MIT, Apache 2.0, Unlicense**.
* **Thấy chữ GPL, AGPL, SSPL:** Đọc kỹ hướng dẫn sử dụng trước khi liều, kẻo "dâng" trắng trợn công sức cho đối thủ.