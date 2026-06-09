+++
title = "Cloudflare Stack: 'Vũ khí bí mật' cho anh em tự build homelab"
date = 2026-04-05
description = "Vượt CGNAT, bảo mật dịch vụ, và host mọi thứ gần như miễn phí. Tại sao Cloudflare là bộ công cụ đỉnh nhất cho dân tự vọc vạch hiện nay."
authors = ["hgn"]
[taxonomies]
categories = ["Self-hosting"]
tags = ["cloudflare", "self-hosting", "security"]

[extra]
#cover.image = "images/cloudflare-stack-#cover.png"
#cover.alt = "Cloudflare Stack Illustration"
+++

Nếu bạn đang tự host dịch vụ tại nhà trong năm 2026, chắc hẳn bạn đã từng va phải "bức tường" của nhà mạng. Nào là CGNAT không cho mở port, nào là không có IP tĩnh... Trước đây, việc đưa một dịch vụ lên internet an toàn là cả một vấn đề đau đầu.

Nhưng từ khi mình khám phá ra **Cloudflare Stack**, mọi thứ đã thay đổi hoàn toàn. Đây giống như một bộ "cheat code" giúp giải quyết hầu hết mọi vấn đề hạ tầng chỉ với chi phí bằng vài ly cà phê mỗi năm.

### 1. Cloudflare Tunnel: Sát thủ diệt CGNAT
Đây chính là linh hồn trong setup của mình. Thay vì phải lúi húi mở port trên router (vừa rủi ro vừa khó nếu dùng mạng gia đình bình thường), mình chỉ cần chạy một container `cloudflared` siêu nhẹ. Nó sẽ tạo một đường truyền an toàn từ nhà mình đến Cloudflare.
- **Không cần IP tĩnh:** IP nhà bạn có đổi xoành xoạch cũng không thành vấn đề.
- **Vượt CGNAT:** Hoạt động hoàn hảo ngay cả khi nhà mạng giấu bạn sau một lớp IP riêng.
- **Bảo mật sẵn có:** Bạn được hưởng luôn khả năng chống DDoS và WAF của Cloudflare ngay lập tức.

### 2. Tên miền giá rẻ (Tầm 180k/năm là có "hàng xịn")
Đã chơi homelab thì phải có tên miền. Thay vì đâm đầu vào `.com` hay `.net` đắt đỏ, mình chọn những đuôi ít phổ biến hơn. Như tên miền của mình chỉ tốn khoảng **180.000 VNĐ/năm**. Một cái giá quá rẻ để thay thế dãy IP loằng ngoằng bằng `dichvu.tenmien.com` chuyên nghiệp.

### 3. Lưu trữ hào phóng: D1 & R2
Cloudflare bây giờ không chỉ để điều hướng mạng nữa.
- **D1 (SQL Database):** Cực kỳ hợp cho mấy app nhỏ. Mình dùng nó cho app quản lý chi tiêu cá nhân. Gói miễn phí của nó hào phóng đến mức mình chưa bao giờ phải trả xu nào.
- **R2 (Object Storage):** Lưu trữ tương thích S3 nhưng **không tốn phí tải về (egress fee)**. Mình dùng cái này để lưu backup và ảnh cho mấy ứng dụng serverless.

### 4. Tự do với Serverless: Workers & Pages
Tại sao cái gì cũng phải chạy trên server ở nhà?
- **Cloudflare Pages:** Toàn bộ blog này của mình đang host ở đây. Nó nhanh, miễn phí và tự động cập nhật mỗi khi mình đẩy code lên GitHub.
- **Cloudflare Workers:** Mình đã chuyển mấy dịch vụ "nhẹ cân" như web chia sẻ text (bin) hay tracker chi tiêu lên đây. 
Việc này giúp server ở nhà mình "nhẹ gánh" hơn, và mỗi lần nâng cấp hay rollback cũng dễ hơn hẳn—chỉ cần một lệnh `git push` là xong.

### 5. Zero Trust: "VPN" kiểu mới
Dùng VPN truyền thống đôi khi hơi phiền phức. Với **Cloudflare Zero Trust**, mình có thể đặt một lớp bảo mật (như đăng nhập bằng Google, GitHub hoặc mã OTP gửi qua mail) trước bất kỳ dịch vụ nào. Mình có thể truy cập dashboard ở nhà từ điện thoại ở bất cứ đâu mà không cần thực sự "connect" vào VPN—nó cứ thế hoạt động mượt mà qua trình duyệt nhưng vẫn cực kỳ an toàn. **Lưu ý nhỏ là đây là công cụ bảo mật và kết nối, không phải công cụ ẩn danh (privacy).** Tuy nó giúp cách ly kết nối của bạn, nhưng nó không được thiết kế để giấu bạn hoàn toàn khỏi nhà mạng hay internet như mấy dịch vụ VPN "fake IP" thường thấy đâu nhé.

### Kết luận
Dùng Cloudflare Stack không chỉ là để tiết kiệm tiền, mà quan trọng nhất là nó **loại bỏ mọi rào cản**. Nó giúp mình tập trung vào việc *mình đang xây dựng cái gì*, thay vì phải loay hoay với việc *làm sao để nó online và không bị hack*.

Nếu bạn mới bắt đầu hành trình self-hosting, đây chính là bộ công cụ mà bạn nên tìm hiểu ngay và luôn.
