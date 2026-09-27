---
title: Định dạng tệp .sflow
description: Đặc tả kỹ thuật, cấu trúc nội bộ và hướng dẫn sử dụng tiêu chuẩn trao đổi của FloWorks
---

# 📄 Định dạng tệp `.sflow`

Định dạng `.sflow` là tiêu chuẩn trao đổi và lưu trữ gốc của **FloWorks**. Nó cho phép đóng gói toàn bộ luồng công việc vào một tệp duy nhất bao gồm cấu trúc đồ thị, tham số nút, dữ liệu đã xử lý và ghi chú dán, tạo điều kiện chia sẻ, lưu trữ hoặc tái tạo thí nghiệm một cách xác định.

---

## 📦 Tệp `.sflow` là gì?

Tệp `.sflow` về bản chất là **một tệp ZIP được đổi tên**. Bằng cách đổi phần mở rộng thành `.zip`, bạn có thể kiểm tra nội dung của nó bằng bất kỳ trình quản lý tệp hoặc công cụ dòng lệnh nào.

Cấu trúc nội bộ tối thiểu bao gồm:

| Thành phần | Mô tả |
|------------|-------------|
| `diagram.json` | Manifest chính: xác định các nút, kết nối, chế độ xem, ghi chú dán và siêu dữ liệu tuần tự hóa. |
| `data/` | Thư mục chứa dữ liệu của mỗi nút ở định dạng `.npy` (mảng nhị phân NumPy). |
| `metadata.json` *(tùy chọn)* | Thông tin bổ sung: tác giả, phiên bản FloWorks, mô tả và nhãn. |

=== "🌳 Cấu trúc trực quan"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (tùy chọn, cho ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – Trái tim của luồng

Tệp JSON này mô tả toàn bộ cấu trúc liên kết, vị trí các phần tử trên canvas và trạng thái chế độ xem tại thời điểm lưu.

### Ví dụ tối thiểu
```json
{
  "nodes": [
    {
      "id": "n1",
      "type": "OscilloscopeNode",
      "pos": [150, 200],
      "params": { "channel": "primary", "simulation": false }
    },
    {
      "id": "n2",
      "type": "GraphExporterNode",
      "pos": [450, 200],
      "params": { "theme": "dark", "export_format": "png" }
    }
  ],
  "connections": [
    {
      "from": "n1",
      "to": "n2",
      "from_port": "out",
      "to_port": "data_in"
    }
  ],
  "viewport": { "x": 0, "y": 0, "scale": 1.0 },
  "stickers": [
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Kiểm tra ngưỡng", "user_modified": true }
  ]
}
```

### Các trường chính
| Trưng | Loại | Mô tả |
|-------|------|-------------|
| `nodes` | `Array` | Danh sách các đối tượng `{id, type, pos, params}`. `type` phải khớp với `node_registry.py`. |
| `connections` | `Array` | Danh sách các kết nối `{from, to, from_port, to_port}`. Các cổng là chuỗi, không phải chỉ mục. |
| `viewport` | `Object` | `(x, y, scale)` để khôi phục chính xác vị trí và thu phóng của canvas. |
| `stickers` | `Array` | Ghi chú dán được tuần tự hóa với tọa độ chuẩn hóa thành 96 dpi. |

!!! tip "Tuần tự hóa nút"
    Các tham số cụ thể của mỗi nút được quản lý thông qua `SerializableMixin`. Chỉ các thuộc tính đưc khai báo trong `SERIALISABLE = [...]` mới được lưu. Các nút không được đăng ký trong hệ thống sẽ tự động bị bỏ qua trong quá trình tải.

---

## 💾 `data/` – Dữ liệu đã xử lý và mảng NumPy

Khi một luồng được thực thi, các nút có thể lưu trữ kết quả của chúng trong các tệp `.npy` bên trong thư mục này.

- Tên tệp thường khớp với `id` của nút hoặc các tham chiếu nội bộ.
- Các mảng được lưu trữ ở định dạng nhị phân NumPy, **bảo toàn nghiêm ngặt chiều ban đầu** (1D, 2D, 3D, v.v.). Công cụ không bao giờ áp dụng `flatten()`.
- Trong `diagram.json`, dữ liệu được tham chiếu với tiền tố `__npy__:`:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Nếu một nút không tạo dữ liệu hoặc được cấu hình để không lưu chúng, tệp tương ứng có thể bị bỏ qua.

??? note "Khả năng tương thích bên ngoài"
    Các tệp `.npy` là phổ quát trong hệ sinh thái Python. Bạn có thể đọc chúng bên ngoài FloWorks bằng:
    ```python
    import numpy as np
    du_lieu = np.load("data/node_1.npy")
    print(du_lieu.shape)
    ```

---

## 🏷️ `metadata.json` (Tùy chọn)

Chứa thông tin mô tả không ảnh hưởng đến việc thực thi, lý tưởng cho khả năng truy xuất nguồn gốc và quản lý dự án:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Phân tích độ rung trong động cơ ba pha",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["kỹ thuật", "độ rung", "FFT", "đa kênh"]
}
```

---

## 🔄 Quy trình lưu và tải

FloWorks triển khai một cơ chế mạnh mẽ để đảm bảo tính toàn vẹn của dữ liệu:

1. **Lưu:**
   - Đồ thị được duyệt và các nút được tuần tự hóa qua `SerializableMixin`.
   - Các mảng được trích xuất vào `data/` và được tham chiếu trong JSON bằng `__npy__:`.
   - Mọi thứ được đóng gói vào một ZIP có phần mở rộng `.sflow`.
2. **Tải an toàn:**
   - Một **bản sao lưu tạm thời trong bộ nhớ** của sơ đồ hiện tại được tạo.
   - Tệp `.sflow` mới được giải nén và phân tích.
   - Nếu xảy ra bất kỳ lỗi nào (JSON không hợp lệ, thiếu nút, `.npy` bị hỏng), **bản sao lưu được tự động khôi phục** mà không mất công việc.
3. **ScriptNode đặc biệt:**
   - Lưu `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` và `python_path`.
   - Khi tải, mã được biên dịch lại, các cổng động được xây dựng lại và trạng thái `persist` được tự động khôi phục.
4. **StickyNotes và DPI:**
   - Tọa độ và kích thước được chuẩn hóa thành **96 dpi** khi lưu.
   - Khi tải, chúng được chia tỷ lệ theo DPI của màn hình hiện tại, đảm bảo tính nhất quán trực quan giữa các độ phân giải khác nhau.

---

## 🛠️ Sử dụng bên ngoài và tự động hóa

Định dạng `.sflow` được thiết kế để minh bạch và có thể lập trình. Bạn có thể đọc hoặc tạo nó từ các tập lệnh bên ngoài:

=== "🐍 Python (Đọc)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        du_lieu_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Nút: {len(graph['nodes'])}")
        print(f"Dữ liệu: {du_lieu_n1.shape}")
    ```

