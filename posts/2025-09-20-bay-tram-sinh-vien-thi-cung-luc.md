---
title: "700 sinh viên thi cùng lúc"
slug: bay-tram-sinh-vien-thi-cung-luc
date: 2025-09-20
updated: 2026-08-16
tags: [dotnet, angular, sql-server, performance]
cover: /images/og-cover.jpg
excerpt: "Ngày thi tuần sinh hoạt công dân, khoảng 700 sinh viên vào cùng lúc. Không phải demo. Ghi lại những gì mình siết ở API, SQL, và cái nút nộp bài."
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

Nếu thiết kế như CRUD bình thường — mỗi lần lưu là một transaction nặng, mỗi lần vào đề là join bảy bảng không index — phút cuối sẽ là phút chết. Không phải vì 700 là con số lớn trên giấy. Vì 700 người làm **cùng một việc, cùng một giây**.

## Việc mình sợ nhất không phải sập hết

Sập hết còn rõ. Khó hơn là sập một phần: có người vào được, có người quay vòng spinner, có người nộp rồi thấy “lỗi mạng”, nộp lại, rồi hỏi mình bài nào được tính.

Mình từng thấy đúng kiểu đó trên các hệ thống khác. Người vận hành không hỏi stack. Họ hỏi: em A mất bài không? Em B nộp hai lần thì lấy lần nào? Phòng máy tầng 3 vào chậm hơn tầng 2 là sao?

Vậy tiêu chí của mình không phải “request/giây đẹp”. Là: một attempt một người. Lưu nháp không đẻ bản ghi rác. Nộp xong thì khóa. Mất mạng rồi vào lại vẫn thấy bài cũ.

## API: mỏng, đúng việc

Backend là ASP.NET Core. Mình tách endpoint theo nghiệp vụ thi, không nhét hết vào một controller khổng lồ tên đại loại “Student”.

Lấy đề: một payload đủ để render. Không kèm lịch sử mười hai kỳ trước, không kèm thống kê khoa, không kèm dữ liệu quản trị. Sinh viên không cần biết phòng thi còn bao nhiêu chỗ trống.

Lưu nháp: chỉ đáp án đổi. Idempotent theo mã attempt. Gọi hai lần cùng một payload thì vẫn một bản ghi. Cái này nghe lý thuyết. Ngày thi nó cứu người bấm liền tay vì sợ mất mạng.

Nộp bài: khóa attempt. Không cho nộp lần hai. Nếu client gửi lại vì timeout, server nhận ra cùng một lần nộp, trả về trạng thái đã khóa, không tạo bài mới.

Auth và phân quyền nằm ở filter, không nằm rải trong từng action. Swagger để người làm giao diện đối chiếu contract trước khi ngày thi tới. Đừng để ngày D mới phát hiện field đổi tên.

## SQL: đừng để báo cáo đụng ngày thi

Cùng một database phục vụ thi và phục vụ báo cáo. Đó là thực tế, không phải kiến trúc đẹp trong sách.

Nếu màn hình thống kê chạy một câu lấy gần như cả bảng lúc 700 người đang nộp, hai việc sẽ giành I/O. Người thi thấy chậm. Người coi báo cáo cũng thấy chậm. Rồi cả hai gọi mình.

Mình tách query thi khỏi query báo cáo. Pagination và filtering là mặc định, không phải “tính năng sau”. Index theo attempt, sinh viên, kỳ thi. Nhìn execution plan trước khi đổ lỗi cho frontend.

Có hôm mình suýt để một màn hình “theo dõi tiến độ” refresh liên tục. Nghe hữu ích. Thực ra là tự bắn mình: mỗi vài giây lại hỏi SQL cùng lúc với người đang nộp. Tắt refresh tự động, để người coi bấm lấy số mới. Ồn hơn một chút cho cán bộ. Êm hơn cho thí sinh.

## UI: đừng tự bắn mình

Giao diện sinh viên không được gọi API mỗi lần đổi đáp án. Debounce lưu nháp. Hiện rõ ba trạng thái: đã lưu, đang gửi, lỗi mạng. Sinh viên phòng máy không cần biết phía sau là gì. Họ cần biết bài còn đó.

Mất kết nối mười giây rồi nộp lại: server phải nhận diện cùng một attempt. Client phải giữ mã đó, không tạo phiên mới vì “cho chắc”. “Cho chắc” là cách sinh ra hai bài.

Nút nộp chỉ bấm được một lần trên UI. Vẫn phải khóa trên server. UI chỉ là lịch sự. Server mới là luật.

Có chỗ mình từng để thông báo lỗi bằng tiếng Anh ngắn. Phòng máy không đọc. Đổi thành câu tiếng Việt cụ thể: chưa gửi được, bài vẫn còn trên máy, hãy bấm lại. Khiếu nại giảm ngay cái hôm đó.

## Việc mình không làm

Không gắn hàng đợi ngày trước kỳ thi. Không đổi database. Không viết lại frontend. Không hứa “cloud”. Chỉ siết hợp đồng, siết query, siết trạng thái form.

Hệ thống đứng. Đó là tiêu chí. Còn “đẹp kiến trúc” để lần sau, khi không có 700 người đang nhìn đồng hồ.

Nếu bạn đang làm một kỳ thi online cho trường: đừng bắt đầu từ diagram. Bắt đầu từ một attempt, một lần nộp, và một câu SQL bạn dám chạy đúng giờ mở đề.
