---
title: "Model free đủ dùng cho việc gì trong phần mềm trường"
titleEn: "What free models are good enough for in school software"
slug: model-free-cho-phan-mem-truong
date: 2026-08-30
updated: 2026-08-30
tags: [ai, open-weight]
cover: /images/og-cover.jpg
excerpt: "DeepSeek V4, Nemotron 3.5, MiMo-V2.5, Muse Glimmer: ba tháng qua model mở ra dồn dập, và OpenCode cho dùng vài cái không mất tiền. Ghi chú về việc nào giao được, việc nào chưa."
excerptEn: "DeepSeek V4, Nemotron 3.5, MiMo-V2.5, Muse Glimmer: open models landed in a rush these three months, and OpenCode serves some for free. Notes on which jobs to hand them and which not yet."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Phần mềm trường có một ràng buộc mà người làm SaaS ít gặp: dữ liệu sinh viên không được rời khỏi máy trường nếu chưa ai ký. Nên câu hỏi với mình không phải "model free có giỏi không", mà là "việc gì giao cho model free được mà không phải gửi dữ liệu thật đi đâu".

Mình dùng OpenCode làm chỗ thử, vì nó có provider Zen với vài model free không cần API key. Model mặc định của mình là `opencode/deepseek-v4-flash-free`.

## Bốn cái tên, số liệu theo nguồn

DeepSeek V4 ra bản preview ngày 24/4/2026. Hai cỡ: V4 Pro 1.6 nghìn tỷ tham số tổng, 49 tỷ hoạt động; V4 Flash 284 tỷ tổng, 13 tỷ hoạt động. Ngữ cảnh 1 triệu token, có chế độ nghĩ và không nghĩ, giấy phép MIT. Theo Artificial Analysis, giá API Flash là 0.14 đô vào và 0.28 đô ra mỗi triệu token, Pro là 1.74 và 3.48. Ngày 31/7 DeepSeek mở trọng số bản Flash 0731, Artificial Analysis chấm 50 điểm Intelligence Index, lọt top 3 model mở.

Nemotron của NVIDIA có dòng 3 với ba cỡ: Nano 31.6 tỷ ra tháng 12/2025, Super 120 tỷ tháng 3/2026, Ultra 550 tỷ ngày 4/6/2026, giấy phép OpenMDW-1.1 của Linux Foundation. Ngày 11/8 ra Nemotron 3.5 Lightning: MoE 31.6 tỷ tổng, 3.6 tỷ hoạt động, chạy trên một GPU. Theo bài của Akash, trọng số BF16 cần khoảng 60GB, bản NVFP4 vừa một RTX 5090.

MiMo-V2.5 của Xiaomi ra ngày 28/4/2026, MIT. Bản thường 310 tỷ tổng, 15 tỷ hoạt động, đa phương thức. Bản Pro 1.02 nghìn tỷ tổng, 42 tỷ hoạt động, tối ưu cho agent và coding. Cả hai ngữ cảnh 1 triệu token. Số tham số mình lấy theo bài của GSMArena, vì trang Xiaomi không ghi.

Muse Glimmer của Meta ra ngày 10/8/2026, 30 tỷ tham số, Apache 2.0. Theo blog Meta, model chưa tới 20GB, vừa card 24GB kể cả bộ nhớ làm việc, nhắm vào agent chạy cục bộ và coding. Đây là bản mở đầu tiên của Meta sau khi họ thay Llama bằng Muse Spark đóng hồi tháng 4.

Trên OpenCode Zen, theo trang tài liệu, danh sách free lúc này có MiMo-V2.5, Nemotron 3 Ultra, Nemotron 3.5 Lightning, Muse Spark 1.2 Contributor, cùng vài cái khác, đều ghi là "trong thời gian giới hạn". DeepSeek V4 Flash free có trong API nhưng cuối tháng 8 có issue báo nó không hiện trong menu chọn của TUI. Mình cũng dính, phải gõ tay tên model trong config.

## Việc giao được

Sinh test. Mình đưa một service tính điểm rèn luyện, không có dữ liệu thật, chỉ có code. DeepSeek V4 Flash viết xUnit đủ case, có hai case biên thừa. Với việc này, model free và model trả tiền khác nhau ở độ dài câu trả lời hơn là chất lượng test.

Viết SQL nháp từ mô tả. "Danh sách sinh viên theo khoa, học kỳ, có phân trang, sort được theo tên". Ra được câu SQL có OFFSET FETCH đúng cú pháp SQL Server. Schema mình đưa là schema, không có dòng dữ liệu nào.

Đọc log lỗi. Dán stack trace .NET đã xóa tên người, hỏi nguyên nhân. Cả DeepSeek V4 Flash lẫn Nemotron 3.5 Lightning đều chỉ đúng chỗ null. Việc này không cần model lớn.

