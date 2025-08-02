+++
title = "Giới thiệu Immich: Giải pháp thay thế Google Photo"
date = 2025-07-23
description = "Khám phá Immich, giải pháp sao lưu ảnh và video mã nguồn mở, tự host giúp bạn toàn quyền kiểm soát dữ liệu của mình và là một sự thay thế mạnh mẽ cho Google Photos."

[taxonomies]
categories = ["Technology", "Self-hosting"]
tags = ["self-hosting", "homelab"]

[extra]
cover.image = "images/self-hosting-immich-introduction-cover.png"
cover.alt = "Immich Introduction Cover Image"
+++

## Immich là gì?

Hình ảnh và video không chỉ là file, mà còn là cả một bầu trời kỷ niệm. Mất đi thì tiếc hùi hụi. Bạn chắc chắn cần có một chiến lược sao lưu ngon nghẻ, và "tiêu chuẩn vàng" trong làng backup chính là **quy tắc 3-2-1**: giữ ít nhất **ba** bản sao dữ liệu, trên ít nhất **hai** thiết bị khác nhau, và có **một** bản sao được cất ở một nơi khác (off-site).

Dù các dịch vụ "trên mây" như Google Photos là một chỗ backup off-site tuyệt vời, nhưng nếu chỉ phụ thuộc vào nó thì cũng hơi run. Lỡ một ngày đẹp trời bạn mất quyền truy cập tài khoản, hay dịch vụ đổi chính sách thì sao? Đây chính là lúc Immich vào việc.

Immich là một giải pháp sao lưu ảnh và video mã nguồn mở, do bạn tự host, cho phép bạn tạo ra một bản sao cục bộ thứ hai cực kỳ quan trọng cho kho báu kỷ niệm của mình, ngay trên máy chủ tại nhà bạn. Nó chính là mảnh ghép hoàn hảo cho quy tắc 3-2-1, cho bạn một bản sao tại nhà, truy cập vèo vèo mọi lúc. Không chỉ để backup, Immich còn là một lựa chọn thay thế xịn sò cho Google Photos, giúp bạn toàn quyền kiểm soát và giữ riêng tư cho dữ liệu của mình.

![Immich logo](/images/self-hosting-immich-google-photos-import.png)

## Mấy tính năng hay ho của Immich

Immich không chỉ là một công cụ backup đơn thuần, mà là cả một trình quản lý ảnh đầy đủ đồ chơi. Dưới đây là vài món nổi bật:

-   **Tự động sao lưu:** Có app trên điện thoại (Android và iOS) tự động backup ảnh và video ngay khi bạn vừa chụp xong.
-   **Hỗ trợ nhiều người dùng:** Tạo tài khoản riêng cho người thân, bạn bè, mỗi người một thư viện ảnh riêng tư, không lo đụng hàng.
-   **Album và Chia sẻ:** Gom ảnh vào album và chia sẻ cho người khác qua link công khai. Bạn còn có thể đặt mật khẩu và hẹn giờ cho link tự hủy.
-   **Thông tin ảnh và Bản đồ:** Immich đọc được dữ liệu EXIF của ảnh, từ thông số máy ảnh đến vị trí chụp. Bạn có thể xem lại hành trình của mình trên bản đồ thế giới tương tác.

### AI "nhà trồng": Phép thuật đích thực

Điểm ăn tiền của Immich chính là các tính năng AI mạnh mẽ, chạy ngay tại nhà bạn, giúp thư viện ảnh của bạn sống động y như Google Photos:

-   **Nhận diện khuôn mặt:** Immich tự động tìm và gom nhóm các khuôn mặt trong ảnh. Bạn chỉ cần đặt tên một lần, và bùm, sau này muốn tìm ảnh của ai đó chỉ cần một nốt nhạc.
-   **Nhận diện vật thể và cảnh:** Mấy em AI này nhận ra hàng nghìn thứ trong ảnh, từ "hoàng hôn", "bãi biển", đến "mấy con chó", "xe hơi". Nhờ vậy, bạn có thể tìm ảnh cái rẹt mà không cần phải ngồi tag thủ công.
-   **Tìm kiếm thông minh:** Kết hợp các từ khóa để tìm chính xác thứ bạn cần. Ví dụ, tìm "ảnh của Gấu ở bãi biển" để ra kết quả ngay và luôn.

![Các tính năng AI của Immich](/images/demo-immich-ai.png)

## Cài Immich như thế nào?

