---
title: "Grok 4.5, 4.6 và Grok Build: thử vào việc backend .NET"
titleEn: "Grok 4.5, 4.6 and Grok Build: trying them on .NET backend work"
slug: grok-4-6-va-grok-build
date: 2026-08-24
updated: 2026-08-24
tags: [ai, grok]
cover: /images/posts/grok-4-6-va-grok-build/cover.jpg
excerpt: "xAI giờ là SpaceXAI, ra Grok 4.5 rồi 4.6 trong hơn một tháng, kèm một coding agent chạy trong terminal. Ghi chú đọc và thử với vài việc backend quen tay."
excerptEn: "xAI is now SpaceXAI, shipped Grok 4.5 then 4.6 inside five weeks, plus a terminal coding agent. Reading-and-trying notes on a few familiar backend jobs."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Mình không phải người dùng Grok. Phần lớn ngày mình ở trong OpenCode hoặc Claude Code. Nhưng tháng 8 này Grok 4.6 ra, giá bằng một nửa mấy model đầu bảng, nên mình bỏ một buổi tối đọc và thử. Đây là ghi chú của người ngoài cuộc, không phải bài so sánh có kiểm soát.

## Bối cảnh, ngắn thôi

Theo VentureBeat, SpaceX mua xAI từ tháng 2/2026 và phần AI giờ tự gọi là SpaceXAI. Tên đổi, sản phẩm vẫn tên Grok.

Grok 4.5 ra ngày 8/7/2026. xAI nói nó được huấn luyện nhắm vào coding, tác vụ agent, khoa học kỹ thuật. Giá API 2 đô mỗi triệu token vào, 6 đô ra, ngữ cảnh 500 nghìn token.

Grok 4.6 ra ngày 12/8, chỉ năm tuần sau. Giá giữ nguyên 2 và 6 đô cho prompt dưới 200 nghìn token, gấp đôi khi vượt ngưỡng đó. Bản nhanh hơn thì giá gấp đôi. Theo trang TestingCatalog thì có bốn mức effort: low, medium, high, xhigh. Nó lên Cursor, OpenRouter, Microsoft Foundry, và Grok Build của chính họ.

Về số liệu, theo VentureBeat dẫn Artificial Analysis: chỉ số Intelligence Index 61, lên năm điểm so với Grok 4.5 High, ngang GPT-5.6 Sol Max, sau Opus 5 và Fable 5. CursorBench v3.2 lên 69.9% từ 66.7%. DeepSWE v1.1 đạt 65.9%, còn cách GPT-5.6 Sol Max ở 73%. Mình chép để bạn khỏi tìm, còn ý nghĩa thật với việc của mình nằm ở phần dưới.

## Grok Build là gì

Theo bài giới thiệu trên x.ai, Grok Build là coding agent chạy trong terminal, ra bản beta sớm tháng 5/2026. Cài bằng một lệnh curl. Có plan mode để duyệt, sửa từng bước hoặc viết lại kế hoạch trước khi nó chạy. AGENTS.md, plugin, hook, skill, MCP server dùng được luôn. Có subagent chạy song song trên worktree riêng. Có cờ `-p` để chạy không giao diện trong script.

Lúc đầu chỉ cho người trả gói SuperGrok hoặc X Premium Plus, theo trang x.ai. CIO Dive thì ghi là gói SuperGrok Heavy 300 đô một tháng. Hai nguồn lệch nhau, mình không rõ tầng nào đúng vào lúc nào. Đến giữa tháng 7, theo alphamatch.ai, xAI mở mã nguồn phần TUI và agent lên GitHub của xai-org.

Cái làm mình chú ý không phải tính năng, vì Claude Code và Codex đều có những thứ đó rồi, mà là việc nó nhận AGENTS.md và MCP có sẵn. Repo của mình đã có AGENTS.md cho OpenCode. Không phải viết lại gì để thử.

## Thử ba việc backend

Mình gọi Grok 4.6 qua OpenRouter, trong OpenCode, để giữ nguyên bộ tool và AGENTS.md quen thuộc. Repo là một service ASP.NET Core nhỏ, không phải hệ thống thật của trường.

