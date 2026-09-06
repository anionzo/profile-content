---
title: "Trí tuệ nhân tạo tổng quát: tên gọi, và hướng đi qua GPT-6 Astra"
titleEn: "Artificial General Intelligence: the name, and the path through GPT-6 Astra"
slug: tri-tue-nhan-tao-tong-quat
date: 2026-09-06
updated: 2026-09-06
tags: [ai, agi]
cover: /images/posts/tri-tue-nhan-tao-tong-quat/cover.jpg
excerpt: "Trí tuệ nhân tạo tổng quát không có một định nghĩa chung — OpenAI, DeepMind, và cả hợp đồng Microsoft từng đo nó theo cách khác nhau. Bài này đọc GPT-6 Astra như một điểm trên con đường đó, không phải bằng chứng đã tới đích."
excerptEn: "Artificial general intelligence has no single agreed definition — OpenAI, DeepMind, and even the Microsoft contract have each measured it differently. This post reads GPT-6 Astra as one point on that road, not proof of arrival."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

[Bài trước](/blog/gpt-6-astra) mình viết về GPT-6 Astra ngay hôm nó công bố, đọc thông báo và safety card, chưa kịp hỏi câu đứng sau tất cả: cái đích Astra được quảng cáo là đang tiến tới — trí tuệ nhân tạo tổng quát (artificial general intelligence, viết tắt AGI) — nghĩa là gì, và LLM có phải con đường duy nhất tới đó không. Bài này viết tiếp từ chỗ đó.

Chữ đáng chú ý trong cụm này không phải "trí tuệ nhân tạo", cái đó ai cũng biết. Chữ đáng chú ý là "tổng quát". AI mình dùng mỗi ngày — Claude Code viết migration, Codex đọc log IIS, một model chấm điểm rèn luyện — đều là AI hẹp: giỏi một việc được huấn luyện riêng, muốn ghép chúng thành quy trình vẫn cần tay người. AGI, theo cách hiểu ban đầu, là hệ thống không cần ghép tay như vậy nữa: học và làm được nhiều loại việc khác nhau như một người, chuyển từ việc này sang việc khác mà không cần huấn luyện lại riêng cho từng việc.

## AGI nghĩa là gì — và ai đang định nghĩa nó

Vấn đề là không ai dùng chung một định nghĩa. OpenAI Charter, viết từ 2018, định nghĩa AGI là "hệ thống tự trị cao, vượt con người ở phần lớn công việc có giá trị kinh tế". Google DeepMind năm 2024 đưa ra khung khác hẳn: sáu bậc năng lực, từ "chưa có AI" tới "siêu việt", đo trên hai trục rộng/hẹp cộng một trục tự trị riêng — theo khung này, một hệ thống có thể "tổng quát" mà chưa "siêu việt", hoặc ngược lại.

Chỗ định nghĩa va thẳng vào tiền là điều khoản AGI trong hợp đồng OpenAI–Microsoft. Bản 2019 quy định: nếu ban giám đốc OpenAI tự tuyên bố đã đạt AGI, quyền truy cập công nghệ của Microsoft bị giới hạn lại. Điều khoản đó đổi ba lần trong sáu năm — định nghĩa theo năng lực (2018), rồi một ngưỡng tài chính cụ thể (cuối 2024), rồi một thủ tục có ngày hết hạn (10/2025), rồi bị gỡ hẳn hồi tháng 4/2026, thay bằng việc để một hội đồng chuyên gia độc lập xác nhận tuyên bố của OpenAI thay vì để OpenAI tự phán. Một cụm từ kỹ thuật bị kéo qua kéo lại theo áp lực kinh doanh, không phải theo một cột mốc đo được.

Vì vậy khi Jensen Huang nói Nvidia "đã đạt AGI" hồi tháng 3, phần lớn giới nghiên cứu không đồng ý — không hẳn vì họ biết rõ hơn, mà vì ông đang dùng định nghĩa khác. Câu hỏi đúng không phải "AGI đã tới chưa", mà là "AGI theo nghĩa của ai".

> [!NOTE]
> Ba người đứng đầu ba phòng lab lớn cũng không chung mốc thời gian: Sam Altman nói "vài năm nữa", Dario Amodei mô tả 2027 như năm có thể xuất hiện "một quốc gia thiên tài trong trung tâm dữ liệu", Demis Hassabis nghiêng về khoảng 2029–2030. Không ai trong ba người đứng sau con số của người kia.