Thế giới self-host thay đổi nhanh như chong chóng, các bước cài đặt cũng có thể thay đổi theo. Hướng dẫn bên dưới là một khởi đầu ổn, nhưng tốt nhất bạn nên ghé qua trang tài liệu chính thức để có thông tin mới nhất nhé. Bạn có thể tìm thấy nó ở [trang chủ Immich](https://immich.app).

Cách ngon nhất để cài Immich là dùng Docker và Docker Compose.

### Cần chuẩn bị gì?

-   Một máy chủ đã cài sẵn Docker và Docker Compose.
-   Một user có quyền chạy lệnh Docker.

### Các bước cài đặt

1.  **Tạo thư mục cho Immich:**
    ```bash
    mkdir immich-app
    cd immich-app
    ```

2.  **Tải về file `docker-compose.yml` và `example.env`:**
    ```bash
    wget https://github.com/immich/immich/releases/latest/download/docker-compose.yml
    wget https://github.com/immich/immich/releases/latest/download/example.env
    ```

3.  **Đổi tên `example.env` thành `.env`:**
    ```bash
    mv example.env .env
    ```

4.  **Chỉnh sửa file `.env`:**
    Mở file `.env` lên và chỉnh lại các biến. Quan trọng nhất là `UPLOAD_LOCATION`, đây là đường dẫn đến nơi bạn sẽ cất giữ ảnh và video. Bạn cũng nên đặt một mật khẩu thật mạnh cho database nữa.

5.  **Khởi động Immich:**
    ```bash
    docker-compose up -d
    ```

    Lệnh này sẽ tải về các image cần thiết và cho dàn container Immich chạy ngầm.

6.  **Truy cập Immich:**
    Giờ thì bạn có thể vào Immich qua địa chỉ `http://<IP-máy-chủ-của-bạn>:2283`. Lần đầu vào, bạn sẽ được yêu cầu tạo tài khoản admin.

## Cách "lùa" ảnh từ Google Photos về?

Cách phổ biến nhất để "di cư" khỏi Google Photos là dùng Google Takeout.

1.  **Yêu cầu Google Takeout:**
    -   Vào trang [Google Takeout](https://takeout.google.com/).
    -   Bỏ chọn hết các sản phẩm, rồi chọn lại mỗi "Google Photos".
    -   Chọn định dạng và cách nhận file. Mình khuyên bạn nên chia nhỏ file export ra (ví dụ 50GB một cục) để dễ xử lý hơn.
    -   Ngồi chờ email từ Google gửi link tải về.

2.  **Nhập vào Immich:**
    Sau khi tải về và giải nén đống file từ Google Takeout, bạn có vài cách để nhập vào Immich:

    -   **Giao diện web:** Kéo thả file thẳng vào web Immich. Cách này hợp với số lượng file ít ít.
    -   **Immich CLI:** Với thư viện ảnh khổng lồ, dùng Immich CLI sẽ ổn định hơn. Bạn có thể chạy nó ngay trên máy chủ chứa file ảnh. Xem thêm thông tin về CLI trong tài liệu chính thức của Immich nhé.
    -   **[`immich-go`](https://github.com/simulot/immich-go):** Đây là một công cụ dòng lệnh bên thứ ba rất được ưa chuộng để nhập từ Google Takeout. Nó có mấy trò hay ho như tự tạo album dựa trên cấu trúc thư mục của Google Takeout.

## Cách "lùa" ảnh từ iCloud về?

Với anh em nhà Táo, bước đầu tiên là phải lấy được ảnh ra khỏi iCloud đã. Cách triệt để nhất là yêu cầu Apple cho tải về toàn bộ dữ liệu.

1.  **Yêu cầu dữ liệu từ Apple:**
    -   Vào cổng Dữ liệu và Quyền riêng tư của Apple ở [privacy.apple.com](https://privacy.apple.com/).
    -   Đăng nhập bằng Apple ID của bạn.
    -   Tìm mục "Get a copy of your data" (Nhận bản sao dữ liệu của bạn) rồi nhấn vào.
    -   Chọn "iCloud Photos" trong danh sách.
    -   Làm theo hướng dẫn để xác nhận. Apple sẽ chuẩn bị dữ liệu và báo cho bạn khi nào có thể tải về.

2.  **Nhập vào Immich:**
    Khi đã tải về và giải nén kho ảnh iCloud, bạn có thể dùng các cách nhập tương tự như với Google Photos:

    -   **Giao diện web:** Ngon cho số lượng ảnh nhỏ.
    -   **Immich CLI hoặc [`immich-go`](https://github.com/simulot/immich-go):** Cực kỳ khuyến khích cho thư viện lớn để đảm bảo quá trình nhập mượt mà và đáng tin cậy.

Bài này chỉ là lướt sơ qua về Immich thôi. Trong các bài sau, mình sẽ vọc vạch sâu hơn các tính năng của nó và khám phá nhiều chủ đề hay ho hơn.

## Đôi lời về việc ủng hộ Immich

Immich là một dự án mã nguồn mở và miễn phí, được xây dựng bởi một cộng đồng đầy nhiệt huyết. Bạn có thể thấy nút "Buy me a coffee" hay link donate đâu đó trong app. Cần phải hiểu rõ rằng đây chỉ là một cách để bạn bày tỏ sự cảm kích và ủng hộ các lập trình viên. Việc donate không mở khóa thêm tính năng nào cả; mọi thứ đều có sẵn cho tất cả mọi người ngay từ đầu. Nếu bạn thấy Immich hữu ích, hãy cân nhắc ủng hộ để dự án tiếp tục phát triển nhé.
