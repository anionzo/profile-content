---
title: "Model mở đủ dùng cho việc gì trong phần mềm trường"
titleEn: "What open models are good enough for in school software"
slug: model-mo-cho-phan-mem-truong
date: 2026-08-30
updated: 2026-08-30
tags: [ai, open-weight]
cover: /images/posts/model-mo-cho-phan-mem-truong/cover.jpg
excerpt: "DeepSeek V4, MiMo-V2.5, Nemotron 3.5, Muse Glimmer: ba tháng qua model mở ra dồn dập, phần lớn từ Trung Quốc. Ghi chú về việc nào giao được, và vì sao mình không đẩy code của trường qua endpoint miễn phí."
excerptEn: "DeepSeek V4, MiMo-V2.5, Nemotron 3.5, Muse Glimmer: open models landed in a rush these three months, most of them Chinese. Notes on which jobs I hand over, and why the school's code does not go through a free endpoint."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Phần mềm trường có một ràng buộc mà người làm SaaS ít gặp: dữ liệu sinh viên không được rời khỏi máy trường nếu chưa ai ký. Ba tháng nay model mở ra liên tục, phần lớn là model Trung Quốc, và chất lượng thì đủ để mình phải ngồi nghĩ lại. Nhưng câu hỏi của mình không dừng ở "model mở có giỏi không". Nó là "việc nào giao được, và chạy nó ở đâu".

Chỗ chạy quan trọng ngang chất lượng. Cùng một model có thể gọi qua ba đường: endpoint miễn phí không cần API key, API trả tiền có điều khoản, hoặc tự chạy trên máy mình. Mình dùng đường thứ hai cho việc thật, và để mắt tới đường thứ ba.

## Bốn cái tên, số liệu theo nguồn

DeepSeek V4 ra bản preview ngày 24/4/2026. Hai cỡ: V4 Pro 1.6 nghìn tỷ tham số tổng, 49 tỷ hoạt động; V4 Flash 284 tỷ tổng, 13 tỷ hoạt động. Ngữ cảnh 1 triệu token, có chế độ nghĩ và không nghĩ, giấy phép MIT. Theo Artificial Analysis, giá API Flash là 0.14 đô vào và 0.28 đô ra mỗi triệu token, Pro là 1.74 và 3.48. Ngày 31/7 DeepSeek mở trọng số bản Flash 0731, Artificial Analysis chấm 50 điểm Intelligence Index, lọt top 3 model mở.

MiMo-V2.5 của Xiaomi ra ngày 28/4/2026, MIT. Bản thường 310 tỷ tổng, 15 tỷ hoạt động, đa phương thức. Bản Pro 1.02 nghìn tỷ tổng, 42 tỷ hoạt động, tối ưu cho agent và coding. Cả hai ngữ cảnh 1 triệu token. Số tham số mình lấy theo bài của GSMArena, vì trang Xiaomi không ghi.

Nemotron của NVIDIA có dòng 3 với ba cỡ: Nano 31.6 tỷ ra tháng 12/2025, Super 120 tỷ tháng 3/2026, Ultra 550 tỷ ngày 4/6/2026, giấy phép OpenMDW-1.1 của Linux Foundation. Ngày 11/8 ra Nemotron 3.5 Lightning: MoE 31.6 tỷ tổng, 3.6 tỷ hoạt động, chạy trên một GPU. Theo bài của Akash, trọng số BF16 cần khoảng 60GB, bản NVFP4 vừa một RTX 5090.

Muse Glimmer của Meta ra ngày 10/8/2026, 30 tỷ tham số, Apache 2.0. Theo blog Meta, model chưa tới 20GB, vừa card 24GB kể cả bộ nhớ làm việc, nhắm vào agent chạy cục bộ và coding. Đây là bản mở đầu tiên của Meta sau khi họ thay Llama bằng Muse Spark đóng hồi tháng 4.

Điểm chung dễ thấy: hai cái mạnh nhất trong danh sách đều là model Trung Quốc, và cả hai đều MIT. Với mình đó là tin tốt, vì MIT nghĩa là nếu sau này trường có máy, mình tải trọng số về chạy trong nhà mà không phải xin phép ai.

## Vì sao mình không dùng endpoint miễn phí

