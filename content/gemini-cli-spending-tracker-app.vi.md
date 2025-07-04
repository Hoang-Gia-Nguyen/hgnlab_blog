---
title: "Mình đã sử dụng Gemini CLI để tạo ứng dụng theo dõi chi tiêu chỉ trong 2 tiếng"
date: 2025-07-04
description: "Một dự án cho vui, vừa giúp mình vọc Gemini CLI vừa hỗ trợ 1 số mục đích cá nhân"
taxonomies:
  categories:
    - "Technology"
    - "AI"
    - "Developer Tools"
  tags:
    - "Gemini CLI"
---

Disclaimer: đây chỉ là dự án cho vui, vừa giúp mình vọc Gemini CLI vừa hỗ trợ 1 số mục đích cá nhân

## Thành phần của ứng dụng
- Cloudflare pages + workers: host web interface và tạo các REST API tương tác với database.
- Cloudflare D1 Database: lưu trữ dữ liệu chi tiêu của mình trong 1 năm qua.

## Quá trình thực hiện
### BƯỚC 1: Chuẩn bị data
Đầu tiên là cần chuẩn bị dữ liệu. Trước đây chi tiêu hằng ngày mình ghi lại vào 1 file Google sheet, mỗi tháng mình tạo một file riêng.

Với ý định là sẽ đưa dữ liệu này lên Cloudflare D1 Database (cơ bản là 1 dịch vụ database SQLite free được cung cấp bởi Cloudflare), mình cần chuẩn bị một vài thứ.

Đầu tiên mình tải tất cả các file Google Sheet ghi chép chi tiêu của mình dưới dạng csv về bỏ vào 1 folder. Từ đây mình gọi Gemini CLI:

"Check all csv files in folder abc, propose me what should be do to clean data".

Mình đã rất ngạc nhiên khi Gemini CLI có thể liệt kê ra rất nhiều gợi ý để clean data như:
- Một số typo cơ bản trong description
- Các khoản chi tiêu có số tiền (cột Amount) không tương xứng so với thông tin trong cột Description (thường là do nhập liệu mình bị dư/thiếu số 0).
- Các dòng có Category bất thường, không thống nhất với đa số các dòng khác.


Sau khi đưa ra gợi ý như vậy, Gemini CLI sẽ hỏi mình nếu mình muốn nó trực tiếp thay đổi nội dung file, hoặc mình sẽ tự chỉnh sửa bằng tay, khúc này mình thấy ok nên đã đồng ý để Gemini CLI sửa data giúp mình.

Sau đó mình yêu cầu Gemini CLI 

```
"transform all this data to SQL command so that I can push data into SQL data base".
```

Sau vài giây, Gemini đã viết ra 1 file csv_to_sql.py và yêu cầu được thực thi file đó với lệnh ***"python3 csv_to_sql.py"***

Mình yes và nhận được output là một file SummarizeFinance2025.sql. File này đã được mình copy paste thẳng vào console của Cloudflare D1 Database, tèn tèn, database của mình đã có ~500 dòng dữ liệu chi tiêu.

Một điều thú vị là khi Gemini CLI chạy thử file python csv_to_sql lần đầu, đã có error. Tuy nghiên Gemini CLI đã tự suy nghĩ cách sửa, fix và chạy lại, và vẫn lỗi. Lúc này mình thấy từ terminal hiện lại "Google Search: python error ...". Sau khi search Gemini cung cấp cho mình vài thông tin nó lấy được, thử sửa lại lần nữa và lúc này script python đã chạy ok. Toàn bộ quá trình này là tự động, việc của mình chỉ là giám sát, review khi Gemini hỏi và quyết định accept giải pháp và Gemini đề nghị hay không.

### BƯỚC 2: Tạo web-interface và cái cloudflare worker để tương tác với D1 Database
Bước này thât ra mình không muốn đưa chi tiết lên đây vì cơ bản là mình nói chuyện như sai vặt thằng Gemini CLI (và mình cũng quên capture lại màn hình terminal lúc đó).

