# Disk Forensic Imaging 

## 1. Tổng quan quy trình

Quy trình forensic imaging một ổ đĩa gồm **4 bước chính**:

1. **Xác định ổ đĩa mục tiêu** (Identify target drive)
2. **Chuẩn bị môi trường** – thiết lập write-blocker, audit trail
3. **Tạo forensic image** (Create image file)
4. **Xác minh tính toàn vẹn** (Verify integrity)

> [!IMPORTANT]
> Sử dụng **Linux** làm môi trường thực hiện vì kernel Linux hỗ trợ nhiều file system, và các công cụ mã nguồn mở đáp ứng được yêu cầu về **độ tin cậy chứng cứ** (evidential reliability).

---

## 2. Thông tin kết nối máy ảo (Lab)

| Thông tin | Giá trị |
|-----------|---------|
| Username  | `analyst` |
| Password  | `forensics` |
| IP        | `10.49.190.54` |

---

## 3. Write-Blocker

- **Write-blocker** là thiết bị ngăn chặn mọi thao tác ghi lên thiết bị lưu trữ trong quá trình phân tích.
- Hoạt động bằng cách **chặn tất cả lệnh ghi** giữa ổ đĩa đang image và máy tính kết nối.
- **Bắt buộc** khi thao tác với ổ đĩa vật lý để đảm bảo không thay đổi dữ liệu gốc.

---

## 4. Audit Trail – Ghi nhật ký hoạt động

**Audit trail** = bản ghi theo trình tự thời gian, theo dõi mọi hành động trong hệ thống nhằm đảm bảo **trách nhiệm giải trình** và **truy vết**.

### 4.1. Cấu hình Bash History

Chạy các lệnh sau **đầu phiên làm việc** để ghi lại toàn bộ lệnh đã thực thi:

```bash
# Bật ghi lịch sử lệnh
set -o history

# Nối (append) vào file history thay vì ghi đè khi thoát shell
shopt -s histappend

# Ghi lại TẤT CẢ lệnh, không lọc bỏ lệnh nào
export HISTCONTROL=
export HISTIGNORE=

# Đặt file lưu history
export HISTFILE=~/.bash_history

# Không giới hạn số dòng / số lệnh lưu trữ
export HISTFILESIZE=-1
export HISTSIZE=-1

# Thêm timestamp cho mỗi lệnh (định dạng: YYYY-MM-DD HH:MM)
export HISTTIMEFORMAT="%F-%R "
```

### 4.2. Ghi lại toàn bộ phiên terminal

Sử dụng công cụ **`script`** (có sẵn trên hầu hết distro Linux) hoặc **`ttyrec`** để ghi lại toàn bộ session:

```bash
# Ghi toàn bộ phiên terminal vào file
script session_log.txt
```

### 4.3. Lưu output mỗi lệnh

> [!TIP]
> Luôn redirect output của mỗi lệnh ra file khi có thể, ví dụ: `command > output.txt 2>&1`

---

## 5. Xác định ổ đĩa mục tiêu

### 5.1. Liệt kê thiết bị đã mount

```bash
df
```

- Hiển thị các filesystem đã mount.
- Ổ đĩa OS thường nằm ở `/dev/root`.
- **Lưu ý:** Ổ đĩa chưa mount hoặc virtual disk gắn qua loop interface sẽ **không hiện** trong `df`.

### 5.2. Liệt kê tất cả block device

```bash
lsblk -a
```

**Output mẫu (trích):**

```
NAME    MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop11    7:11   0   1.1G  0 loop          ← Ổ đĩa mục tiêu (1.1 GB)
xvda    202:0    0    40G  0 disk
└─xvda1 202:1    0    40G  0 part /         ← Ổ đĩa OS (40 GB)
xvdh    202:112  0     1G  0 disk
```

> [!NOTE]
> Xác định ổ đĩa mục tiêu dựa vào **kích thước** (1.1 GB). Loop device có thể thay đổi giữa các lần boot — luôn kiểm tra lại.

> [!IMPORTANT]
> Đảm bảo ổ đĩa lưu image có **đủ dung lượng trống** (trong ví dụ: xvda có 40 GB, đủ chứa image 1.1 GB).

### 5.3. Xem thông tin chi tiết loop device

```bash
sudo losetup -l /dev/loop11
```

**Output mẫu:**

