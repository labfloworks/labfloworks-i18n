---
title: Quản lý dự án
description: Cách lưu, mở, xuất và bảo vệ luồng của bạn trong FloWorks.
---

# 📁 Quản lý dự án

FloWorks lưu các luồng của bạn vào các tệp có phần mở rộng **`.sflow`**. Các tệp này chứa mọi thông tin của dự án: nút, kết nối, cấu hình và ghi chú dán.

---

## Tạo, mở và lưu

| Hành động | Menu | Phím tắt |
|--------|------|-------|
| **Dự án mới** | Tệp → Mới | `Ctrl + N` |
| **Mở dự án** | Tệp → Mở | `Ctrl + O` |
| **Lưu** | Tệp → Lưu | `Ctrl + S` |
| **Lưu thành…** | Tệp → Lưu thành… | `Ctrl + Shift + S` |

**Quy tắc vàng:**
Các luồng **hoàn toàn tương thích với mọi phiên bản** FloWorks (Core, Lite, Pro). Bạn không cần chuyển đổi hay sửa đổi gì: chỉ cần mở và chạy.

---

## Xuất và nhập

- Để **chia sẻ một luồng**, sao chép tệp `.sflow` sang thiết bị khác.
- Để **mang một luồng bên ngoài vào**, sử dụng **Tệp → Mở** và chọn tệp.
- Nếu bạn cần **xuất dữ liệu số** (ví dụ sang CSV), hãy sử dụng công cụ **Bảng tính** trong bảng bên và lưu bảng từ đó.

---

## Khôi phục khi đóng ứng dụng đột ngột

FloWorks **không tự động lưu**. Vì vậy điều quan trọng là:

- Lưu thường xuyên (`Ctrl + S`), đặc biệt trước khi chạy các luồng với phần cứng thực.
- Nếu ứng dụng đóng đột ngột, các thay đổi chưa lưu có thể bị mất.
- Để làm việc hoàn toàn yên tâm, hãy tạo thói quen lưu sau mỗi thay đổi quan trọng.

---

## Tổ chức được khuyến nghị

- Tạo một thư mục cho mỗi dự án hoặc khách hàng, và lưu tất cả các tệp `.sflow` liên quan vào đó.
- Sử dụng **ghi chú dán** trên canvas để ghi chép các phần của luồng.
- Gán **tên mô tả cho các nút** (nhấp đúp  tên) để dễ dàng tìm và hiểu luồng nhiều tuần sau đó.

---

## Các thực hành tốt

- Trước khi chạy một luồng với thiết bị thực, hãy lưu tệp.
- Nếu làm việc theo nhóm, hãy sử dụng hệ thống quản lý phiên bản (Git, bản sao thủ công) để không ghi đè các luồng quan trọng.
- Sao lưu các luồng hiệu chuẩn hoặc chẩn đoán cho các hệ thống quan trọng.

---

> **Lời khuyên:** Một luồng được tổ chức và lưu tốt là nền tảng của công việc chuyên nghiệp trong FloWorks. Đừng đánh giá thấp sức mạnh của một cái tên rõ ràng và một thư mục ngăn nắp.