## Vì sao LLM thành con đường chính

Hai năm trước, cuộc đua còn xoay quanh việc scale: thêm tham số, thêm dữ liệu, model to hơn trả lời đúng hơn. Từ cuối 2024 sang 2025, trọng tâm chuyển sang reasoning — model tự sinh một chuỗi suy nghĩ dài trước khi trả lời — rồi sang agent: model gọi công cụ, tự chạy nhiều bước, tự sửa khi sai. Claude Code và Codex mình dùng mỗi ngày là sản phẩm của bước chuyển đó, không phải của việc model "thông minh hơn" theo nghĩa chung chung.

Bước kế, đang diễn ra ngay lúc này, là computer use: model tự thao tác máy tính như người dùng thật thay vì chỉ trả lời qua một API có khuôn sẵn. Đây là hướng LLM đang đi để tiến gần định nghĩa "tổng quát" hơn — không hẳn vì model hiểu thế giới sâu hơn, mà vì phạm vi việc nó chạm tay vào rộng ra.

## GPT-6 Astra nằm ở đâu trên hướng đó

Astra là ví dụ rõ nhất hiện có. OpenAI xếp nó là model đầu tiên ở mức rủi ro an ninh mạng Critical, và Chủ tịch Greg Brockman kết một buổi giới thiệu bằng câu "chào mừng tới kỷ nguyên AGI" — phát ngôn quảng bá, không phải kết quả đo. Trên Terminal-Bench Science, OpenAI báo Astra đạt 64.6%, so với 52.6% của Claude Fable 5.1 và 22.4% của chính GPT-5.6 Sol, bản tiền nhiệm của họ. Trên ARC-AGI-2 — benchmark suy luận trừu tượng khó nhất đang công khai — các trang tổng hợp xếp Astra khoảng 95%, Sol khoảng 92.5%, Claude Opus 5 khoảng 90.4%, trong khi người trung bình chỉ đạt 66% dù một nhóm người vẫn giải được 100% nếu đủ thời gian.

Những con số đó đáng đọc như tín hiệu hướng đi, không phải bằng chứng đã tới đích. Giá, ngữ cảnh và safety card của Astra mình đã nói ở bài trước, ở đây chỉ nhắc phần liên quan: năng lực computer use tăng đi cùng rủi ro tăng, và OpenAI tự đo an toàn bằng bài kiểm tra do chính họ đặt ra, nên số đẹp cỡ nào cũng nên chờ ai đó ngoài họ xác nhận lại.

## Cái còn thiếu để gọi đúng tên

Phản biện có nguồn rõ nhất mình thấy đến từ hai người đối lập nhau về giải pháp nhưng giống nhau ở kết luận. Gary Marcus nói thẳng: AGI "chắc chắn chưa tới, theo đúng những định nghĩa tôi đã nhiều lần đặt ra" — và đặt cược công khai tỷ lệ 10 ăn 1 rằng AI sẽ không làm được bộ việc ông chỉ định trước cuối 2027. Yann LeCun đi xa hơn về kiến trúc: ông cho rằng bản thân transformer, dù scale bao nhiêu, cũng không dẫn tới AGI, vì LLM không có mô hình thế giới để dự đoán và lập kế hoạch như một người thật. Ông rời Meta cuối 2025 và gọi được 1.03 tỷ đô cho AMI Labs hồi tháng 3/2026 để theo hướng world model thay vì LLM — khoản cược tổ chức lớn nhất từ trước tới giờ rằng LLM không phải con đường đúng.

Gộp từ nhiều nguồn, danh sách chỗ thiếu gồm: học liên tục sau khi triển khai, vì model hiện tại đóng băng sau huấn luyện; trí nhớ dài hạn thật sự qua nhiều phiên; khả năng tự đặt mục tiêu thay vì chỉ nhận lệnh; độ tin cậy đủ để giao việc không giám sát; hiểu thế giới vật lý ngoài văn bản; và chi phí năng lượng cho quy mô "tổng quát". Một benchmark khác của OpenAI, GDPval, đo việc kinh tế thật, và model tốt nhất đang tiệm cận chất lượng chuyên gia ở nhiều đầu việc, nhanh và rẻ hơn cả trăm lần — nhưng đó vẫn là làm tốt một việc cụ thể được giao, đúng cái AGI được kỳ vọng phải vượt qua.

