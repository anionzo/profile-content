---
title: "Claude 5 và Fable 5.1: cái gì đổi với người viết code hằng ngày"
titleEn: "Claude 5 and Fable 5.1: what changes for someone who writes code every day"
slug: claude-5-va-fable-5-1
date: 2026-09-02
updated: 2026-09-02
tags: [ai, llm, claude]
cover: /images/posts/claude-5-va-fable-5-1/cover.jpg
excerpt: "Ghi chú đọc và thử sau ba tháng Anthropic ra cả dòng Claude 5: Fable, Sonnet, Opus, rồi Fable 5.1 hôm qua. Cái nào đáng bấm trong Claude Code, cái nào chỉ là số trên slide."
excerptEn: "Reading-and-trying notes after three months of Claude 5 releases: Fable, Sonnet, Opus, then Fable 5.1 yesterday. What matters inside Claude Code, what is just a slide number."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Mình dùng Claude Code cho việc lặt vặt mỗi ngày: viết migration EF Core, sinh test xUnit cho một service, đọc một đoạn log dài để tìm request nào làm SQL Server chậm. Ba tháng qua Anthropic đổi gần hết dàn model, nên mình ngồi đọc lại và thử vài thứ. Đây là ghi chú, không phải review.

## Dòng Claude 5 gồm những gì, tính đến hôm nay

Fable 5 ra ngày 9/6/2026. Anthropic gọi nó là model "hạng Mythos" đầu tiên mở cho công chúng, tức cùng lớp với Mythos vốn chỉ cấp cho đối tác qua Project Glasswing. Cách họ làm cho an toàn khá lạ: một số chủ đề nhạy cảm, câu hỏi được chuyển sang Opus 4.8 trả lời thay.

Sonnet 5 ra ngày 30/6. Giá 2 đô mỗi triệu token vào, 10 đô ra, và theo trang Anthropic thì từ 10/8 đó là giá cố định. Nó là mặc định cho gói Free và Pro. Một chi tiết dễ bỏ qua: Sonnet 5 dùng tokenizer mới, cùng một đoạn văn bản tốn khoảng 1.0 đến 1.35 lần token so với trước. Nghĩa là hóa đơn không chỉ phụ thuộc vào giá niêm yết.

Opus 5 ra ngày 24/7, giữ nguyên giá 5 và 25 đô như Opus 4.8. Nó thành mặc định trên gói Max, và bắt đầu có nút chỉnh mức effort thấp, vừa, cao.

Haiku 4.5 thì cũ hơn, từ tháng 10/2025, giá 1 và 5 đô, ngữ cảnh 200 nghìn token. Mình vẫn để nó cho việc rẻ tiền như đặt tên commit hay tóm tắt diff.

Rồi hôm qua, 1/9, Fable 5.1 ra cùng Mythos 5.1. Theo Anthropic, hai bản là cùng một model, chỉ khác lớp bảo vệ. Fable 5.1 mở cho mọi người, Mythos 5.1 chỉ dành cho chương trình truy cập tin cậy.

## Cái mình để ý ở Fable 5.1

Giá vào ra không đổi: 10 và 50 đô mỗi triệu token. Cái đổi là giá đọc cache, giảm 75% xuống còn 0.25 đô. Anthropic ước tính việc bình thường rẻ hơn khoảng 25%, việc agent chạy dài rẻ hơn tới khoảng 45%.

Con số đó đúng chỗ đau của mình. Khi cho agent lục một repo phần mềm trường, phần lớn token là đọc lại cùng những file đó vòng này qua vòng khác. Giảm giá cache là giảm đúng phần tốn nhất, không phải phần đẹp nhất.

Thứ hai là mức effort. Fable 5.1 có năm mức: Low, Medium, High, XHigh, Max. Mặc định trong Claude Code là High, trên Claude.ai là Medium. Anthropic nói ở mức thấp và vừa nó bằng hoặc hơn Fable 5, còn ở mức cao thì hơn hẳn. Mình chưa có cách đo điều đó, chỉ nói lại theo trang họ.