```
NAME        SIZELIMIT OFFSET AUTOCLEAR RO BACK-FILE                 DIO LOG-SEC
/dev/loop11         0      0         0  0 /home/ubuntu/example1.img   0     512
```

→ Cho biết: file gốc phía sau (`BACK-FILE`), sector size, chế độ read-only...

### 5.4. Lấy UUID và loại filesystem

```bash
sudo blkid /dev/loop11
```

**Output mẫu:**

```
/dev/loop11: UUID="1895de04-f9ee-4b8b-b49d-9ef55770073c" TYPE="ext4"
```

→ Ghi lại **UUID** và **TYPE** (ext4) vào ghi chú phục vụ báo cáo.

### 5.5. Thông tin phần cứng (ổ đĩa vật lý)

```bash
sudo hdparm -I /dev/sdX
```

→ Lấy thông tin nhà sản xuất, serial number, firmware... (chỉ áp dụng cho ổ đĩa vật lý).

---

## 6. Tạo Forensic Image

Sau khi xác định được thiết bị, bước tiếp theo là **sao chép (image) ổ đĩa** sang file raw image (`.img`) để phân tích sau này. Việc này cũng có thể thực hiện sang một ổ đĩa khác (phổ biến trong thực tế), nhưng ở đây ta sẽ lưu thành file trên hệ thống.

### 6.1. Các công cụ tạo image phổ biến

| Công cụ | Mô tả |
|---------|-------|
| **dd** | Tiện ích Unix tiêu chuẩn để sao chép và chuyển đổi file, thường dùng tạo raw disk image |
| **dc3dd** | Phiên bản nâng cao của `dd`, bổ sung tính năng **hashing** và **logging** cho forensic imaging |
| **ddrescue** | Công cụ khôi phục dữ liệu, sao chép hiệu quả từ ổ đĩa bị hỏng, cố gắng cứu tối đa dữ liệu |
| **FTK Imager** | Công cụ có giao diện GUI, tạo forensic image, dễ sử dụng và nhiều tính năng |
| **Guymager** | Công cụ forensic imaging có GUI, hỗ trợ nhiều định dạng image và log chi tiết |
| **EWF tools (ewfacquire)** | Công cụ tạo và xử lý image định dạng Expert Witness Format (EWF) |

### 6.2. Tạo image bằng dc3dd

Sử dụng **dc3dd** với các tham số sau:

- **`if`** (input file) – thiết bị nguồn cần sao chép
- **`of`** (output file) – file đích để ghi dữ liệu
- **`log`** – file lưu toàn bộ output của quá trình imaging

```bash
sudo dc3dd if=/dev/loop11 of=example1.img log=imaging_loop11.txt
```

**Output mẫu:**

```
dc3dd 7.2.646 started at 2024-06-28 22:58:59 +0000
compiled options:
command line: dc3dd if=/dev/loop11 of=example1.img log=imaging_loop11.txt
device size: 2252800 sectors (probed),    1,153,433,600 bytes
sector size: 512 bytes (probed)
  1153433600 bytes ( 1.1 G ) copied ( 100% ),   13 s, 84 M/s

input results for device `/dev/loop11':
   2252800 sectors in
   0 bad sectors replaced by zeros

output results for file `example1.img':
   2252800 sectors out

dc3dd completed at 2024-06-28 22:59:12 +0000
```

**Các thông tin quan trọng từ output:**

| Thông tin | Giá trị | Ý nghĩa |
|-----------|---------|----------|
| Device size | 2252800 sectors / 1,153,433,600 bytes | Kích thước ổ đĩa nguồn |
| Sector size | 512 bytes | Kích thước mỗi sector |
| Sectors in | 2252800 | Số sector đã đọc từ nguồn |
| Bad sectors | 0 | Không có sector lỗi |
| Sectors out | 2252800 | Số sector đã ghi ra file → **khớp với input** |
| Tốc độ | 84 MB/s trong 13 giây | Thời gian hoàn thành |

### 6.3. Xác nhận file image đã tạo

```bash
ls -alh example1.img
```

**Output mẫu:**

```
-rw-r--r-- 1 root root 1.1G Jun 28 22:28 example1.img
```

→ Kích thước file image (**1.1G**) **khớp với kích thước ổ đĩa nguồn** → quá trình imaging thành công.

> [!TIP]
> File log `imaging_loop11.txt` chứa toàn bộ output của dc3dd — giữ lại file này làm bằng chứng cho audit trail.

