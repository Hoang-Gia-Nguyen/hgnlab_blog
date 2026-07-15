+++
title = "AI Agents: MCP vs SKILL — Hai Mặt Của Cùng Một Đồng Xu?"
date = 2026-07-15
draft = false
description = "MCP giúp AI kết nối với công cụ bên ngoài, SKILL định hình cách AI hành xử. Cùng tìm hiểu hai pattern này bổ trợ cho nhau thế nào trong hệ sinh thái AI agent."

[taxonomies]
categories = ["AI"]
tags = ["ai-agents", "mcp", "skill", "llm", "architecture", "protocol"]

[extra]
+++

Nếu bạn đang theo dõi mảng AI agent gần đây, chắc hẳn bạn đã nghe qua hai thuật ngữ có vẻ khá giống nhau: **MCP** và **SKILL**. Cả hai đều giúp AI agent trở nên mạnh mẽ hơn. Cả hai đều cung cấp cho mô hình những thứ vượt ra ngoài dữ liệu huấn luyện sẵn có. Nhưng thực ra, chúng giải quyết những vấn đề hoàn toàn khác nhau — và hiểu được sự khác biệt này là chìa khóa để xây dựng những agent thực sự hiệu quả.

Mấy tháng gần đây mình đã mày mò cả hai pattern này trên nhiều nền tảng khác nhau, và đây là những gì mình đúc kết được.

### MCP là gì?

**MCP (Model Context Protocol)** là một giao thức mở do Anthropic phát triển, giúp chuẩn hóa cách các ứng dụng AI kết nối với công cụ và nguồn dữ liệu bên ngoài. Hãy tưởng tượng nó như một **cổng USB-C cho AI** — một đầu nối phổ quát cho phép bất kỳ mô hình AI nào cũng có thể nói chuyện với bất kỳ công cụ hay nguồn dữ liệu nào thông qua một giao diện được định nghĩa rõ ràng.

Thay vì phải tự mày mò tích hợp từng công cụ một (Slack API một kiểu, database connector một kiểu, web search lại một kiểu khác), MCP cung cấp một giao thức duy nhất. Mô hình AI nói bằng MCP, và bất kỳ server nào triển khai giao thức này đều có thể phục vụ tools, resources, và prompts cho mô hình đó.

**Ý tưởng cốt lõi:** MCP là về *kết nối*. Nó trả lời câu hỏi: "AI agent của mình làm thế nào để với tay ra thế giới bên ngoài?"

**Ví dụ về MCP trong thực tế:**
- Một MCP server bọc GitHub API cho phép agent của bạn tạo PR, review code, và quản lý issues.
- Một filesystem MCP server cho phép agent đọc và ghi file trong một thư mục sandbox.
- Một database MCP server cho phép agent chạy truy vấn PostgreSQL.

### SKILL là gì?

**SKILL** (trong ngữ cảnh của các nền tảng như Codex) lại là một câu chuyện hoàn toàn khác. SKILL là một tập hợp các chỉ dẫn cục bộ — một file `SKILL.md` — định nghĩa *cách một AI agent nên hành xử* trong một lĩnh vực hoặc tác vụ nhất định. Nó không phải là giao thức để kết nối với công cụ bên ngoài. Nó là một cuốn sổ tay hướng dẫn dành cho chính mô hình AI.

Một SKILL chỉ dẫn cho agent:
- Cần xem xét bối cảnh gì trước khi hành động
- Những quy ước và pattern nào cần tuân theo
- Cấu trúc đầu ra như thế nào
- Những quy tắc an toàn hoặc ràng buộc nào áp dụng
- Những file tham khảo nào cần tải

**Ý tưởng cốt lõi:** SKILL là về *định hình hành vi*. Nó trả lời câu hỏi: "AI agent của mình làm sao để biết phải làm gì và làm thế nào cho đúng?"

**Ví dụ về SKILL trong thực tế:**
- Một SKILL "code-reviewer" hướng dẫn agent kiểm tra bugs, lỗ hổng bảo mật, và vi phạm coding style.
- Một SKILL "zola-write" chỉ dẫn agent cách viết blog post, front matter ra sao, và tone giọng thế nào.
- Một SKILL "latex-compile" dạy agent cách biên dịch TeX project và những phương án dự phòng cần thử.

### Sự Khác Biệt Cốt Lõi