Thứ ba, và với mình là cái thật nhất: lớp bảo vệ an ninh mạng bớt báo nhầm. Anthropic nói người dùng Claude Code sẽ thấy trung bình ít hơn khoảng 60% lần bị chặn so với Fable 5. Ai từng bị nó dừng lại giữa chừng chỉ vì trong log có địa chỉ IP và chuỗi token phiên sẽ hiểu con số này nghĩa là gì.

Benchmark thì họ đưa Terminal-Bench 4.0 lên 55.8% từ 42.0%, CursorBench 3.2.0 lên 73.4% từ 70.5%. Mình chép lại cho đủ, còn tin bao nhiêu thì tùy bạn.

## Thử ba việc quen

Migration EF Core: mình đưa một entity mới có quan hệ với bảng sinh viên và bảo nó viết migration cộng script SQL kèm theo. Cả Sonnet 5 lẫn Fable 5.1 đều viết được, khác nhau ở chỗ Fable 5.1 tự hỏi về index cho cột lọc theo khoa và học kỳ trước khi mình nhắc. Sonnet 5 chỉ làm đúng cái mình nói.

Sinh test: cho một service tính điểm rèn luyện, yêu cầu test xUnit. Sonnet 5 đủ dùng, nhanh, rẻ. Với mình việc này không cần Fable.

Đọc log: dán một đoạn log IIS vài trăm dòng, hỏi request nào có thời gian phản hồi bất thường. Đây là chỗ Fable 5.1 khác nhất: nó nhóm theo endpoint, nhận ra một màn hình báo cáo tự refresh đang gọi lặp, và không bị lớp an toàn chặn vì trong log có IP nội bộ. Bản trước thì có lúc bị.

## Một thứ ngoài model

Claude Code từ bản 2.1.224 cho hai phiên đang mở nhắn cho nhau, không phải copy ngữ cảnh qua lại giữa hai cửa sổ terminal. Theo MacRumors thì tính năng ra cùng đợt Sonnet 5. Với mình tính năng này đổi cách làm hơn cả điểm benchmark: một phiên lo backend, một phiên lo Angular, và chúng tự thống nhất contract API.

Nói gọn: Sonnet 5 cho việc thường, Opus 5 khi cần suy nghĩ hơn nhưng ngân sách có hạn, Fable 5.1 khi để agent chạy dài trên repo lớn. Model mạnh nhất không phải model đúng nhất cho mọi tác vụ, nó chỉ là cái đắt nhất khi bạn không nhìn hóa đơn.

## Nguồn

