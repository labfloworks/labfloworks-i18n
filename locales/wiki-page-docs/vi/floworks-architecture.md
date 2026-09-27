---
title: Kiến trúc FloWorks
description: Tổng quan về các thành phần và hoạt động bên trong cho người dùng cuối
---

# Kiến trúc FloWorks – Góc nhìn dành cho người dùng

FloWorks là một ứng dụng máy tính để bàn cho phép bạn xây dựng các chuỗi xử lý tín hiệu thông qua sơ đồ luồng. Kết nối các khối (nút) trên một canvas tương tác và xem kết quả theo thời gian thực. Để làm điều này khả thi, ứng dụng được tổ chức thành nhiều mô-đun làm việc cùng nhau. Dưới đây là giải thích, không đi sâu vào chi tiết kỹ thuật, về chức năng của từng phần và cách chúng liên quan đến nhau.

---

## Cấu trúc tổng quát

Ứng dụng bao gồm các khu vực chức năng sau:

| Khu vực | Chức năng |
|------|------------|
| **Khởi động và cửa sổ chính** | Khởi động chương trình, hiển thị cửa sổ, menu và điều phối mọi thao tác của người dùng. |
| **Công cụ thực thi** | Tính toán thứ tự thực thi của các nút, phát hiện phụ thuộc và vòng lặp, truyền dữ liệu từ nút này sang nút khác. |
| **Cảnh và sơ đồ** | Quản lý canvas nơi bạn đặt các nút, các kết nối giữa chúng, ghi chú dán và các thao tác hoàn tác/làm lại. |
| **Nút và xử lý** | Chứa tất cả các loại khối bạn có thể sử dụng: nguồn tín hiệu, phép toán, tập lệnh tùy chỉnh, xuất đồ thị, v.v. |
| **Kết nối trực quan** | Vẽ các đường nối giữa các nút (đường cong mượt hoặc trực giao), tạo hiệu ứng động để hiển thị luồng dữ liệu và tránh chồng chéo. |
| **Giao diện người dùng** | Bao gồm khung nhìn sơ đồ (phóng to, di chuyển), thanh công cụ, bảng tham số, bảng phân tích (thống kê, con trỏ) và hộp thoại cấu hình. |
| **Hỗ trợ phần cứng thực** | Cho phép giao tiếp với thiết bị thí nghiệm (máy hiện sóng, máy phát, đồng hồ LCR) để thu thập hoặc tạo tín hiệu thực. |
| **Xuất đồ thị** | Tạo hình ảnh chất lượng cao (PNG, PDF, SVG) với khả năng tùy chỉnh trực quan đầy đủ. |
| **Chủ đề và giao diện** | Thay đổi giao diện toàn bộ ứng dụng (tối, sáng, tương phản cao) và cho phép điều chỉnh kích thước phông chữ. |
| **Ngôn ngữ** | Dịch toàn bộ giao diện sang nhiều ngôn ngữ và cho phép thay đổi ngôn ngữ tức thì. |
| **Quản lý dự án** | Lưu và mở các tệp `.sflow` chứa toàn bộ sơ đồ, bao gồm cấu hình, tập lệnh và kết quả. |
| **Kiểm tra và chẩn đoán** | Các công cụ nội bộ để xác minh mọi thứ hoạt động chính xác (không hiển thị cho người dùng cuối). |

---

## Cách hoạt động bên trong

### Khởi động và cửa sổ chính
Khi mở FloWorks, môi trường đồ họa được thiết lập, mật độ điểm ảnh của màn hình được phát hiện (để mọi thứ hiển thị sắc nét trên mn hình 4K và màn hình thông thường) và cửa sổ chính được hiển thị. Cửa sổ này tập trung mọi thành phần: khu vực vẽ, menu, thanh công cụ và các bảng bên.

### Công cụ luồng
Khi bạn nhấn "Thực thi" (hoặc F5), một công cụ nội bộ duyệt qua tất cả các nút theo đúng thứ tự, tôn trọng các kết nối. Nó biết nút nào phụ thuộc vào nút khác và ngăn chặn các chu trình vô hạn. Nó hỗ trợ một nút nhận nhiều đầu vào có tên và tạo ra nhiều đầu ra. Dữ liệu di chuyển giữa các nút mà không làm mất cấu trúc ban đầu.

### Cảnh sơ đồ
Canvas nơi bạn xây dựng sơ đồ là một cảnh thông minh:

