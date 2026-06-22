# 📁 Ghi Chú Pháp Y Số — Hệ Thống Tập Tin (File Systems)

---

## 1. Thiết Bị Lưu Trữ Là Gì?

Ổ cứng (HDD), USB, thẻ SD,... về bản chất chỉ là **một chuỗi các bit (0 và 1)**. Bản thân các bit này không có ý nghĩa gì nếu không được **tổ chức theo một quy tắc** nhất định.

> **File System (Hệ thống tập tin)** chính là bộ quy tắc đó — nó quy định cách các bit được sắp xếp, đánh dấu và liên kết để tạo thành các tập tin và thư mục mà con người có thể hiểu được.

**🔍 Ý nghĩa trong Pháp Y Số:**  
Khi phân tích một ổ đĩa, điều đầu tiên cần xác định là nó sử dụng **file system nào**, vì mỗi loại có cấu trúc khác nhau → cách trích xuất và phục hồi dữ liệu cũng khác nhau.

---

## 2. FAT — File Allocation Table

### 2.1. FAT là gì?

FAT là một hệ thống tập tin do Microsoft phát triển từ **cuối thập niên 1970**. Dù không còn là mặc định trên Windows hiện đại, FAT vẫn được sử dụng rộng rãi trên USB, thẻ SD, và máy ảnh kỹ thuật số.

### 2.2. Ba thành phần cốt lõi của FAT

FAT hoạt động dựa trên **3 cấu trúc dữ liệu chính**:

| Thành phần | Chức năng | Ví dụ dễ hiểu |
|---|---|---|
| **Cluster** | Đơn vị lưu trữ cơ bản. Mỗi tập tin = một nhóm các cluster chứa dữ liệu. | Như từng **ngăn kéo** trong tủ hồ sơ — mỗi file chiếm 1 hoặc nhiều ngăn kéo. |
| **Directory** | Lưu thông tin nhận dạng tập tin: tên file, cluster bắt đầu, độ dài tên file. | Như **bảng mục lục** — cho biết file nào nằm ở ngăn kéo nào. |
| **File Allocation Table** | Một danh sách liên kết (linked list) ghi lại trạng thái của từng cluster và con trỏ đến cluster tiếp theo trong chuỗi. | Như **bản đồ kho hàng** — cho biết ngăn kéo nào đang được dùng, ngăn nào trống, và thứ tự liên kết giữa các ngăn. |

### 2.3. Luồng hoạt động tổng quát

```
Bit thô trên đĩa
    ↓ được nhóm thành
Cluster (đơn vị lưu trữ)
    ↓ được ghi nhận vị trí trong
File Allocation Table (bảng FAT)
    ↓ được liên kết với tên file qua
Directory (thư mục)
```

**🔍 Ý nghĩa trong Pháp Y Số:**  
- Khi một file bị **xóa**, Directory đánh dấu nó là đã xóa, nhưng **dữ liệu trong cluster vẫn còn** cho đến khi bị ghi đè → đây là cơ sở để **khôi phục file đã xóa**.  
- Bảng FAT cho biết cluster nào đang trống, đang dùng, hoặc bị hỏng → giúp xác định **dữ liệu ẩn hoặc tàn dư (slack space, unallocated space)**.

---

## 3. Các Phiên Bản FAT: FAT12, FAT16, FAT32

Sự khác biệt giữa các phiên bản nằm ở **số bit dùng để đánh địa chỉ cluster** → quyết định số lượng cluster tối đa → quyết định dung lượng tối đa.

| Thuộc tính | FAT12 | FAT16 | FAT32 |
|---|---|---|---|
| **Số bit địa chỉ** | 12 | 16 | 28 (không phải 32) |
| **Số cluster tối đa** | 4.096 (2¹²) | 65.536 (2¹⁶) | 268.435.456 (2²⁸) |
| **Kích thước cluster** | 512B – 8KB | 2KB – 32KB | 4KB – 32KB |
| **Dung lượng tối đa** | 32MB | 2GB | 2TB |
| **Giới hạn file tối đa** | — | 4GB - 1 byte | 4GB - 1 byte |

### Lưu ý quan trọng:

