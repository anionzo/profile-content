---
title: "Đăng ký Muse từ Việt Nam mà không cần fake VPN"
titleEn: "Sign Up for Muse from Vietnam Without a Fake VPN"
slug: dang-ky-muse-khong-can-fake-vpn
date: 2026-09-26
updated: 2026-09-26
tags: [ai, google, tools]
cover: /images/posts/dang-ky-muse-khong-can-fake-vpn/cover.jpg
excerpt: "Muse — personal AI agent của Meta — chỉ mở cho người dùng Mỹ từ 18 tuổi. Bài này đi đường vòng: để Gemini Spark điều khiển trình duyệt từ xa của Google điền form đăng ký hộ, thay vì dựng VPN giả và đối mặt với tài khoản bị khóa vĩnh viễn."
excerptEn: "Muse, Meta's personal AI agent, is US-only and 18+. This post takes the long way around: instead of a fake VPN and a permanently banned account, let Gemini Spark drive Google's remote browser and fill in the signup form for you."
status: published
readingTimeOverride: null
series: null
canonicalUrl: null
ogImage: null
---

Trong những ngày cuối tháng 9 này, feed AI của mình bị thuốc trừ đầy một loại thuốc mới: **Muse** — personal AI agent của Meta. Nó không chỉ trả lời câu hỏi, mà tự mở browser, tự đặt vé, tự chọn chỗ ngồi rồi hỏi bạn duyệt trước khi thanh toán.

Nghe thì hấp dẫn. Nhưng mở app ra thì có một chi tiết nhỏ: **Muse chỉ dành cho người dùng Mỹ, từ 18 tuổi trở lên**. Còn mình thì đang ở Việt Nam.

Vậy nên bài này là câu trả lời cho câu hỏi mà nhiều anh em chắc cũng đang gặp: **làm sao vào được Muse mà không cần dựng VPN giả?**

## Bài toán "chỉ Mỹ" của Meta

Muse chính thức ra mắt ngày 08/09/2026, chạy trên iOS, Android, web và cả trong WhatsApp. Meta nói phần lớn nhu cầu sẽ miễn phí, phần nâng cao thì trả tiền — theo các bài viết đưa tin thì khoảng $20 và $100 mỗi tháng.

Nhưng danh sách quốc gia thì không có Việt Nam. Mở `muse.ai/join` từ IP Việt, bạn sẽ gặp đúng cái lỗi mà người ta đang bàn tới trên Hacker News:

> *"We couldn't continue with that account."*

Cách mọi người hay xử lý là bật VPN Mỹ. Mình không khuyên, vì ba lý do:

1. **Nó bị chặn thường xuyên.** Meta kiểm tra nhiều lớp hơn chỉ IP.
2. **Tài khoản bay mà không cảnh báo.** Bay thì mất trắng, mà không có lý do ghi rõ.
3. **Nó lâu hơn.** Đây là cách chậm nhất trong ba cách.

Còn có một hướng ít ai nhắc tới: **để một agent AI khác điền giúp**. Nếu thao tác đăng ký không chạy từ IP của bạn, mà chạy từ hạ tầng cloud của Google thì sao? Đó chính là lối đi mình sẽ thử trong bài này.

## Gemini Spark là gì và tại sao nó làm được

![Muse tự mở browser, chọn chỗ ngồi rồi chờ bạn duyệt](/images/posts/dang-ky-muse-khong-can-fake-vpn/muse-tu-dat-ve.jpg)

Trước hết, để hiểu vì sao Muse đáng để bật VPN giả: nó là một agent có **browser riêng**, chạy trong một máy ảo riêng, và mọi hành động nhạy cảm đều phải chờ bạn bấm đồng ý.

Cái mà mình cần là **một browser chạy ở nước khác mà vẫn điền được form thật**. Và Gemini Spark của Google vừa có đúng cái đó.

Spark là agent 24/7 của Google, chạy trên cloud nên vẫn làm việc được sau khi bạn đóng máy tính. Nó có một công cụ tên là **Remote browser**: một bản Chrome thật, chạy trên máy tính cloud của Google, có cookie riêng, và bạn có thể "tiếp quản" bất kỳ lúc nào.

