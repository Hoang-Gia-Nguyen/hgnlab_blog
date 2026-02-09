+++
title = "Hậu free tier: cách một hobbyist tiếp tục sống chung với LLM"
date = 2026-02-09
draft = false
description = "Sau khi bị siết RPM/TPM, mình buộc phải nhìn lại toàn bộ cách mình dùng LLM và thiết kế lại một kiến trúc vừa rẻ, vừa bền cho nhu cầu cá nhân."
[taxonomies]
categories = ["Technology"]
[extra]
cover.image = "images/llm-usage-shift-cover.png"
cover.alt = ""
+++

# Từ “xài chùa” sang “tối ưu chi phí có chủ đích”
Sau bài viết trước về việc [các LLM provider siết RPM/TPM và cách nó làm workflow của mình gãy vụn ra sao](@/ai-rate-limits-shock.vi.md), mình nhận ra một điều khá rõ ràng: vấn đề không nằm ở việc free tier ngày càng tệ, mà nằm ở chỗ mình đã quen với việc sử dụng LLM như một tài nguyên “vô hạn”. Khi còn miễn phí, mình không cần nghĩ nhiều đến tần suất gọi API, số token bị đốt, hay việc một đoạn context bị nhét lại bao nhiêu lần trong quá trình làm việc. Mọi thứ “chạy được” là đủ.

Nhưng khi free tier không còn đủ dùng, phản xạ đầu tiên của mình không phải là tìm cách né giới hạn hay săn thêm tài khoản, mà là dừng lại và hỏi một câu khác: mình thực sự cần LLM để làm gì, và phần nào trong workflow của mình xứng đáng để trả tiền? Từ câu hỏi đó, cách mình tiếp cận LLM bắt đầu thay đổi. mình không còn nhìn LLM như một thứ phải luôn miễn phí, cũng không mặc định rằng cứ trả tiền theo token là hợp lý. Thay vào đó, mình bắt đầu tách các nhu cầu sử dụng ra, đo lại chi phí thực sự, và chọn mô hình phù hợp cho từng loại công việc.

Bài viết này là câu chuyện về quá trình chuyển đổi đó: từ việc “xài chùa khi còn được” sang một cách dùng LLM có chủ đích hơn về chi phí. Không phải để tối ưu đến mức cực đoan, mà để giữ cho các automation cá nhân và workflow coding của mình tiếp tục vận hành mượt mà, bền vững, và không biến mỗi lần làm việc với LLM thành một lần lo lắng về token.

# Nhánh 1: Pay-on-demand cho automation nhỏ, token thấp

## Vì sao mình chấp nhận trả tiền (nhưng không phải trả tiền bừa)
Sau khi free tier không còn đủ dùng, phản xạ đầu tiên của mình không phải là tìm cách lách giới hạn hay săn thêm tài khoản, mà là tự hỏi lại: mình đang dùng LLM để làm gì hằng ngày? Khi nhìn kỹ hơn vào các automation cá nhân, mình nhận ra một điều khá đơn giản: phần lớn chúng không cần LLM “miễn phí”, mà cần LLM ổn định.

Automation khác với việc chat hay hỏi vặt. Nó chạy đều đặn, có nhịp, và nếu bị nghẽn vì quota hay RPM thì cả workflow phía sau cũng đứng lại. Ở thời điểm đó, việc trả tiền không còn mang cảm giác “mất đi lợi thế free”, mà giống như mua một mảnh hạ tầng nhỏ để mọi thứ tiếp tục vận hành trơn tru. Quan trọng hơn, mình không chấp nhận trả tiền cho mọi thứ — mà chỉ trả tiền cho những chỗ thực sự cần LLM can thiệp.

## Không phải automation nào cũng cần LLM “xịn”
Trước đây mình từng có một suy nghĩ khá phổ biến: đã gọi LLM thì phải gọi model mạnh nhất, thông minh nhất, cho “đáng tiền”. Nhưng khi áp tư duy đó vào automation, mọi thứ nhanh chóng trở nên lãng phí.

Phần lớn automation của mình đã được xử lý bằng code thuần:
- lọc dữ liệu,
- gom nội dung,
- áp rule,
- chuẩn hoá input.

