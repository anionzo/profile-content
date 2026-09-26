---
title: "OpenRouter niêm yết typesafe/jev-router miễn phí: Dùng thử AI System One điều hướng 1M token từ 25/9"
titleEn: "OpenRouter Lists typesafe/jev-router for Free: Testing 1M-Context System One Routing from September 25th"
slug: openrouter-niem-yet-typesafe-jev-router
date: 2026-09-26
updated: 2026-09-26
tags: [ai, llm, openrouter, backend, architecture]
cover: /images/posts/openrouter-niem-yet-typesafe-jev-router/cover.jpg
excerpt: "Từ ngày 25/9/2026, OpenRouter chính thức mở miễn phí endpoint typesafe/jev-router với giá $0/1M token và context 1.000.000 token. Phân tích chi tiết chính sách giá free, hạn mức rate limit 50 vs 1.000 req/ngày và những lưu ý thực tế khi triển khai."
excerptEn: "Starting September 25, 2026, OpenRouter officially listed the typesafe/jev-router endpoint for free ($0/1M tokens) with a 1,000,000-token context window. A detailed breakdown of its free-tier quotas (50 vs 1,000 req/day), pricing mechanics, and production caveats."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Nếu bạn thường xuyên gọi API qua OpenRouter, có một tin tức về hạ tầng rất đáng chú ý vừa diễn ra: **Từ ngày 25/9/2026, OpenRouter đã chính thức niêm yết endpoint `typesafe/jev-router` và mở dùng hoàn toàn MIỄN PHÍ ($0.00 / 1M token).**

Đây là sự kết hợp giữa OpenRouter và startup TypeSafe AI của Diogo Almeida (cựu nhân sự nghiên cứu OpenAI), đưa mô hình phán đoán **Jev** vào làm tầng điều hướng (Dynamic Router) ở cửa ngõ API. 

Với cửa sổ ngữ cảnh khổng lồ lên tới **1.000.000 token** và mức giá hiển thị 0 đồng, đây là cơ hội tuyệt vời để giới lập trình viên cắm vào hệ thống thử nghiệm mà không tốn chi phí. Nhưng đằng sau chữ "Free" này có cơ chế tính toán, hạn ngạch và những đánh đổi kỹ thuật nào mà bạn cần biết?

![OpenRouter niêm yết typesafe/jev-router](/images/posts/openrouter-niem-yet-typesafe-jev-router/cover.jpg)

## 1. Mổ xẻ chính sách giá: Miễn phí cái gì và tính phí ra sao?

Nhiều anh em nhìn vào bảng giá của OpenRouter có thể sẽ thắc mắc: *Mô hình Jev gốc vốn có phí, tại sao Jev Router lại ghi $0?*

Dưới đây là cơ chế phân tách chi phí rất rõ ràng:

### Jev gốc vs. Jev Router trên OpenRouter
1. **Model phán đoán Jev 1.13 (`typesafe/jev-1.13`):** 
   - Đây là mô hình System One gốc chuyên trả về typed decision.
   - Giá niêm yết: **0.042 USD cho 1 triệu token đầu vào**, token đầu ra miễn phí ($0). Bạn tự viết logic gọi Jev để phân loại hoặc kiểm duyệt.
2. **Jev Router (`typesafe/jev-router` - Niêm yết từ 25/9/2026):**
   - Giá niêm yết trên OpenRouter: **$0.00 / 1M input tokens** và **$0.00 / 1M output tokens**.
   - OpenRouter **không thu bất kỳ khoản phụ phí nào** cho bước phân tích prompt và ra quyết định định tuyến của Jev.
   - Dòng chữ trên trang chủ OpenRouter ghi rõ: *"This model is free to use"*.

Nói cách khác: **Toàn bộ công đoạn "suy nghĩ" để chọn model của Jev Router được tài trợ miễn phí.**

![Quy trình phân luồng của typesafe/jev-router](/images/posts/openrouter-niem-yet-typesafe-jev-router/diagram-router-workflow.jpg)

---

## 2. Hạn ngạch Free Tier trên OpenRouter: 50 hay 1.000 requests/ngày?

Vì `typesafe/jev-router` được xếp vào nhóm mô hình miễn phí (`:free`), nó chịu sự điều tiết của **chính sách Rate Limit tầng miễn phí** của OpenRouter. Cụ thể có 2 kịch bản hạn ngạch:

