+++
title = "App chia sẻ text chỉ 10MB RAM: Chuyện mình tự chế công cụ copy-paste riêng"
date = 2026-02-04
description = "Đôi khi liều thuốc tốt nhất cho những cơn 'đau đầu' khi tự host hệ thống lại chính là một đoạn script đơn giản, một cái Docker container và duy nhất một file .txt."
authors = ["hgn"]
[taxonomies]
categories = ["Self-hosting"]
tags = ["self-hosting", "hobby"]

[extra]
cover.image = "images/clipboard-share-cover.png"
cover.alt = "Clipboard Share"

+++
# Mở đầu
Việc copy một đoạn văn bản từ máy tính này sang máy tính khác thì có gì khó đâu nhỉ? Ấy thế mà nếu bạn đang chơi hệ "self-hosting", câu trả lời thường là: "Khó không tưởng!". Mình đã từng kinh qua đủ các "ông lớn" như Ghostboard, Etherpad, hay PrivateBin để tìm một cách đồng bộ clipboard thật mượt mà. Nhưng rồi, hết lỗi WebSocket khó chịu, đến việc ngốn RAM vô tội vạ, rồi cấu hình bảo mật thì rắc rối quá mức cần thiết... mấy cái gọi là "giải pháp" đó cuối cùng lại làm mình tốn công hơn cả vấn đề ban đầu. Mình đâu có cần một bộ tính năng đồ sộ; mình chỉ cần một cách để chuyển chữ qua lại thôi. Thế là, mình quyết định tự tay "chế" luôn cho nhanh.

# The Need for a Simple Solution
Sau những lần "vật lộn" đó, mình muốn tìm một dịch vụ siêu nhẹ, dễ triển khai, có thể chia sẻ text giữa các máy tính mà không cần cấu hình phức tạp hay ngốn tài nguyên. Tiêu chuẩn của mình là:
- **Minimal Footprint**: Cứ ngốn quá 20MB RAM là mình coi như quá nặng.
- **Lưu trữ đơn giản**: Không cần mấy cái cơ sở dữ liệu phức tạp như PostgreSQL hay Redis làm gì cho mệt, vài dòng text thôi mà.
- **Truy cập mọi nơi**: Phải dùng được cả với curl trong terminal lẫn giao diện web đơn giản trên điện thoại. Và có thể truy cập với 1 url đơn giản duy nhất mà không cần path phức tạp.

Mình chợt nhận ra mình chẳng cần một trình soạn thảo cộng tác hoành tráng làm gì. Thứ mình cần chỉ là một cái "cầu nối" để mình có thể POST một đoạn code từ server và GET nó về laptop ngay tức thì.

# Triển khai và Cài đặt
Script PHP thì khá đơn giản
```php
<?php
$file = 'data.txt';

// Xử lý lưu dữ liệu (POST)
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $input = $_POST['text'] ?? '';
    if (file_put_contents($file, $input) !== false) {
        echo "OK";
    } else {
        header('HTTP/1.1 500 Internal Error');
        echo "Lỗi: Không có quyền ghi vào file data.txt";
    }
    exit;
}

// Đọc dữ liệu (GET)
$content = file_exists($file) ? htmlspecialchars(file_get_contents($file)) : '';
?>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Quick Note</title>
    <style>
        body { margin: 0; background: #eee; font-family: sans-serif; }
        #status { position: fixed; top: 5px; right: 5px; font-size: 12px; color: gray; }
        textarea { width: 100vw; height: 100vh; padding: 20px; border: none; outline: none; font-size: 18px; box-sizing: border-box; }
    </style>
</head>
<body>
    <div id="status">Sẵn sàng</div>
    <textarea id="note"><?php echo $content; ?></textarea>

    <script>
        const note = document.getElementById('note');
        const status = document.getElementById('status');
        let timer;

        // CHỈ LƯU - KHÔNG TỰ ĐỘNG LOAD VỀ ĐỂ TRÁNH XUNG ĐỘT KHI ĐANG TEST
        note.addEventListener('input', () => {
            status.innerText = "Typing...";
            clearTimeout(timer);
            timer = setTimeout(async () => {
                status.innerText = "Saving...";
                const fd = new FormData();
                fd.append('text', note.value);

                try {
                    const res = await fetch('', { method: 'POST', body: fd });
                    if (res.ok) {
                        status.innerText = "Saved at " + new Date().toLocaleTimeString();
                    } else {
                        status.innerText = "SERVER ERROR (Check write permissions)";
                    }
                } catch (e) {
                    status.innerText = "CONNECTION ERROR";
                }
            }, 800);
        });
    </script>
</body>
</html>
```
Để việc triển khai trở nên nhanh gọn lẹ, mình đã đóng gói đoạn script này vào một Docker image. Cách này giúp mình có thể chạy dịch vụ ở bất cứ máy nào hỗ trợ Docker mà không cần lăn tăn cài đặt môi trường. Về phần base image, mình chọn bản PHP server nhỏ gọn và đơn giản nhất có thể để tối ưu dung lượng.

