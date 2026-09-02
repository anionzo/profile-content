---
title: "700 sinh viên thi cùng lúc"
titleEn: "700 students taking an exam at once"
slug: bay-tram-sinh-vien-thi-cung-luc
date: 2025-09-20
updated: 2026-08-16
tags: [dotnet, angular, sql-server, performance]
cover: /images/og-cover.jpg
excerpt: "Ngày thi tuần sinh hoạt công dân, khoảng 700 sinh viên vào cùng lúc. Không phải demo. Ghi lại những gì mình siết ở API, SQL, và cái nút nộp bài."
excerptEn: "Civic-education exam day: about 700 students at once. Not a demo. Notes on what I tightened in the API, SQL, and the submit button."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Ở IDX HUIT mình tham gia hệ thống thi và sinh hoạt công dân trực tuyến. Ngày thi, khoảng **700 sinh viên** vào cùng lúc. Không phải demo trên slide. Là giờ hành chính, mạng trường, máy phòng máy, và một kỳ vọng rất cụ thể: vào được, làm được, nộp được, không mất bài.

Bài này không kể “scale triệu user”. Mình chưa làm chuyện đó. Chỉ kể những việc mình làm để 700 phiên không đè chết API, và để một sinh viên mất Wi‑Fi mười giây không biến thành hai bài thi.

## Ngày thi trông như thế nào

Trước giờ mở đề, phòng máy còn lộn xộn. Có người login sớm. Có người còn hỏi mật khẩu. Có máy mở sai trình duyệt. Phía mình thì nhìn log: session tăng, rồi đứng yên một lúc, rồi tăng tiếp.

Khi đề mở, tải không đều. Năm phút đầu: login, lấy đề, tải câu, lưu câu đầu. Giữa giờ: lác đác lưu nháp, đổi đáp án. Cuối giờ: nộp bài đồng loạt, như ai đó vừa thổi còi.

Nếu thiết kế như CRUD bình thường — mỗi lần lưu là một transaction nặng, mỗi lần vào đề là join bảy bảng không index — phút cuối sẽ là phút chết. Không phải vì 700 là con số lớn, trên giấy nó chẳng là gì, mà vì 700 người làm **cùng một việc, cùng một giây**.

## Việc mình sợ nhất không phải sập hết

Sập hết còn rõ. Khó hơn là sập một phần: có người vào được, có người quay vòng spinner, có người nộp rồi thấy “lỗi mạng”, nộp lại, rồi hỏi mình bài nào được tính.

Mình từng thấy đúng kiểu đó trên các hệ thống khác. Người vận hành không hỏi stack. Họ hỏi: em A mất bài không? Em B nộp hai lần thì lấy lần nào? Phòng máy tầng 3 vào chậm hơn tầng 2 là sao?

Nên tiêu chí của mình không phải một con số request/giây đẹp, mà là mấy thứ rất cụ thể: mỗi người đúng một attempt, lưu nháp không đẻ bản ghi rác, nộp xong thì khóa, và mất mạng rồi vào lại vẫn thấy bài cũ.

## API: mỏng, đúng việc

Backend là ASP.NET Core. Mình tách endpoint theo nghiệp vụ thi, không nhét hết vào một controller khổng lồ tên đại loại “Student”.

Lấy đề: một payload đủ để render. Không kèm lịch sử mười hai kỳ trước, không kèm thống kê khoa, không kèm dữ liệu quản trị. Sinh viên không cần biết phòng thi còn bao nhiêu chỗ trống.

Lưu nháp: chỉ đáp án đổi. Idempotent theo mã attempt. Gọi hai lần cùng một payload thì vẫn một bản ghi. Nghe thì lý thuyết, nhưng ngày thi chính nó cứu mấy bạn bấm liền tay vì sợ mất mạng.

Nộp bài: khóa attempt. Không cho nộp lần hai. Nếu client gửi lại vì timeout, server nhận ra cùng một lần nộp, trả về trạng thái đã khóa, không tạo bài mới.

Auth và phân quyền nằm ở filter, không nằm rải trong từng action. Swagger để người làm giao diện đối chiếu contract trước khi ngày thi tới. Đừng để ngày D mới phát hiện field đổi tên.

## SQL: đừng để báo cáo đụng ngày thi

Cùng một database phục vụ thi và phục vụ báo cáo. Đó là thực tế, không phải kiến trúc đẹp trong sách.

Nếu màn hình thống kê chạy một câu lấy gần như cả bảng lúc 700 người đang nộp, hai việc sẽ giành I/O. Người thi thấy chậm, người coi báo cáo cũng thấy chậm, rồi cả hai cùng gọi mình.

Mình tách query thi khỏi query báo cáo. Pagination và filtering là mặc định, không phải “tính năng sau”. Index theo attempt, sinh viên, kỳ thi. Nhìn execution plan trước khi đổ lỗi cho frontend.

Có hôm mình suýt để một màn hình “theo dõi tiến độ” refresh liên tục. Nghe hữu ích. Thực ra là tự bắn mình: mỗi vài giây lại hỏi SQL cùng lúc với người đang nộp. Mình tắt refresh tự động, để cán bộ coi thi tự bấm khi cần số mới. Họ hơi phiền hơn một chút, nhưng phía thí sinh êm hẳn.

