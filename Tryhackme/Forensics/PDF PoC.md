
# 📌 Bối cảnh — Tại sao PDF lại nguy hiểm?

Hầu hết mọi người đều tin tưởng file PDF. Đó chính là lý do kẻ tấn công yêu thích nó.

Không giống file `.exe` bị email filter chặn ngay lập tức, một file `.pdf` trông hoàn toàn vô hại — nó có thể là hóa đơn, hợp đồng, CV xin việc. Nhưng bên trong cấu trúc của PDF, kẻ tấn công có thể nhúng **JavaScript** chạy tự động ngay khi nạn nhân mở file, trước cả khi họ kịp đọc một chữ nào.

Lab này sẽ cho bạn thấy điều đó hoạt động ra sao — từ góc nhìn của **kẻ tấn công**, **nạn nhân**, lẫn **chuyên gia phân tích**.

---

## 🧱 Phần 1 — Hiểu cấu trúc của một file PDF

Trước khi tạo lab, bạn cần biết: **PDF không phải là file nhị phân huyền bí**. Ở dạng cơ bản nhất, nó chỉ là một file văn bản thuần (plain text) chứa các **Object** (đối tượng) được liên kết với nhau theo cấu trúc:

```
%PDF-[version]      ← Khai báo phiên bản
N 0 obj             ← Bắt đầu một đối tượng số N
  << ... >>         ← Nội dung đối tượng (dictionary)
endobj              ← Kết thúc đối tượng
trailer             ← Bảng chỉ mục cuối file
%%EOF               ← Kết thúc file
```

Điểm mấu chốt: **PDF hỗ trợ JavaScript thông qua chuẩn Acrobat**. Đây là tính năng hợp lệ được thiết kế cho các form tương tác, nhưng cũng chính là con dao hai lưỡi bị lợi dụng để tấn công.

---

## ⚗️ Phần 2 — Tạo file PDF chứa JavaScript (Góc nhìn kẻ tấn công)

### Bước 2.1: Soạn thảo file thủ công

Mở **Notepad** (Windows) hoặc bất kỳ text editor nào trên Linux (`nano`, `gedit`...), rồi dán chính xác đoạn sau:

```
%PDF-1.1
1 0 obj
<< /Type /Catalog /Pages 2 0 R /OpenAction 3 0 R >>
endobj

2 0 obj
<< /Type /Pages /Count 1 /Kids [ 4 0 R ] >>
endobj

3 0 obj
<< /Type /Action /S /JavaScript /JS (app.alert("Canh bao! Ban vua kich hoat JavaScript an trong PDF!");) >>
endobj

4 0 obj
<< /Type /Page /Parent 2 0 R /MediaBox [ 0 0 612 792 ] >>
endobj

trailer
<< /Root 1 0 R >>
%%EOF
```

### Bước 2.2: Lưu file đúng cách

- **Windows:** Chọn *Save As* → *Save as type: All Files* → đặt tên `lab_thu_nghiem.pdf`
  _(Nếu không chọn "All Files", Windows sẽ tự thêm đuôi `.txt` thành `lab_thu_nghiem.pdf.txt`)_
- **Linux:**
  ```bash
  nano lab_thu_nghiem.pdf
  # Dán nội dung vào → Ctrl+O → Enter → Ctrl+X
  ```

---

## 🔍 Phần 3 — Mổ xẻ cấu trúc (Góc nhìn chuyên gia phân tích)

Bây giờ hãy đọc lại file bạn vừa tạo một cách có chủ đích:

### Object 1 — Bảng mục lục (Catalog)
```
1 0 obj
<< /Type /Catalog /Pages 2 0 R /OpenAction 3 0 R >>
endobj
```
Đây là "bộ não" của file PDF, điều phối toàn bộ tài liệu.

> 🚨 **Dấu hiệu đáng ngờ:** `/OpenAction 3 0 R`
> Lệnh này ra lệnh cho trình đọc PDF: *"Ngay khi mở file, hãy thực thi Object số 3 ngay lập tức"* — không hỏi, không báo trước.

### Object 3 — Payload độc hại
```
3 0 obj
<< /Type /Action /S /JavaScript /JS (app.alert("Canh bao!...")) >>
endobj
```

| Trường | Ý nghĩa |
|---|---|
| `/Type /Action` | Khai báo đây là một hành động thực thi |
| `/S /JavaScript` | Loại hành động: chạy JavaScript |
| `/JS (...)` | Nội dung script cần thực thi |

Trong lab này, payload chỉ là một `app.alert()` vô hại để minh họa. Nhưng trong thực tế, kẻ tấn công có thể thay bằng mã khai thác lỗ hổng của Acrobat Reader, hoặc script tải về và chạy malware.