> [!NOTE]
> Trong trường hợp sử dụng ổ đĩa vật lý, quy trình tương tự nhưng thay `/dev/loop11` bằng device path thực tế (ví dụ: `/dev/sdb`).

### 6.4. Các loại imaging khác

Ngoài imaging ổ đĩa, còn có thể thực hiện các loại imaging sau:

| Loại | Mô tả |
|------|-------|
| **Remote** | Tạo image qua mạng, cho phép thu thập dữ liệu mà không cần truy cập vật lý vào thiết bị |
| **USB Images** | Tạo image nội dung của ổ USB |
| **Docker Images** | Tạo snapshot của filesystem và cấu hình Docker container (không hoàn toàn là forensic image truyền thống) |

---

## 7. Mount & Xác minh Forensic Image

Sau khi tạo image, cần **mount image** để xác minh rằng quá trình imaging hoàn tất đúng cách và image có thể sử dụng để phân tích.

### 7.1. Kiểm tra trạng thái mount hiện tại

```bash
df -h
```

→ Xác nhận rằng ổ đĩa loop (ví dụ `loop11`) **chưa được mount** (không xuất hiện trong danh sách).

### 7.2. Mount image

Thực hiện theo 3 bước:

**Bước 1:** Tạo thư mục mount point

```bash
sudo mkdir -p /mnt/example1
```

**Bước 2:** Mount file image vào mount point

```bash
sudo mount -o loop example1.img /mnt/example1
```

- **`-o loop`** – sử dụng loop device để mount file image như một ổ đĩa
- **`example1.img`** – file image đã tạo ở bước trước
- **`/mnt/example1`** – thư mục mount point

**Bước 3:** Xác minh bằng cách liệt kê nội dung

```bash
ls /mnt/example1/
```

**Output mẫu:**

```
dir01  dir03  dir05  dir07  dir09  dir11  dir13  dir15  dir17  dir19  dir21  dir23  dir25
dir02  dir04  dir06  dir08  dir10  dir12  dir14  dir16  dir18  dir20  dir22  dir24  lost+found
```

→ Có thể liệt kê được các thư mục trong mount point `/mnt/example1` → **image hoạt động bình thường**, sẵn sàng để phân tích.

> [!TIP]
> Thư mục `lost+found` là thư mục mặc định của filesystem ext4, cho thấy image chứa filesystem ext4 như đã xác nhận qua `blkid` trước đó.

---

## 8. Tóm tắt nhanh – Các lệnh theo thứ tự thực hiện

| Bước | Lệnh | Mục đích |
|------|-------|----------|
| 1 | `set -o history` | Bật ghi lịch sử lệnh |
| 2 | `shopt -s histappend` | Nối history thay vì ghi đè |
| 3 | `export HISTCONTROL=` | Ghi tất cả lệnh |
| 4 | `export HISTIGNORE=` | Không bỏ qua lệnh nào |
| 5 | `export HISTFILE=~/.bash_history` | Đặt file history |
| 6 | `export HISTFILESIZE=-1` | Không giới hạn file size |
| 7 | `export HISTSIZE=-1` | Không giới hạn số lệnh |
| 8 | `export HISTTIMEFORMAT="%F-%R "` | Thêm timestamp |
| 9 | `script session_log.txt` | Ghi toàn bộ session |
| 10 | `df` | Xem thiết bị đã mount |
| 11 | `lsblk -a` | Liệt kê tất cả block device |
| 12 | `sudo losetup -l /dev/loopX` | Thông tin loop device |
| 13 | `sudo blkid /dev/loopX` | Lấy UUID & filesystem type |
| 14 | `sudo hdparm -I /dev/sdX` | Thông tin phần cứng (nếu ổ vật lý) |
| 15 | `sudo dc3dd if=/dev/loopX of=example1.img log=imaging_loopX.txt` | Tạo forensic image |
| 16 | `ls -alh example1.img` | Xác nhận kích thước file image |
| 17 | `sudo mkdir -p /mnt/example1` | Tạo mount point |
| 18 | `sudo mount -o loop example1.img /mnt/example1` | Mount image |
| 19 | `ls /mnt/example1/` | Xác minh nội dung image |

> [!CAUTION]
> Thay `/dev/loopX` bằng loop device thực tế trên máy bạn (xác định qua `lsblk -a` với size ~1.1 GB).

