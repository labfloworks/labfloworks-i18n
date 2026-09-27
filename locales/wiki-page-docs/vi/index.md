---
title: FloWorks
description: Phòng thí nghiệm trực quan đa năng cho xử lý tín hiệu, thiết bị khoa học và tự động hóa.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Phòng thí nghiệm trực quan đa năng cho tín hiệu, thiết bị và AI

Xử lý khoa học • DSP • VISA/SCPI • Tự động hóa • Machine Learning

![Ảnh chụp màn hình FloWorks](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[Bắt đầu với FloWorks](getting-started.md){ .md-button }
[Giải phẫu giao diện](interface-anatomy.md){ .md-button .md-button--primary }
[Triết lý](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## FloWorks là gì?
FloWorks là một **phòng thí nghiệm trực quan mã nguồn mở** (Python + PySide6), nơi bạn xây dựng hệ thống bằng cách kt nối các khối (nút) thay vì viết từng dòng code.

Hãy tưởng tượng một Canvas kỹ thuật số, nơi bạn nối các máy phát tín hiệu, bộ lọc toán học, bộ điều khiển phần cứng (VISA/SCPI) và các mô hình trí tuệ nhân tạo bằng các dây cáp ảo. Mọi thứ đều dựa trên **luồng dữ liệu**: bạn nối đầu ra của một khối với đầu vào của khối khác để xử lý thông tin, tự động hóa thiết bị hoặc phân tích kết quả theo thời gian thực.

Phần mềm hướng đến sinh viên, nhà nghiên cứu, kỹ sư và bất kỳ ai muốn thử nghiệm, học hỏi hoặc tạo nguyên mẫu các hệ thống phức tạp một cách trực quan, không bị rào cản bởi lập trình truyền thống.

### Sứ mệnh
Tập trung hóa quy trình làm việc thử nghiệm trong một công cụ trực quan, mở và dễ tiếp cận. Chúng tôi muốn người dùng tập trung vào *thử nghiệm và khám phá*, chứ không phải đấu tranh với sự phức tạp của phần mềm hay chi phí bn quyền.

### Tầm nhìn
Một thế giới nơi rào cản duy nhất giữa một ý tưởng thử nghiệm và việc thực hiện nó là sự tò mò của người thử nghiệm. FloWorks mong muốn trở thành nền tảng tham chiếu cho khoa học và kỹ thuật, được xây dựng bởi và cho cộng đồng toàn cầu, phá bỏ những bức tường của các công cụ độc quyền.

### Nguyên tắc
* **Tự do Tuyệt đối (MIT License):** Kiến thức và công cụ phải tự do và có thể tiếp cận được với tất cả mọi người.
* **Mở rộng Vô hạn:** Nếu thiếu một khối, bất kỳ ai cũng có thể tạo và tích hợp nó vào hệ sinh thái bằng Python.
* **Minh bạch Trực quan:** Mỗi bước của quy trình đều có thể được kiểm tra, gỡ lỗi và hiểu một cách trực quan.
* **Kết nối Thế giới Thực:** Không chỉ là mô phỏng; cho phép điều khiển trực tiếp thiết bị khoa học thực tế ngay trên Canvas.

Khác với các công cụ đóng hoặc chuyên biệt cao, FloWorks được thiết kế như một hệ sinh thái mô-đun, có thể mở rộng, trong đó mỗi thành phần là một nút có thể tái sử dụng và kết nối được.

---

## Khả năng chính

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Hệ sinh thái Nút mở rộng**

    Danh mục kỹ thuật được tổ chức theo tầng: Nguồn, Xử lý, Điều khiển, Phần cứng và Scripting.

    Đăng ký động, tuần tự hóa khai báo và hợp đồng rõ ràng cho phát triển nhanh.

    [:material-arrow-right: Tài liệu tham khảo Nút](node-reference.md)

-   **:material-connection: Tích hợp VISA/SCPI**

    Kết nối trực tiếp với máy hiện sóng, máy đo LCR và máy phát tín hiệu.

    Hỗ trợ đa kênh, mô phỏng tích hợp qua `PyVISA-py` và quản lý tường lửa ở chế độ di động.

    [:material-arrow-right: Thiết bị](instrumentation.md)

-   **:material-package-variant-closed: Định dạng di động `.sflow`**

    Chuẩn ZIP tự chứa với đồ thị JSON, mảng `.npy` và siêu dữ liệu.

    Tính tái lập hoàn toàn của thí nghiệm và chuẩn hóa DPI tự động.

    [:material-arrow-right: Định dạng .sflow](sflow-format.md)

-   **:material-translate: Quốc tế hóa nâng cao**

    Thay đổi ngôn ngữ khi đang chạy mà không cần khởi động lại ứng dụng.

    Bản dịch JSON phân cấp và lưu trữ tùy chọn.

    [:material-arrow-right: Hướng dẫn i18n](translation-guide.md)

-   **:material-tools: SDK và Phát triển nhanh**

    Mẫu cơ sở (`template_node.py`), mixin tuần tự ha và hướng dẫn từng bước.

    Kiến trúc sẵn sàng cho plugin và mở rộng cộng đồng.

    [:material-arrow-right: Tạo Nút](adding-a-new-node.md)

</div>

---

## Lĩnh vực ứng dụng

| Lĩnh vực | Ứng dụng |
|----------|----------|
| 🎓 **Giáo dục** | Vật lý, điện tử, toán học, phòng thí nghiệm STEM |
| ⚙️ **Kỹ thuật** | DSP, điều khiển, thiết bị, đo lường |
| 🤖 **AI** | ML, tối ưu hóa, pipeline lai |
| 🔬 **Nghiên cứu** | Tự động hóa và thu thập dữ liệu |
| 🔌 **Phần cứng** | VISA/SCPI, mô phỏng và hệ thống lai |

---

!!! tip "Mới sử dụng FloWorks?"

    Hãy bắt đầu với phần **Bắt đầu với FloWorks**, sau đó đọc **Giải phẫu giao diện** để hiểu kiến trúc giao diện đồ họa, và cuối cùng khám phá **Kiến trúc Tổng quát** để hiểu luồng dữ liệu và cấu trúc của công cụ tôpô.

---

!!! info "Mô hình Open Core"

    FloWorks sử dụng mô hình **Free/Open Core** theo giấy phép **MIT License**.

    Nhân cốt lõi vẫn tự do và mở, trong khi các tiện ích mở rộng doanh nghiệp, giáo trình hoặc marketplace trong tương lai sẽ là tùy chọn.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Xử lý trực quan • Thiết bị • Khoa học • AI

<small>Tài liệu được xây dựng bằng MkDocs Material</small>

</div>
