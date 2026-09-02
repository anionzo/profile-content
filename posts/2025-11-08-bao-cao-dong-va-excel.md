---
title: "Báo cáo động và cái bẫy SELECT *"
titleEn: "Dynamic reports and the SELECT * trap"
slug: bao-cao-dong-va-excel
date: 2025-11-08
updated: 2026-08-16
tags: [sql-server, dotnet, excel, learning]
cover: /images/posts/bao-cao-dong-va-excel/cover.jpg
excerpt: "Cột bật tắt được, lọc theo khoa, xuất Excel. Nghe như UI. Phần đau nằm ở SQL, và ở chỗ xem lưới với xuất file không được đi chung một đường."
excerptEn: "Toggle columns, filter by faculty, export Excel. Sounds like UI work. The pain is SQL — and not letting grid view and file export share one bad path."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Người dùng nội bộ ở trường không hỏi bạn dùng kiến trúc gì. Họ hỏi: cột này ẩn được không, lọc theo khoa được không, xuất Excel được không, và tại sao lần này chờ hai mươi giây.

Ở IDX HUIT mình làm các module báo cáo cho hệ thống quản lý đào tạo và điều hành khoa. Việc nghe như frontend. Phần đau nằm ở SQL, và ở chỗ mình từng để hai việc khác nhau dùng chung một query.

## Họ cần gì, thật ra

Không phải dashboard màu. Là một lưới: thấy đúng cột họ đang làm việc, lọc đúng khoa / khóa / học kỳ, rồi tải một file mở được bằng Excel trên máy phòng ban.

Có người muốn hai mươi cột. Có người chỉ muốn mã, họ tên, lớp, trạng thái. Nếu bạn ép mọi người xem hết, họ kêu rối. Nếu bạn giấu cột trên giao diện nhưng vẫn lấy hết dưới SQL, họ kêu chậm — và họ đúng.

## Báo cáo động, với mình, là bốn thứ

Danh sách cột bật tắt được. Bộ lọc theo từng cột hay gặp. Phân trang khi xem trên lưới. Xuất đúng những cột đang hiện ra Excel.

Nếu mỗi tổ hợp cột là một stored procedure, bạn sẽ có một ngăn kéo không đóng được. Nếu một query lấy hết cột rồi giao diện ẩn, bạn phạt SQL mỗi lần mở trang.

Mình chọn lối giữa: một catalog cột phía server. Tên field, kiểu, lọc được không, sort được không. Frontend chỉ gửi cột đang hiện và bộ lọc. API kiểm tra field nằm trong catalog. Không nhận chuỗi tùy ý nhét vào `ORDER BY`. Đó là SQL injection đội lốt “linh hoạt”.

## Entity Framework không phải kẻ thù. Cũng không phải chiếc búa cho mọi đinh

CRUD thì dùng bình thường. Báo cáo nặng thì mình viết SQL có chủ đích, nhìn plan, thêm index theo cặp lọc hay gặp — khoa với học kỳ, lớp với trạng thái, những thứ người ta bấm mỗi tuần.

Đừng để một câu “tiện” lấy cả bảng rồi lọc trong bộ nhớ. Trên máy dev với hai trăm dòng thì chạy ngon, lên dữ liệu thật của trường là chết.

Mình đã dính đúng chuyện đó một lần: local nhanh, lên dữ liệu thật thì lưới quay mãi. Mở execution plan ra thấy một cái table scan to đùng. Không phải Angular chậm. Là mình lười.

## Xem và xuất không phải một việc

Màn hình hàng nghìn sinh viên mà lấy thật nhiều dòng “cho tiện xuất Excel” sẽ làm lệch hết lần mở trang thường.

Hai lối:

Xem trên lưới: page nhỏ, đếm tổng khi cần, chấp nhận ước lượng nếu người ta chỉ cần lật trang.

Xuất Excel: đường riêng. Stream theo lô. Không `ToList()` cả bộ rồi mới ghi file. Chỉ serialize cột đang bật. Đặt tên sheet và header bằng tiếng Việt có dấu. Đừng để `Cot1`, `Cot2` — người nhận file sẽ gọi điện.

