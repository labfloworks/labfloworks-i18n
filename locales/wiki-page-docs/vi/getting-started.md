---
title: Bắt đầu với FloWorks
description: Hướng dẫn nhanh để thiết lập môi trường, chạy luồng đầu tiên và truy cập phiên bản di động.
---

# 🚀 Bắt đầu với FloWorks

Hướng dẫn này sẽ đưa bạn từ con số không đến khi có luồng xử lý tín hiệu đầu tiên đang chạy. FloWorks là một ứng dụng sơ đồ luồng cho tín hiệu, được xây dựng bằng Python và PySide6, hỗ trợ phần cứng thực (VISA/SCPI), mô phỏng tích hợp, viết kịch bản nâng cao và thay đổi ngôn ngữ khi đang chạy.

---

## 🌊 Luồng ví dụ đầu tiên của bạn

Chúng ta sẽ tạo một luồng đơn giản: tạo một tín hiệu hình sin và hiển thị nó theo thời gian thực.

1. **Thêm nút**
   Trên thanh công cụ phía trên, chọn `Nguồn` → chọn `Bộ tạo tín hiệu Nâng cao`. Sau đó, từ `Xử lý` → chọn, ví dụ, `Phổ`.
2. **Kết nối**
   Nhấn `Ctrl+Nhấp` vào cổng đầu ra (`phải`) của bộ tạo. Sau đó, nhấp vào cổng đầu vào (`trái`) của máy hiện sóng. Hoặc đơn giản là nhấp vào cổng đầu ra và kéo (giữ nhấn) đến cổng đầu vào của nút tiếp theo.
3. **Cấu hình (tùy chọn)**
   Nhấp vào một nút, một trình chỉnh sửa tham số sẽ xuất hiện ở bảng bên trái để điều chỉnh điều kiện hoạt động của nút đã chọn. Ở phía dưới có một đồ thị trực quan biểu diễn dữ liệu được tạo ra hoặc thu thập bởi các nút.
4. **Chạy**
   Nhấn `F5` hoặc nút ▶ trên thanh công cụ. Công cụ topo sẽ tính toán thứ tự thực thi, xử lý dữ liệu và bạn sẽ thấy sóng trong bảng đồ họa. **Bộ kết nối sẽ được tạo hiệu ứng động cho thấy luồng đang hoạt động!**

---

## 🧠 Hiểu về các cổng: phân loại theo màu

Trong FloWorks, mỗi cổng thuộc về một **danh mục chức năng** được xác định bằng một màu. Các kết nối hợp lệ được thực hiện **luôn luôn giữa các cổng cùng màu**: đầu ra của một danh mục chỉ được kết nối với đầu vào của cùng danh mục đó. Ngoài ra, đường kết nối tự động nhận màu của các cổng mà nó nối, giúp đọc trực quan dễ dàng hơn.

| Loại | Màu | Mục đích | Ví dụ điển hình |
|------|-------|-----------|----------------|
| `control` | Trắng | Luồng điều khiển / kích hoạt. | Tín hiệu khởi động đến một nút thu thập. |
| `exec` | Xám | Thực thi các thao tác hoặc bước. | Kích hoạt một hàm hoặc lệnh gọi lại. |
| `data` | Xanh lá | Dữ liệu chung / tín hiệu số. | Đầu ra của một bộ tạo hoặc cảm biến. |
| `int` | Xanh dương | Số nguyên. | Chỉ số, kích thước bộ đệm, ID. |
| `float` | Xanh lơ | Số dấu phẩy động. | Biên độ, tần số, ngưỡng. |
| `string` | Tím | Chuỗi văn bản. | Tên tệp, nhãn. |
| `bool` | Hồng | Giá trị Boolean (`True`/`False`). | Cờ trạng thái, cho phép. |
| `array` | Xanh đậm | Mảng / vectơ. | Tín hiệu đa kênh, danh sách mẫu. |
| `trigger` | Cam | Kích hoạt / sự kiện rời rạc. | Xung đồng bộ, cạnh. |

**Quy tắc vàng:**

