---
title: "Ba thế giới, một site"
titleEn: "Three worlds, one site"
slug: ba-the-gioi-mot-site
date: 2026-07-20
updated: 2026-09-02
tags: [astro, design, theme]
cover: /images/og-cover.jpg
excerpt: "Stoa, Mirai, Đồng, rồi Mực, Sơn, Gốm, Phố, Lụa, Nguyệt — cùng một HTML. Đổi da bằng một thuộc tính và một bảng màu."
excerptEn: "Stoa, Mirai, Drum, then Ink, Lacquer, Clay, Street, Silk, Moon — one HTML. Skin changes with one attribute and a color table."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Cùng một bố cục: tên lớn, chân dung, việc đã làm. Đổi da bằng một thuộc tính trên thẻ `html`. Ban đầu chỉ có ba thế giới. Rồi thành chín. HTML không đổi, chỉ có bảng token và một ảnh nền thay theo.

Mình viết Angular đủ để biết khi nào theme khác sản phẩm. Chín bản HTML là chín chỗ sửa khi thêm trang Liên hệ. Một layout cộng token thì thêm trang một lần. Đó là lý do, không phải vì mình thích sưu tập màu.

## Cách nó chạy, không có ma thuật

Trình duyệt nhớ thế giới đang chọn và sáng hay tối. F5 không mất. Một đoạn script trong phần đầu trang gắn class trước khi sơn, tránh chớp sai màu — chuyện đó khó chịu hơn một theme xấu.

Header có một danh sách. Bấm một cái là đổi. Không có nhãn “Giao diện” cho dài dòng, tên thế giới là đủ.

Mỗi thế giới có giấy, mực, vàng hoặc patina, một vệt nhấn, một ảnh wash, và một bộ màu riêng cho khung trên cùng. Nút trong trang và nút trên dải mực không được lấy chung một màu — mình đã dính, kể ở dưới.

## Ba thế giới gốc

**Stoa** là cẩm thạch mát, vàng lá, chữ khắc. Nút chính là mực đá. Muốn cảm giác sảnh, không phải cảm giác bảo tàng giả.

**Mirai** là giấy kem, đĩa nắng hổ phách, một vệt son. Nhiệt độ lấy từ một conference site mình thích. Không lấy skyline hay đèn neon, chỉ lấy cái nóng và màu kem.

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

Token nút trùng nền. Nguyệt từng lấy nắng và mực cùng một họ bạc. Nút “Bắt đầu một việc cùng nhau” biến mất trên dải mực. Người xem tưởng trang thiếu nút, thật ra nút vẫn đó, chỉ là cùng màu với nền.

Giờ dải mực dùng một token, trang dùng một token khác. Tách ra nghe hơi vụn, nhưng không tách thì cứ mỗi lần bật dark mode lại phải đoán.

Đường kẻ header từng là họa tiết lặp. Trông rẻ, như giấy gói. Đổi thành hai sợi tóc và một dấu giữa. Hình dấu đổi theo theme. Ít chi tiết hơn mà nhìn rõ hơn.

Stoa tối và Lụa tối từng nuốt chữ phụ. Mình phải ngồi từng theme, từng sáng tối, bấm hết menu. Không có shortcut nào cho việc đó, vì màu trên giấy với màu trên máy là hai chuyện khác nhau.

Suốt quá trình mình giữ một luật nhỏ: không tách chín repo, không để mã hex nằm trong component. Hex nằm ở bảng, component chỉ gọi tên. Về sau mỗi thế giới có thêm một module hiệu ứng nhỏ riêng — bụi nắng ở Stoa, mực loang ở Mực, lụa bay ở Lụa — nhưng luật màu không đổi: module đọc màu qua biến CSS, không có hex, và ai tắt animation trong hệ điều hành thì không thấy gì cả. Còn câu mình hỏi trước mỗi lần thêm một da mới không phải là ảnh nền sẽ ra sao, mà là nút chính còn đọc được không, trên mọi da, kể cả khi trời tối.

<!-- lang:en -->

Same layout: big name, portrait, work done. Skin changes with one attribute on `html`. At first three worlds. Then nine. The HTML stays put, only the token table and a wash image change with it.

I have written enough Angular to know when a theme is not a product. Nine HTML copies are nine places to edit when Contact grows. One layout plus tokens means one place. That is the reason — not a hobby of collecting colors.

## How it runs, no magic

The browser remembers the chosen world and light or dark. Refresh keeps it. A small head script sets the class before paint so the wrong skin does not flash — worse than an ugly theme.

The header has a list. One click switches. No long "Theme" label, the world name is enough.

Each world has paper, ink, gold or patina, one accent, a wash image, and its own chrome colors. In-page buttons and band buttons must not share one color — I learned that the hard way below.

## The first three

**Stoa** is cool marble, gold leaf, carved type. Primary buttons are stone ink. Hall, not fake museum.

**Mirai** is cream paper, amber sun disc, one vermillion mark. Heat taken from a conference site I like. No skyline or neon, just the heat and the cream.

**Drum** is patina, star center, concentric rings. Wash is a drum in a hall. Another post covers the circular grid. Here: Drum is not a drum SVG stuck in a corner.

## The six after

People saw three and said too few. I added five from objects I know: ink, lacquer, clay, street, silk. Then Moon, because night needs a voice that is not "grey dark mode".

**Ink** — dó paper, vermillion seal, wash strokes.  
**Lacquer** — gloss black, vermillion, gold leaf.  
**Clay** — celadon, fired earth, kiln heat.  
**Street** — cream porch, moss-green shutters.  
**Silk** — ivory, indigo, one peach mark.  
**Moon** — indigo night, crescent, stars.

Each must survive long Contact copy, small header buttons, and the big mid-page band. A theme that only looks good on the hero is not a theme.

## Where it breaks — I broke it

Button tokens matched the band. Moon once used the same silver family for sun and ink. "Start something together" vanished on the ink band. People thought the page had no button, when really it was there, just the same color as the ground.

Now the band has one token, the page another. Splitting them sounds fussy, but without it every dark mode becomes a guessing game.

Header rules were a repeating ornament. Looked cheap, like wrapping paper. Two hairlines and a center mark now. The mark changes with theme. Less detail, and it reads clearer.

Dark Stoa and dark Silk once swallowed secondary type. I sat through every world, light and dark, every menu. There is no shortcut for that, because paper color and screen color are two different things.

Through all of it I kept one small rule: no nine repos, no hex values inside components. Hex lives in the table, components only call names. Later each world got a small motion module of its own — sun dust in Stoa, bleeding ink in Ink, drifting silk in Silk — but the color rule held: modules read colors through CSS variables, no hex, and anyone with animations turned off in their OS sees nothing at all. And the question I ask before adding any new skin is not what the wash image will look like, but whether the primary button still reads on every skin, including at night.