Mấy tháng nay chỗ nào cũng có model miễn phí. OpenCode Zen, theo trang tài liệu của họ, đang để MiMo-V2.5, Nemotron 3 Ultra, Nemotron 3.5 Lightning, Muse Spark 1.2 Contributor và vài cái khác ở mức không mất tiền, kèm chữ "trong thời gian giới hạn". Cài xong là gọi được, không cần API key. Rất dễ để tiện tay dùng nó cho mọi thứ.

Mình không làm vậy, và lý do không phải chất lượng.

Cái rời khỏi máy mình khi gọi một model coding không phải là câu hỏi. Nó là file. Là tên bảng, tên cột, chuỗi kết nối lỡ nằm trong appsettings, logic tính điểm rèn luyện, cách hệ thống thi khóa bài khi hết giờ. Đó là source của phần mềm trường, và trường trả tiền để có nó. Với một endpoint trả tiền, mình còn có trang điều khoản để đọc và để đưa cho người ký. Với một endpoint miễn phí đang chạy "trong thời gian giới hạn", mình không có gì để đọc, cũng không có gì để đưa.

Nên quy tắc của mình gọn: dùng model mở thì được, kể cả model Trung Quốc, nhưng gọi qua đường có hóa đơn và có điều khoản. Miễn phí mình chỉ để thử trên repo nháp, loại mà xóa đi cũng không ai mất gì.

## Việc giao được

Mình gọi DeepSeek V4 Flash và MiMo qua API trả tiền, trong OpenCode, để giữ nguyên bộ tool và AGENTS.md quen thuộc. Giá Flash 0.14 và 0.28 đô mỗi triệu token nghĩa là cả tháng làm mấy việc dưới đây tốn chưa tới một bữa cà phê.

Sinh test. Mình đưa một service tính điểm rèn luyện, chỉ có code, không có dữ liệu thật. DeepSeek V4 Flash viết xUnit đủ case, có hai case biên thừa. Với việc này, model mở và model đầu bảng khác nhau ở độ dài câu trả lời hơn là chất lượng test.

Viết SQL nháp từ mô tả. "Danh sách sinh viên theo khoa, học kỳ, có phân trang, sort được theo tên". Ra được câu SQL có OFFSET FETCH đúng cú pháp SQL Server. Schema mình đưa là schema, không có dòng dữ liệu nào.

Đọc log lỗi. Dán stack trace .NET đã xóa tên người, hỏi nguyên nhân. Cả DeepSeek V4 Flash lẫn Nemotron 3.5 Lightning đều chỉ đúng chỗ null. Việc này không cần model lớn.

Dịch thông báo. Chuyển thông báo lỗi tiếng Anh ngắn sang câu tiếng Việt cụ thể cho phòng máy. Cái này MiMo làm ổn, và mình có thể đọc lại từng câu trước khi dùng.

Viết tài liệu nội bộ từ code. Mô tả một endpoint từ controller và DTO. Model mở đủ, chỉ cần nhắc nó đừng bịa field không có.

## Việc chưa giao

Migration EF Core trên schema thật. Không phải vì model không viết được, mình thử với schema giả và nó viết đúng. Mà vì schema thật là thứ mình muốn giữ trong phạm vi đã có người ký, và tới giờ chưa ai ký cho việc đó.

Refactor xuyên module. Ngữ cảnh 1 triệu token nghe rộng, nhưng đưa cả hệ thống đào tạo vào một lượt thì vừa tốn tiền vừa cho ra một diff mình không đọc hết nổi. Mình cắt theo module, mỗi lượt một cái, và tự đọc lại.

Chạy agent dài không người canh. Model mở làm việc lặp thì ổn, nhưng việc chạy hai tiếng mà mình không ngồi nhìn thì chưa. Không phải vì nó hỏng, mà vì lúc nó đi lệch mình muốn có mặt.

## Chạy tại chỗ thì sao

Muse Glimmer vừa card 24GB và Nemotron 3.5 Lightning bản 4-bit vừa RTX 5090 là hai cái mở ra một lối khác: máy trong phòng kỹ thuật, dữ liệu và source đều không rời mạng trường. Mình chưa có card đó để thử, nên phần này là đọc chứ chưa làm. Nếu năm sau phòng mua được một máy như vậy, đây là hai cái mình chạy đầu tiên, và lúc đó chuyện điều khoản coi như xong.

Tính đến giờ, cách mình chia không phải là mở với đóng, cũng không phải Trung Quốc với Mỹ. Nó là ai đang giữ code của mình lúc mình bấm Enter.