```
"I want a web interface that I can input date, amount, description, category. Date input is a datepicker which default is current date. Amount shows unit Vietnam dong. Description is free text. Category is a drop down selection. List of category: ..."
```

```
"When I click add new expense button, I need these data to be written into Cloudflare D1 Database. Write me cloudflare worker script to do show"
```

```
"I want a month picker, when I select month, fetch data from database and show list of transactions that month below. Update web interface and write me cloudlfare worker script to do so"
```

```
"I want a total summarize and category-based summarize. I also want a pie-chart to improve visualization."
```

Kết quả tạo ra từ Gemini CLI thật sự gây bất ngờ khi tất cả các file html, js, css và worker script gần như có thể run ngay lập tức.

Chỉ xuất hiện một số trường hợp có lỗi khi deploy, tuy nhiên mình có thể tự dò và sửa nhanh được. Nhưng mình nghĩ nếu mình không đủ kiến thức thì vẫn có thể ném lỗi này vào lại terminal của Gemini CLI nhờ nó sửa code.

![Giao diện thêm chi tiêu](/images/add_new_expense.png)
![Giao diện danh sách chi tiêu](/images/detail_expense_list.png)
![Giao diện thông tin chi tiêu theo danh mục](/images/category_budget_info.png)
![Giao diện đồ thị](/images/budget_graph.png)

## Cảm nhận
- Gemini CLI là một trợ lý, không đơn thuần là một chat bot. Cảm giác giống như bạn thuê được một trợ lý junior để thực thi công việc cho bạn (nghiên cứu tài liệu, viết code, sửa file, quét dọn, etc...), còn bạn tập trung vào định hướng, xây dựng requirements, đưa ra hướng dẫn cụ thể, review kết quả của trợ lý và đưa ra quyết định cuối cùng.
- Kết quả của Gemini CLI đúng ý bạn hay không rất phụ thuộc vào cách đưa ra hướng dẫn của bạn. Điều này cũng tương tự như khi bạn làm việc với một trợ lý con người nên không có gì để phàn nàn.
- Gemini CLI đọc hiểu rất tốt cả 1 folder code. Nó có thể phân tích được file nào nên đặt vào thư mục nào, nếu viết thêm 1 function mới thì nên viết vào đâu.
- Gemini CLI có thể dần dần hiểu được phong cách làm việc của bạn. Khi mình duy trì làm việc trên 1 terminal trong 1 thời gian dài, vài prompt đầu tiên có thể phải yêu cầu rất nhiều và review sửa đi sửa lại output của Gemini CLI. Tuy nhiên càng về sau mình cảm nhận rõ được là Gemini CLI đưa ra output đúng với mong muốn của mình hơn mà không cần phải nói quá nhiều với nó nữa. Hiện tại mình chỉ mới quan sát được hiện tượng này trong những session rất dài với Gemini CLI. Tuy nhiên mình thấy trong tài liệu cũng có những hướng dẫn để mình nạp thông tin cần thiết, giúp cho Gemini CLI luôn hoạt động đúng ý mình trong bất ký session nào.
- Bạn vẫn rất nên là người review và đưa ra quyết định cuối cùng. Gemini CLI mỗi khi muốn thực hiện 1 hành động gì đó như sửa file hay chạy 1 câu lệnh đều sẽ đưa ra cho bạn các lựa chọn "Allow once", "Always allow this action" và "No". Mình khuyến nghị luôn chọn Allow once để có thể review cho từng action của Gemini CLI, bởi sản phẩm cuối cùng và kết quả cuối cùng sẽ luôn là bạn chịu trách nhiệm về chất lượng.
- Bạn rất nên có kiến thức chuyên môn đủ mạnh về vấn đề mình đang hợp tác làm cùng Gemini CLI để có thể đưa ra hướng dẫn cũng như review kết quả một cách hiệu quả. Chỉ trừ trường hợp bạn đang chủ đích sử dụng Gemini CLI để học một thứ gì đó mới.

> Tổng kết lại, đây là một trải nghiệm làm việc rất WOW! với Gemini CLI.
