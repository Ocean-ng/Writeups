Q4: kjones tải về file .msi (nó sẽ không chạy độc lập giống .exe) mà gọi `msiexec.exe` để cài đặt

Q5,6: tên dịch vụ đăng kí (ServiceName) sẽ được tìm thấy trong `System.evxt`


## 1. Thông tin chung
- **Hostname:** `WKSTN-01.maromalix.corp`
- **User bị ảnh hưởng:** `kjones`

## 2. Tóm tắt chuỗi tấn công (Attack Chain)

### Giai đoạn 1: Truy cập ban đầu và Thực thi (Initial Access & Execution)
- Người dùng `kjones` tải xuống một tệp cài đặt định dạng `.msi` và mở tệp này.
- Hành động mở tệp `.msi` đã kích hoạt việc thực thi một tệp độc hại (rogue file) trên hệ thống.

### Giai đoạn 2: Thiết lập điều khiển từ xa và Duy trì truy cập (Persistence & RMM)
- Quá trình thực thi tệp độc hại dẫn đến việc cài đặt công cụ quản trị từ xa (RMM) trên máy nạn nhân.
- Dịch vụ mới được đăng ký trên hệ thống (ghi nhận qua `System.evtx` - Event ID `7045`), bao gồm:
  - **Service Name:** `AteraAgent` (`"C:\Program Files\ATERA Networks\AteraAgent\Agent\AteraAgent.exe"`) chạy dưới quyền `LocalSystem`.
  - **Service Name:** `Splashtop® Remote Service` (`"C:\Program Files (x86)\Splashtop\Splashtop Remote\Server\SRService.exe"`).

### Giai đoạn 3: Thu thập thông tin hệ thống (Discovery)
- Sau khi có quyền truy cập thông qua RMM, kẻ tấn công bắt đầu thực hiện các hoạt động dò tìm thông tin (Discovery).
- Một tiến trình thuộc RMM agent đóng vai trò là tiến trình cha (Parent Process) sinh ra toàn bộ các câu lệnh tiếp theo của kẻ tấn công trên hệ thống.
- Kẻ tấn công thực hiện truy vấn các khóa Registry (`reg query`) để kiểm tra trạng thái hiện tại của hệ thống trước khi thực hiện thay đổi.

### Giai đoạn 4: Nỗ lực đánh cắp thông tin xác thực và tắt Defender (Bị chặn)
1. **Thử dump LSASS lần 1:**
   - Kẻ tấn công chạy PowerShell (`C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`) dưới quyền `NT AUTHORITY\SYSTEM` để dump bộ nhớ LSASS nhằm lấy thông tin xác thực.
   - Hành động này bị Microsoft Defender chặn ngay lập tức thông qua AMSI với tên phát hiện: `HackTool:PowerShell/Lsassdump.A` (Threat ID: `2147807171`).
2. **Thử dừng dịch vụ Windows Defender trực tiếp:**
   - Khi dump LSASS thất bại, kẻ tấn công chạy lệnh `C:\Windows\System32\sc.exe stop WinDefend` để dừng trực tiếp dịch vụ Defender.
   - Lệnh này tiếp tục bị Microsoft Defender chặn với tên phát hiện: `Trojan:Win32/MpTamperSrvDisableAV.D` (Threat ID: `2147752485`).

### Giai đoạn 5: Vô hiệu hóa Windows Defender bằng kỹ thuật BYOVD (Defense Evasion)
- Khi cả hai cách trên đều bị chặn, kẻ tấn công khởi chạy một phiên PowerShell tương tác (`ProcessId: 7316`, thư mục làm việc `C:\Users\kjones\AppData\Roaming\`) và thả 2 tệp xuống hệ thống:
  1. `HWAuidoOs2Ec.sys` (Kernel driver)
  2. `kd.exe` (Tệp thực thi dùng để tắt Windows Defender)
- Kẻ tấn công đăng ký `HWAuidoOs2Ec.sys` làm dịch vụ kernel với tên **`HwAudio`** thông qua lệnh (ghi nhận tại `Sysmon` - Event ID `1`):
  ```cmd
  "C:\Windows\system32\sc.exe" create HwAudio type= kernel binPath= C:\Users\kjones\AppData\Roaming\HWAuidoOs2Ec.sys
  ```
- Tiếp theo, kẻ tấn công thực thi `kd.exe`. Phân tích tĩnh tệp `kd.exe` trong IDA cho thấy:
  - Tiến trình mục tiêu mà `kd.exe` nhắm tới để kết thúc là **`MsMpEng.exe`**.
  - `kd.exe` giao tiếp với driver đã nạp bằng hàm `DeviceIoControl` và truyền mã điều khiển (IOCTL code) là **`0x2248DC`** (`2248DCh`) cùng với PID của `MsMpEng.exe` để tiêu diệt tiến trình của Windows Defender từ tầng Kernel.

### Giai đoạn 6: Đánh cắp thông tin xác thực thành công (Credential Access)
- Sau khi Windows Defender (`MsMpEng.exe`) bị vô hiệu hóa, kẻ tấn công thực hiện lại việc dump bộ nhớ của LSASS thành công và tạo tệp dump trên ổ đĩa (ghi nhận qua `Sysmon` - Event ID `11` FileCreate).

## 3. Các nguồn Log và Công cụ phân tích chính
- **System.evtx (Event ID 7045):** Xác định các dịch vụ RMM được cài đặt mới.
- **Microsoft-Windows-Sysmon/Operational.evtx:**
  - **Event ID 1 (Process Create):** Truy vết cây tiến trình RMM, các lệnh truy vấn Registry, lệnh tạo dịch vụ kernel `HwAudio` và thời điểm thực thi `kd.exe`.
  - **Event ID 11 (FileCreate):** Xác định các tệp thả xuống hệ thống (`HWAuidoOs2Ec.sys`, `kd.exe`) và thời điểm tệp dump LSASS được tạo trên ổ đĩa.
- **Microsoft-Windows-Windows Defender/Operational.evtx (Event ID 1116 / 1117):** Xác định thời điểm phát hiện hành vi dump LSASS (`HackTool:PowerShell/Lsassdump.A`) và lệnh tắt Defender (`Trojan:Win32/MpTamperSrvDisableAV.D`).
- **IDA:** Phân tích tệp thực thi `kd.exe` để xác định tiến trình mục tiêu (`MsMpEng.exe`) và mã IOCTL (`0x2248DC`) truyền vào hàm `DeviceIoControl`.