### Objects 2 và 4 — Lớp ngụy trang
```
2 0 obj  ← Khai báo tập hợp các trang
4 0 obj  ← Định nghĩa một trang trắng hợp lệ
```
Không có chúng, nhiều trình đọc PDF sẽ từ chối mở file. Đây là **kỹ thuật tạo vỏ bọc** để file trông "bình thường" và qua được kiểm tra sơ bộ.

---

## 💥 Phần 4 — Trải nghiệm góc nhìn nạn nhân

Mở `lab_thu_nghiem.pdf` bằng các phần mềm khác nhau để thấy sự khác biệt:

| Trình đọc | Kết quả | Lý do |
|---|---|---|
| **Chrome / Edge / Firefox** | Trang trắng, không có gì xảy ra | Trình duyệt hiện đại đã tắt JavaScript trong built-in PDF viewer |
| **Foxit Reader** | Pop-up cảnh báo bật lên ngay lập tức | Hỗ trợ JavaScript theo chuẩn Acrobat |
| **Adobe Acrobat Reader** | Có thể hỏi xác nhận trước khi chạy JS (tùy phiên bản và cài đặt) | Các phiên bản gần đây thêm lớp hỏi trước khi thực thi JavaScript |

> 💡 **Bài học thực tế:** Người dùng doanh nghiệp thường dùng Acrobat hoặc Foxit vì cần tính năng đầy đủ. Đây chính là nhóm mục tiêu mà kẻ tấn công nhắm đến.

---

## 🕵️ Phần 5 — Điều tra dấu vết bằng `peepdf`

### Giới thiệu công cụ

`peepdf` là công cụ phân tích PDF viết bằng Python, tác giả: **Jose Miguel Esparza**. Nó có sẵn trên **Kali Linux** và **REMnux**, giúp bạn nhanh chóng phát hiện các object đáng ngờ mà không cần mở file.

### Chạy phân tích cơ bản

```bash
peepdf lab_thu_nghiem.pdf
```

Output thực tế sẽ trông như thế này (dựa trên format thực của peepdf):

```
Warning: PyV8 is not installed!!
File: lab_thu_nghiem.pdf
MD5: [hash]
SHA1: [hash]
SHA256: [hash]
Size: [size] bytes
Version: 1.1
Binary: False
Linearized: False
Encrypted: False
Updates: 0
Objects: 4
Streams: 0
URIs: 0
Comments: 0
Errors: 0

Version 0:
  Catalog: 1
  Info: No
  Objects (4): [1, 2, 3, 4]
  Streams (0): []
  Encoded (0): []
  Objects with JS code (1): [3]
  Suspicious elements:
    /OpenAction (1): [1]    ← 🚨 Dấu hiệu 1
    /JS (1): [3]            ← 🚨 Dấu hiệu 2
    /JavaScript (1): [3]    ← 🚨 Dấu hiệu 3
```

> ⚠️ Dòng `Warning: PyV8 is not installed!!` là bình thường — PyV8 là thư viện tùy chọn cho phân tích JavaScript nâng cao, thiếu nó vẫn phân tích được cấu trúc file.

> 🔴 **Ba dấu hiệu đỏ trong `Suspicious elements`** là những thứ bạn cần tìm khi phân tích PDF lạ. Chúng không có nghĩa là file chắc chắn độc hại, nhưng **đáng để điều tra sâu hơn**.

### Xem nội dung object đáng ngờ (chế độ tương tác)

```bash
# Khởi động chế độ tương tác với flag -i
peepdf -i lab_thu_nghiem.pdf

# Trong shell của peepdf (dấu nhắc PPDF>), xem nội dung Object 3:
PPDF> object 3
```

Output:
```
<< /Type /Action
   /S /JavaScript
   /JS (app.alert("Canh bao! Ban vua kich hoat JavaScript an trong PDF!");)
>>
```

### Trích xuất toàn bộ JavaScript

Trong chế độ tương tác, bạn có thể dùng lệnh `extract` để lấy toàn bộ JS trong file:

```bash
PPDF> extract js
```

Hoặc dùng lệnh một dòng từ terminal (không cần vào interactive mode):

```bash
echo 'extract js > output_js.txt' > script.txt
peepdf -s script.txt lab_thu_nghiem.pdf
```


---

> ⚠️ **Lưu ý đạo đức:** Kiến thức trong tài liệu này chỉ dành cho mục đích học tập, nghiên cứu bảo mật, và phòng thủ. Việc tạo hoặc phát tán file độc hại nhắm vào người dùng thực là hành vi vi phạm pháp luật.