Mình từng để hai việc dùng chung query. Người xem lưới chờ người xuất. Tách ra xong thì khiếu nại giảm. Không phải vì mình làm gì giỏi, chỉ là thôi bắt họ xếp hàng chung.

## Giao diện đừng giấu trạng thái

Giữ bộ lọc khi reload. Mất bộ lọc là mất lòng tin, vì người ta tưởng hệ thống tự đổi số.

Nút xuất tắt khi đang chạy. Một dòng chữ “đang tạo file” rõ hơn vòng xoay vô hạn. File xong thì báo xong, lỗi thì báo lỗi, đừng im lặng.

Có hôm xuất lâu vì dữ liệu lớn. Hệ thống im, họ bấm lại, thế là hai job chạy song song và càng chậm hơn. Cái vòng đó không phải lỗi người dùng.

Suốt mấy module đó mình không mua BI, không vẽ biểu đồ cho đẹp slide, cũng không hứa gì về real time. Báo cáo nội bộ chỉ cần đúng, đủ cột, ra được file, và ba thứ đó đã hơn một dashboard không ai mở lần hai.

Còn có một câu mình hay tự hỏi mỗi khi thấy chữ “xuất Excel” được thêm vào cuối sprint: lưới và xuất có đang đi chung một câu SQL không? Thường là có.

<!-- lang:en -->

Internal school users do not ask what architecture you use. They ask: can this column hide, can I filter by faculty, can I export Excel, and why did this take twenty seconds.

At IDX HUIT I build report modules for training and faculty systems. It sounds like frontend work. The pain is SQL — and the time I let two different jobs share one query.

## What they actually need

Not a colorful dashboard. A grid: the columns they work with, filters for faculty / cohort / term, then a file that opens in Excel on an office machine.

Some want twenty columns. Some want code, name, class, status. Force everyone to see everything and they call it noisy. Hide columns in the UI but still select everything in SQL and they call it slow — and they are right.

## Dynamic reports, for me, are four things

Toggleable columns. Filters on the usual fields. Pagination on the grid. Export exactly the columns on screen to Excel.

If every column mix is a stored procedure, you get a drawer that never closes. If one query pulls every column and the UI hides them, you punish SQL on every open.

I took the middle path: a server-side column catalog. Field name, type, filterable, sortable. Frontend sends visible columns and filters. API checks fields against the catalog. No free-form strings in `ORDER BY`. That is SQL injection dressed as "flexibility".

## Entity Framework is not the enemy. Or a hammer for every nail

CRUD is fine with it. Heavy reports get deliberate SQL, a plan read, indexes on the filter pairs people hit weekly — faculty with term, class with status.

Do not "conveniently" load a whole table and filter in memory. It runs fine on a dev box with two hundred rows and dies on real school data.

I hit exactly that once: fast locally, then the grid spun forever on real data. The execution plan showed one big table scan. Not Angular being slow. Me being lazy.

## Viewing and exporting are not one job

A screen of thousands of students that loads huge pages "for easy Excel" warps every normal open.

Two paths:

Grid view: small pages, count totals when needed, accept estimates if people only flip pages.

Excel export: a separate path. Stream in batches. Do not `ToList()` the world then write a file. Serialize only visible columns. Vietnamese headers with diacritics. Not `Col1`, `Col2` — the person who gets the file will call you.

I once shared one query for both. Grid waiters queued behind exporters. After the split, complaints dropped. Not because I did anything clever, I just stopped making them share a line.

## UI should not hide state

Keep filters across reload. Losing filters loses trust, because people think the system changed the numbers on them.

Disable export while it runs. "Building file" beats an endless spinner. Say done when it is done, say error when it fails, and never go silent.

One day a large export ran long. The system went quiet, people clicked again, and now two jobs ran side by side and everything got slower. That loop is not the user's fault.

Across those modules I did not buy a BI tool, did not draw charts for a pretty slide, and did not promise real time. Internal reports only need to be right, have the right columns, and produce a file, and those three already beat a dashboard nobody opens twice.

There is one question I keep asking myself whenever "export Excel" gets bolted onto the end of a sprint: are the grid and the export still sharing one SQL statement? Usually they are.
