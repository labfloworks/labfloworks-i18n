---
title: Tài liệu tham khảo kỹ thuật Nút
description: Danh mục cập nhật, hợp đồng mở rộng và khả năng nâng cao của hệ thống nút FloWorks
---

# 🧩 Tài liệu tham khảo kỹ thuật Nút

FloWorks không phụ thuộc vào một danh mục tĩnh. Phần mềm sử dụng **hệ thống đăng ký động** dựa trên các hợp đồng rõ ràng. Điều này cho phép mở rộng nền tảng mà không cần chạm vào công cụ tôpô. Dưới đây là danh mục đã triển khai, các khả năng kỹ thuật thực tế và giao thức mở rộng an toàn.

---

## 📂 Các danh mục lõi

=== "📦 Xem theo tầng"
    <div class="grid cards" markdown>

    - **📥 Nguồn/Đầu vào**
      Tạo hoặc thu các tín hiệu ban đầu. Hỗ trợ mô phỏng tích hợp, phần cứng thực (VISA/SCPI) và chế độ đa kênh.
    - **⚙️ Xử lý**
      Biến đổi, kết hợp hoặc phân tích dữ liệu. Bảo toàn chiều và tự động nội suy khi cần.
    - **🔀 Điều khiển/Luồng**
      Phân nhánh, lặp hoặc điều kiện hóa Chạy. Bao gồm hỗ trợ gốc cho tín hiệu kích hoạt.
    - **🐍 Scripting/Nâng cao**
      Thực thi mã Python động với các cng tham số (`# @param`), cổng động (`# @input`/`# @output`) và lưu trạng thái (`persist`).
    - **🔌 Phần cứng/Thiết bị**
      Giao diện cho máy hiện sóng, máy đo LCR và máy phát tín hiệu.
    - **📤 Đầu ra/Xuất**
      Hiển thị, xuất hoặc lưu trữ kết quả. Hỗ trợ chủ đề trực quan, hồ sơ người dùng và định dạng chuyên nghiệp (PNG/PDF/SVG).

    </div>

---

## 📋 Danh mục kỹ thuật đã triển khai

| Nút | Loại | Trách nhiệm chính | Đặc điểm chính |
|-----|------|-------------------|----------------|
| `SumNode` | Xử lý | Toán tử số học (+, -, *, /) cho hai đầu vào. | Tự động nội suy tín hiệu có độ phân giải khác nhau (FFTs). Bảo toàn chiều. |
| `RhombusNode` | Điều khiển | Điều kiện (phân nhánh Có/Không). | Hai cổng đầu ra. Đánh giá điều kiện theo ngưỡng hoặc logic boolean. |
| `TriggerNode` | Điều khiển | Bộ lặp/bộ tích lũy với kích hoạt bên ngoài. | Nhận `(x, y, "trigger")`. Tích lũy đến N lần lặp và phát kết quả xếp chồng/trung bình. |
| `ScriptNode` | Nâng cao | Môi trường script Python tích hợp. | QScintilla, tự động hoàn thành, `# @param`, cổng động, `persist`, mẫu, bảng điều khiển lỗi, trình thông dịch bên ngoài có timeout. |
| `OscilloscopeNode` | Phần cứng | Thu từ máy hiện sóng (SDS) hoặc máy đo LCR. | Chế độ mô phỏng, hộp thoại tường lửa tích hợp, **hỗ trợ đa kênh** (`out_primary`, `out_secondary`), menu "Hiển thị kênh". |
| `GeneratorNode` | Nguồn | Gửi tín hiệu đến máy phát (SDG) hoặc mô phỏng đầu ra. | Cấu hình điều chế/sweep, hộp thoại mô phỏng tích hợp. |
| `GraphExporterNode` | Đầu ra | Trình xuất đồ thị chuyên nghiệp. | Cấu hình nhấp đúp, trục tùy chỉnh, chủ đề, hồ sơ đã lưu, v.v. |

---

## 🔍 ScriptNode: Khả năng cốt lõi

> **🐍 Môi trường script tích hợp**
>
> - **Trình soạn thảo mã tích hợp:** Làm nổi bật cú pháp cơ bản, đánh số dòng và gập mã.
> - **Bảng tham số động:** Chỉ thị `# @param TÊN : kiểu = giá trị` chèn các điều khiển có thể chỉnh sửa (spinbox, trường văn bản, v.v.) vào bảng bên.
> - **Cổng động:** `# @input tên` và `# @output tên` tạo cổng theo thời gian thực. Script nhận từ điển `inputs` và trả về `outputs`.
> - **Lưu trạng thái:** Từ điển toàn cục `persist` lưu giá trị giữa các lần Chạy.
> - **Mẫu và Nhập/Xuất:** Menu thả xuống với các script cơ bản. Người dùng có thể lưu script vào `nodes/script_node/templates/` hoặc nhập/xuất tệp `.py` bên ngoài.
> - **Bảng điều khiển lỗi tích hợp:** Hiển thị lỗi cú pháp/thực thi với dòng chính xác được chỉ trong trình soạn thảo.
> - **Trợ giúp và i18n:** Chú giải công cụ theo ngữ cảnh, nút `?` với hướng dẫn nhanh và tất cả văn bản sử dụng `tr()` để dịch.
> - **Trình thông dịch bên ngoài có timeout:** Đường dẫn có thể cấu hình (`# @python_path` hoặc nút "Duyệt…"). Thực thi cô lập có giới hạn thời gian và dự phng về trình thông dịch nội bộ.
> - **Tuần tự hóa đầy đủ:** Lưu script, tham số, cổng động và trạng thái `persist`. Khi tải `.sflow`, tự động xây dựng lại cổng và tham số.

---

## 📚 Tài nguyên liên quan

- [📖 Bản đồ mã và kiến trúc](architecture-ii.md) → Trách nhiệm theo mô-đun và luồng công việc.
- [🌐 Hướng dẫn quốc tế hóa (i18n)](i18n.md) → Cách thêm ngôn ngữ và quản lý khóa `tr()`.
- [🛠️ Thêm nút mới (hướng dẫn)](adding-a-new-node.md) → Từng bước với ví dụ thực tế.
- [📦 Hướng dẫn build và phân phối](build.md) → Đóng gói PyInstaller, hooks và chữ ký số.

---

💡 **Thiếu nút trong danh mục này?**
FloWorks được thiết kế để mở rộng. Nếu bạn cần một nút chưa tồn tại, hãy tạo theo hợp đồng `BaseNode` và đăng ký. Cộng đồng và marketplace trong tương lai sẽ liên tục mở rộng hệ sinh thái mà không phá vỡ tính tương thích.