- **FAT32 dùng 28-bit** chứ không phải 32-bit. 4 bit còn lại dành cho mục đích dự phòng.
- Một số cluster không dùng để lưu file mà dành cho **mục đích quản trị**: đánh dấu cuối chuỗi, đánh dấu vùng hỏng, v.v.
- **Windows giới hạn** chỉ cho phép format FAT32 tối đa **32GB**, mặc dù FAT32 hỗ trợ đến 2TB. Tuy nhiên, ổ đĩa được format FAT32 dung lượng lớn hơn trên hệ điều hành khác vẫn **đọc được trên Windows**.
- **FAT12** gần như không còn gặp trong thực tế hiện nay.
- **FAT16 và FAT32** vẫn được dùng trên USB, thẻ SD, máy ảnh kỹ thuật số nhưng ngày càng bị hạn chế do giới hạn file 4GB.

**🔍 Ý nghĩa trong Pháp Y Số:**  
- Xác định đúng phiên bản FAT giúp chọn **công cụ và phương pháp phân tích phù hợp**.
- FAT32 rất phổ biến trên **bằng chứng USB và thẻ nhớ** — là thứ thường gặp nhất khi thu thập chứng cứ số.

---

## 4. exFAT — Extended File Allocation Table

### 4.1. Tại sao cần exFAT?

Khi camera và thiết bị số hỗ trợ video/ảnh độ phân giải cao, file dễ dàng **vượt quá 4GB** (giới hạn của FAT32). Trong khi đó:
- **NTFS** (file system mặc định của Windows hiện đại) quá nặng, nhiều tính năng bảo mật không cần thiết cho thiết bị nhúng.
- → Các nhà sản xuất thiết bị đã yêu cầu Microsoft tạo ra **exFAT** — nhẹ hơn NTFS nhưng vượt qua giới hạn của FAT32.

### 4.2. Thông số kỹ thuật exFAT

| Thuộc tính | Giá trị |
|---|---|
| **Kích thước cluster** | 4KB – 32MB |
| **Kích thước file tối đa** | 128 PB (Petabyte) |
| **Dung lượng ổ đĩa tối đa** | 128 PB |
| **Số file tối đa / thư mục** | 2.796.202 |

### 4.3. Đặc điểm nổi bật

- Là **mặc định** cho thẻ SD có dung lượng **trên 32GB** (SDXC).
- Giảm bớt overhead so với FAT32 → nhẹ hơn, hiệu quả hơn.
- Được hỗ trợ rộng rãi trên cả Windows, macOS, Linux, và hầu hết thiết bị số.

**🔍 Ý nghĩa trong Pháp Y Số:**  
- Khi phân tích **thẻ nhớ từ camera, drone, hoặc thiết bị ghi hình** → rất có thể gặp exFAT.
- exFAT có cấu trúc đơn giản hơn NTFS → phân tích và phục hồi dữ liệu tương đối **dễ dàng hơn**.
- Cần nắm rõ sự khác biệt exFAT vs FAT32 để tránh nhầm lẫn khi phân tích chứng cứ.

---

## 5. NTFS — New Technology File System

### 5.1. NTFS là gì?

FAT đơn giản và làm tốt việc tổ chức dữ liệu, nhưng **thiếu bảo mật, độ tin cậy, và khả năng phục hồi**. Microsoft phát triển NTFS để khắc phục những hạn chế này.

- Ra đời năm **1993** (Windows NT 3.1), trở thành **mặc định từ Windows XP** trở đi.
- Là file system chính trên **mọi máy tính Windows hiện đại**.

### 5.2. Các tính năng quan trọng của NTFS

#### 📝 Journaling (Ghi nhật ký)

NTFS ghi lại **nhật ký các thay đổi metadata** vào file `$LOGFILE` ở thư mục gốc của volume. Nếu hệ thống bị crash hoặc mất điện đột ngột, NTFS dùng nhật ký này để **khôi phục lại trạng thái nhất quán** của file system mà không mất dữ liệu.

> Vì tính năng này, NTFS được gọi là **journaling file system** (hệ thống tập tin có ghi nhật ký).

**🔍 Pháp Y Số:** `$LOGFILE` chứa lịch sử thay đổi metadata → có thể dùng để **truy vết hành vi** (file nào bị sửa, xóa, tạo mới gần đây).

---

#### 🔒 Access Controls (Kiểm soát truy cập)

Khác với FAT (không phân biệt người dùng), NTFS cho phép:
- Gán **chủ sở hữu (owner)** cho từng file/thư mục.
- Thiết lập **quyền truy cập (permissions)** riêng cho từng user hoặc nhóm user.

**🔍 Pháp Y Số:** Kiểm tra permissions giúp xác định **ai có quyền truy cập/sửa/xóa** một file cụ thể — rất quan trọng khi điều tra.

---

#### 📸 Volume Shadow Copy (Bản sao lưu ẩn)

