---
title: "CLM-8B mở mã nguồn: Chấm action nhanh gấp 9 lần Jev với độ trễ 16.5ms"
titleEn: "Open-Source CLM-8B: Scoring Agent Actions 9x Faster than Jev with 16.5ms Latency"
slug: clm-8b-cham-action-nhanh-gap-9-lan-jev
date: 2026-09-26
updated: 2026-09-26
tags: [ai, llm, backend, architecture, open-source]
cover: /images/posts/clm-8b-cham-action-nhanh-gap-9-lan-jev/cover.jpg
excerpt: "Chỉ một tuần sau khi TypeSafe AI gây sốt với mô hình phán đoán Jev, nhóm nghiên cứu Stanford và NVIDIA đã tung ra CLM-8B: mô hình System One mã mở Apache 2.0, dùng kiến trúc Contrastive Dual-Encoder đẩy độ trễ xuống 16.5ms và tương thích 100% API của TypeSafe."
excerptEn: "Just a week after TypeSafe AI made waves with its Jev decision model, researchers from Stanford and NVIDIA dropped CLM-8B: an open-source Apache 2.0 System One model using a Contrastive Dual-Encoder architecture that cuts decision latency to 16.5ms with drop-in TypeSafe API compatibility."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Mới hôm qua, mình vừa viết bài phân tích về mô hình **Jev** của TypeSafe AI — một mô hình "mất tiếng" không thèm sinh chữ mà chỉ trả về quyết định có kiểu (typed decision) với tốc độ dưới 100ms. Lúc đó, mình đã nghĩ mức giá 0.042 USD / triệu token và độ trễ 100ms của Jev đã là đỉnh cao tối ưu cho hạ tầng backend rồi.

Thế nhưng trong thế giới AI năm 2026, kỷ lục sinh ra chỉ để bị phá vỡ sau vài ngày.

Ngày 23/9/2026, nhóm nghiên cứu từ **Đại học Stanford và NVIDIA Research** (nhóm Contrastive-LM) đã chính thức công bố **CLM-8B** (Contrastive Language Model 8B). Đây là một mô hình System One mã nguồn mở hoàn toàn (Apache 2.0), được thiết kế với mục tiêu duy nhất: **chấm điểm các hành động ứng viên (action scoring) với tốc độ bàn thờ**.

Trong các bài kiểm tra zero-shot, CLM-8B đạt tốc độ phán đoán nhanh hơn Jev tới **9 lần**, ép độ trễ xuống mức không tưởng: **16.5 mili-giây**. Khi số lượng hành động ứng viên tăng lên khoảng 1.000 lựa chọn, tốc độ của nó thậm chí nhanh gấp **13 lần** Jev nhờ một chiêu bài kiến trúc cực kỳ thông minh.

![CLM-8B chấm action nhanh gấp 9 lần Jev](/images/posts/clm-8b-cham-action-nhanh-gap-9-lan-jev/cover.jpg)

## 1. Bản chất kiến trúc: Dual-Encoder Contrastive thay vì Sampler

Để hiểu vì sao CLM-8B lại nhanh đến mức khủng khiếp như vậy, chúng ta phải nhìn vào sự khác biệt cốt lõi về mặt kiến trúc giữa nó và Jev.

- **Jev (TypeSafe):** Sử dụng bộ lấy mẫu song song (Parallel Sampler) kết hợp phương pháp huấn luyện RLCD (Reinforcement Learning for Calibrated Decisions). Dù không sinh từng token nối tiếp như LLM truyền thống, Jev vẫn phải xử lý toàn bộ prompt ngữ cảnh kèm câu hỏi qua một mạng nơ-ron hợp nhất trong mỗi lần suy luận.
- **CLM-8B (Stanford & NVIDIA):** Đoạn tuyệt hoàn toàn với cơ chế đó. Họ sử dụng kiến trúc **Dual-Encoder Contrastive** (hai bộ mã hóa tương phản):
  1. Lấy mô hình nền **Qwen3-8B** và đóng băng hoàn toàn trọng số (frozen backbone).
  2. Gắn thêm hai đầu chiếu tuyến tính nhỏ (projection heads): một **State Head** (để mã hóa trạng thái/ngữ cảnh hiện tại) và một **Action Head** (để mã hóa các hành động khả dĩ).
  3. Huấn luyện hai đầu chiếu này bằng hàm mất mát tương phản **InfoNCE Loss**. Mục tiêu toán học rất rõ ràng: kéo vector biểu diễn của trạng thái và hành động đúng lại gần nhau trong không gian tiềm ẩn (latent space), đồng thời đẩy các hành động sai (hard negatives) ra thật xa.