=== "📤 Python (Tạo cơ bản)"
    ```python
    import zipfile
    import json
    import numpy as np

    graph = {
        "nodes": [{"id": "gen", "type": "GeneratorNode", "pos": [100, 100], "params": {}}],
        "connections": [],
        "viewport": {"x": 0, "y": 0, "scale": 1.0}
    }

    with zipfile.ZipFile("nuevo.sflow", "w", zipfile.ZIP_DEFLATED) as z:
        z.writestr("diagram.json", json.dumps(graph, indent=2))
        z.writestr("data/gen.npy", np.array([1.0, 2.0, 3.0]))
    ```

---

## 🔮 Khả năng tương thích và khả năng mở rộng trong tương lai

Định dạng `.sflow` tuân theo các nguyên tắc **thiết kế mở rộng và tương thích ngược**:

- ✅ **Các phần mới:** Các phiên bản tương lai có thể thêm các thư mục như `thumbnails/`, `logs/` hoặc `plugins/` mà không làm hỏng các trình tải cũ.
- ✅ **Các trường tùy chọn:** Trình phân tích bỏ qua các khóa không xác định trong `diagram.json`, cho phép thêm siêu dữ liệu thử nghiệm.
- ✅ **Phiên bản hóa:** Trường `floworks_version` trong `metadata.json` cho phép ứng dụng áp dụng các quá trình di chuyển tự động nếu định dạng phát triển.

!!! warning "Quy tắc vàng"
    Không bao giờ sửa đổi thủ công `diagram.json` khi ứng dụng đang mở. Hệ thống phụ thuộc vào tính nhất quán giữa cấu trúc liên kết, mảng và trạng thái chế độ xem. Luôn sử dụng luồng lưu/tải gốc.

---

## 📚 Tài nguyên liên quan
- [🗺️ Bản đồ mã và kiến trúc](architecture-ii.md) → Cách `file_io.py` và `SerializableMixin` quản lý định dạng.
- [📦 Hướng dẫn bản dựng di động](guia-ejecutable-portable.md) → Đóng gói và đường dẫn an toàn cho tài nguyên.
- [🧩 Tài liệu tham khảo kỹ thuật về nút](node-reference.md) → Hợp đồng tuần tự hóa theo loại nút.
