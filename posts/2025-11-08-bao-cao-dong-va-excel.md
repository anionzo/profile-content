---
title: "Báo cáo động và cái bẫy SELECT *"
slug: bao-cao-dong-va-excel
date: 2025-11-08
updated: 2026-08-16
tags: [sql-server, dotnet, excel, learning]
cover: /images/og-cover.jpg
excerpt: "Cột bật tắt được, lọc theo khoa, xuất Excel. Nghe như UI. Phần đau nằm ở SQL, và ở chỗ xem lưới với xuất file không được đi chung một đường."
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

Đừng để một câu “tiện” lấy cả bảng rồi lọc trong bộ nhớ. Chạy được trên máy dev với hai trăm dòng. Chết trên dữ liệu trường.

Mình đã dính đúng chuyện đó một lần. Local nhanh. Lên dữ liệu thật thì lưới quay. Plan chỉ ra scan. Không phải Angular chậm. Là mình lười.

## Xem và xuất không phải một việc

Màn hình hàng nghìn sinh viên mà lấy thật nhiều dòng “cho tiện xuất Excel” sẽ làm lệch hết lần mở trang thường.

Hai lối:

Xem trên lưới: page nhỏ, đếm tổng khi cần, chấp nhận ước lượng nếu người ta chỉ cần lật trang.

Xuất Excel: đường riêng. Stream theo lô. Không `ToList()` cả bộ rồi mới ghi file. Chỉ serialize cột đang bật. Đặt tên sheet và header bằng tiếng Việt có dấu. Đừng để `Cot1`, `Cot2` — người nhận file sẽ gọi điện.

Mình từng để hai việc dùng chung query. Người xem lưới chờ người xuất. Tách ra xong thì khiếu nại giảm. Không phải vì mình giỏi. Vì mình thôi bắt họ xếp hàng chung.

## Giao diện đừng giấu trạng thái

Giữ bộ lọc khi reload. Mất lọc là mất lòng tin: người ta tưởng hệ thống tự đổi số.

Nút xuất tắt khi đang chạy. Một dòng chữ “đang tạo file” rõ hơn vòng xoay vô hạn. File xong thì nói file xong. Lỗi thì nói lỗi, đừng im.

Có hôm xuất lâu vì dữ liệu lớn. Im thì họ bấm lại. Bấm lại thì hai job. Hai job thì chậm hơn. Vòng đó không phải lỗi người dùng.

## Việc mình không làm

Không mua BI. Không vẽ biểu đồ cho đẹp slide. Không hứa “real time”. Báo cáo nội bộ cần đúng, đủ cột, ra được file. Làm được ba thứ đó đã hơn một dashboard không ai mở lần hai.

Nếu bạn đang thêm “xuất Excel” vào cuối sprint: hỏi trước xem lưới và xuất có đang đi chung một câu SQL không. Thường là có. Thường là chỗ đau.
