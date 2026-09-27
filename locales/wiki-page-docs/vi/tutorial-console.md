# 🧪 Hướng dẫn tương tác Bảng điều khiển FloWorks

Chào mừng bạn đến với phòng thí nghiệm thử nghiệm của FloWorks. Phần này dành cho người dùng nâng cao hơn; đây là một thiết bị đầu cuối **Python** để điều khiển mọi thứ liên quan đến canvas, tức là các nút và kết nối của chúng, một cách tuần tự và bằng các dòng mã. Đây là một thiết bị đầu cuối được liên kết với chương trình mà nó có thể điều khiển và xác định các hành vi hoặc thói quen cho những người dùng đòi hỏi hơn.

Hướng dẫn này sẽ **từng bước** cho bạn thấy cách điều khiển và phân tích sơ đồ luồng của bạn mà không cần chạm vào chuột. Mỗi ví dụ đã được xác thực trong bảng điều khiển tương tác và phản ánh cấu trúc dữ liệu thực của chương trình.

---

## 1. Làm quen với địa hình

Bảng điều khiển tiêm ba đối tượng toàn cục: `app` (cửa sổ chính), `graph` (cảnh/sơ đồ) và `selected_node` (nút hiện đang được chọn trên canvas). Tất cả các lệnh đều xuất phát từ ba đối tượng này.

### Xem tất cả các nút

```python
>>> graph.nodes
```

**Ví dụ đầu ra:**
```
Các nút trong cảnh:
  [0] Bộ tạo tín hiệu Nâng cao (loại: SignalSourceNode, danh mục: Sources)
  [1] FFT (loại: FFTNode, danh mục: Processing)
```

Chỉ mục trong dấu ngoặc vuông (`[0]`, `[1]`) là cách chính của bạn để truy cập một nút. Thứ tự là thứ tự tạo trên canvas.

#### Thay thế: đếm nút hoặc lọc theo loại

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### Xem tất cả các kết nối

```python
>>> graph.connections
```

**Ví dụ đầu ra:**
```
Các kết nối trong cảnh:
  [0] Bộ tạo tín hiệu Nâng cao (out) → FFT (input)
```

Đầu ra hiển thị tên nút nguồn, cổng đầu ra, mũi tên, nút đích và cổng đầu vào. Nếu một kết nối không xuất hiện, luồng không thể được thực thi.

#### Thay thế: xem kết nối của một nút duy nhất

```python
>>> selected_node.connectors
```

### Xem nút đã chọn

Nhấp vào một nút trên canvas và sau đó thực thi:

```python
>>> selected_node
```