LLM chỉ xuất hiện ở đoạn cuối, khi cần xử lý ngôn ngữ hoặc đưa ra một quyết định mang tính “con người” hơn: tóm tắt, phân loại, gợi ý hành động. Với những bài toán như vậy, mình không cần LLM phải suy luận phức tạp hay sáng tạo cao. Thứ mình cần là một model đủ tốt, phản hồi ổn định, và chi phí dễ kiểm soát.

Ở thời điểm này, mình đang dùng pay-on-demand với Groq, đồng thời trải nghiệm song song Gemini 2.0 Flash vì mức giá khá tốt. Groq nổi tiếng với throughput rất cao, và điều đó đúng là ấn tượng. Nhưng với nhu cầu cá nhân của mình, throughput cao lại không phải yếu tố quyết định.

Automation của mình không phải nhất thiết là hệ thống realtime, cũng không phải service phục vụ hàng nghìn request mỗi phút. Nó chạy rải rác trong ngày, mỗi lần gọi LLM là một nhiệm vụ nhỏ, token thấp. Vì vậy, thay vì tối ưu tốc độ tuyệt đối, mình quan tâm nhiều hơn đến:

- giá trên mỗi request/token.
- và mức độ “đủ thông minh” cho những quyết định đơn giản.

Nói cách khác, throughput cao với mình cũng chỉ là nice to have, chứ không phải lý do chính để chọn provider.

## Ví dụ cụ thể: app mail analysis
Một ví dụ rõ nhất cho cách mình dùng pay-on-demand LLM là ứng dụng sync Gmail cá nhân. Phần lớn công việc ở đây hoàn toàn không cần LLM:

- sync mail,
- lọc theo sender, label, thời gian,
- trích nội dung chính,
- loại bỏ chữ ký, quảng cáo, noise.

Tất cả đều được xử lý bằng logic code. LLM chỉ được gọi ở bước cuối, khi mình cần nó đọc phần nội dung đã được rút gọn và trả lời vài câu hỏi rất cụ thể: mail này có quan trọng không, có cần phản hồi sớm không, và nếu có thì nên làm gì tiếp theo.

Vì input đã được kiểm soát chặt, lượng token sử dụng rất thấp và ổn định. LLM ở đây không phải là trung tâm của hệ thống, mà chỉ là một lớp “decision layer” mỏng. Chính cách sử dụng này khiến pay-on-demand trở nên hợp lý: mình trả tiền cho đúng phần giá trị mà LLM mang lại, không hơn.

## LLM như một “hàm thông minh”, không phải trung tâm hệ thống
Sau một thời gian vọc vạch, mình nhận ra vấn đề không nằm ở việc dùng cloud LLM hay local LLM, mà nằm ở cách đặt vai trò cho LLM trong kiến trúc. Khi coi LLM là trung tâm, mọi thứ đều bị đẩy vào context, token tăng nhanh và chi phí mất kiểm soát. Nhưng khi coi LLM như một hàm thông minh — input rõ ràng, output giới hạn — chi phí trở nên dự đoán được và hợp lý hơn rất nhiều.

Cách tiếp cận này hoạt động rất tốt với automation nhỏ và token thấp. Tuy nhiên, nó bắt đầu bộc lộ giới hạn khi mình đem cùng tư duy đó áp vào coding agent và những codebase lớn hơn. Ở đó, token không còn nhỏ, context không còn gọn, và pay-per-token nhanh chóng trở thành một bài toán khác hoàn toàn.

# Nhánh 2: Thuê GPU theo phiên để chạy “local LLM”

## Khi pay-per-token bắt đầu phản tác dụng
Cách mình dùng pay-on-demand LLM cho automation nhỏ hoạt động rất ổn, nhưng mọi thứ bắt đầu lệch pha khi mình đem cùng tư duy đó áp vào coding agent. Ở đây, LLM không còn chỉ đọc một đoạn input ngắn rồi trả lời, mà phải liên tục tương tác với code: mở file, hiểu cấu trúc, sửa một chỗ, rồi lại đọc lại những phần liên quan để đảm bảo không phá vỡ thứ khác.