Dịch thông báo. Chuyển thông báo lỗi tiếng Anh ngắn sang câu tiếng Việt cụ thể cho phòng máy. Cái này MiMo làm ổn, và mình có thể đọc lại từng câu trước khi dùng.

Viết tài liệu nội bộ từ code. Mô tả một endpoint từ controller và DTO. Model free đủ, chỉ cần nhắc nó đừng bịa field không có.

## Việc chưa giao

Migration EF Core trên schema thật. Không phải vì model không viết được, mình thử với schema giả và nó viết đúng. Mà vì để nó xem schema thật thì phải gửi tên bảng, tên cột, ràng buộc lên một endpoint free mà mình không có cam kết gì về lưu trữ. Với model trả tiền mình còn có điều khoản để đọc, với free thì không.

Refactor xuyên module. Ngữ cảnh 1 triệu token nghe rộng, nhưng bản free trên Zen, theo freellm.net, giới hạn 200 nghìn. Đủ cho một module, không đủ cho cả hệ thống đào tạo.

Chạy agent dài không người canh. Ở mức free mình không có cam kết về rate limit. Có buổi nó chạy êm, có buổi bị chặn giữa chừng. Việc cần chạy hai tiếng thì mình không giao.

## Chạy tại chỗ thì sao

Muse Glimmer vừa card 24GB và Nemotron 3.5 Lightning bản 4-bit vừa RTX 5090 là hai cái mở ra một lối khác: máy trong phòng kỹ thuật, dữ liệu không rời mạng trường. Mình chưa có card đó để thử, nên phần này là đọc chứ chưa làm. Nếu năm sau phòng mua được một máy như vậy, đây là hai cái mình chạy đầu tiên.

Tính đến giờ, cách mình chia là: code không có dữ liệu thì model free làm, dữ liệu thật thì hoặc trả tiền có điều khoản, hoặc chạy trong nhà. Free không có nghĩa là không có giá, chỉ là giá không nằm trên hóa đơn.

## Nguồn

