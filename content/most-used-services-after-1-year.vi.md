+++
title = "Một năm homelab — những thứ mình thực sự dùng"
date = 2026-03-07
draft = false
description = "Sau một năm homelab và một giai đoạn đặc biệt không đụng tay được vào hệ thống, đây là những self-hosted service mình thực sự dùng hàng ngày: từ media streaming, quản lý ảnh, đến backup và notification."

[taxonomies]
categories = ["Technology", "Self-hosting"]
tags = ["homelab", "self-hosted", "jellyfin", "immich", "vaultwarden", "ntfy", "uptime-kuma", "restic", "backup", "media-server"]

[extra]
#cover.image = "images/homelab-one-year-#cover.png"
#cover.alt = "Một năm homelab — những thứ mình thực sự dùng"
+++


# Một năm homelab — những thứ mình thực sự dùng

Mình vừa trải qua một giai đoạn khá đặc biệt. Sau một thời gian dài tập trung chuẩn bị cho vợ sinh, mình gần như không đụng tay vào homelab thêm được gì. Nhưng bù lại, đây lại là giai đoạn mình *dùng* nhiều nhất những dịch vụ đã setup từ trước.

Và giờ thì em bé đã chào đời rồi 🎉

Nhìn lại, mình thấy biết ơn vì đã dành thời gian dựng lên những thứ đó. Không phải cái nào cũng tỏa sáng trong giai đoạn này — nhưng có một số thứ thực sự trở thành một phần trong cuộc sống hàng ngày của gia đình. Dưới đây là danh sách thật sự, không phải danh sách "nghe có vẻ hay ho".

---

## 1. Media streaming — giải trí gia đình không thể thiếu

Cày phim vẫn là hoạt động giải trí đơn giản và phổ biến nhất. Điểm thú vị là trong khi hầu hết các dịch vụ khác trong homelab gần như chỉ có mình mình xài, thì media streaming lại được cả nhà dùng — từ vợ đến bố mẹ hai bên.

Những khoảng giải lao ngắn ngủi trong giai đoạn chăm con sơ sinh mà có ngay một bộ phim quen để bật lên, không cần đăng nhập, không quảng cáo, không giật lag — thực sự là cứu cánh. Mình đang dùng **Jellyfin**, open-source, nhẹ, và hoạt động ổn định hơn mình tưởng.

---

## 2. Quản lý ảnh — tài sản số quan trọng nhất lúc này

Mình vẫn dùng Google Photos, nhưng việc có thêm một bản lưu trữ ngay tại nhà tạo cho mình cảm giác yên tâm rất khó tả. Ở giai đoạn này, ảnh gia đình — đặc biệt là những khoảnh khắc đầu đời của con — là tài sản số quan trọng nhất.

**Immich** đang là giải pháp chính, kết hợp với một vài script tự viết để tự động hóa việc sync và tổ chức kho ảnh. Không hoàn hảo, nhưng đủ dùng và quan trọng là dữ liệu nằm trên máy mình.

---

## 3. Mail archive, tóm tắt và thông báo — cái mình tự build

Cái này hơi khác một chút so với các dịch vụ còn lại vì mình tự build thay vì cài tool có sẵn.

Hệ thống tự lấy mail từ các provider (Gmail và vài cái khác), lưu vào local, sau đó dùng LLM để tổng hợp, phân loại, tóm tắt và gợi ý hành động. Cứ mỗi 2 tiếng máy tự chạy một lần, mình nhận được bản tóm tắt gọn gàng thay vì phải lội qua hàng chục email mỗi ngày.

Stack khá đơn giản: **mbsync** để sync mail, **notmuch** làm database, và một LLM service để xử lý ngôn ngữ tự nhiên.

Mình biết ngoài kia có nhiều tool làm việc tương tự. Nhưng với người có thể tự code, viết nhanh một cái ứng dụng làm đúng thứ mình muốn thường nhanh hơn là cài tool rồi vật lộn với config cho đến khi nó chạy đúng ý. Cái này mình không khuyến khích mọi người theo — chỉ là cách mình chọn.

---

## 4. Vaultwarden — quản lý mật khẩu tự host

Cái thời đại mà bạn có cả nghìn tài khoản, mỗi nơi một mật khẩu khác nhau, thêm vào đó là secret phrases, passkeys, ghi chú bảo mật — bạn *cần* một password manager. Không còn lựa chọn nào khác.

Nhưng tốt hơn nữa là khi những dữ liệu nhạy cảm đó nằm trên máy chủ của chính bạn, thay vì phải đặt niềm tin hoàn toàn vào một bên thứ ba. **Vaultwarden** là bản self-hosted tương thích với Bitwarden — client đẹp, đa nền tảng, và hoạt động rất ổn.

---

## 5. Uptime Kuma — để không bao giờ phải hỏi "cái này còn chạy không?"

Khi bạn đã có một mớ dịch vụ chạy thường trực, điều tệ nhất là đến lúc cần dùng mới phát hiện ra nó đã chết từ hồi nào.

**Uptime Kuma** là giải pháp monitor beginner-friendly nhất mình từng dùng. Setup trong vài phút, dashboard trực quan, thông báo qua nhiều kênh. Nó không phải thứ mình nghĩ đến nhiều hàng ngày — nhưng đúng nghĩa là "silent guardian" của hệ thống.

---

## 6. Backup — vì không ai muốn học bài học này theo cách khó

Khi hệ thống scale lên, câu hỏi không còn là "liệu có bị mất dữ liệu không" mà là "khi bị mất, mình recover được không và nhanh đến đâu."

Mình đang dùng **restic** kết hợp với **Cloudflare R2** (free tier). Do muốn giữ trong hạn mức miễn phí, mình viết custom script để chỉ backup những thứ thực sự cần thiết, không backup bừa bãi. Và quan trọng là cũng có script để recover nhanh — vì backup không có recover test thì cũng như không có backup.

---

## 7. Ntfy — điện thoại là cửa ngõ giữa server và con người

Mình không thể ngồi trực bên máy server. Cũng không thể mỗi nửa tiếng lại mở dashboard lên kiểm tra. Cuộc sống không cho phép vậy — nhất là giai đoạn vừa rồi.

Vì thế điện thoại mới là kênh giao tiếp thực sự giữa server và mình. Khi có gì bất thường — một dịch vụ chết, một backup chạy xong, một cron job fail — mình muốn biết ngay, ở bất cứ đâu, không cần chủ động đi kiểm tra.

**Ntfy** giải quyết đúng bài toán đó. Một HTTP endpoint đơn giản: bất kỳ script hay service nào trong hệ thống cũng có thể gửi notification về điện thoại chỉ với một câu `curl`. Không cần tích hợp phức tạp, không cần SDK, không cần tài khoản bên thứ ba.

Lý do mình chọn Ntfy thực ra rất thực dụng: mình cài nó đầu tiên, thấy chạy ổn, rồi chưa bao giờ có lý do để thử cái khác. Đôi khi đó là dấu hiệu tốt nhất của một công cụ.

---

## Tạm kết

Nhìn lại, những thứ thực sự hữu ích không phải là những thứ nghe "xịn" nhất lúc setup — mà là những thứ đủ ổn định để chạy mà không cần mình để mắt, và đủ hữu dụng để người khác trong gia đình cũng dùng được.

Mình vẫn còn nhiều thứ muốn nghịch tiếp trong homelab. Nhưng trước mắt, có một em bé cần chăm — và may mắn là cái hệ thống cũ đã lo được phần còn lại 🙂
