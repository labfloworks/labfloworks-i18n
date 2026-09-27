# Triết lý

## Tổng quan

FloWorks là một ứng dụng máy tính để bàn cho phép bạn tạo các chuỗi xử lý tín hiệu thông qua sơ đồ luồng trực quan.
Kéo, kết nối và cấu hình các nút; kết quả được tính toán và hiển thị theo thời gian thực.
Làm việc với tín hiệu mô phỏng hoặc kết nối thiết bị thực (máy hiện sóng, bộ tạo tín hiệu, đồng hồ LCR) mà không cần viết mã, mặc dù bạn có một môi trường viết kịch bản mạnh mẽ nếu muốn mở rộng chức năng.

---

## Các tính năng chính

- **Sơ đồ tương tác** – Xây dựng luồng công việc của bạn bằng cách nối các nút bằng các đường đại diện cho luồng dữ liệu.
- **Xử lý thời gian thực** – Mỗi sửa đổi được phản ánh ngay lập tức trên đồ thị và hình ảnh hóa.
- **Mô phỏng và phần cứng thực**  Tạo tín hiệu thử nghiệm hoặc thu thập dữ liệu trực tiếp từ thiết bị phòng thí nghiệm.
- **Nút viết kịch bản nâng cao** – Tích hợp mã Python của riêng bạn với trợ giúp tự động hoàn thành, tham số động có thể chỉnh sửa và bộ nhớ liên tục giữa các lần thực thi.
- **Hình ảnh hóa chuyên nghiệp** – Tín hiệu, phổ, phổ spectrogram và đồ thị chất lượng cao sẵn sàng để xuất.
- **Đa ngôn ngữ** – Giao diện phát hiện ngôn ngữ hệ thống và cho phép chuyển đổi giữa tiếng Tây Ban Nha, tiếng Anh và các ngôn ngữ khác bất cứ lúc nào.
- **Chủ đề trực quan** – Chế độ tối, sáng và tương phản cao để phù hợp với sở thích hoặc nhu cầu hỗ trợ tiếp cận của bạn.
- **Quản lý dự án toàn diện** – Lưu công việc của bạn vào các tệp `.sflow` và khôi phục chính xác như bạn đã để lại, với hoàn tác và làm lại không giới hạn.

---

## Cách làm việc với FloWorks

### Nút
Một nút là một phần của quá trình xử lý. Chúng được tổ chức thành ba loại:

- **Nguồn** – Chèn tín hiệu vào đầu luồng. Ví dụ: máy hiện sóng (thực hoặc mô phỏng), bộ tạo hàm hoặc phép toán toán học.
- **Xử lý** – Biến đổi dữ liệu. Cộng, trừ, điều kiện, bộ lọc… bao gồm một nút đặc biệt để viết kịch bản Python của riêng bạn.
- **Đích** – Hiển thị hoặc xuất kết quả. Trình xem đồ thị và trình xuất đồ thị chuyên nghiệp là những nút được sử dụng nhiều nhất.

### Kết nối
Các liên kết giữa các nút được vẽ dưới dạng đường cong mượt hoặc đường trực giao. Hoạt ảnh luồng cho bạn biết hướng của dữ liệu tại mọi thời điểm. Hệ thống tự động sắp xếp các cáp để chúng không chồng chéo.

### Hình ảnh hóa
Mỗi khi một nút tạo ra tín hiệu, nó có thể được xem trong bảng đồ thị tích hợp. Bạn có thể khám phá các biểu diễn khác nhau (dạng sóng, phổ, phổ spectrogram) và điều chỉnh tỷ lệ bằng chuột.

---

## Các nút nổi bật
Đây là các nút tối thiểu không thể thiếu, cần thiết để triết lý của chương trình có ý nghĩa.

### Nút tạo tín hiệu
Nguồn tín hiệu có thể tạo các mô phỏng dạng sóng tùy chỉnh do người dùng xác định. Cho phép chọn hoặc nhập dạng sóng mong muốn từ menu ngữ cảnh.

### Nút viết kịch bản
Một môi trường lập trình đầy đủ bên trong sơ đồ:

- **Trình soạn thảo có làm nổi bật cú pháp**, tự động hoàn thành và bảng điều khiển lỗi.
- **Tham số động** – Xác định các biến có thể chỉnh sửa từ bảng nút mà không cần sửa đổi mã.
- **Cổng có thể cấu hình** – Thêm đầu vào và đầu ra bổ sung trực tiếp từ trình soạn thảo.
- **Trạng thái liên tục** – Lưu các giá trị giữa các lần thực thi; mọi thứ được lưu cùng với dự án.

### Trình xuất đồ thị
Một nút đích tạo hình ảnh chất lượng cao cho báo cáo hoặc ấn phẩm. Cho phép cấu hình kích thước, độ phân giải, định dạng, trong số các tùy chọn khác.

---

## Tùy chỉnh

- **Ngôn ngữ** – Ứng dụng tự động phát hiện ngôn ngữ hệ thống và lưu tùy chọn của bạn. Bạn có thể thay đổi nó từ menu mà không cần khởi động lại.
- **Giao diện** – Chọn chủ đề tối, sáng hoặc tương phản cao tùy theo ánh sáng xung quanh hoặc nhu cầu thị giác của bạn.

---

## Dự án và tệp

Lưu toàn bộ sơ đồ của bạn vào một tệp `.sflow`.
Khi mở, bạn sẽ khôi phục tất cả các nút, kết nối, kịch bản, tham số và cấu hình hình ảnh hóa.
Các thao tác hoàn tác và làm lại cho phép bạn thử nghiệm mà không sợ mất công việc trước đó.

---

FloWorks được thiết kế để bạn tập trung vào phân tích tín hiệu chứ không phải các chi tiết kỹ thuật của việc triển khai. Kéo, kết nối và khám phá.
