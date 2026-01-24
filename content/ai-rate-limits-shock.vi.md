+++
title = "Cơn Sốc Giới Hạn API AI Miễn Phí"
date = 2025-12-25
draft = false
description = "Tại sao các nhà cung cấp AI lại siết chặt giới hạn API miễn phí, cách nó ảnh hưởng đến anh chị em hobbyist, và mình nên làm gì tiếp theo."
[taxonomies]
categories = ["AI"]
[extra]
cover.image = "images/cover-ai-limits.png"
cover.alt = "Biểu đồ hiển thị giới hạn free-tier API ngày càng co lại"
+++

Trong năm vừa qua, gần như tất cả các nhà cung cấp AI lớn đều siết chặt giới hạn API miễn phí hoặc low-tier — cắt giảm drastically requests-per-minute (RPM) và requests-per-day (RPD), hoặc thậm chí loại bỏ hoàn toàn quyền truy cập API miễn phí. Nếu mình chỉ dùng web UI để chat, thì có thể mình chẳng nhận ra gì. Nhưng nếu mình viết automation scripts, build mini apps, hay run các personal integrations trên free tier, thì... ơi là ơi, cảm giác tổn thương lắm.

**Cái gì thay đổi và sao nó shock vậy?**
- **Web users**: trải nghiệm tương tác vẫn ok — hạn lượng yêu cầu thường vẫn đủ dùng hằng ngày.
- **API users**: automation tạch. Background jobs, CLI tools, bots, hay personal integrations bắt đầu chạm giới hạn hoặc bị block phía sau paywall.
- **Hobbyist như mình**: ngay cả những khoản phí tháng nhỏ cũng là quyết định khó — app này chẳng kiếm được tiền gì, mà còn lo sợ expose API key dưới mô hình pay-as-you-go.

**Các big player đã làm gì (trong 1–2 năm qua)**
Tính đến 2025-12-25, dưới đây là snapshot từ các nhà cung cấp (giới hạn thay đổi theo account, tier, và model):
- **OpenAI**:
  - Hiện tại: RPM/RPD/TPM được quản lý ở tầng org/project; giá trị cụ thể tùy model và tier. Xem: https://platform.openai.com/docs/guides/rate-limits
  - Xu hướng thay đổi: dần dần loại bỏ free API credits; chuyển hướng sang pay-as-you-go; siết chặt per-org policies; tự động nâng tier khi spend tăng.
- **Google (Gemini)**:
  - Hiện tại: giới hạn phân tầng (Free, Tier 1–3) với RPM/TPM/RPD; xem live limits trong AI Studio. Docs: https://ai.google.dev/gemini-api/docs/rate-limits (cập nhật 2025-12-23); AI Studio: https://aistudio.google.com/usage?timeRange=last-28-days&tab=rate-limit
  - Batch API: có cap riêng cho enqueued tokens per model/tier.
- **Anthropic (Claude)**:
  - Hiện tại: giới hạn tùy plan/account; tính theo requests/tokens/concurrency; xem trong Console. Docs: https://platform.claude.com/docs
- **OpenRouter**:
  - Hiện tại: free models bị limit 50 requests/day; nếu mua ≥10 credits thì limit tăng lên 1000 requests/day. FAQ: https://openrouter.ai/docs/faq#how-are-rate-limits-calculated
- **Khác (Cohere, Mistral, v.v.)**:
  - Mỗi cái có policy riêng — cứ check docs của từng nơi. Cohere: https://docs.cohere.com/; Mistral: https://docs.mistral.ai/

**Tại sao các nhà cung cấp lại "siết" giới hạn?**
- **Chặn fraud & abuse**: một số tổ chức tạo hàng trăm, hàng ngàn free accounts rồi chạy commercial apps trên free keys mà chẳng trả tiền.
- **Áp lực tính toán**: model ngày càng mạnh, users và requests tăng vũ bão → inference cost tăng, capacity tight.
- **Thời dùng thử hết rồi**: AI giờ đã mainstream, mọi người phụ thuộc vào nó — bây giờ là lúc làm tiền thôi.

**Web vs API — tác động khác nhau**
- **Web UI**: con người click chậm chậm, có anti-abuse controls, session-based rate limits → dễ dàng giữ UX tốt.
- **API**: máy tính call thật nhanh, spike concurrency cao; RPM/RPD siết chặt để bảo vệ infra & cost, nhưng cái này break những ứng dụng nhỏ lẻ như của mình.

**Giá tiền chưa tới mức "chết" — nhưng vẫn là áp lực với hobbyist**
- Pay-as-you-go có giá tương đối hợp lý, nhưng với hobby projects vẫn là một khoản chi khá lăn tăn. 
- **Risk**: expose API keys trong personal tools hoặc small apps → khó manage.
- **Cách giảm thiểu**: server-side proxies, scoped keys, daily caps, usage alerts, dùng smaller models để test.

**Mình sẽ làm gì tiếp theo?**
- Tìm kiếm thêm các nhà cung cấp hào phóng hơn.
- Cân nhắc trả một khoản tiền nhỏ hằng tháng cho mấy cái thực sự cần, đặt giới hạn trừ tiền.
- Thêm circuit breakers vào automation (retry/backoff, queue, chạy vào buổi tối trong giới hạn). Bây giờ phải hết sức cẩn thận với những tính năng cố retry liên tục.
- Ưu tiên local hoặc hybrid setups (chạy small local models cho routine tasks, cloud khi thực sự cần).

**Nguồn tham khảo (checked 2025-12-25)**
- OpenAI — Rate limits & usage tiers: https://platform.openai.com/docs/guides/rate-limits
- Google Gemini — Rate limits: https://ai.google.dev/gemini-api/docs/rate-limits; Models: https://ai.google.dev/models/gemini; AI Studio: https://aistudio.google.com/usage?timeRange=last-28-days&tab=rate-limit
- Anthropic Claude — Docs: https://platform.claude.com/docs; Status: https://status.anthropic.com/
- OpenRouter — Limits: https://openrouter.ai/docs/faq#how-are-rate-limits-calculated
- Mistral — Docs: https://docs.mistral.ai/
- Cohere — Docs: https://docs.cohere.com/

Nếu bạn dùng automation trên free AI APIs, chắc bạn đã cảm nhận cái "cơn sốc" này giống mình. Mình vẫn còn đang suy nghĩ có nên chuyển sang các nhà cung cấp khác hay chấp nhận trả một khoản tiền nhỏ mỗi tháng. Dù sao, mình hi vọng sẽ sớm tìm được cách, khi đó mình sẽ share được những tips và setups để giữ các tiny projects của chúng ta tiếp tục sống.
