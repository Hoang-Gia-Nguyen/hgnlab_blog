+++
title = "Chạy LLM Tại Gia: AI Riêng Tư Nằm Trong Tầm Tay Bạn"
date = 2025-08-02
draft = false
description = "Khám phá cách bạn có thể tự chạy các mô hình ngôn ngữ lớn (LLM) ngay tại nhà, tận hưởng sự riêng tư và kiểm soát hoàn toàn với các công cụ như Ollama và LM Studio."

[taxonomies]
categories = ["Technology", "Self-hosting", "AI"]
tags = ["local-llm", "homelab", "ai-rieng-tu", "ai-tu-host", "ollama", "lm-studio", "ai-tren-phan-cung-ca-nhan", "ai-offline"]

[extra]
cover.image = "images/local-llm-cover.png"
cover.alt = "Running Large Language Models (LLMs) Locally Cover"
+++

Thế giới Trí tuệ Nhân tạo đang phát triển cực nhanh, và các Mô hình Ngôn ngữ Lớn (LLMs) như GPT-3 cùng những người anh em mã nguồn mở của nó đang dẫn đầu cuộc cách mạng này. Từ trước đến nay, để tiếp cận và sử dụng những công cụ mạnh mẽ này, mình thường phải dựa vào các dịch vụ đám mây, gửi dữ liệu của mình đến các máy chủ từ xa. Tuy nhiên, mọi thứ đang thay đổi. Giờ đây, việc **chạy LLMs tại nhà** ngay trên phần cứng của riêng bạn ngày càng khả thi – và mang lại cảm giác tự chủ đáng kinh ngạc. Hãy cùng mình khám phá những khả năng thú vị của **AI cục bộ**, được hỗ trợ bởi chiếc Mac Mini M4 của mình nhé.

# Tại Sao Nên Chạy LLMs Trên Phần Cứng Của Riêng Bạn? Lợi ích của AI Riêng Tư

Bạn có thể tự hỏi, tại sao phải tốn công chạy những mô hình phức tạp này cục bộ khi đã có sẵn các ứng dụng từ các nhà cung cấp lớn? Với mình, câu trả lời nằm ở vài yếu tố chính sau, làm nổi bật lợi thế của **AI riêng tư**:

- **Quyền Riêng Tư**: Đây là điều quan trọng nhất. Khi bạn xử lý dữ liệu bằng **LLM cục bộ**, thông tin của bạn sẽ nằm nguyên trên máy. Dữ liệu nhạy cảm, các cuộc trò chuyện cá nhân, hay nội dung độc quyền sẽ không bao giờ rời khỏi mạng lưới an toàn của nhà bạn. Mức độ kiểm soát này mang lại sự an tâm đáng kể. [Sam Altman cảnh báo không có bảo mật pháp lý khi dùng ChatGPT làm nhà trị liệu](https://techcrunch.com/2025/07/25/sam-altman-warns-theres-no-legal-confidentiality-when-using-chatgpt-as-a-therapist/)
- **Tiết Kiệm Chi Phí (Dài Hạn)**: Mặc dù có thể có một khoản đầu tư ban đầu vào phần cứng, nhưng việc **chạy LLMs tại nhà** có thể tiết kiệm chi phí hơn về lâu dài, đặc biệt nếu bạn có ý định sử dụng thường xuyên. Bạn sẽ tránh được các khoản phí định kỳ liên quan đến việc call APIs, vốn có thể tăng lên rất nhanh.
- **Độ Trễ và Độ Tin Cậy**: **Xử lý cục bộ** loại bỏ sự phụ thuộc vào mạng. Phản hồi thường nhanh hơn và đáng tin cậy hơn vì bạn không phải chịu ảnh hưởng của các vấn đề kết nối internet hoặc độ trễ vốn có khi gửi yêu cầu đến các máy chủ ở xa.
- **Tùy Chỉnh và Kiểm Soát**: Chạy mô hình cục bộ giúp bạn kiểm soát tốt hơn mô hình cụ thể mà bạn sử dụng, các tham số của nó và cách nó được tích hợp vào quy trình làm việc của bạn. Bạn không bị giới hạn bởi các dịch vụ của một nhà cung cấp đám mây cụ thể.
- **Học Hỏi và Thử Nghiệm**: Đối với một người thích mày mò và tìm hiểu cách hoạt động bên trong của công nghệ như mình, việc **chạy LLMs cục bộ** mang lại trải nghiệm học hỏi vô giá. Nó cho phép khám phá sâu hơn và thử nghiệm với các mô hình và cấu hình khác nhau.

# Chọn Mô Hình Phù Hợp: LLMs Mã Nguồn Mở Dành Cho Máy Tính Của Bạn
Cộng đồng **LLM mã nguồn mở** đã hoạt động cực kỳ tích cực trong việc phát triển và phát hành các LLMs mạnh mẽ có thể chạy trên phần cứng phổ thông. Những điều bạn nên cân nhắc khi chọn một mô hình là:
- **Số lượng tham số**: Trong tên mô hình, bạn có thể thấy '1.5B', '7B', '14B', '70B',... Đây là số lượng tham số trong mô hình. Các mô hình ngôn ngữ lớn hơn với nhiều tham số hơn có xu hướng đạt hiệu suất tốt hơn, nhưng chúng đòi hỏi tài nguyên tính toán nhiều hơn đáng kể để chạy. Mô hình 8B (8 tỷ tham số) là một kích thước phổ biến, tạo ra sự cân bằng tuyệt vời giữa hiệu suất và mức tiêu thụ tài nguyên cho hầu hết các tác vụ hàng ngày của người dùng thông thường.
- **Lượng tử hóa (Quantization)**: Trong tên mô hình, đôi khi bạn có thể thấy '4bit', '8bit'. Đây là lượng tử hóa. Nói một cách đơn giản, lượng tử hóa làm giảm độ chính xác của các số được sử dụng để biểu diễn các tham số của mô hình (ví dụ: từ số 32-bit xuống 4-bit). Điều này làm giảm đáng kể kích thước tệp và dung lượng bộ nhớ của mô hình, cho phép nó chạy hiệu quả trên các thiết bị có tài nguyên hạn chế, như laptop và PC mini, với tổn thất hiệu suất tối thiểu.

![Parameter list example](/images/local-llm-parameterlist.png)

Đối với dự án này, mình đã chọn mô hình ***deepseek/deepseek-r1-0528-qwen3-8b***. Đây là một phiên bản tinh gọn của mô hình lớn hơn **DeepSeek-R1-0528**. Nó nổi tiếng với khả năng suy luận và suy diễn mạnh mẽ, khiến nó trở thành lựa chọn tuyệt vời cho nhiều tác vụ mà không đòi hỏi tài nguyên khổng lồ như phiên bản đầy đủ. Vì mình đang dùng máy Mac, mình sẽ chọn định dạng mô hình MLX từ **LM Studio** để tận dụng hiệu suất của Apple Silicon.

# Yêu Cầu Phần Cứng: Máy Tính Cá Nhân Của Bạn Đã Sẵn Sàng Cho AI Chưa?

Thực tế là rất nhiều phần cứng phổ thông hiện nay, bao gồm cả laptop hiện đại và máy tính để bàn nhỏ gọn, hoàn toàn có khả năng **chạy LLM tại nhà**. Các mô hình này, thường trong khoảng 7 tỷ tham số, đủ mạnh để thực hiện các tác vụ như tóm tắt tài liệu, brainstorm và viết các đoạn mã. Chúng mang lại sự cân bằng tuyệt vời giữa hiệu suất và khả năng tiếp cận, và nhờ các kỹ thuật tối ưu hóa như lượng tử hóa, chúng có thể chạy trên phần cứng bình thường hiệu quả đến ngạc nhiên. Điều này giúp **AI trên phần cứng cá nhân** trở nên dễ tiếp cận hơn rất nhiều.

Mình đang viết bài này trên chiếc Mac Mini M4, phiên bản cơ bản với 16GB bộ nhớ hợp nhất, và đây là một ví dụ hoàn hảo. Mặc dù bộ nhớ hợp nhất trên Apple Silicon khác với VRAM chuyên dụng trên GPU truyền thống, nhưng nó phục vụ cùng mục đích để chạy các mô hình cục bộ. Chiếc Mac Mini của mình có thể xử lý thoải mái một mô hình 8B tham số. Mình có thể nhận được sự hỗ trợ **AI cục bộ** nhanh chóng cho quy trình làm việc hàng ngày mà không cần đầu tư vào một máy chủ chuyên dụng, cao cấp.

# Bắt Đầu Với Ollama và LM Studio cho AI Tự Host

Đối với hầu hết người dùng, việc bắt đầu với **LLMs cục bộ** đã trở nên cực kỳ đơn giản nhờ các công cụ như **Ollama** và **LM Studio**.

[***Ollama***](https://ollama.com/): Đây là một dự án mã nguồn mở tuyệt vời giúp việc tải xuống, chạy và quản lý **LLMs trên macOS** (và Linux) trở nên cực kỳ dễ dàng. Chỉ với vài lệnh đơn giản trong terminal, bạn có thể tải xuống các mô hình được xây dựng sẵn và bắt đầu tương tác với chúng. Đối với mô hình mình đã chọn, một lệnh như `ollama run deepseek-r1:8b` sẽ tải xuống và chạy mô hình.

[***LM Studio***](https://lmstudio.ai/): Ứng dụng này (chỉ dành cho MAC) cung cấp giao diện đồ họa thân thiện với người dùng để khám phá, tải xuống và chạy **LLMs**. Nó đặc biệt hữu ích cho những ai thích cách tiếp cận trực quan hơn là dòng lệnh. **LM Studio** cũng cung cấp các tính năng như giao diện trò chuyện tích hợp và khả năng tạo máy chủ cục bộ. **LM Studio** còn cung cấp nhiều mô hình khác nhau được tối ưu hóa cho Apple Silicon.

Mình thấy cả **Ollama** và **LM Studio** đều là những lựa chọn tuyệt vời. Sự đơn giản của **Ollama** rất tốt cho các thử nghiệm nhanh, trong khi **LM Studio** cung cấp một môi trường toàn diện hơn để khám phá các mô hình khác nhau và khả năng của chúng. Việc có native support cho Apple Silicon trong cả hai công cụ này đảm bảo hiệu suất tối ưu trên chip M4 của mình.

# Ví Dụ AI Cục Bộ: Tóm Tắt Danh Sách Giao Dịch Và Đưa Ra Thông Tin Chi Tiết Với AI Riêng Tư Của Bạn

Để đưa thiết lập **LLM cục bộ** của mình vào thực tế, mình đã quyết định thực hiện một dự án đơn giản nhưng có khả năng mang lại nhiều thông tin: tóm tắt danh sách giao dịch cá nhân của mình. Mình đã xuất một tệp CSV các giao dịch ngân hàng gần đây (mà không cần ẩn đi bất kỳ thông tin nào cả) và muốn xem liệu mô hình DeepSeek có thể xác định các mẫu chi tiêu và cung cấp một bản tóm tắt cấp cao hay không.

Sử dụng **LM Studio**, mình đã tải mô hình `deepseek/deepseek-r1-8b` và tạo một prompt bao gồm dữ liệu giao dịch của mình cùng với các hướng dẫn rõ ràng:

"Hãy phân tích danh sách giao dịch sau đây và cung cấp bản tóm tắt về thói quen chi tiêu của tôi. Xác định các danh mục chi phí lớn nhất, bất kỳ xu hướng đáng chú ý nào và những thông tin chi tiết chung về việc tiền của tôi đang đi đâu."

![Prompt example](/images/local-llm-example.png)

Kết quả thật đáng ngạc nhiên. Nhưng để bảo vệ quyền riêng tư của mình, mình sẽ chỉ chia sẻ phần cuối của phản hồi:

```
**General Insights:**

*   Your spending is concentrated in everyday needs like food and transportation, alongside discretionary categories (Entertainment) and utility/baby expenses.
*   There's a clear preference for takeaway/delivered food items rather than cooking at home, based on transaction descriptions.
*   Transportation costs are significant but consist mainly of occasional Grab/Taxi use rather than daily commute expenses.
*   Baby-related purchases are frequent and appear to be a significant ongoing expense.
```

# Không chỉ có Chat UI: API Để Tự Động Hóa Với AI Tự Host Của Bạn
Một lợi thế quan trọng của việc sử dụng các ứng dụng như **Ollama** và **LM Studio** là khả năng hoạt động như các máy chủ cục bộ cho các **mô hình AI** của bạn. Điều này có nghĩa là bạn không bị giới hạn bởi giao diện người dùng của chúng để tương tác. Cả hai công cụ đều cung cấp một API cục bộ, mà bạn có thể truy cập từ mã của riêng mình.

Ví dụ, chúng cung cấp các thư viện Python cho phép bạn gửi các prompt đến **LLMs cục bộ** của mình và nhận phản hồi một cách có lập trình. Điều này mở ra một thế giới khả năng tự động hóa. Bạn có thể xây dựng một script Python để tóm tắt tệp nhật ký hàng ngày, tự động tạo bản nháp email dựa trên ghi chú cuộc họp hoặc thậm chí tạo một chatbot tùy chỉnh tích hợp với các công cụ hiện có của bạn. Bằng cách tận dụng các API này, bạn đang biến thiết lập **AI tự host** của mình thành một công cụ mạnh mẽ để cải thiện năng suất cá nhân và tự động hóa các tác vụ.

# Tiếp Tục Hành Trình: Khám Phá AI Trên Phần Cứng Cá Nhân

**Chạy LLMs cục bộ** chỉ là khởi đầu cho hành trình khám phá thế giới **AI tự host** của mình. Khi các mô hình trở nên hiệu quả hơn và việc tích hợp công cụ tiếp tục được cải thiện, mình rất hào hứng khám phá những cách mới để tận dụng công nghệ này cho năng suất cá nhân, tự động hóa và học hỏi, tất cả trong khi vẫn duy trì quyền kiểm soát dữ liệu của mình. Hãy theo dõi để biết thêm các cập nhật về những cuộc phiêu lưu **tự host** và **AI trên phần cứng cá nhân** của mình nhé!

# Tại Sao Nên Chạy LLMs Trên Phần Cứng Của Riêng Bạn?

Bạn có thể tự hỏi, tại sao phải tốn công chạy những mô hình phức tạp này cục bộ khi đã có sẵn các ứng dụng từ các nhà cung cấp lớn? Với mình, câu trả lời nằm ở vài yếu tố chính sau:

- **Quyền Riêng Tư**: Đây là điều quan trọng nhất. Khi bạn xử lý dữ liệu bằng LLM cục bộ, thông tin của bạn sẽ nằm nguyên trên máy. Dữ liệu nhạy cảm, các cuộc trò chuyện cá nhân, hay nội dung độc quyền sẽ không bao giờ rời khỏi mạng lưới an toàn của nhà bạn. Mức độ kiểm soát này mang lại sự an tâm đáng kể. [Sam Altman cảnh báo không có bảo mật pháp lý khi dùng ChatGPT làm nhà trị liệu](https://techcrunch.com/2025/07/25/sam-altman-warns-theres-no-legal-confidentiality-when-using-chatgpt-as-a-therapist/)
- **Tiết Kiệm Chi Phí (Dài Hạn)**: Mặc dù có thể có một khoản đầu tư ban đầu vào phần cứng, nhưng việc chạy LLMs cục bộ có thể tiết kiệm chi phí hơn về lâu dài, đặc biệt nếu bạn có ý định sử dụng thường xuyên. Bạn sẽ tránh được các khoản phí định kỳ liên quan đến việc call APIs, vốn có thể tăng lên rất nhanh.
- **Độ Trễ và Độ Tin Cậy**: Xử lý cục bộ loại bỏ sự phụ thuộc vào mạng. Phản hồi thường nhanh hơn và đáng tin cậy hơn vì bạn không phải chịu ảnh hưởng của các vấn đề kết nối internet hoặc độ trễ vốn có khi gửi yêu cầu đến các máy chủ ở xa.
- **Tùy Chỉnh và Kiểm Soát**: Chạy mô hình cục bộ giúp bạn kiểm soát tốt hơn mô hình cụ thể mà bạn sử dụng, các tham số của nó và cách nó được tích hợp vào quy trình làm việc của bạn. Bạn không bị giới hạn bởi các dịch vụ của một nhà cung cấp đám mây cụ thể.
- **Học Hỏi và Thử Nghiệm**: Đối với một người thích mày mò và tìm hiểu cách hoạt động bên trong của công nghệ như mình, việc chạy LLMs cục bộ mang lại trải nghiệm học hỏi vô giá. Nó cho phép khám phá sâu hơn và thử nghiệm với các mô hình và cấu hình khác nhau.

# Chọn Mô Hình Phù Hợp: LLMs Mã Nguồn Mở Dành Cho Máy Tính Của Bạn
Cộng đồng mã nguồn mở đã hoạt động cực kỳ tích cực trong việc phát triển và phát hành các LLMs mạnh mẽ có thể chạy trên phần cứng phổ thông. Những điều bạn nên cân nhắc khi chọn một mô hình là:
- **Số lượng tham số**: Trong tên mô hình, bạn có thể thấy '1.5B', '7B', '14B', '70B',... Đây là số lượng tham số trong mô hình. Các mô hình ngôn ngữ lớn hơn với nhiều tham số hơn có xu hướng đạt hiệu suất tốt hơn, nhưng chúng đòi hỏi tài nguyên tính toán nhiều hơn đáng kể để chạy. Mô hình 8B (8 tỷ tham số) là một kích thước phổ biến, tạo ra sự cân bằng tuyệt vời giữa hiệu suất và mức tiêu thụ tài nguyên cho hầu hết các tác vụ hàng ngày của người dùng thông thường.
- **Lượng tử hóa (Quantization)**: Trong tên mô hình, đôi khi bạn có thể thấy '4bit', '8bit'. Đây là lượng tử hóa. Nói một cách đơn giản, lượng tử hóa làm giảm độ chính xác của các số được sử dụng để biểu diễn các tham số của mô hình (ví dụ: từ số 32-bit xuống 4-bit). Điều này làm giảm đáng kể kích thước tệp và dung lượng bộ nhớ của mô hình, cho phép nó chạy hiệu quả trên các thiết bị có tài nguyên hạn chế, như laptop và PC mini, với tổn thất hiệu suất tối thiểu.

![Parameter list example](/images/local-llm-parameterlist.png)

Đối với dự án này, mình đã chọn mô hình ***deepseek/deepseek-r1-0528-qwen3-8b***. Đây là một phiên bản tinh gọn của mô hình lớn hơn **DeepSeek-R1-0528**. Nó nổi tiếng với khả năng suy luận và suy diễn mạnh mẽ, khiến nó trở thành lựa chọn tuyệt vời cho nhiều tác vụ mà không đòi hỏi tài nguyên khổng lồ như phiên bản đầy đủ. Vì mình đang dùng máy Mac, mình sẽ chọn định dạng mô hình MLX từ LM Studio để tận dụng hiệu suất của Apple Silicon.

# Yêu Cầu Phần Cứng: Máy Chủ Của Bạn Đã Sẵn Sàng Cho AI Chưa?

Thực tế là rất nhiều phần cứng phổ thông hiện nay, bao gồm cả laptop hiện đại và máy tính để bàn nhỏ gọn, hoàn toàn có khả năng chạy một LLM "nhỏ". Các mô hình này, thường trong khoảng 7 tỷ tham số, đủ mạnh để thực hiện các tác vụ như tóm tắt tài liệu, brainstorm và viết các đoạn mã. Chúng mang lại sự cân bằng tuyệt vời giữa hiệu suất và khả năng tiếp cận, và nhờ các kỹ thuật tối ưu hóa như lượng tử hóa, chúng có thể chạy trên phần cứng bình thường hiệu quả đến ngạc nhiên.

Mình đang viết bài này trên chiếc Mac Mini M4, phiên bản cơ bản với 16GB bộ nhớ hợp nhất, và đây là một ví dụ hoàn hảo. Mặc dù bộ nhớ hợp nhất trên Apple Silicon khác với VRAM chuyên dụng trên GPU truyền thống, nhưng nó phục vụ cùng mục đích để chạy các mô hình cục bộ. Chiếc Mac Mini của mình có thể xử lý thoải mái một mô hình 8B tham số. Mình có thể nhận được sự hỗ trợ local AI nhanh chóng cho quy trình làm việc hàng ngày mà không cần đầu tư vào một máy chủ chuyên dụng, cao cấp.

# Bắt Đầu Với Ollama và LM Studio

Đối với hầu hết người dùng, việc bắt đầu với local LLMs đã trở nên cực kỳ đơn giản nhờ các công cụ như Ollama và LM Studio.

[***Ollama***](https://ollama.com/): Đây là một dự án mã nguồn mở tuyệt vời giúp việc tải xuống, chạy và quản lý LLMs trên macOS (và Linux) trở nên cực kỳ dễ dàng. Chỉ với vài lệnh đơn giản trong terminal, bạn có thể tải xuống các mô hình được xây dựng sẵn và bắt đầu tương tác với chúng. Đối với mô hình mình đã chọn, một lệnh như `ollama run deepseek-r1:8b` sẽ tải xuống và chạy mô hình.

[***LM Studio***](https://lmstudio.ai/): Ứng dụng này (chỉ dành cho MAC) cung cấp giao diện đồ họa thân thiện với người dùng để khám phá, tải xuống và chạy LLMs. Nó đặc biệt hữu ích cho những ai thích cách tiếp cận trực quan hơn là dòng lệnh. LM Studio cũng cung cấp các tính năng như giao diện trò chuyện tích hợp và khả năng tạo máy chủ cục bộ. LM Studio còn cung cấp nhiều mô hình khác nhau được tối ưu hóa cho Apple Silicon.

Mình thấy cả Ollama và LM Studio đều là những lựa chọn tuyệt vời. Sự đơn giản của Ollama rất tốt cho các thử nghiệm nhanh, trong khi LM Studio cung cấp một môi trường toàn diện hơn để khám phá các mô hình khác nhau và khả năng của chúng. Việc có native support cho Apple Silicon trong cả hai công cụ này đảm bảo hiệu suất tối ưu trên chip M4 của mình.

# Ví Dụ Local AI: Tóm Tắt Danh Sách Giao Dịch Và Đưa Ra Thông Tin Chi Tiết

Để đưa thiết lập LLM cục bộ của mình vào thực tế, mình đã quyết định thực hiện một dự án đơn giản nhưng có khả năng mang lại nhiều thông tin: tóm tắt danh sách giao dịch cá nhân của mình. Mình đã xuất một tệp CSV các giao dịch ngân hàng gần đây (mà không cần ẩn đi bất kỳ thông tin nào cả) và muốn xem liệu mô hình DeepSeek có thể xác định các mẫu chi tiêu và cung cấp một bản tóm tắt cấp cao hay không.

Sử dụng LM Studio, mình đã tải mô hình `deepseek/deepseek-r1-8b` và tạo một prompt bao gồm dữ liệu giao dịch của mình cùng với các hướng dẫn rõ ràng:

"Hãy phân tích danh sách giao dịch sau đây và cung cấp bản tóm tắt về thói quen chi tiêu của tôi. Xác định các danh mục chi phí lớn nhất, bất kỳ xu hướng đáng chú ý nào và những thông tin chi tiết chung về việc tiền của tôi đang đi đâu."

![Prompt example](/images/local-llm-example.png)

Kết quả thật đáng ngạc nhiên. Nhưng để bảo vệ quyền riêng tư của mình, mình sẽ chỉ chia sẻ phần cuối của phản hồi:

```
**General Insights:**

*   Your spending is concentrated in everyday needs like food and transportation, alongside discretionary categories (Entertainment) and utility/baby expenses.
*   There's a clear preference for takeaway/delivered food items rather than cooking at home, based on transaction descriptions.
*   Transportation costs are significant but consist mainly of occasional Grab/Taxi use rather than daily commute expenses.
*   Baby-related purchases are frequent and appear to be a significant ongoing expense.
```

# Không chỉ có Chat UI: API Để Tự Động Hóa
Một lợi thế quan trọng của việc sử dụng các ứng dụng như Ollama và LM Studio là khả năng hoạt động như các máy chủ cục bộ cho các mô hình AI của bạn. Điều này có nghĩa là bạn không bị giới hạn bởi giao diện người dùng của chúng để tương tác. Cả hai công cụ đều cung cấp một API cục bộ, mà bạn có thể truy cập từ mã của riêng mình.

Ví dụ, chúng cung cấp các thư viện Python cho phép bạn gửi các prompt đến LLMs cục bộ của mình và nhận phản hồi một cách có lập trình. Điều này mở ra một thế giới khả năng tự động hóa. Bạn có thể xây dựng một script Python để tóm tắt tệp nhật ký hàng ngày, tự động tạo bản nháp email dựa trên ghi chú cuộc họp hoặc thậm chí tạo một chatbot tùy chỉnh tích hợp với các công cụ hiện có của bạn. Bằng cách tận dụng các API này, bạn đang biến thiết lập AI cục bộ của mình thành một công cụ mạnh mẽ để cải thiện năng suất cá nhân và tự động hóa các tác vụ.

# Tiếp Tục Hành Trình

Chạy LLMs cục bộ chỉ là khởi đầu cho hành trình khám phá thế giới AI tự host của mình. Khi các mô hình trở nên hiệu quả hơn và việc tích hợp công cụ tiếp tục được cải thiện, mình rất hào hứng khám phá những cách mới để tận dụng công nghệ này cho năng suất cá nhân, tự động hóa và học hỏi, tất cả trong khi vẫn duy trì quyền kiểm soát dữ liệu của mình. Hãy theo dõi để biết thêm các cập nhật về những cuộc phiêu lưu tự host và AI của mình nhé!