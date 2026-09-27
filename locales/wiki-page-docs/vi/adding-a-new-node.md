---
title: Hướng dẫn Thêm Nút Mới vào FloWorks
description: Hướng dẫn từng bước để tạo, đăng ký và tích hợp các nút tùy chỉnh vào công cụ luồng của FloWorks.
---

# 📘 Hướng dẫn dành cho Nhà phát triển: Cách Thêm Nút Mới vào FloWorks

Hướng dẫn này mô tả toàn bộ quy trình tạo một loại nút mới trong FloWorks, đảm bảo rằng nó được tích hợp chính xác với công cụ luồng, giao diện người dùng, chủ đề trực quan và hệ thống quốc tế hóa.

---

## 📋 Mục lục
- [📘 Hướng dẫn dành cho Nhà phát triển: Cách Thêm Nút Mới vào FloWorks](#-hướng-dẫn-dành-cho-nhà-phát-triển-cách-thêm-nút-mới-vào-floworks)
  - [📋 Mục lục](#-mục-lục)
  - [1. Giới thiệu về Kiến trúc](#1-giới-thiệu-về-kiến-trúc)
  - [2. Sử dụng Mẫu `template_node.py`](#2-sử-dụng-mẫu-template_nodepy)
  - [3. Từng bước: Tạo Nút Tùy chỉnh](#3-từng-bước-tạo-nút-tùy-chỉnh)
    - [3.1. Sao chép và Đổi tên Mẫu](#31-sao-chép-và-đổi-tên-mẫu)
    - [3.2. Xác định Cổng và Nhãn](#32-xác-định-cổng-và-nhãn)
    - [3.3. Triển khai Logic Xử lý](#33-triển-khai-logic-xử-lý)
    - [3.4. Tùy chỉnh Giao diện (Tùy chọn)](#34-tùy-chỉnh-giao-diện-tùy-chọn)
    - [3.5. Thêm Tham số Cấu hình (Tùy chọn)](#35-thêm-tham-số-cấu-hình-tùy-chọn)
    - [3.6. Cho phép Nút tuần tự hóa (lưu / tải cấu hình)](#36-cho-phép-nút-tuần-tự-hóa-lưu--tải-cấu-hình)
  - [4. Tích hợp vào Hệ thống](#4-tích-hợp-vào-hệ-thống)
  - [5. Quốc tế hóa (i18n)](#5-quốc-tế-hóa-i18n)
  - [6. Chủ đề Trực quan](#6-chủ-đề-trực-quan)
  - [7. Danh sách Kiểm tra và Xử lý Sự cố](#7-danh-sách-kiểm-tra-và-xử-lý-sự-cố)
    - [✅ Danh sách Kiểm tra](#-danh-sách-kiểm-tra)
    - [🐛 Vấn đề Thường gặp](#-vấn-đề-thường-gặp)
  - [8. Kết luận](#8-kết-luận)

---

## 1. Giới thiệu về Kiến trúc

FloWorks được xây dựng trên PySide6 và sử dụng mô hình các nút có thể kết nối đại diện cho một luồng xử lý tín hiệu.

---

## 2. Sử dụng Mẫu `template_node.py`

Để tạo các nút mới dễ dàng hơn, tệp `nodes/template_node.py` được cung cấp. Mẫu này bao gồm:
- Hỗ trợ đầy đủ cho quốc tế hóa (kết nối với `languageChanged`, phương thức `update_language`).
- Hỗ trợ đầy đủ cho chủ đề (phương thức `update_theme`).
- Trợ giúp tích hợp với định dạng HTML ba phần.
- Quản lý nhiều cổng đầu vào/đầu ra có thể cấu hình.
- Nhiều đầu ra với `get_output_for_port`.
- Hiển thị trên đồ thị thông qua `get_display_signal`.
- Menu ngữ cảnh có thể dịch đợc.

Luôn khuyến khích xuất phát từ mẫu này khi phát triển một nút mới.

---

## 3. Từng bước: Tạo Nút Tùy chỉnh

### 3.1. Sao chép và Đổi tên Mẫu
1. Sao chép `nodes/template_node.py` với tên nút mới của bạn, ví dụ `nodes/mi_nodo.py`.
2. Đổi tên lớp từ `TemplateNode` thành một tên mô tả, ví dụ `MiNodoNode`.
3. Điều chỉnh các lệnh nhập nếu cần.

### 3.2. Xác định Cổng và Nhãn
!!! warning "Quan trọng: Sự khớp Tên"
    Tên cổng trong `PORTS`, `PORT_LABELS` và các khóa của từ điển được trả về bởi `execute_program` phải **hoàn toàn giống nhau** (bao gồm cả chữ hoa/chữ thường). Mẫu hiện bao gồm một ánh xạ bí danh (`'data_in'` → cổng trái đầu tiên) để tăng độ mạnh.

Chỉnh sửa từ điển `PORTS` ở phần trên cùng của tệp. Mỗi mục có định dạng:
```python
"ten_cong": ("canh", phan_so)
```
- **Các cạnh có thể có:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Phân số:** giá trị từ `0.0` đến `1.0` cho biết vị trí dọc theo cạnh.

**Ví dụ cho một nút có một đầu vào và hai đầu ra:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
Từ điển `PORT_LABELS` chứa văn bản sẽ hiển thị bên cạnh mỗi cổng. Khuyến khích sử dụng các khóa dịch thay vì văn bản cố định (xem phần Quốc tế hóa).

### 3.3. Triển khai Logic Xử lý
Phương thức chính là `execute_program(self, input_data)`. Phương thức này được công cụ luồng gọi khi nút nhận được dữ liệu.

**`input_data` có thể là:**
- `None` nếu không có đầu vào.
- Một bộ `(x, y)` cho các tín hiệu thời gian.
- Một mảng 1D.
- Một từ điển `{ten_cong: du_lieu}` trong các nút có nhiều đầu vào.

**Giá trị trả về:**
- Đối với các nút có một đầu ra duy nhất, trả về trực tiếp dữ liệu (ví dụ: bộ `(x, y)`).
- Đối với các nút có nhiều đầu ra, trả về một từ điển trong đó các khóa khớp với tên các cổng đầu ra được xác định trong `PORTS`.

```python
def execute_program(self, input_data):
    # Xử lý input_data và tạo kết quả
    ket_qua_magnitude = (freq, mag)
    ket_qua_phase = (freq, phase)
    return {
        "magnitude": ket_qua_magnitude,
        "phase": ket_qua_phase
    }
```

!!! tip "Lưu ý về tên cổng chung"
    Công cụ luồng đôi khi có thể truyền một từ điển với các khóa như `'data_in'` thay vì tên cổng thực tế (đặc biệt nếu người dùng không nhấp chính xác vào vòng tròn). Mẫu hiện đã bao gồm mã để xử lý trường hợp này:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    Điều này ngăn nút bị lỗi do kết nối không chính xác.

Mẫu đã bao gồm một ví dụ được chú thích. Ngoài ra, nó triển khai `get_output_for_port(self, port_name)` để công cụ có thể định tuyến từng đầu ra:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Tùy chỉnh Giao diện (Tùy chọn)
Phương thức `paint()` vẽ nền, tiêu đề, trạng thái và bất kỳ văn bản bổ sung nào. Bạn có thể sửa đổi:
- Màu sắc (tự động cập nhật bằng `update_theme`).
- Văn bản trạng thái (sử dụng thuộc tính `self._status`).
- Thông tin tóm tắt (ví dụ: đỉnh biên độ).

Mẫu hiển thị một ví dụ cơ bản.

### 3.5. Thêm Tham số Cấu hình (Tùy chọn)
Nếu nút của bạn yêu cầu các tham số có thể điều chỉnh bởi người dùng (ví dụ: kích thước cửa sổ, tần số cắt), bạn có thể:
1. Thêm các thuộc tính trong `__init__` (ví dụ: `self.window_size = 512`).
2. Tạo một hộp thoại cấu hình (kế thừa từ `QDialog`).
3. Kết nối hộp thoại trong `open_config_dialog()` (phương thức đã có trong mẫu).
4. Cập nhật các tham số từ hộp thoại và gọi `self.update()`.

### 3.6. Cho phép Nút tuần tự hóa (lưu / tải cấu hình)
Để nút có thể lưu và khôi phục các tham số của nó khi sao chép/dán, hoàn tác/làm lại, hoặc khi sử dụng các lệnh Lưu/Mở trong menu Tệp, nó phải kế thừa từ mixin tuần tự hóa và khai báo các thuộc tính của nó.

1. Nhập mixin vào tệp của bạn:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Thay đổi kế thừa của lớp để bao gồm mixin trước `QGraphicsObject`:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. Xác định danh sách `SERIALISABLE` ở cấp lớp, với tên các thuộc tính bạn muốn duy trì. Chỉ hỗ trợ các kiểu đơn giản (`int`, `float`, `str`, `bool`), danh sách, từ điển, hoặc mảng NumPy (những mảng này được tự động lưu dưới dạng các tệp `.npy` bên trong `.sflow`).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. Đảm bảo các thuộc tính đó được khởi tạo trong `__init__`:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

Với điều này, bạn không cần viết các phương thức `serialize`/`deserialize`; mixin tự động xử lý việc lưu và khôi phục các giá trị.

Nếu nút của bạn yêu cầu logic bổ sung khi tải (ví dụ: kết nối lại một thiết bị phần cứng), bạn có thể ghi đè `deserialize` bằng cách gọi phương thức cha trước:
```python
def deserialize(self, data):
    super().deserialize(data)   # khôi phục các thuộc tính từ SERIALISABLE
    self._iniciar_dispositivo()
```

---

## 4. Tích hợp vào Hệ thống

Sau khi tạo tệp nút, bạn chỉ cần dán nó vào thư mục `nodes` để nó xuất hiện trong giao diện và hoạt động với phần còn lại của hệ thống.

---

## 5. Quốc tế hóa (i18n)

Tất cả các văn bản hiển thị phải có thể dịch được thông qua `tr("khoa", default="...")`. Mẫu đã triển khai điều này. Bạn cần thêm các khóa tương ứng vào các tệp JSON trong `locales/`.

**Cấu trúc được đề xuất:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Mô tả chú thích",
       "ports": {
         "input": "Đầu vào",
         "output1": "Đầu ra 1",
         "output2": "Đầu ra 2"
      },
       "status": {
         "no_data": "Không có dữ liệu",
         "ready": "Sẵn sàng"
      },
       "menu": {
         "show_output": "Hiển thị đầu ra",
         "configure": "Cấu hình..."
      },
       "help_title": "Trợ giúp - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>
<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

Trợ giúp HTML tuân theo định dạng ba phần chung cho tất cả các nút (mô tả cụ thể + "Cách suy nghĩ về hệ thống" + "Phím tắt và mẹo"). Mẫu đã bao gồm cấu trúc này trong `get_help_text()`.

---

## 6. Chủ đề Trực quan

Phương thức `update_theme(self, theme)` nhận một từ điển với các màu được xác định bởi chủ đề hiện tại. Mẫu tự động cập nhật:
- Nền nút (`node_normal_bg`)
- Viền (`node_selected_border`)
- Màu tiêu đề và văn bản (`node_normal_text`)
- Màu cổng (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Đảm bảo rằng trong `MainWindow` (hoặc `ThemeUpdater`), `node.update_theme()` được gọi cho mỗi nút khi chủ đề thay đổi.

---

## 7. Danh sách Kiểm tra và Xử lý Sự cố

### ✅ Danh sách Kiểm tra
- [ ] Nút được tạo chính xác từ thanh công cụ.
- [ ] Các cổng hiển thị ở vị trí mong đợi và có thể phát hiện để kết nối (`Ctrl+nhấp`).
- [ ] Khi nhận dữ liệu đầu vào, `execute_program` được gọi và tín hiệu được xử lý.
- [ ] Các đầu ra được lan truyền chính xác đến các nút đã kết nối.
- [ ] Menu ngữ cảnh cho phép thay đổi kênh hiển thị (nếu có nhiều đầu ra).
- [ ] Khi nhấp vào nút, tín hiệu được chọn được vẽ trong tiện ích đồ thị.
- [ ] Nhấp đúp mở trợ giúp với định dạng phù hợp.
- [ ] Ngôn ngữ thay đổi chính xác (văn bản tiêu đề, cổng, menu).
- [ ] Chủ đề thay đổi chính xác (màu nút và cổng).
- [ ] Sao chép/dán hoạt động không có lỗi.

!!! tip "Kt nối cổng chính xác"
    Khi kết nối các nút, hãy đảm bảo nhấp chính xác vào vòng tròn của cổng đích. Nếu bạn nhấp vào thân nút, hệ thống sẽ sử dụng tên chung (`'data_in'`). Mẫu hiện dung thứ các tên này, nhưng đó là một thực hành tốt để kết nối trực tiếp với vòng tròn để đảm bảo định tuyến đúng nhiều đầu ra.

### 🐛 Vấn đề Thường gặp

| Triệu chứng | Nguyên nhân có thể | Giải pháp |
|---------|---------------|----------|
| Mũi tên kết nối không neo vào cổng. | Vòng tròn cổng không có `setData(0, port_name)` hoặc `get_port_scene_pos` chưa được triển khai. | Kiểm tra rằng trong `_create_ports` có `circle.setData(0, port_name)` và `get_port_scene_pos` sử dụng tên đó. |
| Các đầu ra không đến các nút đã kết nối. | `execute_program` không trả về từ điển (đối với nhiều đầu ra) hoặc `get_output_for_port` chưa được triển khai. | Đảm bảo `execute_program` trả về `{ten_cong: du_lieu}` và `get_output_for_port` trả về giá trị tương ứng. |
| Khi nhấp vào nút không vẽ gì cả. | `get_display_signal` không trả về một bộ `(x, y)` hợp lệ hoặc `display_channel` không khớp với đầu ra hiện có. | Kiểm tra rằng `get_display_signal` sử dụng kênh đã chọn và dữ liệu là mảng NumPy. |
| Văn bản không cập nhật khi thay đổi ngôn ngữ. | Tín hiệu `languageChanged` không được kết nối hoặc `update_language` không cập nhật các phần tử. | Kiểm tra kết nối trong `__init__`: `language_manager.languageChanged.connect(self.update_language)`. |
| Chủ đề không được áp dụng. | `update_theme` không được gọi khi tạo nút hoặc khi thay đổi chủ đề. | Trong `MainWindow`, sau khi tạo nút, hãy gọi `node.update_theme(self.theme_manager.current_theme())`. |
| Mũi tên chỉ vào trung tâm nút. | Nhấp vào thân thay vì vòng tròn, hoặc tên không khớp với `PORTS`. | Nhấp trực tiếp vào vòng tròn. Xác minh rằng `get_port_scene_pos` có ánh xạ bí danh. |
| `NameError: name 'self' is not defined` khi nhập. | Các thuộc tính thể hiện được khai báo bên ngoài `__init__`. | Tất cả các thuộc tính như `self.tham_so_cua_toi` phải được định nghĩa bên trong `__init__`. |
| Tham số bị mất khi sao chép/mở `.sflow`. | Nút không kế thừa từ `SerializableMixin` hoặc `SERIALISABLE` chưa được xác định. | Triển khai bước 3.6 của hướng dẫn này. |

---

## 8. Kết luận

Theo hướng dẫn này và sử dụng mẫu `template_node.py`, bạn sẽ có thể thêm các nút mới vào FloWorks một cách hiệu quả và nhất quán với phần còn lại của hệ thống. Luôn nhớ duy trì khả năng tương thích với i18n và chủ đề để có trải nghiệm người dùng chuyên nghiệp.

Hãy đóng góp các nút của riêng bạn!