Việc một: thêm bảng mới có khóa ngoại tới sinh viên, viết migration EF Core và script SQL. Grok 4.6 làm được, gọn, đặt tên migration đúng quy ước trong repo. Nó không tự hỏi về index, nhưng khi mình nhắc thì thêm đúng cặp cột lọc.

Việc hai: tách một controller lớn thành hai theo nghiệp vụ, giữ nguyên route công khai. Ở mức effort high, nó dò được chỗ nào Angular đang gọi bằng cách grep trong thư mục frontend, và không đổi route nào. Chỗ này mình thấy ổn hơn kỳ vọng.

Việc ba: dán execution plan của một query báo cáo chậm và hỏi nên đánh index gì. Nó chỉ đúng table scan, đề xuất index có include column. Trả lời hơi dài, phần giải thích lặp lại điều mình đã nói trong câu hỏi.

Chuyện tốc độ: ở mức high nó chậm hơn Sonnet 5 rõ rệt trên máy mình, còn mức medium thì nhanh và vẫn làm được việc một và ba. Mình không đo bằng đồng hồ, chỉ cảm nhận, nên xin đừng trích.

## Grok Imagine, để đủ bộ

Grok Imagine là phần tạo ảnh và video trong app Grok. Theo trang techjacksolutions, bản Video 1.5 ra tháng 6/2026, bản Fast tạo clip 6 giây 720p trong khoảng 25 giây. Với phần mềm trường học nó không có chỗ dùng, mình ghi lại cho đủ vì nó nằm chung một bộ sản phẩm.

## Kết lại

Giá 2 và 6 đô với ngữ cảnh 500 nghìn token là điểm khiến Grok 4.6 đáng có trong danh sách, nhất là khi cần đọc cả một module lớn một lần. Grok Build thì mình chưa thấy lý do rời OpenCode, vì tất cả những gì nó có, mình đã có ở chỗ đang ngồi. Đổi model rẻ hơn đổi thói quen.

## Nguồn