Vấn đề là mỗi vòng lặp như vậy đều tiêu thụ token mới. Không phải vì mình hỏi những câu ngày càng khó, mà vì context phải được gửi lại từ đầu, hết lần này đến lần khác. Khi làm việc với code, đặc biệt là trong những phiên debug hoặc refactor dài, token không còn đại diện cho giá trị mới được tạo ra, mà chỉ là chi phí cho việc nhắc lại những thứ LLM đã “biết” ở vòng trước. Mặc dù hiện nay đã có nhiều công cụ context management cho codebase hay caching service của các LLM provider, lượng token tiêu hao trong use case này vẫn cực kỳ kinh khủng.

Ở thời điểm đó, mình bắt đầu thấy pay-per-token không còn phù hợp với kiểu làm việc mở, lặp, và không thể dự đoán trước như coding nữa.

### Use case 1: Làm việc với workspace lớn, workspace lạ
Use case đầu tiên khiến vấn đề này lộ rõ là khi mình làm việc với những workspace tương đối lớn. Không nhất thiết phải là monorepo khổng lồ, chỉ cần một dự án có:

- nhiều thư mục,
- một vài custom lib,
- cấu trúc không hoàn toàn phẳng,

Là coding agent đã phải “đọc” khá nhiều thứ để hiểu bối cảnh.

Mỗi lần mình yêu cầu sửa một phần logic, LLM lại cần:
- load lại các file liên quan,
- đối chiếu với cấu trúc tổng thể,
- đảm bảo thay đổi không ảnh hưởng tới chỗ khác.

Và nếu kết quả chưa đúng, vòng lặp đó lại lặp lại. Điều này khiến token bị đốt rất nhanh, dù mình không hề cảm thấy đang làm việc gì quá phức tạp. Cảm giác khá khó chịu, vì mình không hỏi nhiều hơn, chỉ là dự án không cho phép làm việc trong những context ngắn gọn.

Lúc này mình nhận ra: với workspace lớn, chi phí token tăng không tỷ lệ với độ khó của vấn đề, mà tỷ lệ với mức độ phân mảnh của context.

### Use case 2: Playground để học
Use case thứ hai thậm chí còn “đốt token” khủng khiếp hơn, và mình nghĩ rất nhiều người gặp phải: dùng LLM như một playground để học framework mới từ đầu. Khi mình chưa hiểu gì về một framework, câu hỏi ban đầu thường rất mơ hồ, và các giả định mình đưa ra cũng thường sai.

Quá trình học lúc này không phải là hỏi một câu rồi nhận một câu trả lời chuẩn, mà là:

- hỏi → thử → sai,
- hỏi lại → sửa → lại sai,
- và tiếp tục hỏi “tại sao” với một đống context chồng chéo.

Mỗi vòng như vậy, context không chỉ dài ra, mà còn chứa rất nhiều thông tin tạm thời, thử nghiệm, thậm chí là hiểu nhầm. LLM phải mang theo toàn bộ lịch sử đó để tiếp tục cuộc hội thoại, và token cứ thế tăng lên theo cấp số cộng. Điều trớ trêu là đây lại là lúc mình cần LLM nhất, vì mình thực sự chưa biết mình đang không biết cái gì.

Sau vài lần như vậy, mình bắt đầu thấy rõ: pay-per-token đặc biệt không thân thiện với quá trình học mang tính khám phá, nơi sai lầm và lặp lại là điều bình thường.

## Mô hình tính tiền per token không phù hợp với những use case trên
Sau hai use case trên, mình bắt đầu dừng lại và nhìn vấn đề ở một góc khác. Trước đó, phản xạ quen thuộc của mình là tự hỏi: model này có đủ thông minh không? hay provider này có tối ưu token tốt không? Nhưng càng làm việc nhiều với coding agent và playground học tập, mình càng thấy những câu hỏi đó không còn trúng trọng tâm.

Vấn đề không phải là LLM trả lời dở, cũng không phải vì mình dùng sai prompt. Vấn đề nằm ở chỗ mô hình trả tiền theo token giả định rằng mỗi token mới đều mang lại giá trị mới. Giả định này đúng với những tương tác ngắn, rõ ràng, nhưng nó bắt đầu sụp đổ khi áp vào những phiên làm việc dài, lặp, và mang tính khám phá.

Trong cả hai trường hợp — workspace lớn và học framework mới — phần lớn token bị tiêu thụ không phải để giải quyết vấn đề mới, mà để duy trì bối cảnh: nhắc lại cấu trúc thư mục, nhắc lại đoạn code đã đọc, nhắc lại những giả định trước đó, kể cả những giả định sai. Token lúc này không còn là chi phí cho “tư duy”, mà là chi phí cho trí nhớ tạm thời.

