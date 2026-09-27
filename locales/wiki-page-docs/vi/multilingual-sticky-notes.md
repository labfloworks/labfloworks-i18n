# Ghi chú dán (Sticky Notes) – Hướng dẫn sử dụng

## Ghi chú dán là gì?

Ghi chú dán (hay *sticky notes*) là các khối văn bản nhỏ mà bạn có thể đặt tự do lên sơ đồ. Chúng dùng để:

- Thêm lời nhắc, tiêu đề hoặc giải thích trực tiếp trên Canvas.
- Tạo các hướng dẫn từng bước để hướng dẫn người dùng trong dự án của bạn.
- Tài liệu hóa các phần của luồng công việc mà không cần rời khỏi FloWorks.
- Để lại bình luận cho chính mình hoặc các cộng tác viên khác.

Các ghi chú có thể thay đổi kch thước (kéo các góc), di chuyển đến bất kỳ đâu trên sơ đồ và được lưu cùng với dự án. Khi mở tệp `.sflow`, tất cả các ghi chú sẽ xuất hiện đúng vị trí bạn đã để lại.

---

## Điểm mới: ghi chú đa ngôn ngữ

Ghi chú dán có thể tự động hiển thị văn bản bằng ngôn ngữ bạn chọn cho ứng dụng.  
Thay vì viết thông điệp cuối cùng bằng một ngôn ngữ duy nhất, bạn có thể chèn **các đnh dấu đặc biệt** sẽ tự động được dịch khi thay đổi ngôn ngữ của FloWorks.

Nhờ đó, cùng một ghi chú có thể được đọc bằng tiếng Tây Ban Nha, tiếng Anh hoặc bất kỳ ngôn ngữ nào khác mà không cần chỉnh sửa văn bản mỗi lần.

---

## Cách viết ghi chú đa ngôn ngữ

Bên trong một ghi chú (tạo bằng cách nhấp đúp hoặc nút 📝 trên Thanh công cụ), bạn có thể sử dụng hai loại đánh dấu:

### 1. Với từ khóa `tr(…)`
Viết `tr("khóa")` và thay `khóa` bằng tên mô tả của cụm từ.

Ví dụ:

```
tr("tutorial.paso1.titulo")
tr("tutorial.paso1.mensaje")
```

### 2. Với dấu ngoặc nhọn đôi `{{…}}`
Viết `{{khóa}}` theo cách tương tự.

Ví dụ:

```
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Cả hai định dạng hoạt động như nhau; hãy chọn định dạng bạn thấy thoải mái (thậm chí có thể kết hợp cả hai trong cùng một ghi chú).

> **Quan trọng**: Văn bản bạn thấy khi chỉnh sửa ghi chú chứa các đánh dấu gốc (ví dụ: `{{tutorial.paso1.titulo}}`).  
> Khi hoàn tất chỉnh sửa và quay lại chế độ xem sơ đồ bình thường, các đánh dấu sẽ được thay thế bằng cụm từ đã dịch sang ngôn ngữ hiện tại của ứng dụng.

---

## Hành vi khi thay đổi ngôn ngữ

- Nếu bạn thay đổi ngôn ngữ từ menu FloWorks (ví dụ: từ tiếng Tây Ban Nha sang tiếng Anh), **tất cả các ghi chú dán chứa đánh dấu sẽ tự động cập nhật**.
- Không cần đóng và mở lại dự án, cũng không cần chỉnh sửa từng ghi chú thủ công.
- Các ghi chú chỉ chứa văn bản thông thường (không có đánh dấu) không bị ảnh hưởng; chúng hiển thị như nhau ở mọi ngôn ngữ.

---

## Ưu điểm của việc sử dụng đánh dấu

- **Hướng dẫn đa ngôn ngữ tức thì** – Một ghi chú duy nhất có thể hướng dẫn người dùng ở nhiều ngôn ngữ khác nhau.
- **Tính nhất quán** – Nếu bạn sửa đổi bản dịch tại một nơi (tệp ngôn ngữ mà nhóm phát triển của bạn duy trì), tất cả các ghi chú sử dụng khóa đó sẽ được cập nhật.
- **Dễ bảo trì** – Bạn có thể viết nội dung một lần và tái sử dụng trong nhiều ghi chú.
- **Linh hoạt** – Kết hợp văn bản cố định với đánh dấu. Ví dụ:

```
🎯 BƯỚC 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

---

## Ví dụ thực tế: hướng dẫn từng bước

Giả sử bạn muốn thêm một ghi chú giải thích bước đầu tiên của hướng dẫn.  
Ở chế độ chỉnh sửa, bạn viết:

```
🎯 BƯỚC 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Khi hoàn tất chỉnh sửa và đang sử dụng ứng dụng bằng tiếng Tây Ban Nha, bạn sẽ thấy:

```
🎯 PASO 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.
```

Nếu bạn chuyển sang tiếng Anh, cùng một ghi chú sẽ hiển thị:

```
🎯 STEP 1
Welcome to FloWorks!
Drag a signal source node to begin.
```

Và tương tự cho bất kỳ ngôn ngữ nào khác đã được cấu hình.

---

## Tóm tắt

- Ghi chú dán làm phong phú sơ đồ của bạn với thông tin văn bản.
- Giờ đây chúng có thể **đa ngôn ngữ** bằng cách sử dụng các đánh dấu `tr("khóa")` hoặc `{{khóa}}`.
- Khi chỉnh sửa bạn sẽ thấy các khóa; khi xem, văn bản đã dịch.
- Thay đổi ngôn ngữ ứng dụng và tất cả các ghi chú sẽ thích ứng ngay lập tức.
- Hoàn hảo để tạo tài liệu trực quan, hướng dẫn hoặc thông báo cần hoạt động trên nhiều ngôn ngữ.

Hãy tận dụng tính năng này để các dự án của bn dễ tiếp cận và dễ chia sẻ hơn!