```dockerfile
FROM php:8.1-apache-bullseye

WORKDIR /var/www/html

COPY ./app/index.php /var/www/html/index.php
COPY ./app/data.txt /var/www/html/data.txt

RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html

EXPOSE 80

CMD ["apache2-foreground"]
```

Khi chạy, bạn chỉ việc mount file .txt của riêng mình vào đường dẫn /var/www/html/data.txt là xong.

**Lưu ý về bảo mật**: Vì công cụ này có thể dùng để truyền các thông tin nhạy cảm như API key, mình luôn khóa nó cẩn thận sau lớp Cloudflare Access, đảm bảo chỉ các thiết bị của mình mới có thể truy cập được. Mình cũng thường xóa sạch dữ liệu nhạy cảm ngay sau khi dùng xong. Có lẽ sắp tới mình sẽ thêm tính năng tự động xóa ghi chú sau mỗi vài giờ cho yên tâm hẳn.

# Hiệu năng: "Sống khỏe" trên một con VM siêu hạt tiêu
Mình không muốn một cái app quản lý clipboard mà lại đòi hỏi tài nguyên như một con server thực thụ. Bằng cách lược bỏ hết những thứ rườm rà, mình đã giảm mức ngốn RAM xuống chỉ còn vỏn vẹn **10MB**.

Sự gọn nhẹ này giúp mình có thể "nhét" dịch vụ này vào một góc nhỏ trên con ***VM e2-micro của Google Cloud*** (vốn chỉ có 1GB RAM ít ỏi) mà vẫn chạy phăm phăm.

Bên cạnh việc tiết kiệm RAM, mình chọn hệ thống lưu trữ bằng file phẳng (flat-file). Thay vì phải quản lý những bộ máy cơ sở dữ liệu nặng nề như SQLite hay PostgreSQL, dịch vụ này chỉ đọc và ghi duy nhất vào một file .txt. Điều này có nghĩa là việc sao lưu chỉ đơn giản là copy một file duy nhất, chẳng phải lo cấu hình kết nối hay chuyển đổi dữ liệu (migration) phức tạp. Đúng chất "cắm là chạy" (plug-and-play) mà dân self-host tụi mình hằng mơ ước.

# Kết luận: Sức mạnh của sự "Vừa đủ"
Việc xây dựng công cụ này nhắc mình nhớ lại lý do tại sao mình lại dấn thân vào con đường self-hosting: đó là sự tự do để giải quyết các vấn đề của chính mình bằng những công cụ được may đo vừa khít với nhu cầu.

- **Niềm vui của "đồ tự chế"**: Có một sự thỏa mãn khó tả khi sử dụng một công cụ do chính tay mình viết ra. Nó không chỉ giải quyết vấn đề mà còn khớp hoàn hảo với thói quen làm việc của mình.
- **Biết mình biết ta**: Mình tự biết đây không phải là một giải pháp xịn sò dành cho doanh nghiệp. Nó thiếu những tính năng bóng bẩy và còn những điểm yếu về kiến trúc — nhưng trong một "phòng thí nghiệm" tại gia (home lab), đơn giản chính là một tính năng, không phải là lỗi.
- **Nền tảng để phát triển**: Đây mới chỉ là phiên bản 1.0. Những hạn chế hiện tại chính là lộ trình để mình học hỏi thêm sau này, dù đó là thêm mã hóa, cải thiện khả năng xử lý đồng thời hay trau chuốt lại giao diện.

Đôi khi, chúng ta tốn quá nhiều thời gian để cấu hình những công cụ phức tạp thay vì thực sự sử dụng chúng. Với việc tạo ra một dịch vụ 10MB chạy trên một file text, mình đã lấy lại được thời gian và tài nguyên hệ thống của mình. Nó có thể không hoàn hảo, nhưng nó giải quyết vấn đề của mình một cách hoàn hảo — và với mình hiện tại, thế là quá đủ.

# GitHub Repository
Bạn có thể tìm thấy toàn bộ code của dự án này (2 files) tại trang [GitHub page](https://github.com/Hoang-Gia-Nguyen/simple-clipboard-share).