## Nguồn

- DeepSeek, [DeepSeek V4 Preview Release](https://api-docs.deepseek.com/news/news260424/)
- Artificial Analysis, [DeepSeek is back among the leading open weights models](https://artificialanalysis.ai/articles/deepseek-is-back-among-the-leading-open-weights-models-with-v4-pro-and-v4-flash) và [bài đăng về V4 Flash 0731](https://x.com/ArtificialAnlys/status/2083306229074739285)
- Xiaomi, [MiMo-V2.5 series open-sourced](https://mimo.mi.com/docs/en-US/news/latest/v2.5-open-sourced)
- GSMArena, [Xiaomi releases open-weight MiMo-V2.5](https://www.gsmarena.com/xiaomi_releases_openweight_mimov25_ai_model_claims_frontierlevel_agentic_capability-news-72585.php)
- NVIDIA, [NVIDIA Debuts Nemotron 3 Family of Open Models](https://nvidianews.nvidia.com/news/nvidia-debuts-nemotron-3-family-of-open-models)
- CNBC, [Nvidia releases Nemotron 3.5 Lightning](https://www.cnbc.com/2026/08/11/nvidia-releases-nemotron-3point5-lightning-open-source-ai-model-.html)
- Akash, [Run Nemotron 3.5 Lightning on one GPU](https://akash.network/bits/run-nvidia-nemotron-3-5-lightning-on-one-gpu-vllm-setup-for-h100-a100-on-akash/)
- Meta, [Introducing Muse Glimmer](https://research.meta.ai/blog/introducing-muse-glimmer-open-agentic-model)
- OpenCode, [Zen docs](https://open-code.ai/en/docs/zen)

<!-- lang:en -->

School software has a constraint SaaS people rarely meet: student data does not leave the school's machines until someone signs for it. Open models have been landing non-stop for three months, most of them Chinese, and they are good enough that I had to sit down and rethink. But my question does not stop at "are open models good". It is "which jobs can go to one, and where does it run".

Where it runs matters as much as how good it is. The same model can be reached three ways: a free endpoint with no API key, a paid API with terms, or a box of my own. I use the second for real work, and keep an eye on the third.

## Four names, numbers per source

DeepSeek V4 shipped as a preview on April 24, 2026. Two sizes: V4 Pro at 1.6 trillion total parameters, 49 billion active; V4 Flash at 284 billion total, 13 billion active. One million token context, thinking and non-thinking modes, MIT license. Per Artificial Analysis, Flash API pricing is 0.14 dollars in and 0.28 out per million tokens, Pro is 1.74 and 3.48. On July 31 DeepSeek released the weights for Flash 0731, which Artificial Analysis scored at 50 on its Intelligence Index, top 3 among open models.

Xiaomi's MiMo-V2.5 shipped on April 28, 2026, MIT. The base model is 310 billion total, 15 billion active, multimodal. The Pro is 1.02 trillion total, 42 billion active, tuned for agents and coding. Both have a 1 million token context. Parameter counts are from GSMArena, since Xiaomi's page does not state them.

NVIDIA's Nemotron 3 line has three sizes: Nano 31.6 billion in December 2025, Super 120 billion in March 2026, Ultra 550 billion on June 4, 2026, under the Linux Foundation's OpenMDW-1.1 license. On August 11 came Nemotron 3.5 Lightning: a 31.6 billion total, 3.6 billion active MoE that runs on one GPU. Per an Akash write-up, BF16 weights need about 60GB, while the NVFP4 build fits an RTX 5090.

Meta's Muse Glimmer shipped on August 10, 2026, 30 billion parameters, Apache 2.0. Per Meta's blog the model is under 20GB and fits a 24GB card including working memory, aimed at local agents and coding. It is Meta's first open release since they replaced Llama with the closed Muse Spark in April.

One thing stands out: the two strongest on that list are both Chinese, and both are MIT. That is good news for me, because MIT means if the department ever buys a machine, I pull the weights down and run them in-house without asking anyone.

## Why I do not use the free endpoints

Free models are everywhere now. OpenCode Zen, per their docs, currently lists MiMo-V2.5, Nemotron 3 Ultra, Nemotron 3.5 Lightning, Muse Spark 1.2 Contributor and a few others at no cost, marked "for a limited time". Install it and you are calling them, no API key. It is very easy to reach for that for everything.

I do not, and the reason is not quality.

What leaves my machine when I call a coding model is not a question. It is files. Table names, column names, a connection string that slipped into appsettings, the conduct-score logic, the way the exam system locks a paper when time runs out. That is the source of the school's software, and the school paid to have it. With a paid endpoint I have a terms page I can read and hand to whoever signs. With a free endpoint running "for a limited time", I have nothing to read and nothing to hand over.

So my rule is short: open models are fine, Chinese ones included, but I call them through a door with an invoice and terms behind it. Free tiers I keep for scratch repos, the kind nobody loses anything over.

## Jobs I hand over

I call DeepSeek V4 Flash and MiMo through the paid API, inside OpenCode, to keep my usual tools and AGENTS.md. At 0.14 and 0.28 dollars per million tokens, a month of the work below costs less than one coffee.

Test generation. A conduct-score service, only code, no real data. DeepSeek V4 Flash wrote xUnit tests with enough cases and two redundant edge cases. For this job, open and frontier differ more in answer length than in test quality.

Draft SQL from a description. "Students by faculty and term, paginated, sortable by name". It produced OFFSET FETCH with correct SQL Server syntax. The schema I gave was the schema, not a single row of data.

Reading error logs. A .NET stack trace with names removed, asking for the cause. Both DeepSeek V4 Flash and Nemotron 3.5 Lightning pointed at the right null. This does not need a big model.

Translating messages. Turning short English errors into concrete Vietnamese for the lab. MiMo did fine, and I can re-read every line before using it.

Internal docs from code. Describing an endpoint from the controller and DTO. Open models are enough, as long as I tell it not to invent fields.

## Jobs I do not hand over yet

EF Core migrations on the real schema. Not because it cannot write them, I tried on a fake schema and it did. But the real schema is something I want kept inside what someone has signed for, and so far nobody has signed for this.

Cross-module refactors. A 1 million token context sounds wide, but feeding the whole training system in one pass costs real money and hands back a diff I cannot fully read. I cut by module, one per pass, and read it myself.

Long unattended agent runs. Open models handle repetitive work fine, but a two-hour run I am not watching is not there yet. Not because it breaks, but because I want to be around when it drifts.

## What about running locally

Muse Glimmer fitting a 24GB card and Nemotron 3.5 Lightning in 4-bit fitting an RTX 5090 open a different path: a box in the tech room, where neither the data nor the source leaves the campus network. I do not have that card yet, so this part is read, not done. If the department buys such a machine next year, these two are the first I run, and the whole terms question goes away.

For now my split is not open versus closed, and not China versus the US. It is who is holding my code at the moment I hit Enter.

## Sources

- DeepSeek, [DeepSeek V4 Preview Release](https://api-docs.deepseek.com/news/news260424/)
- Artificial Analysis, [DeepSeek is back among the leading open weights models](https://artificialanalysis.ai/articles/deepseek-is-back-among-the-leading-open-weights-models-with-v4-pro-and-v4-flash) and [post on V4 Flash 0731](https://x.com/ArtificialAnlys/status/2083306229074739285)
- Xiaomi, [MiMo-V2.5 series open-sourced](https://mimo.mi.com/docs/en-US/news/latest/v2.5-open-sourced)
- GSMArena, [Xiaomi releases open-weight MiMo-V2.5](https://www.gsmarena.com/xiaomi_releases_openweight_mimov25_ai_model_claims_frontierlevel_agentic_capability-news-72585.php)
- NVIDIA, [NVIDIA Debuts Nemotron 3 Family of Open Models](https://nvidianews.nvidia.com/news/nvidia-debuts-nemotron-3-family-of-open-models)
- CNBC, [Nvidia releases Nemotron 3.5 Lightning](https://www.cnbc.com/2026/08/11/nvidia-releases-nemotron-3point5-lightning-open-source-ai-model-.html)
- Akash, [Run Nemotron 3.5 Lightning on one GPU](https://akash.network/bits/run-nvidia-nemotron-3-5-lightning-on-one-gpu-vllm-setup-for-h100-a100-on-akash/)
- Meta, [Introducing Muse Glimmer](https://research.meta.ai/blog/introducing-muse-glimmer-open-agentic-model)
- OpenCode, [Zen docs](https://open-code.ai/en/docs/zen)