NTFS theo dõi các thay đổi của file và tạo ra **các bản sao trước khi thay đổi (shadow copies)**. Cho phép người dùng **khôi phục phiên bản cũ** của file.

> **⚠️ Lưu ý thực tế:** Trong các cuộc tấn công ransomware, kẻ tấn công thường **xóa tất cả shadow copies** trên máy nạn nhân để ngăn họ khôi phục dữ liệu mà không trả tiền chuộc.

**🔍 Pháp Y Số:** Nếu shadow copies chưa bị xóa, chúng ta có thể **phục hồi các phiên bản file trước khi bị mã hóa/xóa/sửa đổi** — cực kỳ giá trị trong điều tra.

---

#### 📎 Alternate Data Streams — ADS (Luồng dữ liệu thay thế)

Trên NTFS, mỗi file có thể chứa **nhiều luồng dữ liệu (data streams)** bên trong nó, không chỉ một.

Ví dụ thực tế:
- Khi tải file từ Internet, trình duyệt gắn thêm một ADS gọi là **Zone Identifier** để đánh dấu file đó là "tải từ Internet" → Windows hiện cảnh báo khi mở.
- **Malware** có thể **giấu mã độc trong ADS** của một file bình thường — file trông vô hại nhưng thực chất chứa payload ẩn.

**🔍 Pháp Y Số:**
- Kiểm tra ADS để tìm **dữ liệu ẩn hoặc mã độc** bị giấu bên trong các file bình thường.
- Phân tích **Zone Identifier** để xác định file nào được **tải từ Internet** và từ nguồn nào.

---

### 5.3. Master File Table (MFT) — Bảng quản lý tập tin chính

MFT là thành phần cốt lõi của NTFS, tương tự File Allocation Table trong FAT nhưng **mạnh mẽ và chi tiết hơn rất nhiều**. Nó là một **cơ sở dữ liệu có cấu trúc** theo dõi mọi đối tượng (file, thư mục) trên volume.

#### Các file quan trọng trong MFT

| File | Chức năng | Ý nghĩa pháp y |
|---|---|---|
| **$MFT** | Bản ghi đầu tiên trên volume. Chứa **danh mục tất cả file** trên ổ đĩa cùng vị trí cluster của chúng. VBR (Volume Boot Record) trỏ đến cluster chứa $MFT. | Phân tích $MFT = có **bản đồ toàn bộ file** trên ổ đĩa, bao gồm cả file đã xóa. |
| **$LOGFILE** | Ghi nhật ký giao dịch (transactional log) của file system, đảm bảo tính toàn vẹn khi xảy ra sự cố. | Truy vết **các thay đổi gần đây** trên file system. |
| **$UsnJrnl** | Update Sequence Number Journal (Nhật ký cập nhật). Nằm trong record `$Extend`. Ghi lại **mọi thay đổi file** và **lý do thay đổi**. Còn gọi là **Change Journal**. | Xác định **file nào bị thay đổi, khi nào, và vì sao** — cực kỳ quan trọng để xây dựng timeline sự kiện. |

---

### 5.4. Công cụ: MFTECmd (Eric Zimmerman)

**MFTECmd** là công cụ dòng lệnh của Eric Zimmerman, dùng để phân tích các file do NTFS tạo ra.

#### Các file hỗ trợ phân tích

| File | Hỗ trợ |
|---|---|
| `$MFT` | ✅ Có |
| `$Boot` | ✅ Có |
| `$UsnJrnl ($J)` | ✅ Có |
| `$LOGFILE` | ❌ Chưa hỗ trợ |

#### Cú pháp cơ bản

```bash
# Phân tích $MFT và xuất ra CSV
MFTECmd.exe -f <đường-dẫn-$MFT> --csv <thư-mục-lưu-kết-quả>

# Phân tích $Boot
MFTECmd.exe -f <đường-dẫn-$Boot> --csv <thư-mục-lưu-kết-quả>

# Phân tích $UsnJrnl kèm $MFT để resolve đường dẫn thư mục cha
MFTECmd.exe -f <đường-dẫn-$J> -m <đường-dẫn-$MFT> --csv <thư-mục-lưu-kết-quả>
```

#### Các tùy chọn hữu ích

| Tùy chọn | Mô tả |
|---|---|
| `-f` | File cần phân tích (bắt buộc) |
| `-m` | File $MFT đi kèm khi phân tích $J |
| `--csv` | Thư mục lưu kết quả CSV |
| `--json` | Thư mục lưu kết quả JSON |
| `--de` | Xem chi tiết một entry cụ thể (ví dụ: `--de 5-5`) |
| `--vss` | Phân tích cả các Volume Shadow Copies |