- Chỉ kết nối các cổng có **màu chính xác giống nhau** (đầu ra ↔ đầu vào cùng danh mục).
- Hệ thống ngăn chặn các kết nối không hợp lệ và làm nổi bật trực quan các cổng tương thích khi kéo.
- Đường kết nối nhận màu của các cổng đã kết nối; nhờ đó mỗi tuyến được nhận diện trong nháy mắt.

**Triết lý FloWorks:**
Các cổng dữ liệu **bảo toàn số chiều** của mảng. Không bao giờ áp dụng làm phẳng tự động: nếu ma trận đi vào, ma trận đi ra, duy trì tính toàn vẹn của các tín hiệu đa chiều của bạn.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Điều hướng trên Canvas

Làm chủ không gian làm việc với các thao tác sau:

| Thao tác | Cách thực hiện |
|--------|--------------|
| **Thu phóng** | Con lăn chuột hoặc `Ctrl + con lăn` |
| **Di chuyển (pan)** | Giữ `Phím cách` và kéo, hoặc dùng nút giữa chuột |
| **Chọn một nút** | Nhấp trái vào nút |
| **Chọn nhiều** | Kéo hình chữ nhật bằng nhấp trái, hoặc `Ctrl + nhấp` vào nhiều nút |
| **Di chuyển vùng chọn** | Kéo bất kỳ nút nào trong vùng chọn |
| **Mở cấu hình** | Nhấp đúp vào nút |

**Mẹo:** Bảng bên trái tự động cập nhật theo cấu hình của nút đã chọn, không cần mở thêm cửa sổ.

---

## ⚡ Phím tắt và thao tác nâng cao

Các phím tắt này biến người dùng thông thường thành **người dùng chuyên nghiệp**:

| Phím tắt | Thao tác |
|-------|--------|
| `F5` | Chạy luồng |
| `Ctrl + S` | Lưu dự án (`.sflow`) |
| `Ctrl + Nhấp` | Kết nối các nút (nhấp cổng đầu ra → nhấp cổng đầu vào) |
| `Ctrl + C` / `Ctrl + V` | Sao chép / dán các nút đã chọn |
| `Ctrl + Z` / `Ctrl + Y` | Hoàn tác / làm lại |
| `Ctrl + Shift + L` | Tự động sắp xếp các nút trên canvas |
| `Del` | Xóa các nút đã chọn |
| `Ctrl + A` | Chọn tất cả các nút |

**Thao tác nâng cao:**

- **Nhân bản một luồng:** chọn một nhóm nút, `Ctrl + C`, `Ctrl + V` và kéo bản sao đến vùng khác.
- **Dọn lưới:** dùng `Ctrl + Shift + L` để sắp xếp toàn bộ canvas chỉ bằng một lệnh.
- **Kết nối nhanh:** `Ctrl + Nhấp` vào cổng đầu ra rồi nhấp thường vào cổng đầu vào; FloWorks sẽ tự động vẽ kết nối.

---

## 🎨 Tùy chỉnh môi trường

FloWorks thích ứng với bạn, chứ không phải bạn phải thích ứng với nó.

### Thay đổi chủ đề khi đang chạy
Từ thanh trên cùng, menu **Xem → Chủ đề**, chọn giữa sáng, tối hoặc khác. Giao diện thay đổi **tức thì**, không cần khởi động lại hay mất luồng làm việc.

### Cỡ chữ
Trong **Xem → Cỡ chữ** chọn giá trị định sẵn hoặc tùy chỉnh. Toàn bộ giao diện điều chỉnh ngay lập tức.

### Ngôn ngữ
Trong **Xem → Ngôn ngữ** chọn ngôn ngữ mong muốn. FloWorks hỗ trợ **thay đổi khi đang chạy**: menu, nút và thông báo được dịch mà không cần khởi động lại ứng dụng.

---

## ❗ Giải quyết các vấn đề thường gặp