## Mốc thời gian: dao động lớn, đừng tin số tròn

Khảo sát chuyên gia diện rộng gần nhất cho trung vị năm 2047 cho mốc "trí tuệ máy mức cao" — rút ngắn so với 2050 của khảo sát trước, nhưng vẫn xa hơn nhiều so với phát biểu của các CEO. Với một thước đo hẹp hơn — model đạt 80% thành công trên việc phần mềm cần từ 8 giờ chuyên gia trở lên — nhóm superforecaster cho trung vị 2028, chuyên gia cho 2030, công chúng cho 2037. Ba nhóm, ba con số, cùng một câu hỏi. Toàn bộ đây là dự đoán, không phải sự kiện đã xảy ra, và người viết bài này không có cơ sở để chọn phe.

## Kết

Làm phần mềm trường mỗi ngày, mình không cần biết Astra có phải AGI hay không để quyết định dùng nó cho việc gì. Hữu ích hơn là theo dõi hướng đi — scaling, rồi reasoning, rồi agent, giờ tới computer use — vì hướng đó cho biết công cụ tuần sau làm được gì, còn cái nhãn thì để dành cho luật sư viết hợp đồng và các CEO lên sân khấu.

## Nguồn

- OpenAI, [OpenAI Charter](https://openai.com/charter/)
- Google DeepMind, [Levels of AGI for Operationalizing Progress on the Path to AGI](https://arxiv.org/pdf/2311.02462)
- Simon Willison, [Tracking the history of the now-deceased OpenAI Microsoft AGI clause](https://simonwillison.net/2026/Apr/27/now-deceased-agi-clause/)
- TechRadar, [Microsoft says once AGI is declared by OpenAI it will be verified by independent experts](https://www.techradar.com/ai-platforms-assistants/chatgpt/microsoft-says-once-agi-is-declared-by-openai-it-will-be-verified-by-independent-experts-heres-why-thats-a-big-deal)
- Fortune, [Nvidia's Jensen Huang says 'we've achieved AGI.' But no one can agree on what that means](https://fortune.com/2026/03/30/agi-definition-jensen-huang-lex-fridman-deepmind-turing-text-cognitive-taxonomy/)
- Bloomberg, [What Is AGI? OpenAI, Anthropic Race for Artificial General Intelligence](https://www.bloomberg.com/news/features/2026-09-04/what-is-agi-openai-anthropic-race-for-artificial-general-intelligence)
- Axios, [Google DeepMind CEO Demis Hassabis says we're close to AGI](https://www.axios.com/2026/05/26/deepmind-ceo-demis-hassabis)
- ScienceBlog, [Dario Amodei's thought experiment for 2027: a "country of geniuses" in a data centre](https://scienceblog.com/t-amodei-country-geniuses-2027-50-million/)
- Yahoo Finance, [OpenAI Declares "AGI Era" With Astra Launch](https://finance.yahoo.com/technology/ai/articles/openai-declares-agi-era-astra-041130711.html)
- BenchLM.ai, [ARC-AGI-2 Leaderboard (September 2026)](https://benchlm.ai/benchmarks/arc-agi-2)
- ARC Prize Foundation, [Results](https://arcprize.org/results)
- OpenAI, [GDPval: measuring the performance of our models on real-world tasks](https://openai.com/index/gdpval/)
- AI Dynamics, [Gary Marcus: AGI is not here according to established definitions](https://artificialintelligencedynamics.com/2026/05/24/gary-marcus-agi-is-not-here-according-to-established-definitions/)
- HPCwire / AIwire, [Yann LeCun's AMI Secures $1B Seed to Develop AI World Models](https://www.hpcwire.com/aiwire/2026/03/11/yann-lecuns-ami-secures-1b-seed-to-develop-ai-world-models/)
- Forecasting Research Institute, [Experts and Superforecasters Update Their AI Timelines](https://forecastingresearch.substack.com/p/leap-wave-8-ai-timelines)
- FutureSearch, [AGI Timeline Predictions: How Top Forecasters Updated, 2023 to 2026](https://futuresearch.ai/blog/agi-timeline-tracker/)

<!-- lang:en -->

[Last time](/blog/gpt-6-astra) I wrote about GPT-6 Astra the day it was announced, reading through the announcement and the safety card, without asking the question behind all of it: what the destination Astra is being marketed toward — artificial general intelligence (AGI) — actually means, and whether LLMs are the only road there. This post picks up from there.

The word worth noticing in that phrase is not "artificial intelligence," everyone already knows that part. It is "general." The AI I use every day — Claude Code writing a migration, Codex reading an IIS log, a model scoring conduct points — is narrow AI: good at one trained task, still needing a human hand to chain them into a workflow. AGI, in its original sense, is a system that no longer needs that hand-chaining: it learns and performs many different kinds of work the way a person does, moving from one task to another without being retrained separately for each.

## What AGI means — and who gets to define it

The problem is nobody uses the same definition. OpenAI's Charter, written in 2018, defines AGI as "highly autonomous systems that outperform humans at most economically valuable work." Google DeepMind's 2024 framework is entirely different: six performance tiers, from "no AI" to "superhuman," measured across a breadth/narrowness axis plus a separate autonomy axis — under this framework, a system can be "general" without being "superhuman," or the reverse.

The place definition collides directly with money is the AGI clause in the OpenAI-Microsoft contract. The 2019 deal specified that if OpenAI's board declared AGI achieved, Microsoft's access to the technology would be restricted. That clause changed three times in six years — a capability definition (2018), then a specific financial threshold (late 2024), then a procedure with a sunset date (October 2025), then removed entirely in April 2026 and replaced with an independent expert panel verifying OpenAI's declaration instead of OpenAI ruling on itself. A technical term got pulled back and forth by business pressure, not by any measurable milestone.

So when Jensen Huang said in March that Nvidia had "achieved AGI," most researchers disagreed — not necessarily because they know better, but because he was using a different definition. The right question is not "has AGI arrived," but "AGI by whose definition."

> [!NOTE]
> The heads of three major labs don't share a timeline either: Sam Altman says "a few years out," Dario Amodei describes 2027 as the year a "country of geniuses in a datacenter" could appear, Demis Hassabis leans toward roughly 2029–2030. None of the three is standing behind the other's number.

## Why LLMs became the main road

Two years ago the race still ran on scale: more parameters, more data, bigger models answering more correctly. From late 2024 into 2025, the focus shifted to reasoning — models generating a long chain of thought before answering — then to agents: models calling tools, running multiple steps on their own, correcting themselves when wrong. Claude Code and Codex, which I use daily, are products of that shift, not of models becoming "smarter" in some general sense.

The next step, happening right now, is computer use: models operating a computer the way a real user would, instead of only answering through a pre-shaped API call. This is the direction LLMs are moving to edge closer to the "general" part of the definition — not necessarily because the model understands the world more deeply, but because the range of work it can touch keeps widening.

## Where GPT-6 Astra sits on that road

Astra is the clearest example available right now. OpenAI rated it the first model at Critical cybersecurity risk, and President Greg Brockman closed a briefing with "welcome to the AGI era" — a promotional line, not a measured result. On Terminal-Bench Science, OpenAI reports Astra at 64.6%, against 52.6% for Claude Fable 5.1 and 22.4% for OpenAI's own prior model, GPT-5.6 Sol. On ARC-AGI-2 — the hardest publicly available abstract-reasoning benchmark — roundup sites place Astra around 95%, Sol around 92.5%, Claude Opus 5 around 90.4%, while the average person scores only 66%, though a human panel still solves 100% given enough time.

Those numbers are worth reading as signals about direction, not proof of arrival. I covered Astra's pricing, context window, and safety card in the last post; here the relevant part is just this: computer-use capability rising comes paired with rising risk, and OpenAI measures its own safety against a test it designed itself, so however clean the numbers look, they're worth waiting on independent confirmation for.

## What's still missing to earn the name

The strongest sourced pushback I found comes from two people who disagree on the fix but agree on the conclusion. Gary Marcus states plainly that AGI "is certainly not here by the definitions I have repeatedly laid out," and has a public 10-to-1 bet that AI will not accomplish his specified tasks by the end of 2027. Yann LeCun goes further on architecture: he argues the transformer itself, no matter how much it's scaled, does not lead to AGI, because LLMs lack a world model to predict and plan the way a person does. He left Meta at the end of 2025 and raised $1.03 billion for AMI Labs in March 2026 to pursue world models instead of LLMs — the largest institutional bet yet that LLMs are not the right path.

Pulled together from multiple sources, the list of gaps includes: continual learning after deployment, since current models freeze after training; genuine long-term memory across sessions; the ability to set its own goals rather than only take instructions; reliability high enough to hand off unsupervised work; understanding of the physical world beyond text; and the energy cost of "general" scale. Another OpenAI benchmark, GDPval, measures real economic work, and the best models are approaching expert-level quality on many tasks, a hundred times faster and cheaper — but that is still doing one assigned task well, exactly what AGI is expected to go beyond.

## Timelines: wide spread, don't trust the round numbers

The most recent large expert survey puts the median at 2047 for "high-level machine intelligence" — shorter than the 2050 median from the prior survey, but still far past what CEOs say publicly. On a narrower measure — a model hitting 80% success on software tasks requiring 8 or more hours of expert effort — superforecasters put the median at 2028, experts at 2030, the public at 2037. Three groups, three numbers, one question. All of this is prediction, not something that has already happened, and I have no basis here to pick a side.

## Closing

Maintaining school software day to day, I don't need to know whether Astra is AGI to decide what to use it for. More useful is tracking the direction — scaling, then reasoning, then agents, now computer use — because that direction tells me what next week's tool can do, while the label stays reserved for contract lawyers and CEOs on stage.

## Sources

- OpenAI, [OpenAI Charter](https://openai.com/charter/)
- Google DeepMind, [Levels of AGI for Operationalizing Progress on the Path to AGI](https://arxiv.org/pdf/2311.02462)
- Simon Willison, [Tracking the history of the now-deceased OpenAI Microsoft AGI clause](https://simonwillison.net/2026/Apr/27/now-deceased-agi-clause/)
- TechRadar, [Microsoft says once AGI is declared by OpenAI it will be verified by independent experts](https://www.techradar.com/ai-platforms-assistants/chatgpt/microsoft-says-once-agi-is-declared-by-openai-it-will-be-verified-by-independent-experts-heres-why-thats-a-big-deal)
- Fortune, [Nvidia's Jensen Huang says 'we've achieved AGI.' But no one can agree on what that means](https://fortune.com/2026/03/30/agi-definition-jensen-huang-lex-fridman-deepmind-turing-text-cognitive-taxonomy/)
- Bloomberg, [What Is AGI? OpenAI, Anthropic Race for Artificial General Intelligence](https://www.bloomberg.com/news/features/2026-09-04/what-is-agi-openai-anthropic-race-for-artificial-general-intelligence)
- Axios, [Google DeepMind CEO Demis Hassabis says we're close to AGI](https://www.axios.com/2026/05/26/deepmind-ceo-demis-hassabis)
- ScienceBlog, [Dario Amodei's thought experiment for 2027: a "country of geniuses" in a data centre](https://scienceblog.com/t-amodei-country-geniuses-2027-50-million/)
- Yahoo Finance, [OpenAI Declares "AGI Era" With Astra Launch](https://finance.yahoo.com/technology/ai/articles/openai-declares-agi-era-astra-041130711.html)
- BenchLM.ai, [ARC-AGI-2 Leaderboard (September 2026)](https://benchlm.ai/benchmarks/arc-agi-2)
- ARC Prize Foundation, [Results](https://arcprize.org/results)
- OpenAI, [GDPval: measuring the performance of our models on real-world tasks](https://openai.com/index/gdpval/)
- AI Dynamics, [Gary Marcus: AGI is not here according to established definitions](https://artificialintelligencedynamics.com/2026/05/24/gary-marcus-agi-is-not-here-according-to-established-definitions/)
- HPCwire / AIwire, [Yann LeCun's AMI Secures $1B Seed to Develop AI World Models](https://www.hpcwire.com/aiwire/2026/03/11/yann-lecuns-ami-secures-1b-seed-to-develop-ai-world-models/)
- Forecasting Research Institute, [Experts and Superforecasters Update Their AI Timelines](https://forecastingresearch.substack.com/p/leap-wave-8-ai-timelines)
- FutureSearch, [AGI Timeline Predictions: How Top Forecasters Updated, 2023 to 2026](https://futuresearch.ai/blog/agi-timeline-tracker/)