Quá trình huấn luyện của nhóm diễn ra qua 3 chặng bài bản: Pre-train trên 60 triệu cặp hỏi đáp Nemotron, Mid-train trên 30 triệu mẫu hard negatives tổng hợp, và Post-train trên 1 triệu chuỗi hành vi tác tử (agentic trajectories).

Nhờ kiến trúc hai đầu tách biệt này, việc "chấm điểm một hành động" quy về một phép tính tích vô hướng (dot-product) cực nhanh giữa vector trạng thái và vector hành động, thay vì phải chạy suy luận qua cả mạng nơ-ron khổng lồ.

![So sánh kiến trúc System One: Jev vs CLM-8B](/images/posts/clm-8b-cham-action-nhanh-gap-9-lan-jev/diagram-clm-vs-jev.jpg)

## 2. Vũ khí bí mật: Bộ đệm hai chiều (Two-sided Caching)

Nếu chỉ dừng ở việc dùng dual-encoder, CLM-8B đã nhanh hơn rồi. Nhưng cú huých giúp nó đạt con số 16.5ms nằm ở khả năng **Two-sided Caching**.

Hãy tưởng tượng bạn đang xây dựng một agent tự động duyệt web hoặc một hệ thống điều hướng cuộc gọi cho sinh viên ở trường:
- Tập hành động khả dĩ thường cố định: `[Chuyển_tiếp_phòng_đào_tạo, Gửi_email_xác_nhận, Hủy_yêu_cầu, Đẩy_vào_hàng_đợi_soát_xét]`.
- Với các mô hình thông thường, mỗi khi có một request mới của sinh viên gửi tới, mô hình lại phải tính toán lại từ đầu toàn bộ mô tả của 4 hành động này.
- Với CLM-8B, vì Action Head hoạt động độc lập, hệ thống có thể **tính toán trước và cache vĩnh viễn vector embedding của toàn bộ các hành động** vào bộ nhớ VRAM!

Khi một request mới ập vào:
1. CLM-8B chỉ mất đúng vài mili-giây để State Head mã hóa chuỗi ngữ cảnh của sinh viên thành một vector duy nhất.
2. Sau đó, nó thực hiện một phép nhân ma trận chớp nhoáng giữa vector đó với danh sách action embedding đã nằm sẵn trong cache.

Đó là lý do vì sao khi số lượng hành động lên tới 1.000 ứng viên (ví dụ như bài toán WikiRacing chọn link bài viết kế tiếp), CLM-8B nhanh hơn Jev tới **13 lần**. Jev bị nghẽn vì độ dài context phình to, còn CLM-8B chỉ đơn giản là nhân thêm vài hàng trong ma trận đã cache sẵn.

## 3. Cuộc chiến benchmark: Đổi ngôi ở vai trò Verifier

Vậy tốc độ nhanh gấp 9 lần có phải đánh đổi bằng sự ngu ngơ hay không? Báo cáo kỹ thuật của Stanford và NVIDIA đưa ra những con số rất sòng phẳng:

### Mặt CLM-8B vượt trội: Vai trò Verifier (Hậu kiểm)
Trong các hệ thống Agentic Coding (như SWE-bench hay Terminal-Bench), một tác vụ cực kỳ tốn kém là **Verifier**: sau khi code xong, agent cần một mô hình System One chạy ngầm kiểm tra xem đoạn diff vừa sinh ra có an toàn không, test case đã pass thật chưa.
- Trên benchmark **DeepSWE**, CLM-8B đạt độ chính xác **81.6%**, vượt xa mức **71.1%** của Jev.
- Trên **Terminal-Bench 2.1**, CLM-8B đạt **87.6%**, trong khi Jev đạt **83.1%**.
- Quan trọng hơn cả: ở vai trò verifier này, CLM-8B chạy **nhanh hơn Jev từ 4.1 đến 5.7 lần**.

### Mặt Jev vẫn giữ lợi thế: Tool-calling Zero-shot
Tuy nhiên, nếu xét về độ chính xác zero-shot ở các bài toán gọi công cụ phức tạp (Berkeley Function Calling Leaderboard - BFCL v4):
- Jev vẫn dẫn đầu với độ chính xác kinh ngạc **99.2%**.
- CLM-8B đạt **95.2%** (thua khoảng 4%).
- Ở bài kiểm tra WikiRacing, Jev hoàn thành tuyệt đối 30/30 thử thách, trong khi CLM-8B đạt 26/30.