| Hạng tài khoản | Tốc độ tối đa | Hạn ngạch theo ngày | Ghi chú thực tế |
|---|---|---|---|
| **Tài khoản Free thuần túy** (Chưa từng nạp tiền) | **20 requests / phút** | **50 requests / ngày** | Phù hợp vọc vạch, test prompt cá nhân |
| **Tài khoản đã nạp $10+** (Lifetime Credit Top-up) | **20 requests / phút** | **1.000 requests / ngày** | Đủ chạy thử nghiệm cho các dự án nội bộ |

> [!TIP]
> Nếu bạn muốn tận dụng 1.000 request miễn phí mỗi ngày với `typesafe/jev-router`, bạn chỉ cần nạp tối thiểu 10 USD vào tài khoản OpenRouter một lần duy nhất. Số tiền 10 USD đó vẫn nằm nguyên trong ví của bạn để dành cho các API trả phí khác, nhưng tài khoản sẽ tự động được mở khóa hạn ngạch 1.000 lượt gọi free/ngày trọn đời.

---

## 3. typesafe/jev-router thực chất làm gì ở tầng Gateway?

Khi bạn gửi request đến `https://openrouter.ai/api/v1/chat/completions` với body:

```json
{
  "model": "typesafe/jev-router",
  "messages": [
    { "role": "user", "content": "Phân tích đoạn log lỗi IIS sau..." }
  ]
}
```

Thay vì đưa câu hỏi cho một con LLM truyền thống trả lời ngay, OpenRouter kích hoạt quy trình 3 bước:
1. **Nạp ngữ cảnh (lên tới 1M token):** Toàn bộ lịch sử hội thoại được nạp vào bộ nhớ đệm.
2. **Jev ra phán đoán dưới 100ms:** Mô hình Jev phân tích độ phức tạp ngữ nghĩa của prompt để ra quyết định:
   - *Độ khó thấp (chào hỏi, dịch thuật đơn giản, format JSON):* Đẩy về Tier giá rẻ như GPT-6 Luna, Claude Haiku 3.5, Gemini Flash.
   - *Độ khó trung bình (viết code nghiệp vụ, kiểm tra policy):* Đẩy về Tier cân bằng như GPT-6 Sol, Claude Sonnet 4.
   - *Độ khó cao (lập luận toán học, suy luận đa bước, agentic coding):* Đẩy về Tier đầu bảng như Claude Opus 5.5, GPT-6 Astra, o1 Pro.
   - *Cấu hình Reasoning Budget:* Cân chỉnh số token suy nghĩ (thinking tokens) vừa đủ, tránh lãng phí thời gian chờ đợi.
3. **Thực thi và phản hồi:** OpenRouter chuyển tiếp request tới model được chọn và stream kết quả về client.

---

## 4. Những điều dân kỹ thuật cần tỉnh táo (Caveats)

Việc được dùng miễn phí một router thông minh là rất hấp dẫn, nhưng ở góc độ kỹ sư vận hành, có hai điểm cốt lõi bạn phải lưu ý trước khi cắm vào production:

### Lưu ý 1: Chưa có dữ liệu thực nghiệm so với Auto Router sẵn có
OpenRouter từ trước đến nay đã có endpoint **`openrouter/auto`** (tự động phân luồng dựa trên uptime, latency và giá nhà cung cấp).
Tính đến thời điểm hiện tại (cuối tháng 9/2026), **chưa có bất kỳ báo cáo dữ liệu thực tế độc lập nào (empirical benchmark)** chứng minh `typesafe/jev-router` thông minh hơn hay tiết kiệm tiền hơn `openrouter/auto`. Chúng ta chưa biết trong một tập 10.000 prompt thực tế, tỷ lệ chọn model của Jev có thực sự tối ưu hay không, hay đôi khi nó lại chọn nhầm mô hình đắt tiền cho một câu hỏi ngớ ngẩn (over-routing).

