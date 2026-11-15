# CyberDefenders: Szechuan Sauce 

## 1. Bài lab có những dữ liệu nào?

Lab có hai máy Windows và dữ liệu mạng. Cần tách dữ liệu theo máy trước khi phân tích:

| Thiết bị | Vai trò | Dùng để trả lời |
|---|---|---|
| **Server / Domain Controller** (`CITADEL-DC01`) | Windows Server 2012 R2; lưu Active Directory | Hệ điều hành, IP, múi giờ, tiến trình malware, `NTDS.dit`, thông tin domain |
| **Desktop** (`DESKTOP-SDN1RPT`) | Windows 10 Enterprise Evaluation | Máy attacker di chuyển ngang tới; người dùng đăng nhập và dấu vết file |
| **PCAP** (`Case001.pcap`) | Ghi lại lưu lượng mạng | RDP, NTP, IP gửi payload và IP liên lạc với malware |
| **Event logs / registry hives** | Nhật ký Windows và cấu hình máy | So sánh thời gian; xác định OS, timezone và hoạt động đăng nhập |


## 2. Diễn biến tấn công

1. **Truy cập ban đầu qua RDP.** Dấu vết mạng cho thấy giao thức được dùng để vào hệ thống là Remote Desktop Protocol (RDP).
2. **Payload được chuyển tới từ `194.61.24.102`.** IP này cũng từng phân giải thành `klient-293.xyz`.
3. **Malware chạy trên Server/DC với tên `coreupdater.exe`.** Đường dẫn trong lab là `C:\Windows\System32\coreupdater.exe`.
4. **Malware liên lạc ra ngoài tới `203.78.103.109`.** Lab ghi nhận IP này có vị trí địa lý ở Thailand. Geolocation chỉ mô tả vị trí đăng ký/định tuyến của IP, không chứng minh danh tính hay vị trí thật của attacker.
5. **Malware chuyển sang tiến trình `spoolsv.exe`.** Vì `spoolsv.exe` là tiến trình hợp lệ của Windows, cần dựa vào bằng chứng bộ nhớ (như `malfind`) chứ không kết luận chỉ từ tên.
6. **Attacker di chuyển ngang qua RDP sang Desktop `DESKTOP-SDN1RPT`.**
7. **Hash tài khoản domain được lấy từ NTDS.** Hash của `jerrysmith` được crack ra mật khẩu `!BETHEYBOO12!`.
8. **Một file bí mật của Beth bị thay đổi/xóa.** Journal NTFS giúp tìm tên file gốc; bản trong Recycle Bin cho biết nội dung cũ.
9. **Kỹ thuật persistence được gắn với Windows Service, MITRE `T1543.003`.** MITRE mô tả kỹ thuật này là tạo hoặc sửa Windows service để chạy lại payload khi hệ thống khởi động hoặc theo lịch.