![Spark tự chạy task và nút Take over task](/images/posts/dang-ky-muse-khong-can-fake-vpn/spark-take-over-task.jpg)

Điểm mình thích nhất ở Spark là nó **cho mình giành lại quyền kiểm soát bất cứ lúc nào**. Website nào bắt nhập thông tin nhạy cảm thì agent dừng lại chờ — không có việc nó tự ý bấm vào ô số thẻ của bạn.

Về mặt tài khoản: Spark cần gói Google AI Pro trở lên. Từ cuối tháng 7/2026, tính năng này mở cho hơn 160 quốc gia, thay vì chỉ Ultra với giá $249/tháng. Nếu bạn ở Việt Nam mà vẫn không thấy nút Spark, khả năng cao là tài khoản Google của bạn chưa bật, chứ không phải khu vực bị chặn.

| Cần chuẩn bị | Chi tiết |
| --- | --- |
| Gói Google | AI Pro trở lên, đã bật Spark |
| Đầu vào | `gemini.google.com/spark` |
| Công cụ trong task | Skills & apps → **Remote browser** |
| Thẻ thanh toán | Thẻ quốc tế, **đứng tên chính bạn** |

## Bốn bước thực hiện

### Bước 1: Mở Spark và bật Remote browser

![Giao diện Describe a task của Gemini Spark](/images/posts/dang-ky-muse-khong-can-fake-vpn/spark-describe-task.jpg)

Vào `gemini.google.com/spark`, tạo một task mới rồi dán mô tả việc cần làm. Trong work panel, chọn **Skills & apps → Remote browser**. Một tab browser cloud mở ra và bạn xem trực tiếp được mọi bước.

Lưu ý thực tế: lần đầu tiên mở browser, Chrome của bạn sẽ hỏi có cho phép kết nối với Spark không. Cứ đồng ý.

### Bước 2: Dán prompt điều khiển

![Spark hỏi xin phép trước khi điều khiển website](/images/posts/dang-ky-muse-khong-can-fake-vpn/spark-allow-browser.jpg)

Lần đầu Spark luôn hỏi *"Let Gemini interact with websites for you?"* với hai nút **Allow / Don't allow**. Bấm **Allow** — không có nút này thì nó không điều khiển được browser.

Rồi dán nguyên khối prompt này. **Giữ nguyên prompt, đừng tự diễn giải lại** — agent hay đi sai chỗ khi prompt bị viết lại:

```text
Mở http://muse.ai/join trong trình duyệt từ xa của bạn.
Cho Spark tương tác với website → sau đó tiếp quản trình duyệt và hoàn tất đăng ký Muse.
Nếu được yêu cầu xác minh tuổi thì làm theo hướng dẫn.
Có thể cần thẻ thanh toán được hỗ trợ và đứng tên chính bạn.
```

Enter. Spark tự mở trang, tự điền các trường đơn giản, tự dừng lại ở những chỗ cần bạn.

### Bước 3: Tiếp quản ở bước xác minh tuổi và bước thẻ

Đến bước xác minh tuổi hoặc nhập thẻ, Spark sẽ dừng lại và chờ bạn. Thao tác như sau:

1. Tap icon **Remote computer** ở đầu thread để xem browser của nó.
2. Bấm **Take over task** → xác nhận lần nữa **Take over task**.
3. Tự bấm phần còn lại: xác minh tuổi, nhập số thẻ.
4. Bấm **Go back to Gemini** ở phía trên để trả quyền điều khiển lại.

Nguyên tắc quan trọng nhất ở bước này: **thẻ phải đứng tên bạn**. Thẻ tín dụng không tên, thẻ ảnh, hay thẻ của người khác thì rất dễ bị từ chối — và tài khoản mới, tài khoản cũ luôn. Nhiều anh em ở VN dùng thẻ có bật thanh toán quốc tế là chạy được.

### Bước 4: Đổi mã mời trong 48 giờ

Đăng ký xong thì vào ngay **Settings → Redeem Invite Code**, dán mã:

```text
NRSMG2
```