Lý do là phương pháp huấn luyện RLCD của TypeSafe tối ưu rất sâu cho tính hiệu chuẩn xác suất ở các câu hỏi logic rẽ nhánh hẹp, trong khi contrastive learning đôi khi bị nhiễu nếu các hành động có ngữ nghĩa quá sát nhau.

## 4. Điểm ăn tiền nhất: Drop-in Replacement và Tự host hoàn toàn

Điều khiến cộng đồng kỹ sư backend phấn khích nhất không chỉ là con số 16.5ms, mà là cách nhóm phát triển đóng gói sản phẩm:

1. **Tương thích 100% API của TypeSafe:**
   CLM-8B cung cấp sẵn wrapper API hỗ trợ đủ cả 3 kiểu truy vấn chuẩn của Jev:
   - **Noul Query:** Đánh giá xác suất một phát biểu đúng hay sai.
   - **Choice Query:** Chọn 1 hành động tốt nhất trong tập khai báo kèm phân bố xác suất.
   - **Score Query:** Chấm điểm thứ tự ưu tiên.
   Điều này có nghĩa là nếu bạn đã viết code kết nối với TypeSafe AI, bạn chỉ cần đổi biến môi trường `BASE_URL` trỏ về server nội bộ của mình là xong. Không cần sửa một dòng code nghiệp vụ nào.

2. **Mã nguồn mở Apache 2.0:**
   Trọng số mô hình được đẩy công khai lên Hugging Face (`Contrastive-LM/CLM-v0.1-8B`). 
   Với kích thước 8 tỷ tham số, một chiếc card đồ họa phổ thông như RTX 4090 hoặc một instance A10G trên cloud là dư sức chạy mô hình này ở mức tải hàng nghìn request mỗi giây.

## Góc nhìn của một kỹ sư backend

Khi bài toán System One được giải quyết bằng mã nguồn mở với độ trễ 16.5ms, rào cản ứng dụng AI vào phần mềm nghiệp vụ coi như đã bị san phẳng hoàn toàn:
- **Bảo mật dữ liệu nội bộ:** Làm việc ở trường đại học hay các cơ quan nhà nước, nỗi sợ lớn nhất khi cắm AI vào hệ thống là việc gửi dữ liệu sinh viên hay thông tin nội bộ lên API cloud của bên thứ ba. Với CLM-8B, mình có thể dựng một container Docker chạy trên server trường, dữ liệu không bao giờ rời khỏi mạng LAN.
- **Độ trễ ngang ngửa SQL Query:** 16.5ms là con số thần kỳ. Một câu query SQL phức tạp có JOIN vài bảng đôi khi cũng mất từ 10 đến 20ms. Khi AI phản hồi ở cùng ngưỡng thời gian đó, chúng ta có thể tự tin đặt nó vào middleware kiểm tra mọi request HTTP gửi vào backend mà không sợ làm chậm trải nghiệm của người dùng.

Jev đã chứng minh triết lý "AI không cần sinh chữ" là một hướng đi đúng đắn. Còn CLM-8B đã làm được điều tuyệt vời hơn: mang triết lý đó trao lại cho cộng đồng mã nguồn mở, biến nó thành một công cụ siêu nhanh, miễn phí và nằm trọn vẹn trong tay các lập trình viên.

## Nguồn tham khảo

