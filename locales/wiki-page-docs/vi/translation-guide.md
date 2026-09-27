---
title: Hướng dẫn Quốc tế hóa (i18n)
description: Hướng dẫn từng bước để thêm và quản lý bản dịch trong FloWorks
---

# 🌐 Hướng dẫn Quốc tế hóa (i18n)

Tài liệu này giải thích cách thêm ngôn ngữ mới vào FloWorks và quản lý hiệu quả các tệp bản dịch.

---

## ➕ Cách Thêm Ngôn ngữ Mới

### Bước 1: Tạo Tệp JSON
Điều hướng đến thư mục `locales/`. Sao chép `en.json` và đổi tên bằng mã hai chữ cái [ISO 639-1](https://vi.wikipedia.org/wiki/ISO_639-1) tương ứng (ví dụ: `fr.json` cho tiếng Pháp, `de.json` cho tiếng Đức).

### Bước 2: Dịch các Chuỗi
Mở tệp JSON mới trong trình soạn thảo văn bản.

!!! warning "Không Sửa đổi Khóa"
    **Không bao giờ thay đổi các khóa** (bên trái của mỗi cặp). Chỉ dịch các giá trị (bên phải).

**Gốc (`en.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "File",
    "edit": "Edit"
  }
}
```

**Ví dụ đã Dịch (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Đảm bảo khóa gốc `"language_name"` chứa tên bản ngữ của ngôn ngữ (ví dụ: `"Français"`, `"Deutsch"`, `"Español"`).

### Bước 3: Xác thực JSON
Xác minh rằng tệp là JSON hợp lệ (không có dấu phẩy cuối, dấu ngoặc kép chính xác, các ký tự thoát phù hợp). Bạn có thể sử dụng trình xác thực trực tuyến như [JSONLint](https://jsonlint.com) hoặc chạy:
```bash
python -m json.tool locales/es.json
```

### Bước 4: Kiểm tra Ngôn ngữ Mới
1. Khởi động FloWorks.
2. Đi tới **INFO → Ngôn ngữ** và chọn ngôn ngữ mới.
3. Xác minh rằng tất cả các thành phần giao diện được cập nhật ngay lập tức (menu, panel, hộp thoại, nhãn nút, v.v.).

### Bước 5: Tự động Phát hiện (Tùy chọn)
Nếu ngôn ngữ hệ thống của người dùng khớp với mã ngôn ngữ mới, FloWorks sẽ tự động sử dụng nó trong lần khởi động đầu tiên (miễn là chưa có tùy chọn nào được lưu trước đó trong `QSettings`).

---

## 🌍 Ngôn ngữ Có sẵn
- **Tiếng Anh** (`en`) – Ngôn ngữ cơ sở / dự phòng
- **Tiếng Tây Ban Nha** (`es`)

---

## ⚙️ Lưu ý Quan trọng và Các thực hành Tốt

!!! info "Cơ chế Dự phòng"
    Ngôn ngữ cơ sở là **tiếng Anh**. Nếu thiếu khóa dịch trong tệp ngôn ngữ, FloWorks sẽ tự động sử dụng chuỗi tiếng Anh làm dự phòng.

!!! warning "Ngăn ngừa Tràn UI"
    Giữ các bản dịch ngắn gọn để tránh phá vỡ bố cục. Nếu văn bản dịch dài hơn đáng kể, hãy cân nhắc viết tắt hoặc dựa vào hệ thống chủ đề để xử lý tỷ lệ động.

!!! tip "Bảo toàn HTML và Placeholder"
    - **Thẻ HTML:** Giữ nguyên tất cả các thẻ HTML (ví dụ: `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Placeholder:** Giữ nguyên cú pháp `{variable}` ở nơi sử dụng (ví dụ: `"Idioma cambiado a: {name} ({code})"`). Không sắp xếp lại hoặc xóa chúng.

---

## 🔗 Tài liệu Liên quan
- [📖 Bản đồ Mã và Kiến trúc](architecture-ii.md)
- [📦 Hướng dẫn Build và Phân phối](build.md)
- [🧩 Tài liệu tham khảo Nút](node-reference.md)