**Ví dụ đầu ra:**
```
Nút: FFT
  Loại: FFTNode
  Danh mục: Processing
  Các cổng: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Lưu ý:** Nếu không có nút nào được chọn, `selected_node` có giá trị `None`. Việc chọn một nút cũng tự động cập nhật bảng tham số bên cạnh.

#### Thay thế: chọn một nút bằng mã

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Thao tác nút và kết nối mà không cần chuột

### Tạo nút mới

Bạn cần biết tên lớp chính xác của nút (giống như trong danh mục). Các đối số là: `(loại, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

Nút xuất hiện trên canvas tại tọa độ (300, 200). Nếu bạn không biết tên chính xác, hãy liệt kê các danh mục (xem phần 7).

#### Thay thế: tạo nhiều nút cùng một lúc

```python
>>> for i, loai in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(loai, 100 + i*200, 300)
```

### Kết nối các nút thủ công

Cú pháp: `graph.connect_nodes(nguồn, đích, 'cổng_đầu_ra', 'cổng_đầu_vào')`. Các cổng phụ thuộc vào từng nút; không bao giờ giả định tên của chúng.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Lưu ý:** Luôn kiểm tra `graph.nodes[N].PORTS` trước khi kết nối. Một nút FFT có `'input'` và `'magnitude'`; một bộ tạo có `'output'`.

#### Thay thế: kết nối đến cổng mặc định

Nếu bạn không biết tên chính xác của cổng đầu vào, một số nút chấp nhận `None` để sử dụng cổng khả dụng đầu tiên:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Xóa một nút

```python
>>> graph.remove_node(graph.nodes[2])
```

Xóa nút và tất cả các kết nối liên quan. Các chỉ mục trong `graph.nodes` sẽ được sắp xếp lại, vì vậy đừng lưu các tham chiếu cũ.

#### Thay thế: xóa tất cả các nút của một danh mục

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### Xem các cổng của một nút

```python
>>> graph.nodes[1].PORTS
```

**Ví dụ đầu ra:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

Khóa là tên cổng (chuỗi). Giá trị là một bộ chứa vị trí trực quan. Chỉ các khóa mới khiến bạn quan tâm để kết nối.

#### Thay thế: xem các cổng dưới dạng danh sách đơn giản

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Thực thi luồng và xem kết quả

### Thực thi toàn bộ đồ thị

```python
>>> graph.execute_flow()
```

Phương thức này thuộc về sơ đồ (`graph`), không phải cửa sổ chính. Nó duyệt qua tất cả các nút theo thứ tự tô pô, thực thi từng nút và lưu kết quả vào bộ nhớ đệm. Nó không trả về gì; dữ liệu được lưu trữ nội bộ.

#### Thay thế: buộc tính toán một nhánh cụ thể

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

Điều này tính toán lại toàn bộ cây thượng nguồn của nút được chỉ định và trả về kết quả trực tiếp, mà không sửa đổi bộ nhớ đệm toàn cục.

### Xem dữ liệu đã lưu trong bộ nhớ đệm của một nút

Nếu bạn cần truy cập dữ liệu đã xử lý của một nút cụ thể, bạn có thể thực hiện theo hai cách:

#### Cách trực tiếp (theo đối tượng)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Ví dụ đầu ra:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Cảnh báo:** Cách này có thể thất bại với `KeyError` nếu thứ tự các nút trong cảnh thay đổi (ví dụ: khi xóa hoặc thêm nút) hoặc nếu thể hiện đối tượng không khớp chính xác với khóa được lưu trong từ điển.

#### Cách thay thế (theo vị trí)
```python
>>> list(graph.node_values.values())[1]
```

Cách này **ổn định hơn** vì nó không phụ thuộc vào danh tính chính xác của đối tượng. Thứ tự của các giá trị tuân theo trình tự mà các nút được thực thi trong lần `graph.execute_flow()` cuối cùng. Chỉ mục `[1]` tương ứng với nút thứ hai trong trình tự đó.

> **💡 Lưu ý:** Nếu bạn muốn xem chỉ mục của từng nút trong thứ tự thực thi, bạn có thể sử dụng:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Chú ý:** `graph.node_values` không phải lúc nào cũng trả về một mảng trực tiếp. Đối với các nút xử lý (FFT, bộ lọc, v.v.), nó trả về một **từ điển** trong đó mỗi khóa là một cổng đầu ra. Đối với các nút nguồn, nó trả về một bộ `(x, y)`.

#### Thay thế: xem dữ liệu của tất cả các nút trên một dòng
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Truy cập trục Y của một nút nguồn

Các nút tạo (SignalSourceNode, FileInputNode, v.v.) trả về một bộ `(thời_gian, tín_hiệu)` khi thực thi. Để chỉ lấy trục Y:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Ví dụ đầu ra:**
```
Scalar NumPy (float64): 1.0
```

#### Thay thế: lấy trục X (thời gian)

```python
>>> x = result[0]
>>> x[:5]
```

### Gán trong Python: một chi tiết quan trọng

Trong Python, các phép gán (`=`) là **câu lệnh**, không phải biểu thức. Bảng điều khiển không in gì sau `x, y = ...` vì không có giá trị trả về.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Để xác minh rằng nó đã hoạt động, hãy đánh giá biến ở dòng tiếp theo:

```python
>>> x
>>> y.shape
```

Hoặc sử dụng `;` để chuỗi một biểu thức trên cùng một dòng:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Hoặc sử dụng `print()` rõ ràng:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Vẽ trên đồ thị chính

### Xóa đồ thị

```python
>>> app.plot_widget.clear_plot()
```

#### Thay thế: xóa và vẽ lại ngay lập tức

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Vẽ một tín hiệu tùy ý từ bảng điều khiển

Bạn có thể tạo mảng với NumPy và gửi chúng trực tiếp đến tiện ích vẽ, mà không cần đi qua bất kỳ nút nào.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Thay thế: vẽ tổng các sin

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Vẽ kết quả của một nút nguồn

Vì nút nguồn trả về `(x, y)`, bạn có thể giải nén trực tiếp:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### Vẽ kết quả của nút FFT (nhiều cổng)

Các nút có nhiều đầu ra (FFT, phân tích thời gian-tần số, v.v.) không trả về một bộ đơn giản. Chúng trả về một `dict`, trong đó mỗi khóa là một cổng đầu ra.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Ví dụ đầu ra:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Lưu ý rằng `'output'` có thể là `None` nếu nút không có cổng chung. Các đầu ra hữu ích là `'magnitude'` và `'phase'`, chúng lần lượt là các bộ `(tần_số, giá_trị)`:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Thay thế: vẽ pha thay vì biên độ

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Thay thế: chồng hai tín hiệu

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # nút khác
>>> app.plot_widget.plot_waveform(x2, y2)  # chồng lên
```

> **💡 Lưu ý:** Nếu bạn cố thực hiện `x, y = result` với một dict, Python sẽ ném `ValueError: too many values to unpack`. Luôn kiểm tra `type(result)` và `result.keys()` trước khi giải nén.

---

## 5. Sửa đổi ứng dụng khi đang chạy

### Cập nhật bảng tham s bên cạnh

Nếu bạn sửa đổi một tham số bằng mã và muốn bảng bên cạnh phản ánh thay đổi:

```python
>>> app.workspace_table.populate()
```

Phương thức này không nhận đối số. Nó làm mới bảng với các giá trị hiện tại của nút đã chọn.

#### Thay thế: buộc chọn nút khác và làm mới

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Thêm nút từ thanh công cụ

```python
>>> app.add_node('SignalSourceNode')
```

Tương đương với việc nhấn nút "+" trên thanh công cụ. Nút được đặt ở vị trí mặc định trên canvas.

### Thay đổi tiêu đề cửa sổ

`setWindowTitle` là phương thức gốc của Qt. Nó hoạt động, nhưng hãy lưu ý rằng ứng dụng có thể có bộ hẹn giờ hoặc sự kiện gọi `update_title()` và tự động ghi đè nó.

```python
>>> app.setWindowTitle('Phòng thí nghiệm tín hiệu của tôi')
>>> app.windowTitle()
```

Để khôi phục tiêu đề "chính thức" mà ứng dụng tính toán từ trạng thái nội bộ của nó (tên dự án, tệp, v.v.):

```python
>>> app.update_title()
```

#### Thay thế: tiêu đề với tên dự án

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Điều hướng và năng suất trong bảng điều khiển

Bảng điều khiển không chỉ là `print()`. Nó có lịch sử, tự động hoàn thành và các khối đa dòng.

| Phím / Lệnh           | Hành động                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Duyệt lịch sử lệnh đã thực thi                |
| `Tab`                     | Tự động hoàn thành biến, thuộc tính và phương thức của namespace     |
| `Ctrl + L`                | Xóa toàn bộ bảng điều khiển (xóa văn bản, không phải trạng thái Python)  |
| `if`, `for`, `def`, `class` | Dấu nhắc đổi từ `>>>` sang `...` cho các khối đa dòng     |
| `Ctrl+C` (trong vùng chọn)   | Sao chép văn bản từ bảng điều khiển                                     |
| `Ctrl+A`                  | Chọn toàn bộ nội dung                                  |

> **💡 Lưu ý:** Tự động hoàn thành sử dụng `rlcompleter` và nhận ra toàn bộ namespace được tiêm (`app`, `graph`, `selected_node`) cộng với bất kỳ biến nào bạn định nghĩa trong phiên.

---

## 7. Công thức nâng cao

### Thay đổi tham số nội bộ của một nút

Các tham số của nút không phải là các thuộc tính phẳng. Chúng được lồng trong từ điển `params`, vốn có các tiểu mục như `'preset'`, `'formula'` hoặc `'advanced'`. Không bao giờ thực hiện `nut.amplitude = 3.0`; điều đó tạo một thuộc tính mới trên đối tượng nhưng không sửa đổi tham số thực.

#### Trường hợp A: sửa đổi một preset (sin, vuông, v.v.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Trường hợp B: sử dụng công thức tùy chỉnh

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

Biểu thức sử dụng `t` làm biến thời gian. Các giá trị trong `vars` là các ký hiệu mà bạn có thể tham chiếu trong công thức. Nếu bạn bỏ qua `vars`, nút sẽ sử dụng giá trị mặc định và công thức có thể không phản ánh thay đổi.

#### Trường hợp C: thay đi tham số nâng cao (tốc độ lấy mẫu, thời lượng)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Trường hợp D: thay đổi tham số của nút không phải tạo (ví dụ: FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Lưu ý:** `getattr(obj, '_generate_signal', lambda: None)()` là một mẫu an toàn: nếu phương thức tồn tại (các nút tạo), nó gọi nó; nếu không, nó không làm gì và không ném lỗi. Đối với các nút xử lý, chỉ `graph.execute_flow()` là đủ.

### Liệt kê tất cả các danh mục nút khả dụng

Import tải danh mục, nhưng không tự động hiển thị nó. Hãy nhớ rằng trong Python, một import thành công không in gì; bạn phải đánh giá đối tượng.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Để có bản tóm tắt có thể đọc được:

```python
>>> for cat, nut in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nut)} nut")
```

#### Thay thế: liệt kê tên nút theo danh mục

```python
>>> {cat: [n.__name__ for n in nut] for cat, nut in NODE_CATEGORIES.items()}
```

### Xem trợ giúp của bất kỳ phương thức nào

```python
>>> help(graph.connect_nodes)
```

Docstring xuất hiện trực tiếp trong bảng điều khiển. Nó rất hữu ích để khám phá các đối số mà một phương thức mong đợi mà không cần mở mã nguồn.

#### Thay thế: xem các thuộc tính đã lọc

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. Phải làm gì nếu có gì đó sai?

- **Lỗi màu đỏ trong bảng điều khiển:** traceback được hiển thị đầy đủ. Ứng dụng không đóng; bạn có thể sửa lệnh và thử lại.
- **Giao diện bị treo:** có lẽ bạn đã viết một vòng lặp vô hạn. Bảng điều khiển chạy trong một luồng riêng biệt, nhưng nếu vòng lặp ảnh hưởng đến luồng GUI, hãy khởi động lại ứng dụng.
- **`None` không mong đợi:** nếu một nút trả về `None` thay vì dữ liệu, hãy xác minh rằng nó được kết nối thượng nguồn (`graph.connections`) và luồng đã được thực thi (`graph.execute_flow()`).
- **`ValueError: too many values to unpack`:** bạn đang cố giải nén một dict như thể nó là một bộ. Hãy sử dụng `result.keys()` trước.
- **`ValueError: not enough values to unpack`:** bạn mong đợi 2 giá trị nhưng nút trả về 1 (dict) hoặc 3 (phổ). Kiểm tra `type(result)` trước khi giải nén.
- **`AttributeError`:** đối tượng không có thuộc tính đó. Sử dụng `dir(obj)` hoặc `[a for a in dir(obj) if 'tu' in a.lower()]` để khám phá tên đúng.
- **Không có gì xảy ra khi thực thi:** kiểm tra rằng có ít nhất một nút nguồn được kết nối vào chui và `graph.execute_flow()` đã được gọi. Các nút xử lý không tự tạo dữ liệu.
- **Đồ thị không thay đổi:** đảm bảo gọi `graph.execute_flow()` sau khi sửa đổi tham số. Chỉ thay đổi `params` không tự động tính toán lại.

---

© 2026 FloWorks — Phòng thí nghiệm Tín hiệu