- DeepSeek, [DeepSeek V4 Preview Release](https://api-docs.deepseek.com/news/news260424/)
- Artificial Analysis, [DeepSeek is back among the leading open weights models](https://artificialanalysis.ai/articles/deepseek-is-back-among-the-leading-open-weights-models-with-v4-pro-and-v4-flash) và [bài đăng về V4 Flash 0731](https://x.com/ArtificialAnlys/status/2083306229074739285)
- NVIDIA, [NVIDIA Debuts Nemotron 3 Family of Open Models](https://nvidianews.nvidia.com/news/nvidia-debuts-nemotron-3-family-of-open-models)
- CNBC, [Nvidia releases Nemotron 3.5 Lightning](https://www.cnbc.com/2026/08/11/nvidia-releases-nemotron-3point5-lightning-open-source-ai-model-.html)
- Akash, [Run Nemotron 3.5 Lightning on one GPU](https://akash.network/bits/run-nvidia-nemotron-3-5-lightning-on-one-gpu-vllm-setup-for-h100-a100-on-akash/)
- Xiaomi, [MiMo-V2.5 series open-sourced](https://mimo.mi.com/docs/en-US/news/latest/v2.5-open-sourced)
- GSMArena, [Xiaomi releases open-weight MiMo-V2.5](https://www.gsmarena.com/xiaomi_releases_openweight_mimov25_ai_model_claims_frontierlevel_agentic_capability-news-72585.php)
- Meta, [Introducing Muse Glimmer](https://research.meta.ai/blog/introducing-muse-glimmer-open-agentic-model)
- OpenCode, [Zen docs](https://open-code.ai/en/docs/zen), [issue #43805](https://github.com/anomalyco/opencode/issues/43805), và [freellm.net về deepseek-v4-flash-free](https://freellm.net/models/opencode/deepseek-v4-flash-free)

<!-- lang:en -->

School software has a constraint SaaS people rarely meet: student data does not leave the school's machines until someone signs for it. So my question is not "are free models good", it is "which jobs can go to a free model without sending real data anywhere".

I use OpenCode as the test bench, because its Zen provider serves a few free models with no API key. My default is `opencode/deepseek-v4-flash-free`.

## Four names, numbers per source

DeepSeek V4 shipped as a preview on April 24, 2026. Two sizes: V4 Pro at 1.6 trillion total parameters, 49 billion active; V4 Flash at 284 billion total, 13 billion active. One million token context, thinking and non-thinking modes, MIT license. Per Artificial Analysis, Flash API pricing is 0.14 dollars in and 0.28 out per million tokens, Pro is 1.74 and 3.48. On July 31 DeepSeek released the weights for Flash 0731, which Artificial Analysis scored at 50 on its Intelligence Index, top 3 among open models.

NVIDIA's Nemotron 3 line has three sizes: Nano 31.6 billion in December 2025, Super 120 billion in March 2026, Ultra 550 billion on June 4, 2026, under the Linux Foundation's OpenMDW-1.1 license. On August 11 came Nemotron 3.5 Lightning: a 31.6 billion total, 3.6 billion active MoE that runs on one GPU. Per an Akash write-up, BF16 weights need about 60GB, while the NVFP4 build fits an RTX 5090.

Xiaomi's MiMo-V2.5 shipped on April 28, 2026, MIT. The base model is 310 billion total, 15 billion active, multimodal. The Pro is 1.02 trillion total, 42 billion active, tuned for agents and coding. Both have a 1 million token context. Parameter counts are from GSMArena, since Xiaomi's page does not state them.

Meta's Muse Glimmer shipped on August 10, 2026, 30 billion parameters, Apache 2.0. Per Meta's blog the model is under 20GB and fits a 24GB card including working memory, aimed at local agents and coding. It is Meta's first open release since they replaced Llama with the closed Muse Spark in April.

On OpenCode Zen, per the docs, the free list right now includes MiMo-V2.5, Nemotron 3 Ultra, Nemotron 3.5 Lightning, Muse Spark 1.2 Contributor, and a few others, all marked "for a limited time". DeepSeek V4 Flash free is in the API, but a late-August issue reports it missing from the TUI picker. I hit that too and typed the model name into config by hand.

## Jobs I hand over

Test generation. A conduct-score service, no real data, only code. DeepSeek V4 Flash wrote xUnit tests with enough cases and two redundant edge cases. For this job, free and paid differ more in answer length than in test quality.

Draft SQL from a description. "Students by faculty and term, paginated, sortable by name". It produced OFFSET FETCH with correct SQL Server syntax. The schema I gave was the schema, not a single row of data.

Reading error logs. A .NET stack trace with names removed, asking for the cause. Both DeepSeek V4 Flash and Nemotron 3.5 Lightning pointed at the right null. This does not need a big model.

Translating messages. Turning short English errors into concrete Vietnamese for the lab. MiMo did fine, and I can re-read every line before using it.

Internal docs from code. Describing an endpoint from the controller and DTO. Free is enough, as long as I tell it not to invent fields.

## Jobs I do not hand over yet

EF Core migrations on the real schema. Not because it cannot write them, I tried on a fake schema and it did. But to see the real schema it needs table names, columns, constraints sent to a free endpoint with no retention commitment I can read. With paid models there are terms; with free there are none.

Cross-module refactors. A 1 million token context sounds wide, but the free tier on Zen, per freellm.net, caps at 200 thousand. Enough for one module, not for the whole training system.

Long unattended agent runs. On free I have no rate-limit commitment. Some sessions run clean, some get cut mid-way. Anything that needs two hours does not go there.

## What about running locally

Muse Glimmer fitting a 24GB card and Nemotron 3.5 Lightning in 4-bit fitting an RTX 5090 open a different path: a box in the tech room, data never leaving the campus network. I do not have that card yet, so this part is read, not done. If the department buys such a machine next year, these two are the first I run.

For now my split is: code without data goes to free models, real data goes either to a paid model with terms or stays in-house. Free does not mean no price, only that the price is not on the invoice.

## Sources

- DeepSeek, [DeepSeek V4 Preview Release](https://api-docs.deepseek.com/news/news260424/)
- Artificial Analysis, [DeepSeek is back among the leading open weights models](https://artificialanalysis.ai/articles/deepseek-is-back-among-the-leading-open-weights-models-with-v4-pro-and-v4-flash) and [post on V4 Flash 0731](https://x.com/ArtificialAnlys/status/2083306229074739285)
- NVIDIA, [NVIDIA Debuts Nemotron 3 Family of Open Models](https://nvidianews.nvidia.com/news/nvidia-debuts-nemotron-3-family-of-open-models)
- CNBC, [Nvidia releases Nemotron 3.5 Lightning](https://www.cnbc.com/2026/08/11/nvidia-releases-nemotron-3point5-lightning-open-source-ai-model-.html)
- Akash, [Run Nemotron 3.5 Lightning on one GPU](https://akash.network/bits/run-nvidia-nemotron-3-5-lightning-on-one-gpu-vllm-setup-for-h100-a100-on-akash/)
- Xiaomi, [MiMo-V2.5 series open-sourced](https://mimo.mi.com/docs/en-US/news/latest/v2.5-open-sourced)
- GSMArena, [Xiaomi releases open-weight MiMo-V2.5](https://www.gsmarena.com/xiaomi_releases_openweight_mimov25_ai_model_claims_frontierlevel_agentic_capability-news-72585.php)
- Meta, [Introducing Muse Glimmer](https://research.meta.ai/blog/introducing-muse-glimmer-open-agentic-model)
- OpenCode, [Zen docs](https://open-code.ai/en/docs/zen), [issue #43805](https://github.com/anomalyco/opencode/issues/43805), and [freellm.net on deepseek-v4-flash-free](https://freellm.net/models/opencode/deepseek-v4-flash-free)