| Vấn đề | Nguyên nhân có thể | Giải pháp |
|----------|---------------|----------|
| Luồng không chạy | Có nút chưa được cấu hình hoặc kết nối bị đứt | Kiểm tra tất cả các nút có tham số hợp lệ và kết nối giữa các cổng tương thích |
| Đồ thị không cập nhật | Luồng đang tạm dừng hoặc không có dữ liệu chảy | Đảm bảo đã nhấn `F5` hoặc ▶, và các nút nguồn đang tạo dữ liệu |
| Không thể kết nối hai nút | Các cổng thuộc loại khác nhau | Xác minh cả hai cổng đều là **dữ liệu** hoặc cả hai đều là **điều khiển** |
| Chương trình chậm với luồng lớn | Quá nhiều nút hoặc đồ thị thời gian thực | Đóng các bảng phân tích không dùng đến hoặc giảm tần số lấy mẫu của các nút nguồn |
| Chủ đề không thay đổi | Một số tiện ích có thể chưa được đăng ký | Khởi động lại ứng dụng và thử lại (sẽ được khắc phục trong các phiên bản tương lai) |

---

## 🧪 Ví dụ thực hành nhanh

Ngoài luồng hình sin ban đầu, hãy thử các dự án nhỏ này để làm chủ FloWorks:

| Ví dụ | Các nút liên quan | Kết quả mong đợi |
|---------|-------------------|--------------------|
| **Bộ lọc thông thấp** | Bộ tạo → Bộ lọc → Bộ xem đồ thị | Bạn sẽ thấy tín hiệu đã lọc |
| **Thu thập mô phỏng** | Bộ tạo → Bộ phân tích THD | Giá trị méo hài của tín hiệu |
| **Kiểm tra th công** | Bộ tạo → Thanh tra dữ liệu | Bảng với các giá trị tín hiệu do bộ tạo gửi |
| **So sánh tín hiệu** | Hai bộ tạo → Bộ cộng → Bộ xem đồ thị | Kết quả phép toán (cộng, trừ, nhân hoặc chia) của hai sóng trên một đồ thị |

Mỗi luồng này có thể được lắp ráp trong chưa đầy một phút, chứng tỏ sự linh hoạt của FloWorks so với viết mã truyền thống.

---

## 📚 Tiếp theo là gì?

| Tài nguyên | Mô tả |
|---------|-------------|
| [🗺️ Hướng dẫn Giải phẫu Giao diện Chính](interface-anatomy.md) | Hiểu kiến trúc và triết lý của giao diện đồ họa |
| [🗺️ Bản đồ Mã và Kiến trúc](philosophy.md) | Cấu trúc đầy đủ, các trình quản lý, hợp đồng và DPI-Awareness. |
| [🧩 Tài liệu tham khảo Kỹ thuật về Nút](node-reference.md) | Danh mục, `ScriptNode`, đa kênh và cách mở rộng hệ thống. |
| [🌐 Hướng dn Quốc t hóa](translation-guide.md) | Thêm ngôn ngữ, xác thực JSON và quản lý khóa `tr()`. |
| [📦 Hướng dẫn Bản dựng Di động](guia-ejecutable-portable.md) | PyInstaller, hooks, `--onefile`, giải quyết lỗi và chữ ký số. |

---

!!! warning "Lưu ý về khả năng tương thích và sử dụng"
    1. **Phiên bản Python:** Bạn có thể dùng 3.9+ và hệ thống 64 bit.
    2. **Tường lửa Windows:** Nếu dùng phần cứng thực (máy hiện sóng VISA/SCPI), hãy cho phép `FloWorks.exe` trong tường lửa. Ứng dụng sẽ hiển thị hộp thoại tùy chỉnh nếu kết nối bị chặn (hộp thoại hệ điều hành không xuất hiện ở chế độ `--windowed`).
    3. **Phím tắt chính:** `F5` (chạy), `Ctrl+S` (lưu `.sflow`), `Ctrl+Nhấp` (kết nối), `Phím cách+nhấp` (di chuyển tự do), `Ctrl+Shift+L` (bố cục tự động).
    4. **Bảo toàn dữ liệu:** Công cụ **không bao giờ** áp dụng `flatten()` lên mảng. Làm việc với bản sao cục bộ nếu bạn cần vectơ hóa.