- Cho phép thêm, di chuyển, kết nối và chọn các nút.
- Hỗ trợ hoàn tác và làm lại không giới hạn cho mọi thao tác.
- Bao gồm các ghi chú dán có thể thay đổi kích thước mà bạn có thể đặt tự do và được lưu cùng dự án.
- Có bộ sắp xếp tự động sắp xếp lại các nút một cách gọn gàng (bằng Ctrl+Shift+L).
- Khi lưu, toàn bộ sơ đồ được đóng gói vào tệp `.sflow` chứa mô tả nút, kết nối, ghi chú và dữ liệu số liên quan.

### Kết nối
Các đường nối giữa các nút được vẽ dưới dạng đường cong mượt hoặc đường trực giao. Hiệu ứng động chấm hoặc gạch chỉ hướng luồng dữ liệu. Một bộ quản lý làn tránh việc nhiều kết nối giữa cùng một cặp nút chồng chéo; nó tự động tách chúng để mọi thứ đều dễ đọc.

### Các loại nút
Các nút là các khối xây dựng cơ bản. Chúng được nhóm thành ba loại:

- **Nguồn** – Tạo tín hiệu. Có thể mô phỏng sóng (sin, vuông, v.v.) hoặc đọc dữ liệu thực từ máy hiện sóng hoặc đồng hồ đã kết nối. Hỗ trợ nhiều kênh đồng thời (ví dụ: trở kháng và pha từ LCR).
- **Xử lý** – Biến đổi dữ liệu. Bao gồm các phép toán số học (cộng, trừ, nhân, chia), quyết định có điều kiện (nhánh Có/Không) và một nút tập lệnh mạnh mẽ cho phép bạn viết mã Python của riêng mình với hỗ trợ trực quan.
- **Đích** – Hiển thị hoặc xuất kết quả. Phổ biến nhất là bộ trực quan đồ thị (máy hiện sóng ảo), nhưng cũng có trình xuất đồ thị chất lượng chuyên nghiệp.

Mỗi nút có các cổng đầu vào (trái/trên) và đầu ra (phải/dưới). Khi kết nối một cổng đầu ra với một cổng đầu vào, tín hiệu sẽ chảy qua chúng.

#### Nút tập lệnh nâng cao
Nút tập lệnh đáng được đề cập đặc biệt. Nó được thiết kế cho người dùng nâng cao muốn thêm xử lý tùy chỉnh mà không cần rời khỏi FloWorks. Nó cung cấp:

- Trình soạn thảo có tô sáng cú pháp, tự động hoàn thành và đánh số dòng.
- Khả năng xác định các tham số có thể chỉnh sửa từ bảng nút mà không cần chạm vào mã (ví dụ: một giá trị số sau đó được sử dụng trong tập lệnh).
- Cổng đầu vào và đầu ra động: bằng cách thêm các chú thích đặc biệt trong tập lệnh, bạn có thể tạo các kết nối mới.
- Bộ nhớ liên tục: một biến đặc biệt (`persist`) giữ giá trị của nó giữa các lần thực thi, hữu ích cho bộ tích lũy hoặc máy trạng thái.
- Các mẫu tập lệnh có sẵn và tùy chọn lưu của riêng bạn.
- Hệ thống trợ giúp tích hợp và bảng điều khiển hiển thị lỗi thực thi.

### Giao diện người dùng
Ngoài canvas, giao diện bao gồm:

- **Thanh công cụ** với tất cả các nút được sắp xếp theo danh mục, menu ngôn ngữ, chủ đề và kích thước phông chữ, cùng quyền truy cập vào trình xem nhật ký.
- **Bảng tham số** hiển thị thông tin về các nút đã chọn và làm nổi bật các sự không tương thích tiềm ẩn (chẳng hạn như cố gắng thực hiện phép toán trên các tín hiệu có độ dài khác nhau).
- **Các bảng phân tích có thể gắn**: thống kê (tối đa, tối thiểu, giá trị hiệu dụng), con trỏ A/B để đo sự khác biệt và điểm ngắm có đánh dấu đỉnh.
- **Hộp thoại chào mừng** thích ứng với độ phân giải màn hình của bạn và cung cấp các tùy chọn ban đầu.

### Kết nối với thiết bị thực
Nếu bạn có phần cứng tương thích (máy hiện sóng Siglent SDS, đồng hồ LCR, máy phát SDG), FloWorks có thể giao tiếp với chúng thông qua giao thức tiêu chuẩn VISA/SCPI. Cấu hình được thực hiện từ các bảng cụ thể trong ứng dụng. Khi bạn thu thập tín hiệu đa kênh (ví dụ: biên độ và pha từ LCR), nút nguồn đóng gói tất cả các kênh và bạn có thể chọn kênh nào sẽ hiển thị thông qua một menu ngữ cảnh đơn giản.