- Anthropic, [Introducing Claude Fable 5.1 and Claude Mythos 5.1](https://www.anthropic.com/claude-fable-and-mythos-5-1)
- Anthropic, [Introducing Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5)
- Anthropic, [Introducing Claude Haiku 4.5](https://www.anthropic.com/news/claude-haiku-4-5)
- Axios, [Anthropic releases new model, Opus 5](https://www.axios.com/2026/07/24/anthropic-releases-new-model-opus-5)
- NBC News, [Anthropic releases Fable 5, the first public Mythos-class model](https://www.nbcnews.com/tech/security/fable-5-anthropic-release-public-mythos-claude-model-rcna349104)
- MacRumors, [Anthropic Launches Claude Sonnet 5](https://www.macrumors.com/2026/06/30/anthropic-claude-sonnet-5/) và [Anthropic Launches Claude Fable 5.1](https://www.macrumors.com/2026/09/01/anthropic-claude-fable-5-1/)

<!-- lang:en -->

I use Claude Code for small daily things: writing an EF Core migration, generating xUnit tests for a service, reading a long log to find which request is slowing SQL Server. Over the past three months Anthropic replaced almost its whole lineup, so I sat down, read, and tried a few things. These are notes, not a review.

## What the Claude 5 line is, as of today

Fable 5 came out on June 9, 2026. Anthropic calls it the first "Mythos-class" model open to the public, meaning the same tier as Mythos, which only partners get through Project Glasswing. The safety approach is unusual: on some sensitive topics the question gets answered by Opus 4.8 instead.

Sonnet 5 came out on June 30. Two dollars per million input tokens, ten out, and according to Anthropic's page that became the permanent price on August 10. It is the default for Free and Pro. One detail that is easy to miss: Sonnet 5 uses a new tokenizer, so the same text costs roughly 1.0 to 1.35 times the tokens it used to. The bill depends on more than the sticker.

Opus 5 came out on July 24, keeping the same 5 and 25 dollar pricing as Opus 4.8. It became the default on Max, and it started shipping an effort toggle with low, medium, and high.

Haiku 4.5 is older, from October 2025, at 1 and 5 dollars with a 200 thousand token context. I still leave it for cheap work like naming commits or summarising a diff.

Then yesterday, September 1, Fable 5.1 shipped alongside Mythos 5.1. According to Anthropic the two are the same model with different safeguard levels. Fable 5.1 is generally available; Mythos 5.1 is for trusted access programs only.

## What I noticed in Fable 5.1

Input and output prices did not move: 10 and 50 dollars per million tokens. What moved is cache read, cut 75 percent to 0.25 dollars. Anthropic estimates typical work gets about 25 percent cheaper and long agentic runs up to about 45 percent.

That number lands exactly where it hurts me. When an agent digs through a school software repo, most of the tokens are re-reading the same files round after round. Cutting cache price cuts the most expensive part, not the prettiest one.

Second is the effort dial. Fable 5.1 has five levels: Low, Medium, High, XHigh, Max. Default in Claude Code is High, on Claude.ai it is Medium. Anthropic says at low and medium it matches or beats Fable 5, and at high it pulls well ahead. I have no way to measure that, I am only repeating their page.

Third, and to me the most real: the cyber safeguards flag less. Anthropic says Claude Code users should see on average around 60 percent fewer interventions per session compared to Fable 5. Anyone who has been stopped mid-task because a log contained an IP address and a session token string knows what that number means.

For benchmarks they list Terminal-Bench 4.0 at 55.8 percent up from 42.0, and CursorBench 3.2.0 at 73.4 percent up from 70.5. I copy them for completeness; how much you trust them is up to you.

## Trying three familiar jobs

EF Core migration: I gave it a new entity related to the student table and asked for the migration plus the SQL script. Both Sonnet 5 and Fable 5.1 wrote it. The difference was that Fable 5.1 asked about an index on the faculty and term filter columns before I brought it up. Sonnet 5 did exactly what I said.

Test generation: a service that computes conduct scores, asking for xUnit tests. Sonnet 5 was enough, fast, cheap. For this job I do not need Fable.

Log reading: pasted a few hundred lines of IIS log and asked which requests had abnormal response times. This is where Fable 5.1 differed most: it grouped by endpoint, spotted an auto-refreshing report screen calling in a loop, and did not get blocked by the safety layer over internal IPs in the log. The previous version sometimes did.

## One thing outside the model

Claude Code from version 2.1.224 lets two open sessions message each other, no more copying context between terminal windows. According to MacRumors it shipped with the Sonnet 5 wave. For me this changes the workflow more than any benchmark point: one session on the backend, one on Angular, and they agree on the API contract themselves.

Short version: Sonnet 5 for everyday work, Opus 5 when I need more thinking on a budget, Fable 5.1 when an agent runs long on a large repo. The strongest model is not the right model for every task, it is just the most expensive one when you stop looking at the bill.

## Sources

- Anthropic, [Introducing Claude Fable 5.1 and Claude Mythos 5.1](https://www.anthropic.com/claude-fable-and-mythos-5-1)
- Anthropic, [Introducing Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5)
- Anthropic, [Introducing Claude Haiku 4.5](https://www.anthropic.com/news/claude-haiku-4-5)
- Axios, [Anthropic releases new model, Opus 5](https://www.axios.com/2026/07/24/anthropic-releases-new-model-opus-5)
- NBC News, [Anthropic releases Fable 5, the first public Mythos-class model](https://www.nbcnews.com/tech/security/fable-5-anthropic-release-public-mythos-claude-model-rcna349104)
- MacRumors, [Anthropic Launches Claude Sonnet 5](https://www.macrumors.com/2026/06/30/anthropic-claude-sonnet-5/) and [Anthropic Launches Claude Fable 5.1](https://www.macrumors.com/2026/09/01/anthropic-claude-fable-5-1/)
