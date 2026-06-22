# DFIR Tools Overview - TryHackMe Shadow Trace

> Ghi chú: Nội dung này được tổng hợp dựa trên danh sách thư mục công cụ trong máy lab TryHackMe Shadow Trace. Vì ảnh chỉ hiển thị tên thư mục, phần mô tả bên dưới giải thích **chức năng thường gặp** của từng nhóm tool trong điều tra số DFIR.

---

## 1. Tổng quan

Trong bài lab DFIR / Windows Forensics, bộ công cụ thường được chia theo từng nhóm nhiệm vụ:

- Thu thập nhanh dữ liệu hệ thống
- Phân tích ổ đĩa và file system
- Phân tích registry
- Phân tích log
- Phân tích trình duyệt
- Phân tích email
- Phân tích malware
- Phân tích memory dump
- Phân tích network
- Khôi phục dữ liệu
- Kiểm tra persistence
- Chuyển đổi timestamp

Mục tiêu chính là dựng lại toàn bộ timeline sự kiện, tìm hành vi đáng ngờ, xác định file độc hại, persistence, nguồn lây nhiễm và dấu vết attacker để lại.

---

## 2. Danh sách tool và nhiệm vụ

| Thư mục / Tool | Nhiệm vụ chính | Khi nào dùng trong DFIR |
|---|---|---|
| `Aurora` | Công cụ hunting / detection dựa trên rule, thường dùng để phát hiện dấu hiệu bất thường trên hệ thống. | Dùng để quét nhanh artifact, dấu hiệu malware, IOC hoặc hành vi đáng ngờ. |
| `Browser Analysis` | Phân tích dữ liệu trình duyệt như history, downloads, cookies, cache, autofill. | Khi cần tìm website attacker truy cập, file đã tải về, URL độc hại hoặc lịch sử duyệt web. |
| `Data Triaging` | Thu thập và xem nhanh dữ liệu quan trọng từ hệ thống. | Dùng ở giai đoạn đầu để nắm tổng quan máy nạn nhân: user, process, file đáng ngờ, log, autorun. |
| `DCode v5` | Chuyển đổi nhiều định dạng thời gian khác nhau. | Khi gặp timestamp dạng Windows FILETIME, Unix epoch, Chrome time, WebKit time, cần đổi sang thời gian dễ đọc. |
| `Disk Analysis` | Phân tích ổ đĩa, phân vùng, file system và dữ liệu trong image. | Khi cần xem nội dung disk image, file bị xóa, metadata, MFT, thư mục người dùng. |
| `Email Analysis` | Phân tích email, file `.eml`, `.msg`, header, attachment. | Khi nghi ngờ tấn công bắt đầu từ phishing email hoặc file đính kèm độc hại. |
| `exiftool` | Đọc metadata của file. | Dùng để xem metadata ảnh, tài liệu Office, PDF, file tải về, thông tin author, thời gian tạo/sửa. |
| `EZ Tools` | Bộ công cụ forensics của Eric Zimmerman. | Dùng rất nhiều trong Windows Forensics: phân tích registry, shellbags, amcache, shimcache, prefetch, lnk, jumplist. |
| `HxD` | Hex editor, xem và chỉnh sửa dữ liệu dạng byte. | Khi cần xem raw content của file, magic bytes, header, footer, string ẩn hoặc kiểm tra file bị đổi đuôi. |
| `LogFileParser` | Phân tích log file. | Dùng để đọc và lọc log hệ thống, ứng dụng, web server hoặc log do attacker tạo ra. |
| `Malware Analysis` | Phân tích file nghi mã độc. | Khi tìm được file `.exe`, `.dll`, script, macro hoặc payload đáng ngờ cần kiểm tra. |
| `Memory Analysis` | Phân tích RAM dump. | Khi có file memory dump, dùng để tìm process độc hại, network connection, injected code, command history. |
| `Network Analysis` | Phân tích lưu lượng mạng, PCAP hoặc kết nối. | Khi cần tìm C2 server, IP lạ, domain độc hại, dữ liệu bị exfiltrate hoặc protocol bất thường. |
| `Nmap` | Quét host, port, service trong mạng. | Dùng khi cần kiểm tra máy nào đang mở port, service nào đang chạy, hoặc xác định bề mặt tấn công. |
| `PersistenceSniper` | Phát hiện cơ chế persistence trên Windows. | Khi cần tìm cách attacker duy trì quyền truy cập: Run key, scheduled task, service, startup folder. |
| `pestudio` | Phân tích tĩnh file PE như `.exe`, `.dll`. | Dùng để kiểm tra file Windows executable: import, string, entropy, signature, indicator đáng ngờ. |
| `Recovery Data` | Khôi phục dữ liệu bị xóa hoặc bị ẩn. | Khi cần tìm file attacker đã xóa, file bị rename, hoặc dữ liệu bị mất trong ổ đĩa/image. |
| `Registry Analysis` | Phân tích Windows Registry. | Dùng để tìm thông tin user, USB, chương trình đã chạy, autorun, MRU, network, persistence. |
| `Sleuth Kit` | Bộ công cụ phân tích disk image bằng CLI. | Dùng để liệt kê file system, tìm file deleted, phân tích inode/MFT, extract file từ image. |
| `SysinternalsSuite` | Bộ công cụ quản trị và điều tra Windows của Microsoft. | Dùng để xem process, autorun, TCP connection, handle, service, DLL, hoạt động hệ thống live. |
| `USB Analysis` | Phân tích dấu vết USB. | Khi cần biết USB nào đã cắm vào máy, thời gian cắm, serial number, volume name. |