### Xuất đồ thị chuyên nghiệp
Nút xuất đồ thị cho phép bạn tạo hình ảnh sẵn sàng cho báo cáo hoặc ấn phẩm. Nhấp đúp vào nó mở ra một hộp thoại với nhiều tùy chọn: bạn có thể tùy chỉnh màu sắc, kiểu đường, nhãn, tỷ lệ, chọn giữa các định dạng PNG, PDF hoặc SVG, và lưu tùy chọn của mình dưới dạng hồ sơ có thể tái sử dụng.

### Tùy chỉnh trực quan
FloWorks bao gồm nhiều chủ đề (tối, sáng, tương phản cao) thay đổi giao diện toàn bộ ứng dụng ngay lập tức mà không cần khởi động lại. Ngoài ra, bạn có thể điều chỉnh kích thước phông chữ toàn cục từ menu (Thông tin → Cỡ chữ) và tất cả các thành phần sẽ thay đổi kích thước cho phù hợp, bao gồm văn bản bên trong nút, ghi chú dán và đồ thị.

### Hệ thống ngôn ngữ
Ứng dụng tự động phát hiện ngôn ngữ hệ thống của bạn khi khởi chạy lần đầu và lưu tùy chọn của bạn. Bạn có thể thay đổi ngôn ngữ bất cứ lúc nào từ menu; tất cả văn bản, menu và trợ giúp sẽ được cập nhật ngay lập tức.

### Dự án và tệp `.sflow`
Toàn bộ công việc của bạn được lưu trong một tệp duy nhất có phần mở rộng `.sflow`. Tệp này chứa toàn bộ sơ đồ: các nút, kết nối, ghi chú, cấu hình, tập lệnh và dữ liệu số đã tạo. Bạn có thể chia sẻ nó với người khác; khi mở trên máy tính khác, ghi chú và các nút sẽ tự động điều chỉnh tỷ lệ để phù hợp với mật độ điểm ảnh của màn hình đó.

---

## Luồng công việc điển hình

1. **Tạo một sơ đồ đơn giản**  
   Chọn một nút nguồn (ví dụ: Máy phát) và một nút Trình xem từ thanh công cụ.  
   Kết nối đầu ra của máy phát với đầu vào của trình xem (Ctrl+nhấp vào cổng đầu ra, sau đó nhấp vào cổng đầu vào).  
   Nhấn F5 để thực thi. Bạn sẽ thấy tín hiệu trên đồ thị.

2. **Sử dụng tập lệnh tùy chỉnh**  
   Thêm một nút Tập lệnh.  
   Viết mã Python của bạn trong trình soạn thảo; bạn có thể xác định các tham số có thể chỉnh sửa và các cổng bổ sung.  
   Kết nối đầu vào và đầu ra của nó như bất kỳ nút nào khác.  
   Thực thi luồng; tập lệnh sẽ được xử lý với dữ liệu của bạn.

3. **Thu thập dữ liệu từ máy hiện sóng thực**  
   Kết nối thiết bị và cấu hình giao tiếp từ bảng nút Máy hiện sóng.  
   Nút thu thập tín hiệu và cung cấp qua các cổng đầu ra của nó (một cổng cho mỗi kênh).  
   Kết nối các cổng này với các nút xử lý khác hoặc với trình xem.

4. **Xuất đồ thị cho báo cáo**  
   Kết nối tín hiệu mong muốn với nút Trình xuất đồ thị.  
   Chọn trong nút (nhấp chuột phải) để cấu hình giao diện trực quan của đồ thị.  
   Bạn cũng có thể tải/lưu hồ sơ để đẩy nhanh việc lấy đồ thị sẵn sàng cho báo cáo và nhn tệp hình ảnh với phần mở rộng đã chọn.

---

## Tất cả những điều này để làm gì

Kiến trúc này được thiết kế để bạn có thể tập trung vào phân tích tín hiệu mà không phải lo lắng về cách tổ chức bên trong của chương trình. Mỗi thành phần có một chức năng rõ ràng và cùng nhau mang lại trải nghiệm mượt mà, từ mô phỏng đến thiết bị thực, qua tùy chỉnh trực quan và xuất kết quả.

Nếu bạn cần mở rộng khả năng của FloWorks (ví dụ: thêm các loại nút mới hoặc kết nối một thiết bị khác), hãy biết rằng có một cấu trúc mô-đun cho phép điều đó, mặc dù đó là lĩnh vực dành cho nhà phát triển. Với tư cách là người dùng cuối, hãy tận hưởng sự linh hoạt mà thiết kế này mang lại.