## UI: đừng tự bắn mình

Giao diện sinh viên không được gọi API mỗi lần đổi đáp án. Debounce lưu nháp. Hiện rõ ba trạng thái: đã lưu, đang gửi, lỗi mạng. Sinh viên trong phòng máy không cần biết phía sau đang chạy gì, chỉ cần biết bài của mình còn đó.

Mất kết nối mười giây rồi nộp lại: server phải nhận diện cùng một attempt. Client phải giữ mã đó, không tạo phiên mới vì “cho chắc”. “Cho chắc” là cách sinh ra hai bài.

Nút nộp trên UI chỉ cho bấm một lần, nhưng chỗ khóa thật vẫn phải là server, vì client có thể gửi lại bất cứ lúc nào mà mình không kiểm soát được.

Có chỗ mình từng để thông báo lỗi bằng tiếng Anh ngắn. Phòng máy không đọc. Đổi thành câu tiếng Việt cụ thể: chưa gửi được, bài vẫn còn trên máy, hãy bấm lại. Khiếu nại giảm ngay cái hôm đó.

Nhìn lại thì mình không làm gì to tát. Không gắn hàng đợi vào ngày trước kỳ thi, không đổi database, không viết lại frontend, cũng không hứa hẹn gì về cloud. Chỉ siết hợp đồng API, siết query, siết trạng thái form. Hệ thống đứng được qua buổi sáng đó, với mình vậy là đủ. Kiến trúc đẹp hơn để dành cho một ngày không có 700 người đang nhìn đồng hồ.

<!-- lang:en -->

At IDX HUIT I work on the online exam and civic-education system. On exam day about **700 students** come in at once. Not a slide demo. Office hours, campus network, lab machines, and one concrete bar: get in, answer, submit, don't lose the paper.

This is not a story about "millions of users". I have not done that. Only what I tightened so 700 sessions don't crush the API, and so ten seconds of dropped Wi‑Fi don't turn into two exam attempts.

## What exam day looks like

Before the paper opens the lab is messy. Early logins. Password questions. Wrong browsers. On our side the logs climb, stall, climb again.

When the paper opens load is uneven. First five minutes: login, fetch paper, load questions, save the first answers. Midway: sparse drafts, changed choices. End: submit all at once, like a whistle blew.

If you design it like ordinary CRUD — heavy transactions on every save, seven unindexed joins on every open — the last minute is the dead minute. Not because 700 is a big number, on paper it is nothing, but because 700 people do **the same thing in the same second**.

## What I fear is not a full outage

A full outage is obvious. Worse is a partial one: some get in, some spin, some submit and see "network error", submit again, then ask which attempt counts.

I have seen that shape on other systems. Ops does not ask about stack. They ask: did student A lose the paper? If B submitted twice, which one counts? Why is floor 3 slower than floor 2?

So my bar was never a pretty requests-per-second chart. It was a few very concrete things: one attempt per person, draft saves that do not spawn junk rows, a submit that locks, and the old paper still there after a dropped connection.

## API: thin, on purpose

Backend is ASP.NET Core. I split endpoints by exam work, not one giant "Student" controller.

Fetch paper: one payload enough to render. No twelve past terms, no faculty stats, no admin junk. Students do not need empty seats in the room.

Draft save: only changed answers. Idempotent on attempt id. Same payload twice still one row. Sounds theoretical, but on exam day it is exactly what saves the people mashing the button because they fear the network.

Submit: lock the attempt. No second submit. If the client retries after timeout, the server sees the same submit, returns locked, does not invent a new paper.

Auth sits in filters, not sprinkled through actions. Swagger so the UI can match the contract before D-day. Do not discover a renamed field on the morning of the exam.

## SQL: keep reports off exam hour

The same database serves exams and reports. That is reality, not a textbook architecture.

If a stats screen runs a near-full-table query while 700 people submit, both fight for I/O. Examinees feel it, report viewers feel it, and then both of them call you.

I split exam queries from report queries. Pagination and filtering are defaults, not "later". Indexes on attempt, student, term. Read the plan before you blame the frontend.

I almost left an auto-refresh "progress" board on. Useful sounding. Self-inflicted: every few seconds it hits SQL next to people submitting. I turned auto-refresh off and let staff click when they want a fresh number. Slightly more annoying for them, a lot quieter for the examinees.

## UI: don't shoot yourself

Student UI must not hit the API on every answer change. Debounce drafts. Show three states clearly: saved, sending, network error. Lab students do not need the backend story, they just need to know the paper is still there.

Ten seconds offline then submit again: server must know the same attempt. Client must keep that id, not open a "safer" new session. "Safer" is how you get two papers.

The submit button only allows one click on the UI, but the real lock still has to live on the server, because the client can retry at any moment and I do not control that.

I once left short English errors. Labs do not read them. Vietnamese, concrete: not sent yet, paper still on the machine, try again. Complaints dropped that same day.

Looking back, none of it was dramatic. No queue bolted on the day before the term, no database swap, no frontend rewrite, no cloud promise. Just tightening the API contract, the queries, and the form state. The system stood through that morning, and for me that was enough. Prettier architecture can wait for a day when 700 people are not watching the clock.