> **Mẹo:** Dùng **EZviewer** (cùng bộ EZtools) để mở và xem các file CSV kết quả một cách trực quan.

---

## 6. Bảng So Sánh Tổng Hợp

| Tiêu chí | FAT12 | FAT16 | FAT32 | exFAT | NTFS |
|---|---|---|---|---|---|
| **Năm ra đời** | Trước 1980 | ~1987 | 1996 | 2006 | 1993 |
| **Dung lượng tối đa** | 32MB | 2GB | 2TB | 128PB | 256TB |
| **File tối đa** | — | ~4GB | ~4GB | 128PB | 256TB |
| **Journaling** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Access Control** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **ADS** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Shadow Copy** | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Nơi hay gặp** | Hiếm | USB cũ | USB, SD ≤32GB | SD >32GB | Windows PC |
| **Độ phức tạp** | Thấp | Thấp | Trung bình | Trung bình | Cao |

---

## 7. Tóm Tắt Nhanh Cho Pháp Y Số

1. **Xác định file system** là bước đầu tiên khi phân tích bất kỳ thiết bị lưu trữ nào.
2. **FAT** hoạt động qua 3 thành phần: Cluster → Directory → File Allocation Table.
3. **File đã xóa trên FAT** thường có thể khôi phục vì dữ liệu không bị xóa ngay — chỉ bị đánh dấu là "xóa".
4. **FAT32** là file system phổ biến nhất trên USB và thẻ nhớ nhỏ.
5. **exFAT** phổ biến trên thẻ nhớ lớn và thiết bị ghi hình hiện đại.
6. **NTFS** là file system chính trên Windows — cung cấp journaling, access control, shadow copy, và ADS.
7. Trong pháp y số, **$MFT** cho bản đồ toàn bộ file, **$UsnJrnl** cho timeline thay đổi, **$LOGFILE** cho nhật ký giao dịch.
8. **Volume Shadow Copies** có thể chứa phiên bản file trước khi bị tấn công — nhưng ransomware thường xóa chúng.
9. **ADS** có thể chứa mã độc ẩn hoặc metadata về nguồn gốc file (Zone Identifier).
10. Công cụ **MFTECmd** (Eric Zimmerman) là công cụ quan trọng để phân tích $MFT, $Boot, và $UsnJrnl.

---

## 8. Xóa File & Khôi Phục Dữ Liệu

### 8.1. Chuyện gì xảy ra khi xóa file?

Khi xóa một file, **file system không xóa dữ liệu thực sự trên đĩa**. Nó chỉ:

1. **Xóa entry** trong bảng quản lý (FAT hoặc MFT) — tức xóa thông tin vị trí file trên đĩa.
2. **Đánh dấu các cluster** mà file chiếm dụng thành **unallocated** (chưa được phân bổ) → sẵn sàng cho dữ liệu mới ghi đè lên.

> **Kết luận:** Dữ liệu vẫn nằm trên đĩa cho đến khi bị **ghi đè** bởi file khác hoặc bởi firmware đĩa khi bảo trì.

### 8.2. Dữ liệu có thể khôi phục được không?

| Trường hợp | Khôi phục? |
|---|---|
| File vừa xóa, cluster **chưa bị ghi đè** | ✅ Có thể khôi phục hoàn toàn |
| File xóa lâu, cluster **đã bị ghi đè một phần** | ⚠️ Khôi phục một phần |
| File xóa, cluster **đã bị ghi đè hoàn toàn** | ❌ Không thể khôi phục |

Ngoài ra, các vùng **unallocated clusters** trên đĩa cũng có thể chứa dữ liệu tàn dư từ các file cũ → có thể khôi phục bằng cách phân tích cấu trúc file (file signature/header) qua hex editor hoặc công cụ chuyên dụng.

---

## 9. Disk Image (Ảnh đĩa)

### 9.1. Disk Image là gì?

**Disk image** là một file chứa **bản sao bit-by-bit** (từng bit một) của toàn bộ ổ đĩa, bao gồm cả **dữ liệu + metadata** → mọi thứ được lưu vào một file duy nhất.

### 9.2. Tại sao dùng Disk Image trong pháp y?