- MarkTechPost, [Contrastive-LM Releases CLM-8B: An Open System One Model That Scores Agent Actions Up to 9x Faster Than Jev](https://www.marktechpost.com/2026/09/23/contrastive-lm-releases-clm-8b-an-open-system-one-model-that-scores-agent-actions-up-to-9x-faster-than-jev/) (23/09/2026)
- VentureBeat, [Stanford and Nvidia's open CLM-8B caches reusable agent actions and runs up to 9x faster than Jev in tests](https://venturebeat.com/technology/stanford-and-nvidias-open-clm-8b-caches-reusable-agent-actions-and-runs-up-to-9x-faster-than-jev-in-tests) (25/09/2026)
- CryptoBriefing, [Stanford and Nvidia's CLM-8B model runs up to 9x faster than Jev](https://cryptobriefing.com/stanford-nvidia-clm-8b-faster-than-jev/) (25/09/2026)
- Hugging Face Model Hub, [Contrastive-LM/CLM-v0.1-8B](https://huggingface.co/Contrastive-LM/CLM-v0.1-8B)
- GitHub Repository, [Contrastive-LM/CLM: TypeSafe-Compatible System One Serving](https://github.com/Contrastive-LM/CLM)

<!-- lang:en -->

Just yesterday, I wrote a deep dive on TypeSafe AI’s **Jev** — a voiceless, non-generative model that skips text generation entirely to return structured, typed decisions with sub-100ms latency. At the time, Jev's pricing ($0.042 per million tokens) and 100ms turnaround felt like the undisputed zenith of backend AI optimization.

Yet in the fast-moving AI landscape of 2026, records are minted only to be shattered within days.

On September 23, 2026, a research coalition from **Stanford University and NVIDIA Research** (the Contrastive-LM group) unveiled **CLM-8B** (Contrastive Language Model 8B). Released under a permissive Apache 2.0 license, this open-weights System One architecture is engineered for a singular mandate: **scoring candidate agent actions at blistering velocity**.

In zero-shot evaluations, CLM-8B scores decisions up to **9 times faster** than Jev, collapsing decision latency down to an astonishing **16.5 milliseconds**. When candidate pools scale up to roughly 1,000 choices, its latency advantage expands to **13 times faster**, courtesy of an elegant architectural innovation.

![CLM-8B scoring agent actions 9x faster than Jev](/images/posts/clm-8b-cham-action-nhanh-gap-9-lan-jev/cover.jpg)

## 1. Architectural Foundations: Dual-Encoder Contrastive vs. Parallel Sampler

Understanding CLM-8B’s speed leap requires examining how its internal architecture fundamentally diverges from Jev.

- **Jev (TypeSafe):** Built around a unified parallel sampler trained via Reinforcement Learning for Calibrated Decisions (RLCD). While it avoids auto-regressive next-token generation, it still routes the unified state-and-query payload through its consolidated model weights on every forward pass.
- **CLM-8B (Stanford & NVIDIA):** Abandons that paradigm in favor of a **Dual-Encoder Contrastive architecture**:
  1. It takes a pre-trained **Qwen3-8B** backbone and completely freezes its base weights.
  2. It attaches two lightweight linear projection heads: a **State Head** (encoding current environment context) and an **Action Head** (encoding candidate choices).
  3. These dual projection heads are optimized via **InfoNCE Contrastive Loss**. The objective minimizes the distance between the state and the ground-truth action in high-dimensional latent space while maximizing the margin against hard negatives.

The training regimen spanned three deliberate phases: pre-training across 60 million Nemotron question-answer pairs, mid-training over 30 million synthetic hard negatives, and post-training on 1 million verified agent trajectories.

Because scoring is decoupled into two independent encoders, evaluating an action simplifies to a lightning-fast dot-product vector calculation rather than a full feedforward pass over hundreds of combined tokens.

![System One Architecture Comparison: Jev vs. CLM-8B](/images/posts/clm-8b-cham-action-nhanh-gap-9-lan-jev/diagram-clm-vs-jev.jpg)

## 2. The Architectural Edge: Two-Sided Caching

While the dual-encoder layout naturally accelerates inference, the secret behind the 16.5ms milestone is **Two-Sided Caching**.

Consider an autonomous browser agent or an automated university ticket routing pipeline:
- The permissible action schema is typically static: `[Route_To_Registrar, Send_Confirmation_Email, Invalidate_Submission, Escalate_To_Manual_Review]`.
- Under conventional setups, every incoming user request forces the model to re-encode the verbose descriptions of all candidate actions from scratch.
- With CLM-8B, because the Action Head operates independently of incoming state context, engineers can **pre-compute and permanently cache action embeddings directly in GPU VRAM**!

When a real-time request hits the server:
1. CLM-8B requires only single-digit milliseconds for the State Head to encode the request payload into a single embedding vector.
2. It then performs a matrix multiplication against the pre-cached action embeddings in memory.

This explains why CLM-8B runs **13 times faster** than Jev when candidate sets swell to 1,000 possibilities (such as selecting subsequent destination links in WikiRacing benchmarks). While Jev suffers from context bloat and linear token expansion, CLM-8B merely computes an additional matrix dot product against pre-allocated memory buffers.

## 3. Benchmark Realities: The Verifier Inversion

Does running 9 times faster compromise decision accuracy? The Stanford-NVIDIA technical report presents an honest, balanced picture:

### Where CLM-8B Dominates: Autonomous Verification
In agentic coding pipelines (like SWE-bench or Terminal-Bench), an expensive bottleneck is the **Verifier**: an internal System One monitor that continuously validates whether a generated patch passes unit tests or adheres to security policies.
- On **DeepSWE**, CLM-8B achieves an accuracy of **81.6%**, substantially outperforming Jev’s **71.1%**.
- On **Terminal-Bench 2.1**, CLM-8B scores **87.6%**, besting Jev’s **83.1%**.
- Most critically: across these verification workloads, CLM-8B completes evaluations **4.1 to 5.7 times faster** than Jev.

### Where Jev Retains the Lead: Complex Zero-Shot Tool-Calling
Conversely, on complex zero-shot tool selection tasks evaluated on the Berkeley Function Calling Leaderboard (BFCL v4):
- Jev maintains its supremacy with an exceptional **99.2%** accuracy rate.
- CLM-8B trails slightly at **95.2%** (a 4% gap).
- On WikiRacing, Jev successfully resolved 30 out of 30 navigation challenges, while CLM-8B achieved 26 out of 30.

This discrepancy stems from TypeSafe's specialized RLCD training, which yields sharper probability calibration across fine-grained semantic boundaries, whereas contrastive projections can experience slight interference when candidate actions share highly overlapping feature representations.

## 4. The Pragmatic Victory: Drop-in Compatibility and Local Self-Hosting

Beyond the raw latency benchmarks, the most compelling aspect of CLM-8B is its packaging:

1. **Drop-in TypeSafe API Compatibility:**
   The repository ships with an out-of-the-box serving adapter that natively mirrors TypeSafe’s query endpoints:
   - **Noul Query:** Calibrated boolean truth evaluation.
   - **Choice Query:** Categorical selection across discrete actions with calibrated confidence distributions.
   - **Score Query:** Ordinal ranking across weighted alternatives.
   If your backend already integrates with TypeSafe Jev, migrating requires nothing more than repointing your `BASE_URL` environment variable to your internal cluster endpoint. Zero business logic rewrites required.

2. **True Apache 2.0 Open Source:**
   Weights are freely accessible on Hugging Face (`Contrastive-LM/CLM-v0.1-8B`).
   At an 8-billion parameter footprint, a commodity consumer GPU like an RTX 4090 or a single enterprise A10G instance can comfortably host the model, serving thousands of concurrent decisions per second.

## Backend Engineer Takeaways

Democratizing System One decision models with open weights and 16.5ms latency removes the final architectural barrier to ambient AI integration:
- **Data Sovereignty & Privacy:** In academic institutions and enterprise IT, transmitting internal student records or sensitive audit logs across third-party cloud APIs poses significant regulatory hurdles. With CLM-8B, we can deploy a dedicated container inside an isolated internal VLAN; sensitive payloads never traverse external networks.
- **SQL-Class Latency:** 16.5ms is a transformational threshold. An indexed SQL join across multiple normalized tables routinely takes between 10 and 20ms. When an AI decision engine operates within that exact latency bracket, backend engineers can fearlessly embed it directly into HTTP request middleware without degrading end-user response times.

Jev proved that non-generative, typed AI decisions are the correct architectural path for software engineering. CLM-8B achieves something even greater: it places that breakthrough into the open-source commons, offering developers an ultra-fast, cost-free, and sovereign primitive for the next generation of resilient software systems.

## References

- MarkTechPost, [Contrastive-LM Releases CLM-8B: An Open System One Model That Scores Agent Actions Up to 9x Faster Than Jev](https://www.marktechpost.com/2026/09/23/contrastive-lm-releases-clm-8b-an-open-system-one-model-that-scores-agent-actions-up-to-9x-faster-than-jev/) (Sep 23, 2026)
- VentureBeat, [Stanford and Nvidia's open CLM-8B caches reusable agent actions and runs up to 9x faster than Jev in tests](https://venturebeat.com/technology/stanford-and-nvidias-open-clm-8b-caches-reusable-agent-actions-and-runs-up-to-9x-faster-than-jev-in-tests) (Sep 25, 2026)
- CryptoBriefing, [Stanford and Nvidia's CLM-8B model runs up to 9x faster than Jev](https://cryptobriefing.com/stanford-nvidia-clm-8b-faster-than-jev/) (Sep 25, 2026)
- Hugging Face Model Hub, [Contrastive-LM/CLM-v0.1-8B](https://huggingface.co/Contrastive-LM/CLM-v0.1-8B)
- GitHub Repository, [Contrastive-LM/CLM: TypeSafe-Compatible System One Serving](https://github.com/Contrastive-LM/CLM)