Khi nhận ra điều đó, mình hiểu rằng đây không còn là bài toán chọn model hay tối ưu prompt nữa. Thứ mình cần là một mô hình chi phí khác, phù hợp hơn với kiểu làm việc không thể đoán trước số vòng lặp. Một mô hình mà ở đó mình có thể thoải mái thử, sai, sửa, và hỏi lại, mà không phải luôn canh xem mỗi vòng lặp đang đốt bao nhiêu token.

Chính khoảnh khắc này khiến mình bắt đầu tìm kiếm một hướng tiếp cận khác: trả tiền cho thời gian làm việc, thay vì trả tiền cho từng đơn vị ngữ cảnh lặp lại. Và từ đó, ý tưởng thuê GPU theo phiên để chạy một “local LLM” bắt đầu trở nên hợp lý, không phải vì nó rẻ hơn tuyệt đối, mà vì nó phù hợp hơn với cách mình thực sự làm việc.

## Thuê GPU theo phiên: “local LLM” nhưng không phải local
Khi đã xác định rằng vấn đề nằm ở mô hình trả tiền theo token, câu hỏi tiếp theo của mình trở nên khá rõ ràng: liệu có cách nào trả tiền cho một phiên làm việc mở, thay vì cho từng vòng lặp của context? Câu trả lời mình tìm thấy là thuê GPU theo giờ để chạy một LLM “local”, dù thực tế nó không hề local theo nghĩa truyền thống.

Với các dịch vụ như RunPod, mình có thể thuê những GPU tầm 20–24GB VRAM theo giờ với mức giá khá dễ chịu. Điều quan trọng ở đây không phải là benchmark hay throughput, mà là cảm giác làm việc hoàn toàn khác: trong suốt phiên đó, mình có thể thoải mái hỏi, sửa, thử, và làm lại mà không cần nghĩ xem mỗi lần lặp như vậy đang đốt thêm bao nhiêu token.

Việc trả tiền theo giờ tạo ra một “khung chi phí cố định” cho mỗi phiên làm việc. Mình biết rằng trong 3–4 tiếng đó, dù mình refactor bao nhiêu lần, đọc bao nhiêu file, hay loay hoay bao lâu với một framework mới, chi phí vẫn nằm trong một giới hạn rất rõ ràng. Điều này đặc biệt phù hợp với những buổi coding không thể đoán trước: lúc thì trôi rất nhanh, lúc thì mắc kẹt ở một chi tiết nhỏ.

Ở thời điểm đó, LLM không còn là một API mà mình phải gọi thật tiết kiệm, mà trở thành một không gian làm việc chung. Mình có thể để coding agent đọc toàn bộ workspace, giữ context lâu hơn, và chấp nhận sự lặp lại như một phần tự nhiên của quá trình suy nghĩ. Thay vì cố ép mọi thứ vào những prompt ngắn gọn để tiết kiệm token, mình có thể tập trung vào việc giải quyết vấn đề.

Thuê GPU theo giờ không khiến LLM thông minh hơn một cách kỳ diệu, nhưng nó thay đổi hoàn toàn tâm lý sử dụng. Mình không còn cảm giác mỗi lần hỏi thêm một câu là đang “mua thêm chi phí”, mà giống như đang tận dụng tối đa một phiên làm việc đã trả tiền. Với mình, đây chính là điểm mà mô hình này bắt đầu tỏ ra vượt trội so với pay-per-token cho những use case dài, lặp, và mang tính khám phá.

## Một vài mẹo để tối ưu phiên sử dụng LLM khi thuê GPU
Sau một thời gian thuê GPU theo phiên, mình nhận ra rằng chi phí không chỉ đến từ số giờ mình làm việc, mà còn đến từ những phút đầu phiên chỉ để… setup lại môi trường. Và vì các service như RunPod tính tiền ngay từ lúc bấm thuê GPU, mỗi phút chờ đợi đều là tiền thật.