| Lợi ích | Giải thích |
|---|---|
| **Bảo toàn bằng chứng gốc** | Phân tích trên bản sao, không động vào ổ đĩa gốc → tránh nhiễm bẩn chứng cứ |
| **Dễ sao chép & chia sẻ** | Có thể copy disk image ra nhiều bản, gửi cho nhiều điều tra viên phân tích song song |
| **Không cần phần cứng đặc biệt** | Copy disk image sang ổ đĩa khác rồi phân tích bình thường |

---

## 10. Khôi Phục File Bằng Autopsy

**Autopsy** là công cụ pháp y số mã nguồn mở, dùng để phân tích disk image và khôi phục file đã xóa.

### 10.1. Các bước thực hiện

#### Bước 1: Mở Autopsy
- Double-click icon **Autopsy** trên Desktop.

#### Bước 2: Tạo Case mới
- Click **New Case** → nhập tên case → **Next** → (thêm thông tin nếu cần) → **Finish**.

#### Bước 3: Chọn loại Data Source
- Chọn **"Disk Image or VM File"** (tùy chọn trên cùng) → **Next**.

#### Bước 4: Chỉ định đường dẫn Disk Image
- Duyệt đến file disk image (ví dụ: `usb.001` trên Desktop) → **Next**.

#### Bước 5: Cấu hình Module
- Click **Deselect All** để tắt hết các module xử lý (tiết kiệm thời gian) → **Next**.
- Autopsy sẽ load disk image.

#### Bước 6: Duyệt nội dung đĩa
- Panel trái: **Data Sources** → expand → click vào device (ví dụ: `usb.001`).
- Panel phải phía trên: danh sách **tất cả file và thư mục** trên đĩa.
- Panel phải phía dưới: **chi tiết** của file được chọn (metadata, hex, v.v.).

#### Bước 7: Nhận diện file đã xóa
- File đã xóa được đánh dấu bằng **dấu X** trên icon.
- Ví dụ: `New Microsoft Excel Worksheet.xlsx~RFcd07702.TMP` với dấu X → file đã bị xóa.

#### Bước 8: Khôi phục file
- **Chuột phải** vào file đã xóa → chọn **Extract File(s)** → chọn thư mục lưu → file được khôi phục.

### 10.2. Tóm tắt luồng làm việc

```
Autopsy → New Case → Disk Image or VM File → chọn file .001
    → Deselect All modules → Load
    → Duyệt Data Sources → Tìm file có dấu X (đã xóa)
    → Chuột phải → Extract File(s) → Khôi phục thành công
```

**🔍 Pháp Y Số:**
- Autopsy tự động nhận diện file đã xóa trên disk image mà không cần phân tích thủ công bằng hex editor.
- Kết hợp với phân tích **$MFT** (MFTECmd) để có cái nhìn toàn diện hơn về lịch sử file trên ổ đĩa.

---

## 11. Windows Artifacts — Bằng Chứng Thực Thi (Evidence of Execution)

Các artifact dưới đây giúp xác định **chương trình nào đã chạy, khi nào, và bao nhiêu lần** trên hệ thống Windows.

---

### 11.1. Windows Prefetch Files

#### Prefetch là gì?

Khi một chương trình chạy trên Windows, hệ thống lưu thông tin về nó vào **Prefetch files** để **tải nhanh hơn** cho lần chạy sau.

| Thuộc tính | Chi tiết |
|---|---|
| **Vị trí** | `C:\Windows\Prefetch` |
| **Phần mở rộng** | `.pf` |
| **Chứa gì** | Thời gian chạy gần nhất, số lần chạy, các file và device handle mà chương trình sử dụng |

**🔍 Pháp Y Số:** Prefetch cho biết **chương trình nào đã từng chạy** trên máy, ngay cả khi chương trình đã bị xóa.

#### Công cụ: PECmd.exe (Eric Zimmerman)

```bash
# Phân tích một file Prefetch cụ thể
PECmd.exe -f <đường-dẫn-file-.pf> --csv <thư-mục-lưu-CSV>

# Phân tích toàn bộ thư mục Prefetch
PECmd.exe -d <đường-dẫn-thư-mục-Prefetch> --csv <thư-mục-lưu-CSV>
```

#### Các tùy chọn hữu ích

| Tùy chọn | Mô tả |
|---|---|
| `-f` | File `.pf` cần phân tích |
| `-d` | Thư mục chứa nhiều file `.pf` (xử lý đệ quy) |
| `-k` | Highlight keyword trong output (mặc định: `temp`, `tmp`) |
| `--csv` | Thư mục lưu kết quả CSV |
| `--json` | Thư mục lưu kết quả JSON |
| `--vss` | Phân tích cả Volume Shadow Copies |
| `-q` | Không hiển thị chi tiết (nhanh hơn khi xuất CSV/JSON) |

