# 📊 Hướng dẫn sử dụng Bảng tính

Bảng tnh cơ bản theo phong cách Excel, nhẹ và có thể nhúng.

---

## 1. Spreadsheet là gì?

Đây là một thành phần bảng tính cung cấp:

- Các ô hỗ trợ công thức (bắt đầu bằng `=`)
- Các hàm có sẵn (SUM, AVERAGE, IF, v.v.)
- Các toán tử số học, logic và so sánh
- Giao diện tối giản, lý tưởng để tích hợp vào ứng dụng Qt

---

## 2. Điều hướng cơ bản

- Nhấp vào một ô để chọn.
- Gõ trực tiếp để nhập văn bản hoặc số.
- Để viết **công thức**, bắt đầu bằng `=` (ví dụ: `=SUM(A1:A5)`).
- Nhấn **Enter** để xác nhận chỉnh sửa.
- Sử dụng phím mũi tên hoặc chuột để di chuyển.

---

## 3. Công thức: các hàm có sẵn

Tất cả các hàm đều viết bằng chữ in hoa và hỗ trợ dải ô (ví dụ: `A1:A5`) hoặc đối số phân tách bằng dấu phẩy.

| Hàm | Chức năng | Ví dụ |
| --- | --------- | ----- |
| `SUM` | Tính tổng các số | `=SUM(A1:A5)` |
| `AVG` / `AVERAGE` | Trung bình | `=AVG(A1:A5)` |
| `COUNT` | Đếm s (không rỗng) | `=COUNT(A1:A5)` |
| `MAX` | Giá trị lớn nhất | `=MAX(A1:A5)` |
| `MIN` | Giá trị nhỏ nhất | `=MIN(A1:A5)` |
| `ABS` | Giá trị tuyệt đối | `=ABS(A1)` |
| `ROUND` | Làm tròn (đối số 2 = số chữ số thập phân) | `=ROUND(A1, 2)` |
| `IF` | Điều kiện (nếu, thì, ngược lại) | `=IF(A1>10, "Có", "Không")` |
| `CONCAT` / `CONCATENATE` | Nối văn bản | `=CONCAT(A1, " ", B1)` |
| `LEN` | Độ dài văn bản | `=LEN(A1)` |
| `INT` | Phần nguyên | `=INT(A1)` |
| `SQRT` | Căn bậc hai | `=SQRT(A1)` |

### 3.1. Ghi chú về hàm

- Các dải ô được chỉ định bằng **dấu hai chấm**: `A1:A5` bao gồm tất cả các ô từ A1 đến A5.
- Các hàm có thể lồng nhau: `=SUM(A1:A5) + MAX(B1:B5)`.
- Các đối số văn bản phải nằm trong dấu ngoặc kép hoặc đơn.

---

## 4. Toán tử không dùng hàm

Ngoài các hàm, bạn có thể sử dụng toán tử trực tiếp trong công thức. Cú pháp tương tự Python.

### 4.1. Số học

| Phép toán | Ví dụ |
| --------- | ----- |
| Cộng | `=A1+A2+A3` |
| Trừ | `=A1-A2` |
| Nhân | `=A1*B1` |
| Chia | `=A1/B1` |
| Modulo | `=A1%B1` |
| Lũy thừa | `=A1**2` (hoặc `=A1^2`) |

### 4.2. So sánh

Trả về `True` hoặc `False` (hiển thị là `Đúng` / `Sai`).

| Toán tử | Ý nghĩa | Ví dụ |
| ------- | ------- | ----- |
| `>` | Lớn hơn | `=A1>B1` |
| `<` | Nhỏ hơn | `=A1<B1` |
| `>=` | Lớn hơn hoặc bằng | `=A1>=10` |
| `<=` | Nhỏ hơn hoặc bằng | `=A1<=10` |
| `==` | Bằng | `=A1==B1` |
| `!=` | Khác | `=A1!=B1` |

### 4.3. Logic và điều kiện

Bạn có thể kết hợp các điều kiện bằng `and`, `or`, `not`.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

Toán tử ba ngôi `if` `else` cũng được hỗ trợ trực tiếp.

---

## 5. Ví dụ thực tế

### 5.1. Tổng doanh số

Giả sử bạn có doanh số ở `B2:B10` và muốn tính tổng:

```excel
=SUM(B2:B10)
```

### 5.2. Giảm giá có điều kiện

Nếu tổng (ở `B12`) vượt quá 100, áp dụng giảm giá 10%; nếu không thì 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Trung bình và đếm

Trung bình điểm ở `C2:C20`, nhưng chỉ khi có ít nhất 5 giá trị:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Dữ liệu không đủ")
```

### 5.4. Văn bản kết hợp

Nối tên (A2) và họ (B2) với một khoảng trắng:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Căn bậc hai của một số

```excel
= SQRT(A1)
```

### 5.6. Làm tròn đến 2 chữ số thp phân

```excel
= ROUND(A1, 2)
```

---

## 6. Mẹo và thủ thuật

- **Tham chiếu tương đối/tuyệt đối:** hiện tại, tất cả các tham chiếu đều là tương đối (như Excel). `$A$1` chưa được hỗ trợ.
- **Dải động:** bạn có thể sử dụng các dải như `A:A` (toàn bộ cột) hoặc `1:1` (toàn bộ hàng).
- **Tự động hoàn thành:** khi gõ `=`, một menu với các hàm có sẵn sẽ xut hiện.
- **Lỗi:** nếu công thức không hợp lệ, ô sẽ hiển thị `#ERROR` và thông báo chi tiết trên thanh trạng thái.
- **Tính toán lại:** các công thức tự động cập nhật khi các ô phụ thuộc thay đổi.

---

## 8. Câu hỏi thường gặp

**Làm thế nào để xuất dữ liệu?**
Hiện tại chưa có xuất dữ liệu tích hợp, nhưng bạn có thể truy cập dữ liệu thông qua mô hình nội bộ.

**Có hỗ trợ đồ thị không?**
Không, đây là bảng tính cơ bản. Bạn có thể kết hợp với các widget khác để trực quan hóa.

---

Thưởng thức việc sử dụng bảng tính nhẹ!

© 2026 — FloWorks