### Lưu ý 2: Độ trễ cộng dồn vào TTFT (Time to First Token)
Mặc dù Jev là mô hình System One chạy rất nhanh (khoảng 70–100ms), nhưng việc chèn thêm một bước phán đoán trước khi chạm tới model thực thi vẫn làm tăng chỉ số **TTFT**.
Với các tác vụ cần phản hồi tức thì (như trợ lý ảo thời gian thực hoặc voice bot), độ trễ cộng thêm này có thể khiến người dùng cảm giác bị "khựng" lại một nhịp so với việc gọi thẳng vào model đích.

---

## Lời khuyên triển khai

- **Với môi trường Development / Staging:** `typesafe/jev-router` là một endpoint "hời" không thể bỏ qua. Bạn được tận dụng 1M token context, miễn phí router, và tận dụng quota 50–1.000 req/ngày để thử nghiệm phân luồng tự động.
- **Với môi trường Production:** Hãy bắt đầu bằng cách route 5–10% traffic thử nghiệm (canary testing), ghi log lại model mà Jev đã chọn và đối chiếu chi phí hóa đơn thực tế trong 2 tuần trước khi cutover toàn bộ hệ thống.

## Nguồn tham khảo

- OpenRouter, [TypeSafe: Jev Router (typesafe/jev-router) Model Overview & Pricing](https://openrouter.ai/typesafe/jev-router) (Niêm yết 25/09/2026)
- OpenRouter, [Free AI Models on OpenRouter & Rate Limit Policy](https://openrouter.ai/collections/free-models)
- OpenRouter, [Jev Documentation & Community Decisions Guide](https://openrouter.ai/docs/guides/community/jev)
- RuntimeWire, [TypeSafe's Jev Router picks models for free, with a claimed million-token window](https://runtimewire.com/article/typesafe-jev-router-openrouter-launch) (26/09/2026)
- TipRanks, [Developer Contest Underscores Early Demand for Jev Model on OpenRouter](https://www.tipranks.com/news/private-companies/developer-contest-underscores-early-demand-for-jev-model-on-openrouter) (25/09/2026)

<!-- lang:en -->

If you regularly route inference requests through OpenRouter, a notable infrastructure update quietly landed this week: **As of September 25, 2026, OpenRouter officially listed the `typesafe/jev-router` endpoint, offering it completely FREE ($0.00 / 1M tokens).**

This deployment represents a joint integration between OpenRouter and Diogo Almeida’s TypeSafe AI, embedding the **Jev** structured decision engine directly into the API gateway tier to function as a dynamic traffic controller.

Featuring a massive **1,000,000-token context window** and a zero-dollar routing price tag, it presents a compelling opportunity for engineering teams to experiment with ambient decision routing without incurring middleware surcharges. But what does "Free" actually mean under OpenRouter's policies, what are the daily rate limits, and what technical trade-offs must you evaluate?

![OpenRouter Lists typesafe/jev-router](/images/posts/openrouter-niem-yet-typesafe-jev-router/cover.jpg)

## 1. Dissecting the Pricing Mechanics: What Is Actually Free?

Developers glancing at the OpenRouter pricing table might wonder: *If base Jev charges for inputs, why is Jev Router listed as $0?*

Here is the exact cost breakdown:

### Base Jev 1.13 vs. Jev Router on OpenRouter
1. **Base Jev 1.13 (`typesafe/jev-1.13`):**
   - The standalone System One model that outputs typed decisions.
   - Listed pricing: **$0.042 per million input tokens**, with free output tokens ($0). You invoke Jev directly within your codebase for custom classification, verification, or triage.
2. **Jev Router (`typesafe/jev-router` - Listed September 25, 2026):**
   - Listed pricing on OpenRouter: **$0.00 / 1M input tokens** and **$0.00 / 1M output tokens**.
   - OpenRouter charges **zero markup** for the upstream prompt analysis or the model-selection pass performed by Jev.
   - The official listing explicitly states: *"This model is free to use"*.

In short: **The cognitive deliberation pass required to evaluate prompt complexity and select an execution model is completely subsidized.**

![Routing Workflow of typesafe/jev-router](/images/posts/openrouter-niem-yet-typesafe-jev-router/diagram-router-workflow.jpg)

---

## 2. OpenRouter Free-Tier Rate Limits: 50 vs. 1,000 Requests Daily

Because `typesafe/jev-router` is designated as a `:free` tier model, it falls under OpenRouter's **free-model platform rate limits**:

| Account Tier | Max Throughput | Daily Allocation | Practical Context |
|---|---|---|---|
| **Standard Free Account** (Zero lifetime deposits) | **20 requests / min** | **50 requests / day** | Ideal for localized prototyping and prompt tests |
| **Funded Account** (Lifetime deposit $\ge$ $10) | **20 requests / min** | **1,000 requests / day** | Robust enough for internal team staging and automation |

> [!TIP]
> To unlock the generous 1,000 free daily request quota for `typesafe/jev-router`, you only need to top up your OpenRouter account balance with at least $10 once. That $10 deposit remains intact in your balance for paid models, while permanently expanding your daily free-model allowance.

---

## 3. What Does typesafe/jev-router Do at the Gateway?

When a client application submits a standard chat completion request:

```json
{
  "model": "typesafe/jev-router",
  "messages": [
    { "role": "user", "content": "Analyze this IIS deadlock stack trace..." }
  ]
}
```

OpenRouter executes a three-phase lifecycle:
1. **Context Ingestion (Up to 1M Tokens):** The conversational history is loaded into memory buffers.
2. **Sub-100ms System One Deliberation:** TypeSafe's Jev model evaluates prompt semantics to determine two parameters:
   - *Target Model Tiering:* Routine queries (formatting, simple summaries) route to cost-efficient models (GPT-6 Luna, Claude Haiku, Gemini Flash); moderate business logic routes to mid-tier engines (GPT-6 Sol, Claude Sonnet); while deeply nested reasoning routes to frontier models (Claude Opus 5.5, GPT-6 Astra, o1 Pro).
   - *Adaptive Reasoning Budget:* Calibrates chain-of-thought depth so extended deliberation tokens are allocated only when strictly necessary.
3. **Execution & Upstream Streaming:** OpenRouter seamlessly proxies the request to the chosen destination and streams the completion back to the client.

---

## 4. Technical Caveats for Production Teams

While zero-cost smart routing sounds ideal, disciplined systems engineers must weigh two critical realities:

### Caveat 1: Absence of Empirical Production Data
OpenRouter has long operated **`openrouter/auto`** (an established heuristic router balancing provider latency, pricing, and availability).
As of late September 2026, there is **zero independent empirical benchmark data** proving `typesafe/jev-router` delivers superior cost-to-quality ratios over legacy auto-routing. We do not yet know its real-world misclassification rate—specifically, whether it risks *over-routing* simple verbose prompts to expensive models or *under-routing* deceptive edge cases to budget tiers.

### Caveat 2: Compounded Latency on Time-to-First-Token (TTFT)
Even though Jev operates within an agile 70–100ms bracket, inserting an intermediate classification pass inherently inflates overall **TTFT**.
For real-time voice agents or conversational interfaces requiring sub-300ms responsiveness, dedicating an extra 100 milliseconds purely to gateway routing can produce a perceptible hitch in responsiveness.

---

## Practical Takeaway

- **For Development & Prototyping:** `typesafe/jev-router` is an exceptional zero-cost utility. Gaining a 1M context window and free intelligent dispatch within a 50 to 1,000 request/day quota makes it an immediate win for experimental projects.
- **For Production Environments:** Adopt a canary deployment strategy—routing 5% to 10% of background traffic through `jev-router`, auditing destination selections and billing logs for two weeks before committing primary production pipelines to this new paradigm.

## References

- OpenRouter, [TypeSafe: Jev Router (typesafe/jev-router) Model Overview & Pricing](https://openrouter.ai/typesafe/jev-router) (Listed Sep 25, 2026)
- OpenRouter, [Free AI Models on OpenRouter & Rate Limit Policy](https://openrouter.ai/collections/free-models)
- OpenRouter, [Jev Documentation & Community Decisions Guide](https://openrouter.ai/docs/guides/community/jev)
- RuntimeWire, [TypeSafe's Jev Router picks models for free, with a claimed million-token window](https://runtimewire.com/article/typesafe-jev-router-openrouter-launch) (Sep 26, 2026)
- TipRanks, [Developer Contest Underscores Early Demand for Jev Model on OpenRouter](https://www.tipranks.com/news/private-companies/developer-contest-underscores-early-demand-for-jev-model-on-openrouter) (Sep 25, 2026)