| Khía cạnh | MCP | SKILL |
|---|---|---|
| **Mục đích** | Kết nối với công cụ/dữ liệu bên ngoài | Định hình hành vi agent |
| **Bản chất** | Giao thức / chuẩn API | File chỉ dẫn / prompt |
| **Nơi tồn tại** | Server riêng biệt (process riêng) | Nhúng trong project hoặc config agent |
| **Cung cấp** | Tools, resources, context providers | Guidelines, conventions, rules |
| **Cách vận hành** | Gọi API runtime | Được parse và áp dụng ở prompt time |
| **Ví von** | Cổng USB-C kết nối công cụ | Sổ tay nhân viên cho agent |

### Tại Sao Phải Phân Biệt?

Vấn đề là: **MCP mà không có SKILL giống như một kho đồ không có quản đốc.** Bạn có tất cả những kết nối mạnh mẽ đến database, API, và file system, nhưng không có chỉ dẫn về việc *dùng* công cụ nào khi nào, *kết hợp* chúng ra sao, hay *tránh* những gì.

**SKILL mà không có MCP giống như quản đốc không có đồ nghề.** Bạn có chỉ dẫn chi tiết và best practices, nhưng không có cách nào để thực thi bất cứ thứ gì ngoài kiến thức có sẵn của mô hình.

Điều kỳ diệu xảy ra khi bạn kết hợp cả hai:

> **SKILL** định nghĩa kế hoạch và luật chơi. **MCP server** cung cấp phương tiện để thực thi.

### Ví Dụ Thực Tế

Giả sử bạn đang xây dựng một trợ lý coding AI.

- **MCP servers** cung cấp quyền truy cập vào: filesystem codebase (đọc/ghi), linter (chạy kiểm tra), test runner (chạy test), và Git (commit/push).
- **Một SKILL "web-dev"** chỉ dẫn cho trợ lý: "Luôn chạy linter sau khi sửa code. Viết test trước. Dùng React hooks, không dùng class components. Kiểm tra accessibility. Kiểm tra build trước khi commit."

Không có SKILL, agent có công cụ nhưng không biết quy ước của team bạn. Không có MCP, agent biết quy ước nhưng không thể chạm vào code.

### Khi Nào Dùng Cái Gì

**Dùng MCP khi:**
- Bạn cần agent tương tác với dịch vụ bên ngoài (API, database, file system).
- Bạn muốn một cách chuẩn hóa để expose tools trên nhiều nền tảng AI khác nhau.
- Bạn đang xây dựng một hệ sinh thái công cụ mà nhiều agent có thể dùng chung.

**Dùng SKILL khi:**
- Bạn cần agent tuân theo các quy ước hoặc hướng dẫn riêng của project.
- Bạn đang hệ thống hóa kiến thức domain, quy tắc an toàn, hoặc quy trình làm việc.
- Bạn muốn đảm bảo chất lượng đầu ra nhất quán qua nhiều tác vụ khác nhau.

### Tương Lai: Sự Hội Tụ

Thành thật mà nói, mình nghĩ ranh giới này sẽ dần mờ đi. Chúng ta đã thấy những MCP server trả về structured prompts — đó là hành vi giống SKILL. Và một số SKILL tham chiếu đến MCP server như backend thực thi. Hai pattern này bổ trợ chứ không cạnh tranh.

Dự đoán của mình? Những AI agent mạnh mẽ nhất sẽ dùng **MCP để vươn ra bên ngoài** và **SKILL để định hướng bên trong**, được kết nối trong một kiến trúc phân lớp nơi context chảy tự nhiên giữa chúng.

### Kết Luận

MCP và SKILL không phải là lựa chọn một mất một còn. Chúng là hai tầng khác nhau trong stack của một AI agent thực thụ. MCP mở cánh cửa ra thế giới bên ngoài. SKILL đảm bảo agent bước qua cánh cửa đó đúng cách.

Nếu bạn đang xây dựng agent ngày hôm nay, hãy đầu tư vào cả hai. Agent của bạn sẽ thông minh hơn, an toàn hơn, và hữu dụng hơn rất nhiều.

*Kinh nghiệm của bạn với MCP và SKILL thế nào? Hãy để lại bình luận hoặc gửi tin nhắn — mình rất muốn nghe cách bạn kết hợp chúng trong project của mình.*