---

### 11.2. Windows 10 Timeline

#### Timeline là gì?

Windows 10 lưu lại **các ứng dụng và file được sử dụng gần đây** vào một **cơ sở dữ liệu SQLite** gọi là **ActivitiesCache.db**.

| Thuộc tính | Chi tiết |
|---|---|
| **Vị trí** | `C:\Users\<username>\AppData\Local\ConnectedDevicesPlatform\{randomfolder}\ActivitiesCache.db` |
| **Chứa gì** | Ứng dụng đã chạy, **thời gian focus** (thời gian người dùng tương tác với ứng dụng) |

#### Công cụ: WxTCmd.exe (Eric Zimmerman)

```bash
WxTCmd.exe -f <đường-dẫn-ActivitiesCache.db> --csv <thư-mục-lưu-CSV>
```

| Tùy chọn | Mô tả |
|---|---|
| `-f` | File `ActivitiesCache.db` cần phân tích (bắt buộc) |
| `--csv` | Thư mục lưu kết quả CSV |

---

### 11.3. Windows Jump Lists

#### Jump Lists là gì?

Jump Lists cho phép người dùng **truy cập nhanh các file đã mở gần đây** từ taskbar (chuột phải vào icon ứng dụng). Dữ liệu này được Windows lưu tự động.

| Thuộc tính | Chi tiết |
|---|---|
| **Vị trí** | `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations` |
| **Chứa gì** | Ứng dụng đã chạy, **lần chạy đầu tiên**, **lần chạy cuối cùng**, liên kết với **AppID** |

#### Công cụ: JLECmd.exe (Eric Zimmerman)

```bash
# Phân tích một file Jump List
JLECmd.exe -f <đường-dẫn-file-JumpList> --csv <thư-mục-lưu-CSV>

# Phân tích toàn bộ thư mục
JLECmd.exe -d <đường-dẫn-thư-mục> --csv <thư-mục-lưu-CSV>
```

#### Các tùy chọn hữu ích

| Tùy chọn | Mô tả |
|---|---|
| `-f` | File Jump List cần phân tích |
| `-d` | Thư mục chứa Jump Lists (xử lý đệ quy) |
| `--csv` | Thư mục lưu kết quả CSV |
| `--json` | Thư mục lưu kết quả JSON |
| `--ld` | Hiển thị thêm thông tin về lnk files |
| `--fd` | Hiển thị đầy đủ thông tin về lnk files |
| `--all` | Xử lý tất cả file (không chỉ `*.automaticDestinations-ms` và `*.customDestinations-ms`) |

---

### 11.4. Bảng Tổng Hợp — Evidence of Execution

| Artifact | Vị trí | Công cụ | Thông tin chính |
|---|---|---|---|
| **Prefetch** | `C:\Windows\Prefetch` | PECmd.exe | Thời gian chạy, số lần chạy, file liên quan |
| **Win10 Timeline** | `...\ConnectedDevicesPlatform\{..}\ActivitiesCache.db` | WxTCmd.exe | Ứng dụng đã dùng, thời gian focus |
| **Jump Lists** | `...\Recent\AutomaticDestinations` | JLECmd.exe | Ứng dụng + file mở gần đây, thời gian đầu/cuối |

> **Mẹo:** Dùng **EZviewer** để mở các file CSV kết quả một cách trực quan.

---

## 12. Windows Artifacts — Bằng Chứng Truy Cập File/Thư Mục (Evidence of File/Folder Access)

Các artifact dưới đây giúp xác định **file/thư mục nào đã được mở, khi nào mở lần đầu, và lần cuối**.

---

### 12.1. Shortcut Files (.lnk)

#### Shortcut Files là gì?

Windows tự động tạo **file shortcut (.lnk)** cho mỗi file được mở (kể cả file từ xa). Shortcut lưu lại thông tin truy cập file.

| Thuộc tính | Chi tiết |
|---|---|
| **Vị trí** | `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\` |
| **Vị trí (Office)** | `C:\Users\<username>\AppData\Roaming\Microsoft\Office\Recent\` |
| **Chứa gì** | Đường dẫn file gốc, thời gian mở đầu/cuối |

> **Quy tắc quan trọng:**
> - **Ngày tạo** shortcut = thời điểm file được **mở lần đầu**.
> - **Ngày sửa đổi** shortcut = thời điểm file được **truy cập lần cuối**.

#### Công cụ: LECmd.exe (Eric Zimmerman)

```bash
# Phân tích một file shortcut
LECmd.exe -f <đường-dẫn-file-.lnk> --csv <thư-mục-lưu-CSV>