- VentureBeat, [SpaceXAI debuts Grok 4.6](https://venturebeat.com/technology/spacexai-debuts-grok-4-6-overtaking-kimi-k3s-performance-and-matching-gpt-5-6-sol-for-worlds-third-best-on-artificial-analysis)
- SpaceXAI, [Introducing Grok Build](https://x.ai/news/grok-build-cli)
- xAI docs, [Grok Build overview](https://docs.x.ai/build/overview)
- CIO Dive, [xAI joins crowded coding agent race with Grok Build](https://www.ciodive.com/news/xAI-coding-agents-Grok-Build/820422/)
- TestingCatalog, [xAI releases Grok 4.6 for long-running agent work](https://www.testingcatalog.com/icymi-xai-releases-grok-4-6-for-long-running-agent-work/)
- DataCamp, [Grok 4.5: Features, Benchmarks, Pricing](https://www.datacamp.com/blog/grok-4-5)
- alphamatch.ai, [Grok Build goes open source](https://www.alphamatch.ai/blog/xai-grok-build-open-source-2026)
- techjacksolutions, [Grok Imagine Video 1.5](https://techjacksolutions.com/ai-brief/ai-video-news-grok-imagine-video-15-launches-25-second-gener/)

<!-- lang:en -->

I am not a Grok user. Most of my day is in OpenCode or Claude Code. But Grok 4.6 came out this August at half the price of the top models, so I spent an evening reading and trying. These are outsider notes, not a controlled comparison.

## Context, briefly

According to VentureBeat, SpaceX acquired xAI in February 2026 and the AI arm now calls itself SpaceXAI. The name changed; the product is still Grok.

Grok 4.5 shipped on July 8, 2026. xAI says it was trained for coding, agentic tasks, science and engineering. API price is 2 dollars per million input tokens, 6 out, with a 500 thousand token context.

Grok 4.6 shipped on August 12, only five weeks later. Same 2 and 6 dollars for prompts under 200 thousand tokens, double above that. The fast variant costs double. According to TestingCatalog there are four effort levels: low, medium, high, xhigh. It landed on Cursor, OpenRouter, Microsoft Foundry, and their own Grok Build.

On numbers, VentureBeat citing Artificial Analysis: Intelligence Index 61, five points over Grok 4.5 High, level with GPT-5.6 Sol Max, behind Opus 5 and Fable 5. CursorBench v3.2 rose to 69.9 percent from 66.7. DeepSWE v1.1 at 65.9 percent, still behind GPT-5.6 Sol Max at 73. I copy them so you do not have to look; what they mean for my work is below.

## What Grok Build is

According to the x.ai announcement, Grok Build is a coding agent that runs in the terminal, in early beta since May 2026. One curl command to install. A plan mode to approve, comment on individual steps, or rewrite the plan before it runs. AGENTS.md, plugins, hooks, skills, and MCP servers work out of the box. Parallel subagents on their own worktrees. A `-p` flag for headless runs inside scripts.

At first it was only for SuperGrok or X Premium Plus subscribers, per x.ai. CIO Dive wrote SuperGrok Heavy at 300 dollars a month. The two sources disagree and I do not know which tier was true when. By mid July, according to alphamatch.ai, xAI open-sourced the TUI and agent on the xai-org GitHub.

What caught my attention was not the feature list, since Claude Code and Codex already have all of it, but that it reads existing AGENTS.md and MCP config. My repo already has an AGENTS.md for OpenCode. Nothing to rewrite to try it.

## Three backend jobs

I called Grok 4.6 through OpenRouter, inside OpenCode, to keep my usual tools and AGENTS.md. The repo was a small ASP.NET Core service, not the school's real system.

Job one: add a new table with a foreign key to students, write the EF Core migration and SQL script. Grok 4.6 did it, compact, and followed the repo's migration naming. It did not ask about indexes on its own, but when prompted it added the right filter column pair.

Job two: split a large controller into two by business area, keeping public routes intact. At high effort it found what Angular was calling by grepping the frontend folder, and changed no route. Better than I expected here.

Job three: paste the execution plan of a slow report query and ask what index to add. It pointed at the table scan and proposed an index with include columns. The answer ran long, and the explanation repeated things I had said in the question.

On speed: at high it was clearly slower than Sonnet 5 on my machine, while medium was quick and still handled jobs one and three. I did not time it, this is a feeling, so please do not quote it.

## Grok Imagine, for completeness

Grok Imagine is the image and video part of the Grok app. According to techjacksolutions, Video 1.5 shipped in June 2026, and the Fast variant makes a 6 second 720p clip in about 25 seconds. It has no place in school software; I note it because it ships in the same bundle.

## Closing

Two and six dollars with a 500 thousand token context is what puts Grok 4.6 on the list, especially when I need one pass over a whole large module. Grok Build gives me no reason to leave OpenCode yet, because everything it has, I already have where I sit. Switching models is cheaper than switching habits.

## Sources

- VentureBeat, [SpaceXAI debuts Grok 4.6](https://venturebeat.com/technology/spacexai-debuts-grok-4-6-overtaking-kimi-k3s-performance-and-matching-gpt-5-6-sol-for-worlds-third-best-on-artificial-analysis)
- SpaceXAI, [Introducing Grok Build](https://x.ai/news/grok-build-cli)
- xAI docs, [Grok Build overview](https://docs.x.ai/build/overview)
- CIO Dive, [xAI joins crowded coding agent race with Grok Build](https://www.ciodive.com/news/xAI-coding-agents-Grok-Build/820422/)
- TestingCatalog, [xAI releases Grok 4.6 for long-running agent work](https://www.testingcatalog.com/icymi-xai-releases-grok-4-6-for-long-running-agent-work/)
- DataCamp, [Grok 4.5: Features, Benchmarks, Pricing](https://www.datacamp.com/blog/grok-4-5)
- alphamatch.ai, [Grok Build goes open source](https://www.alphamatch.ai/blog/xai-grok-build-open-source-2026)
- techjacksolutions, [Grok Imagine Video 1.5](https://techjacksolutions.com/ai-brief/ai-video-news-grok-imagine-video-15-launches-25-second-gener/)