Đổi mã trong vòng **48 giờ kể từ lúc đăng ký**, quá là mất. Đổi xong thì bạn và người bạn giới thiệu cùng nhận **1 tỷ token Muse**.

## Xử lý nhanh mấy lỗi hay gặp

| Lỗi | Nguyên nhân | Xử lý |
| --- | --- | --- |
| *"We couldn't continue with that account"* | IP bị chặn hoặc không đủ điều kiện khu vực | Làm lại từ bước 2, chắc chắn đang dùng **Remote browser** chứ không phải Chrome của bạn |
| Dừng ở bước xác minh tuổi | Agent không tự xác minh được | Tự **Take over task** rồi làm tiếp |
| Bị từ chối ở bước thẻ | Thẻ không hợp lệ hoặc không đứng tên | Đổi sang thẻ quốc tế đứng tên chính mình |
| Không đổi được mã | Đã quá 48 giờ | Mã dùng một lần, không có cách khôi phục — phải đăng ký tài khoản mới |
| Không thấy nút Remote browser | Tài khoản chưa bật Spark | Kiểm tra gói Google AI Pro/Ultra và khu vực tài khoản |

## Mấy điều nên biết trước khi làm

- **Cách này không đảm bảo 100%.** Nó còn phụ thuộc khu vực tài khoản Google của bạn và chính sách riêng của Meta. Có thể không chạy ở mọi nơi, và Meta có thể thay đổi bất cứ lúc nào.
- **Đừng nhập thông tin nhạy cảm vào ô chat.** Chính tài liệu của Google cũng khuyến cáo không nhập thông tin đăng nhập hay số thẻ vào thread — cứ dùng **Take over task** rồi tự nhập trong browser.
- **Đây là vùng xám.** Hãy dùng tài khoản thật và dữ liệu thật, và hiểu rằng một tài khoản đăng ký bằng đường vòng thì rủi ro bị rà soát là của chính bạn.
- **Chỉ đăng ký thôi, đừng nạp tiền vội.** Phần lớn nhu cầu Meta nói là miễn phí. Dùng thử vài ngày rồi hãy quyết định.

## Tóm tắt

1. Vào `gemini.google.com/spark`, tạo task, mở **Skills & apps → Remote browser**.
2. Bấm **Allow** ở bước xin phép, rồi dán prompt mở `http://muse.ai/join`.
3. Gặp bước xác minh tuổi hoặc nhập thẻ thì **Take over task**, tự làm, rồi **Go back to Gemini**.
4. Vào **Settings → Redeem Invite Code**, dán mã mời trong vòng 48 giờ.

Còn một điều nữa là mã mời này mình tự đăng ký trước, nên nếu bạn vào được từ code của mình thì cả hai cùng nhận 1 tỷ token. Cảm ơn nhà tài trợ Muse đã mua giúp mình một tỷ token nói chung. 😄

## Nguồn tham khảo

- [Gemini Apps Help — Take over remote browser tasks](https://support.google.com/gemini/answer/17094507)
- [Meta Newsroom — Introducing Muse: The World's First Personal AI Agent](https://about.fb.com/news/2026/09/introducing-muse-personal-ai-agent)
- [9to5Google — Gemini Spark can now use Chrome to auto browse](https://9to5google.com/2026/07/30/gemini-spark-chrome-auto-browse)

### Nguồn ảnh trong bài

| Ảnh | Nguồn |
| --- | --- |
| Muse tự đặt vé, chọn chỗ ngồi | Meta, qua [Axios](https://www.axios.com/2026/09/08/meta-debuts-muse-personal-ai-agent) |
| Muse trên App Store, đề tuổi 18+ | [Fortune](https://fortune.com) / Getty Images |
| Spark remote browser và nút Take over task | [AlphaSignal](https://alphasignal.ai/news/google-s-gemini-spark-now-controls-your-real-chrome-browser-to-run-web-tasks) |
| Giao diện Describe a task | [AI Agents Library](https://www.aiagentslibrary.com) |
| Màn hình xin phép điều khiển website | [WIRED](https://www.wired.com) |
| Ảnh bìa | [Mashable](https://mashable.com) / Samuel Boivin (Getty Images) |