---

## 3. Giải thích chi tiết từng nhóm

### 3.1. Aurora

`Aurora` thường được dùng cho threat hunting và phát hiện dấu hiệu bất thường dựa trên rule hoặc indicator.

Nhiệm vụ thường gặp:

- Quét artifact đáng ngờ
- Tìm dấu hiệu malware
- Tìm IOC
- Hỗ trợ phát hiện hành vi bất thường
- Dùng trong giai đoạn rà soát nhanh

Trong challenge, có thể dùng khi cần kiểm tra toàn bộ hệ thống xem có dấu hiệu compromise hay không.

---

### 3.2. Browser Analysis

Nhóm này dùng để phân tích dữ liệu trình duyệt.

Các artifact thường cần xem:

- Browser history
- Download history
- Cookies
- Cache
- Login data
- Autofill
- Extension
- Session restore

Dùng để trả lời các câu hỏi như:

- Attacker đã truy cập website nào?
- File độc hại được tải từ URL nào?
- Người dùng có mở link phishing không?
- File được tải xuống lúc mấy giờ?
- Domain nào xuất hiện trong lịch sử trình duyệt?

---

### 3.3. Data Triaging

`Data Triaging` dùng để thu thập thông tin nhanh ở giai đoạn đầu.

Mục tiêu:

- Có cái nhìn tổng quan về hệ thống
- Xác định điểm đáng nghi để đào sâu
- Ưu tiên artifact quan trọng trước

Thông tin thường cần triage:

- User account
- Recent files
- Running process
- Network connection
- Startup items
- Scheduled tasks
- Services
- Event logs
- Browser artifacts
- Registry artifacts

Đây thường là bước nên làm sớm nhất trong quá trình điều tra.

---

### 3.4. DCode v5

`DCode v5` dùng để chuyển đổi timestamp.

Trong Windows Forensics, thời gian có thể xuất hiện dưới nhiều dạng khác nhau:

- Unix epoch
- Windows FILETIME
- Chrome timestamp
- WebKit timestamp
- FAT timestamp
- OLE Automation date

Ví dụ ứng dụng:

- Chuyển timestamp trong Chrome history sang giờ dễ đọc
- Đổi thời gian trong registry
- Đổi thời gian trong event log
- So sánh nhiều artifact theo cùng một timezone

Tool này rất quan trọng khi dựng timeline.

---

### 3.5. Disk Analysis

Nhóm `Disk Analysis` dùng để phân tích ổ đĩa hoặc disk image.

Nhiệm vụ:

- Xem cấu trúc phân vùng
- Duyệt file system
- Tìm file ẩn
- Tìm file bị xóa
- Phân tích metadata
- Xem thư mục user
- Kiểm tra file lạ trong Downloads, Desktop, AppData, Temp

Các vị trí hay kiểm tra trên Windows:

```text
C:\Users\<User>\Desktop
C:\Users\<User>\Downloads
C:\Users\<User>\Documents
C:\Users\<User>\AppData\Roaming
C:\Users\<User>\AppData\Local
C:\Users\<User>\AppData\Local\Temp
C:\ProgramData
C:\Windows\Temp
```


# File Analysis

| Câu hỏi                                                                    | Tool nên dùng                               | Lý do                                                                    |
| -------------------------------------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------------------ |
| What is the architecture of the binary file `windows-update.exe`?          | `pestudio`                                  | Xem PE header, architecture, machine type: x86/x64                       |
| What is the hash SHA-256?                                                  | `PowerShell` hoặc `pestudio`                | Tính hash file chính xác                                                 |
| Identify the URL within the file to use it as an IOC                       | `strings.exe`, `pestudio`, `HxD`            | Trích xuất string trong binary để tìm `http`, `https`, domain            |
| With the URL identified, can you spot a domain that can be used as an IOC? | `strings.exe`, `pestudio`                   | Từ URL tách domain đáng ngờ                                              |
| Input the decoded flag from the suspicious domain                          | `PowerShell`, CyberChef nếu có, hoặc Python | Decode chuỗi nghi là Base64/Hex trong domain                             |
| What library related to socket communication is loaded by the binary?      | `pestudio`                                  | Xem Import Table / Libraries, thường nằm trong danh sách DLL được import |

ta sẽ thấy file .exe này sử dụng kiến trúc 64-bit và mã sha256
<img width="1886" height="959" alt="image" src="https://github.com/user-attachments/assets/04d1d673-c23c-4fd0-bc5a-32faa208b976" />


ta có thể thấy url mà file đã được tải về và decode url ở dưới để có được flag 
<img width="1971" height="840" alt="image" src="https://github.com/user-attachments/assets/da5528b3-2fcf-44f4-a682-d2e24e519f56" />
<img width="2038" height="869" alt="image" src="https://github.com/user-attachments/assets/408fb7f2-7d56-4ff8-baf6-9339dec80f7a" />

lướt tiếp ta tìm được domain khác của url trên
<img width="1188" height="294" alt="image" src="https://github.com/user-attachments/assets/f3f1c099-7285-496d-ba1c-924df15dbabf" />


ta xem thư viện được dùng trong libraries
<img width="1524" height="745" alt="image" src="https://github.com/user-attachments/assets/b3c0ad2f-c7fe-432b-b266-1c98d34f162a" />

# Alert Analysis

<img width="1587" height="694" alt="image" src="https://github.com/user-attachments/assets/4541a68b-04cc-41a8-bc56-cb41e169d2da" />

ta lên cyberchef để decode base64 này thì tìm đc url chứa malware

<img width="1483" height="775" alt="image" src="https://github.com/user-attachments/assets/79ba8b08-8670-49c0-a202-8eee8c643a3c" />

với câu hỏi 2, JavaScript bị obfuscate bằng ASCII decimal code, bạn có thể hỏi AI để nó decode cho mình

<img width="1216" height="763" alt="image" src="https://github.com/user-attachments/assets/bff3691a-6040-4f00-a7f4-827fc70c7cdf" />

download='test.txt' là dành cho câu cuối 











