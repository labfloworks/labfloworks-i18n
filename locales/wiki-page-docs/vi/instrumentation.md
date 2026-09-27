---
title: Thiết bị VISA/SCPI
description: Hướng dẫn kết nối, cấu hình và sử dụng phần cứng thực và mô phỏng trong FloWorks thông qua chuẩn VISA/SCPI.
---

# 🔌 Thiết bị VISA/SCPI

FloWorks tích hợp giao tiếp trực tiếp với các thiết bị thí nghiệm thực thông qua giao thức **SCPI** (Standard Commands for Programmable Instruments) trên lớp trừu tượng **VISA** (Virtual Instrument Software Architecture). Ngoài ra, phần mềm còn cung cấp trình mô phỏng thuần Python để phát triển, kiểm thử và chia sẻ luồng mà không cần phần cứng vật lý.

---

## 🌐 VISA/SCPI là gì?

| Công nghệ | Mô tả |
|-----------|-------|
| **VISA** | Lớp chuẩn trừu tượng hóa giao diện vật lý (USB-TMC, Ethernet/LAN, GPIB, RS‑232). Cho phép chuyển từ thiết bị thực sang thiết bị mô phỏng chỉ bằng cách thay đổi chuỗi kết nối. |
| **SCPI** | Ngôn ngữ lệnh ASCII chuẩn hóa để điều khiển máy phát, máy hiện sóng, đồng hồ vạn năng, máy đo LCR, v.v. Các nhà sản xuất mở rộng chuẩn này, nhưng phần cơ bản là phổ quát. |
| **PyVISA** | Backend Python được FloWorks sử dụng. Hỗ trợ `@py` (mô phỏng thuần) và các backend gốc (`@ni`, `@ivi`, `@keysight`, v.v.). |

---

## ⚙️ Cấu hình điển hình

=== "📍 Chuỗi kết nối (Resource String)"
    Định dạng chuẩn VISA:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (Máy hiện sóng USB)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (Cổng nối tiếp RS-232)
    - `GPIB0::1::INSTR` (GPIB legacy)

=== "⏱️ Thời gian chờ và tùy chọn"
    - **Timeout**: Có thể cấu hình bằng ms. Tăng nếu thiết bị yêu cầu đo dài hoặc quét tần số.
    - **Khởi tạo**: Một số nút cho phép chèn lệnh SCPI tùy chỉnh khi kết nối (ví dụ: `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Các nút phần cứng có sẵn

<div class="grid cards" markdown>

- **🔭 Máy hiện sóng SCPI**
  Bắt dạng sóng trong miền thời gian. Hỗ trợ đa kênh, tự động chia tỷ lệ, trigger phần cứng và menu "Hiển thị kênh" để chuyển tín hiệu khi đang chạy.

- **⚡ Máy đo LCR**
  Đo trở kháng, độ tự cảm, điện dung, điện trở và hệ số tổn hao. Trả về `master_payload` với dữ liệu chính và phụ trong một lần thu.

- **🎛️ Máy phát hàm tùy ý**
  Gửi tín hiệu đến phần cứng SDG hoặc mô phỏng đầu ra. Cấu hình điều chế (AM/FM/PM), sweep lineer/logarit, burst và pha.

- **📊 Đồng hồ vạn năng số (DMM)** *(Đang mở rộng)*
  Giao diện SCPI cho phép đo điện áp DC/AC, dòng điện, điện trở và tần số. Tương thích với Keithley, Agilent và Rigol.

- **🔋 Nguồn cấp điện có thể lập trình** *(Đang mở rộng)*
  Điều khiển điện áp/dòng ra với bảo vệ OVP/OCP. Hữu ích cho các bệ thử tự động.

</div>

---

## 🔄 Luồng công việc điển hình

1. **Thêm nút** vào Canvas từ Thanh công cụ (`Nguồn` hoặc `Thiết bị`).
2. **Cấu hình kết nối**: Chọn backend, nhập chuỗi VISA và điều chỉnh timeout/khởi tạo.
3. **Kết nối luồng**: Nối đầu ra thiết bị với các nút xử lý (FFT, bộ lọc, số học) hoặc trực quan hóa.
4. **Chạy (`F5`)**: Công cụ tôpô yêu cầu thu thập, trình điều khiển phân tích phản hồi SCPI và đóng gói dữ liệu.
5. **Trực quan hóa/Xuất**: Dữ liệu chảy qua đồ thị để được xử lý bởi các nút tiếp theo.

---

## 🛠️ Xử lý sự cố

!!! warning "1. VISA không tìm thấy thiết bị (`VI_ERROR_RSRC_NFOUND`)"
    - **Nguyên nhân:** Chuỗi sai, cáp bị ngt hoặc backend không phát hiện thiết bị.
    - **Cách xử lý:** Chạy `pyvisa-shell` hoặc tiện ích của nhà sản xuất (NI MAX, Keysight Connection Expert) để liệt kê tài nguyên hợp lệ. Kiểm tra quyền người dùng.

!!! warning "2. Hết thời gian chờ trong quá trình thu thập"
    - **Nguyên nhân:** Quét chậm, trigger không thỏa mãn hoặc thiết bị đang bận tác vụ khác.
    - **Cách xử lý:** Tăng timeout trong nút. Kiểm tra cấu hình trigger của máy hiện sóng (`AUTO` hoặc `NORMAL`). Dùng `*CLS` khi bắt đầu.

!!! warning "3. Mô phỏng không phản hồi hoặc lỗi"
    - **Nguyên nhân:** `PyVISA-py` chưa được cài đặt hoặc xung đột với backend khác.
    - **Cách xử lý:** `pip install pyvisa-py`. Trong nút, chọn rõ ràng `@py` làm backend.

!!! warning "4. Lỗi SCPI (`Command Error`, `Execution Error`)"
    - **Nguyên nhân:** Lệnh không được firmware hỗ trợ hoặc cú pháp sai.
    - **Cách x lý:** Tham khảo hướng dẫn lập trình SCPI của thiết bị. Một số nhà sản xuất yêu cầu tiền tố `:` hoặc ký tự kết thúc `\n`. FloWorks tự động thêm `\n`, nhưng bạn có thể điều chỉnh ký tự kết thúc trong trình điều khiển.

!!! info "5. Tạo nút cho thiết bị không được hỗ trợ"
    - Kế thừa từ `BaseNode` và sử dụng mẫu `DeviceBase` trong `instrument/`.
    - Triển khai trình điều khiển `headless` trả về bộ `(x, y)` hoặc `master_payload`.
    - Làm theo [📘 Hướng dẫn: Thêm nút mới](adding-a-new-node.md) để đăng ký cổng, tuần tự hóa và i18n.

---

## 📚 Tài nguyên liên quan

- [🧩 Tài liệu tham khảo kỹ thuật Nút](node-reference.md) → Chi tiết về `oscilloscope_node`, `generator_node` và hợp đồng tuần tự hóa.
- [📦 Hướng dẫn Build Portable](guia-ejecutable-portable.md) → Quản lý tường lửa, `resource_path()` và đóng gói PyInstaller.
- [📘 Thêm nút mới](adding-a-new-node.md) → Cách mở rộng `instrument/` và đăng ký trình điều khiển tùy chỉnh.
- [📄 Định dạng `.sflow`](sflow-format.md) → Cách lưu cấu hình phần cứng và các mảng đã thu.