# Phân tích toàn bộ thư mục chứa shortcut
LECmd.exe -d <đường-dẫn-thư-mục> --csv <thư-mục-lưu-CSV>
```

#### Các tùy chọn hữu ích

| Tùy chọn | Mô tả |
|---|---|
| `-f` | File `.lnk` cần phân tích |
| `-d` | Thư mục chứa shortcut (xử lý đệ quy) |
| `--csv` | Thư mục lưu kết quả CSV |
| `--json` | Thư mục lưu kết quả JSON |
| `-r` | Chỉ xử lý shortcut trỏ đến **removable drives** (USB, v.v.) |
| `--all` | Xử lý tất cả file (không chỉ `*.lnk`) |
| `-q` | Chỉ hiện tên file đang xử lý (nhanh hơn khi xuất CSV/JSON) |

---

### 12.2. IE/Edge History

#### Tại sao IE/Edge History quan trọng?

Lịch sử IE/Edge **không chỉ ghi lại web đã truy cập**, mà còn ghi lại **các file được mở trên hệ thống** (kể cả file không mở bằng trình duyệt). Các file/thư mục truy cập sẽ hiển thị với prefix `file:///*`.

| Thuộc tính | Chi tiết |
|---|---|
| **Vị trí** | `C:\Users\<username>\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat` |
| **Chứa gì** | Lịch sử web + **các file/thư mục đã mở trên hệ thống** |

#### Phân tích bằng Autopsy

1. Mở **Autopsy** → tạo case mới.
2. Chọn loại Data Source: **Logical Files** (không phải Disk Image).
3. Chỉ đến thư mục triage cần phân tích.
4. Ở bước chọn module: **chỉ check "Recent Activity"** → bỏ chọn hết các module khác.
5. Sau khi load xong → panel trái → mục **Web History** → xem các file được truy cập (có prefix `file:///*`).
6. Tab **Data Artifacts** ở panel phải hiển thị chi tiết về file đã truy cập.

---

### 12.3. Jump Lists (Tham chiếu)

Jump Lists cũng cung cấp thông tin về **file đã mở gần đây** bởi từng ứng dụng. Xem chi tiết tại **Mục 11.3** ở trên.

| Thuộc tính | Chi tiết |
|---|---|
| **Vị trí** | `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations` |
| **Công cụ** | JLECmd.exe |

---

### 12.4. Bảng Tổng Hợp — Evidence of File/Folder Access

| Artifact | Vị trí | Công cụ | Thông tin chính |
|---|---|---|---|
| **Shortcut Files** | `...\Windows\Recent\` | LECmd.exe | Đường dẫn file, thời gian mở đầu/cuối |
| **IE/Edge History** | `...\WebCache\WebCacheV*.dat` | Autopsy (Recent Activity) | File/thư mục đã mở (prefix `file:///*`) |
| **Jump Lists** | `...\Recent\AutomaticDestinations` | JLECmd.exe | File mở gần đây theo từng app |

---

## 13. Windows Artifacts — Thiết Bị Ngoài / USB (External Devices)

Các artifact dưới đây giúp xác định **thiết bị USB nào đã từng kết nối, khi nào, và serial number**.

---

### 13.1. Setupapi Dev Log

#### Setupapi Dev Log là gì?

Mỗi khi một **thiết bị mới** được kết nối vào máy Windows, thông tin cài đặt thiết bị đó được ghi vào log file.

| Thuộc tính | Chi tiết |
|---|---|
| **Vị trí** | `C:\Windows\inf\setupapi.dev.log` |
| **Chứa gì** | **Device ID**, **Serial Number**, thời gian kết nối **đầu tiên/cuối cùng** |

#### Cách phân tích

Mở file bằng **Notepad** hoặc text editor bất kỳ → tìm kiếm (Ctrl+F) theo tên thiết bị hoặc serial number. Dòng đầu tiên của mỗi entry chứa **Device ID** và **Serial Number**.

**🔍 Pháp Y Số:** Xác định **thiết bị USB nào đã từng cắm vào máy** và **thời điểm kết nối** — rất quan trọng khi điều tra rò rỉ dữ liệu.

---

### 13.2. Shortcut Files (.lnk) cho USB

Shortcut files (đã học ở **Mục 12.1**) cũng có thể cung cấp thông tin về thiết bị USB đã kết nối:

| Thông tin | Mô tả |
|---|---|
| **Volume Name** | Tên ổ đĩa USB |
| **Volume Type** | Loại ổ đĩa (removable, v.v.) |
| **Volume Serial Number** | Serial number của volume |

#### Vị trí & Công cụ

| Vị trí | Công cụ |
|---|---|
| `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\` | LECmd.exe |
| `C:\Users\<username>\AppData\Roaming\Microsoft\Office\Recent\` | LECmd.exe |

> Xem lại **Mục 12.1** để biết cách dùng LECmd.exe.

---

### 13.3. Bảng Tổng Hợp — External Devices / USB

| Artifact | Vị trí | Công cụ | Thông tin chính |
|---|---|---|---|
| **Setupapi Dev Log** | `C:\Windows\inf\setupapi.dev.log` | Text editor (Notepad) | Device ID, Serial Number, thời gian kết nối |
| **Shortcut Files** | `...\Windows\Recent\` | LECmd.exe | Volume Name, Type, Serial Number |

---

## 14. Tóm Tắt Tổng Hợp — Cheat Sheet Pháp Y Số Windows

### 🗂️ File Systems

| Keyword | Ghi nhớ |
|---|---|
| **FAT** | Cluster + Directory + FAT table. Đơn giản, không bảo mật. USB, thẻ nhớ ≤32GB |
| **exFAT** | FAT nâng cấp, hỗ trợ file >4GB. Thẻ SD >32GB, camera, drone |
| **NTFS** | Mặc định Windows. Có Journaling, Access Control, Shadow Copy, ADS |
| **$MFT** | Bản đồ toàn bộ file trên ổ NTFS (kể cả file đã xóa) |
| **$LOGFILE** | Nhật ký thay đổi metadata |
| **$UsnJrnl** | Timeline mọi thay đổi file + lý do thay đổi |
| **ADS** | Luồng dữ liệu ẩn trong file NTFS (malware, Zone Identifier) |

### 🗑️ Xóa File & Khôi Phục

| Keyword | Ghi nhớ |
|---|---|
| **Xóa file** | Chỉ xóa entry, dữ liệu vẫn còn trên đĩa đến khi bị ghi đè |
| **Disk Image** | Bản sao bit-by-bit toàn bộ ổ đĩa → bảo toàn chứng cứ gốc |
| **Autopsy** | Công cụ mã nguồn mở, load disk image → tìm file có dấu X → Extract |

### 🔧 Bảng Công Cụ Eric Zimmerman

| Công cụ | Phân tích gì | Command mẫu |
|---|---|---|
| **MFTECmd** | $MFT, $Boot, $UsnJrnl | `MFTECmd.exe -f <file> --csv <output>` |
| **PECmd** | Prefetch (.pf) | `PECmd.exe -d <dir> --csv <output>` |
| **WxTCmd** | Win10 Timeline | `WxTCmd.exe -f <file.db> --csv <output>` |
| **JLECmd** | Jump Lists | `JLECmd.exe -d <dir> --csv <output>` |
| **LECmd** | Shortcut (.lnk) | `LECmd.exe -d <dir> --csv <output>` |

### 🔍 Muốn biết gì → Dùng Artifact nào?

| Câu hỏi điều tra | Artifact | Công cụ |
|---|---|---|
| **Chương trình nào đã chạy?** | Prefetch | PECmd |
| **Chạy bao nhiêu lần?** | Prefetch (RunCount) | PECmd |
| **Chạy lần cuối khi nào?** | Prefetch (LastRun) | PECmd |
| **App focus bao lâu?** | Win10 Timeline (Duration) | WxTCmd |
| **File nào mở gần đây?** | Jump Lists / Shortcut Files | JLECmd / LECmd |
| **File mở lần đầu/cuối?** | Shortcut Files (Created/Modified) | LECmd |
| **File nào thay đổi trên ổ đĩa?** | $UsnJrnl | MFTECmd |
| **Toàn bộ file trên ổ đĩa?** | $MFT | MFTECmd |
| **Cluster size?** | $Boot | MFTECmd |
| **USB nào từng cắm vào?** | Setupapi dev log | Notepad |
| **USB kết nối khi nào?** | Setupapi dev log | Notepad |
| **Tên/Serial USB?** | Shortcut Files | LECmd |
| **File mở qua trình duyệt/hệ thống?** | IE/Edge History | Autopsy |
| **File đã xóa?** | Disk Image + Autopsy | Autopsy (dấu X) |
