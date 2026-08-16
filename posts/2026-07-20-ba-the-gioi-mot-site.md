---
title: "Ba thế giới, một site"
slug: ba-the-gioi-mot-site
date: 2026-07-20
updated: 2026-08-16
tags: [astro, design, theme]
cover: /images/og-cover.jpg
excerpt: "Stoa, Mirai, Đồng, rồi Mực, Sơn, Gốm, Phố, Lụa, Nguyệt — cùng một HTML. Đổi da bằng một thuộc tính và một bảng màu."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Cùng một bố cục: tên lớn, chân dung, việc đã làm. Đổi da bằng một thuộc tính trên thẻ `html`. Ban đầu chỉ có ba thế giới. Rồi thành chín. HTML không đổi. Chỉ bảng token và một ảnh nền.

Mình viết Angular đủ để biết khi nào theme khác sản phẩm. Chín bản HTML là chín chỗ sửa khi thêm trang Liên hệ. Một layout cộng token thì thêm trang một lần. Đó là lý do, không phải vì mình thích sưu tập màu.

## Cách nó chạy, không có ma thuật

Trình duyệt nhớ thế giới đang chọn và sáng hay tối. F5 không mất. Một đoạn script trong phần đầu trang gắn class trước khi sơn, tránh chớp sai màu — chuyện đó khó chịu hơn một theme xấu.

Header có một danh sách. Bấm một cái là đổi. Không có nhãn “Giao diện” cho dài. Tên thế giới đủ.

Mỗi thế giới có giấy, mực, vàng hoặc patina, một vệt nhấn, một ảnh wash, và một bộ màu riêng cho khung trên cùng. Nút trong trang và nút trên dải mực không được lấy chung một màu — mình đã dính, kể ở dưới.

## Ba thế giới gốc

**Stoa** là cẩm thạch mát, vàng lá, chữ khắc. Nút chính là mực đá. Muốn cảm giác sảnh, không phải cảm giác bảo tàng giả.

**Mirai** là giấy kem, đĩa nắng hổ phách, một vệt son. Nhiệt độ lấy từ một conference site mình thích. Không lấy skyline. Không lấy đèn neon. Chỉ lấy nóng và kem.

**Đồng** là patina, tâm sao, vòng đồng tâm. Wash là trống trong sảnh. Bài kia nói rõ hơn về lưới tròn. Ở đây chỉ cần biết: Đồng không phải dán SVG trống lên góc.

## Sáu thế giới sau

Người xem ba cái rồi bảo ít. Mình thêm năm cái lấy từ đồ vật mình biết: mực, sơn mài, gốm, phố, lụa. Rồi thêm Nguyệt vì đêm cần một giọng khác, không phải “dark mode xám”.

**Mực** — giấy dó, ấn son, nét thủy mặc.  
**Sơn** — đen bóng, son, vàng lá.  
**Gốm** — men ngọc, đất nung, lửa lò.  
**Phố** — hiên kem, cửa lá sách xanh rêu.  
**Lụa** — ngà, chàm, một vệt đào.  
**Nguyệt** — đêm chàm, lưỡi liềm, tinh tú.

Mỗi cái phải sống được cả chữ dài trên trang Liên hệ, cả nút nhỏ trên header, cả dải chữ lớn giữa trang. Theme chỉ đẹp ở hero thì chưa phải theme.

## Chỗ dễ gãy, mình đã gãy

Token nút trùng nền. Nguyệt từng lấy nắng và mực cùng một họ bạc. Nút “Bắt đầu một việc cùng nhau” biến mất trên dải mực. Người xem tưởng trang thiếu nút. Là nút cùng màu nền.

Giờ dải mực dùng một token, trang dùng một token khác. Tách ra nghe vụn. Không tách thì tối nào cũng phải đoán.

Đường kẻ header từng là họa tiết lặp. Trông rẻ, như giấy gói. Đổi thành hai sợi tóc và một dấu giữa. Hình dấu đổi theo theme. Ít hơn. Rõ hơn.

Stoa tối và Lụa tối từng nuốt chữ phụ. Mình phải ngồi từng theme, từng sáng tối, bấm hết menu. Không có shortcut. Màu trên giấy khác màu trên máy.

## Việc mình không làm

Không tách chín repo. Không viết hiệu ứng cho từng thế giới. Không để hex nằm trong component. Hex nằm ở bảng. Component chỉ gọi tên.

Nếu bạn muốn nhiều da trên một site: đừng bắt đầu bằng ảnh. Bắt đầu bằng việc hỏi nút chính còn đọc được không, trên mọi da, cả khi trời tối.