Mẹo quan trọng nhất với mình là: nếu service cho phép, hãy build một [custom template (custom image)](https://docs.runpod.io/pods/templates/create-custom-template) cho riêng mình.

Hiểu đơn giản, đây là một image đã có sẵn toàn bộ những thứ mình cần:

- thư viện,
- tool,
- runtime,
- và các model mình hay dùng.

Mỗi lần thuê GPU, thay vì nhận một môi trường trắng tinh rồi cài lại từ đầu, mình chỉ cần khởi chạy từ image này là có thể bắt đầu làm việc gần như ngay lập tức.

Một số service có cung cấp network drive để lưu trữ thiết lập cá nhân giữa các phiên. Cách này tiện, nhưng nó có một cái giá khá rõ ràng: bạn phải trả tiền theo dung lượng × số giờ lưu trữ, kể cả khi bạn không hề dùng đến GPU. Với những người làm việc hằng ngày thì có thể chấp nhận được, nhưng với mình — chỉ thuê GPU khi thật sự cần — thì việc trả tiền cho một ổ đĩa “để không” là không hợp lý.

Trong khi đó, build một image cá nhân thì ngược lại. Dù image có nặng, tốc độ kéo image của các server như RunPod rất cao. Mình đang dùng một image khoảng 20GB, mỗi lần khởi tạo môi trường chỉ mất tầm 3–4 phút để kéo về và chạy. So với việc ngồi cài lại môi trường, tải model, cấu hình tool từng bước một, thì đây là một sự khác biệt rất lớn.

Điểm mấu chốt ở đây là: thời gian setup chính là chi phí ẩn. Khi bạn trả tiền theo giờ, mọi phút chờ đợi đều làm giảm hiệu quả của phiên làm việc. Một custom image giúp mình biến mỗi phiên thuê GPU thành một “ca làm việc” đúng nghĩa, thay vì một nửa thời gian chỉ để chuẩn bị.

Với mình, đây là một trong những tối ưu đơn giản nhưng mang lại hiệu quả rõ rệt nhất khi chuyển sang mô hình thuê GPU theo phiên.

# Chốt lại: chọn đúng cách trả tiền cho đúng nhịp làm việc
Nếu nhìn lại tháng vừa rồi thì chi phí mình bỏ ra cho LLM thật ra… khá buồn cười.

* Groq API (pay-on-demand, token thấp): ~0.8 USD
* Runpod (thuê GPU theo phiên): ~4 USD

Trong đó, phần tốn tiền nhất lại không phải lúc mình “làm việc thật”, mà là giai đoạn đầu loay hoay:
- build image riêng đề init pod,
- thử sai vài lần, test automation tools liên tục với các model lớn đắt tiền

Sau khi mọi thứ ổn định:
- mình có 1 image cá nhân chứa sẵn thư viện, model, config quen tay, mỗi lần cần chỉ mất 3 phút init runpod lên. Giờ mỗi phiên làm việc khoảng 4 tiếng chỉ tốn ~1 USD,
- các automation của mình giờ đã chọn được model vừa phải, vừa đáp ứng yêu cầu công việc vừa phù hợp túi tiền cá nhân.

Điều thú vị là: **đây cũng là tháng mình build được nhiều tool cá nhân hữu dụng** nhất kể từ lúc mình bắt đầu vibe code với LLM.

Không phải vì mình dùng model xịn hơn, mà vì mình không còn bị áp lực “đốt token” khi đang học hoặc đang hiểu sai.

Cuối cùng thì mình nhận ra một pattern khá rõ:

Những automation nhỏ, task rõ ràng, gọi ít → pay-on-demand theo token là quá đủ.

Những lúc:
- workspace lớn,
- đọc code người khác,
- học framework mới từ con số 0,
- thử sai liên tục
→ thuê GPU theo phiên rẻ hơn, thoải mái hơn, và hợp tâm lý hơn rất nhiều.

Mình không nghĩ có một mô hình “tối ưu tuyệt đối”.
Chỉ có mô hình phù hợp với nhịp làm việc của bạn ở thời điểm đó.

Với mình hiện tại:

* LLM không phải là thứ mình “bật lên cả ngày”,
* mà là một cái bàn làm việc có thể ***thuê khi cần, trả khi xong***.

Nhìn lại, đây không phải là câu chuyện mình tìm được cách xài LLM rẻ hơn, mà là câu chuyện mình học được cách chọn đúng cách trả tiền cho đúng nhịp làm việc của mình.