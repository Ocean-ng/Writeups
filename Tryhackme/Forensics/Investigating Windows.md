# Investigating Windows 
---

## 0. Kết Nối Vào Máy Tính Mục Tiêu
- **Giao thức:** Remote Desktop Protocol (RDP)
- **Phím tắt:** `Win + R` -> Nhập `mstsc`
- **Thông tin đăng nhập:** `Administrator` / `letmein123!`

---

## PHẦN 1: ĐIỀU TRA BẰNG GIAO DIỆN ĐỒ HỌA (GUI)
*Dành cho những bạn thích sử dụng các công cụ trực quan có sẵn của Windows.*

### Câu 1: What's the version and year of the windows machine?
**Đáp án:** `Windows Server 2016`
- **Cách làm:** Bấm `Win + R` -> gõ `winver`. Hộp thoại hiện ra dòng "Microsoft Windows Server 2016".

<img width="512" height="434" alt="Screenshot 2026-05-16 204500" src="https://github.com/user-attachments/assets/3acbc5c4-701a-4e08-a7c5-89e6c6410d71" />


### Câu 2: Which user logged in last?
**Đáp án:** `Administrator`
- **Cách làm:** Mở Event Viewer (`eventvwr.msc`) -> `Windows Logs` -> `Security`. Lọc (Filter) Event ID `4624` (Logon thành công). Xem event mới nhất ở trên cùng, bạn sẽ thấy `Administrator` vừa đăng nhập.

<img width="2501" height="1171" alt="Screenshot 2026-05-16 204650" src="https://github.com/user-attachments/assets/6292abfb-64e0-4030-86e0-719e8367cc79" />


### Câu 3: When did John log onto the system last?
**Đáp án:** `03/02/2019 5:48:32 PM`
- **Cách làm:** Trong Event Viewer -> `Security`. Lọc Event ID `4624` -> Chọn `Find...` và tìm "John". Kéo xuống phần `Logged` để xem thời gian. *(Lưu ý: Cách GUI này hơi tốn thời gian, khuyến khích dùng CLI ở Phần 2).*

<img width="2503" height="1185" alt="Screenshot 2026-05-16 204838" src="https://github.com/user-attachments/assets/d5929182-1c5a-4caa-8fa3-584fdbb88893" />


### Câu 4: What IP does the system connect to when it first starts?
**Đáp án:** `10.34.2.3`
- **Cách làm:** Bấm `Win + R` -> chạy `regedit`. Truy cập khóa: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`. Xem cột Data, bạn sẽ thấy script gọi đến IP `10.34.2.3`.

<img width="902" height="751" alt="Screenshot 2026-05-16 210030" src="https://github.com/user-attachments/assets/1e7cba93-6b53-4c30-a921-9edeb140ea3d" />


### Câu 5: What two accounts had administrative privileges (other than the Administrator user)?
**Đáp án:** `Jenny, Guest`
- **Cách làm:** Mở Local Users and Groups (`lusrmgr.msc`) -> Chọn `Groups` -> Nhấp đúp vào `Administrators`. Bạn sẽ thấy Guest và Jenny nằm trong nhóm này. Trả lời theo bảng chữ cái.

<img width="2510" height="870" alt="Screenshot 2026-05-16 210159" src="https://github.com/user-attachments/assets/75e536de-c42c-4605-87c9-84a2d8ce27e3" />


### Câu 6: What's the name of the scheduled task that is malicious?
**Đáp án:** `Clean file system`
- **Cách làm:** Mở Task Scheduler (`taskschd.msc`) -> `Task Scheduler Library`. Có một task mang vỏ bọc là "Clean file system" nhưng thực chất là để chạy mã độc.

<img width="2506" height="755" alt="Screenshot 2026-05-16 210338" src="https://github.com/user-attachments/assets/af2d422b-6811-4dbb-81ec-e4b99f78b2cf" />


### Câu 7: What file was the task trying to run daily?
**Đáp án:** `nc.ps1`
- **Cách làm:** Nhấp đúp vào task `Clean file system` -> Tab `Actions`. Ở phần `Add arguments`, bạn sẽ thấy nó đang gọi đến script `C:\TMP\nc.ps1`.

<img width="1485" height="823" alt="Screenshot 2026-05-16 210959" src="https://github.com/user-attachments/assets/e623cf15-7e81-4764-8c0d-09cca3e8e883" />


### Câu 8: What port did this file listen locally for?
**Đáp án:** `1348`
- **Cách làm:** Vẫn ở tab `Actions` của task trên, nhìn kỹ phần tham số, bạn sẽ thấy đoạn `-l 1348` (Có nghĩa là mở port 1348 ở chế độ Listen).

(hình ở trên có đáp án)

### Câu 9: When did Jenny last logon?
**Đáp án:** `Never`
- **Cách làm:** Tài khoản Jenny là một backdoor (cửa hậu) chứ không phải user có người thật sử dụng. Việc tìm kiếm trong Event Viewer sẽ không trả về bất kỳ logon nào của user này.

<img width="1467" height="862" alt="Screenshot 2026-05-16 212931" src="https://github.com/user-attachments/assets/8d83aaae-5ca4-4a8f-917a-5fc9cbe0d41d" />


### Câu 10: At what date did the compromise take place?
**Đáp án:** `03/02/2019`
- **Cách làm:** Mở Event Viewer -> `Security`. Lọc Event ID `4720` (Tạo user). Bạn sẽ thấy user backdoor Jenny được tạo vào ngày `03/02/2019`. Đây chính là ngày hệ thống bị tấn công.

<img width="1468" height="770" alt="Screenshot 2026-05-16 220245" src="https://github.com/user-attachments/assets/0d95a1b9-380c-4c78-adb1-77e1e02ab0e3" />


### Câu 11: At what time did Windows first assign special privileges to a new logon?
**Đáp án:** `03/02/2019 4:04:49 PM`
- **Cách làm:** Trong Event Viewer -> `Security`. Lọc Event ID `4672` (Special Privileges Assigned). Tìm sự kiện cũ nhất trong ngày `03/02/2019`, thời gian chính xác là `4:04:49 PM`.

(hình ở trên có đáp án)

### Câu 12: What tool was used to get Windows passwords?
**Đáp án:** `Mimikatz`
- **Cách làm:** Truy cập vào "đại bản doanh" của hacker tại `C:\TMP`. Bạn sẽ thấy file `mim.exe` và `mim-out.txt`. Đây là viết tắt của **Mimikatz**.

<img width="1235" height="640" alt="Screenshot 2026-05-16 221654" src="https://github.com/user-attachments/assets/fda67e47-de2c-435a-9147-79f5e420abf4" />

<img width="1157" height="786" alt="Screenshot 2026-05-16 222046" src="https://github.com/user-attachments/assets/32c17384-dff9-4ebb-9f8f-90c0df7fdd87" />


### Câu 13: What was the attackers external control and command servers IP?
**Đáp án:** `76.32.97.132`
- **Cách làm:** Hacker thiết lập chuyển hướng (DNS Poisoning). Mở Notepad bằng quyền Administrator -> Mở file `C:\Windows\System32\drivers\etc\hosts`. Bạn sẽ thấy IP này.

<img width="1252" height="853" alt="Screenshot 2026-05-16 222803" src="https://github.com/user-attachments/assets/6f214adb-8217-4212-8c57-e343f5ded158" />


### Câu 14: What was the extension name of the shell uploaded via the servers website?
**Đáp án:** `.jsp`
- **Cách làm:** Truy cập thư mục mã nguồn web `C:\inetpub\wwwroot`. Bạn sẽ thấy một Web Shell lạ có phần đuôi là `.jsp`.

<img width="1201" height="287" alt="Screenshot 2026-05-16 222830" src="https://github.com/user-attachments/assets/67f97408-ea90-495a-8312-70077c8a23a1" />


### Câu 15: What was the last port the attacker opened?
**Đáp án:** `1337`
- **Cách làm:** Mở Windows Defender Firewall (`wf.msc`) -> Chọn `Inbound Rules`. Tìm các rule lạ do hacker tạo ra để đục tường lửa, bạn sẽ thấy port `1337` bị mở.

<img width="1347" height="879" alt="Screenshot 2026-05-16 223307" src="https://github.com/user-attachments/assets/5c67ab97-76f3-4923-9e15-13584f68b053" />


### Câu 16: Check for DNS poisoning, what site was targeted?
**Đáp án:** `google.com`
- **Cách làm:** Mở file `C:\Windows\System32\drivers\etc\hosts` bằng Notepad. Bạn sẽ thấy dòng `76.32.97.132 google.com`. Trang web bị điều hướng sai lệch chính là Google.

(hình ở trên có đáp án)

---

## PHẦN 2: ĐIỀU TRA BẰNG DÒNG LỆNH (CLI)
*Dành cho những bạn thích xử lý nhanh gọn bằng CMD và PowerShell.*

### Câu 1: What's the version and year of the windows machine?
**Đáp án:** `Windows Server 2016`
- **CMD:** 
  ```cmd
  systeminfo | findstr /B /C:"OS Name"
  ```

### Câu 2: Which user logged in last?
**Đáp án:** `Administrator`
- **PowerShell:** 
  ```powershell
  Get-LocalUser | Sort-Object LastLogon -Descending
  ```

### Câu 3: When did John log onto the system last?
**Đáp án:** `03/02/2019 5:48:32 PM`
- **CMD:** 
  ```cmd
  net user John
  ```
  *(Đọc thông tin ở dòng `Last logon`)*.

### Câu 4: What IP does the system connect to when it first starts?
**Đáp án:** `10.34.2.3`
- **CMD:** Truy vấn trực tiếp khóa Registry.
  ```cmd
  reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
  ```

### Câu 5: What two accounts had administrative privileges?
**Đáp án:** `Jenny, Guest`
- **CMD:** 
  ```cmd
  net localgroup administrators
  ```

### Câu 6 & 7 & 8: Tên Task, Tên File, và Port listen?
**Đáp án:** `Clean file system` / `nc.ps1` / `1348`
- **CMD:** Liệt kê toàn bộ Task và thông tin chi tiết.
  ```cmd
  schtasks /query /fo LIST /v
  ```
- **PowerShell:** Lọc các task gọi file trong thư mục `TMP`.
  ```powershell
  Get-ScheduledTask | ForEach-Object { $task = $_; foreach ($action in $task.Actions) { [PSCustomObject]@{ TaskName = $task.TaskName; Execute = $action.Execute } } } | Where-Object { $_.Execute -match 'TMP' }
  ```

### Câu 9: When did Jenny last logon?
**Đáp án:** `Never`
- **CMD:** Dòng `Last logon` sẽ báo là `Never`.
  ```cmd
  net user Jenny
  ```

### Câu 10 & 11: Compromise Date & Special Privileges Time
**Đáp án:** `03/02/2019` / `03/02/2019 4:04:49 PM`
- **PowerShell:** Xem chi tiết Event 4672 (Gán đặc quyền).
  ```powershell
  Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4672} -MaxEvents 15 | Select-Object TimeCreated, @{Name='Account';Expression={$_.Properties[1].Value}}, Message | Format-Table -Auto
  ```

### Câu 12: What tool was used to get Windows passwords?
**Đáp án:** `Mimikatz`
- **CMD:** Liệt kê file, bạn sẽ thấy `mim.exe`.
  ```cmd
  dir C:\TMP
  ```

### Câu 13 & 16: Attacker's C2 IP & DNS Poisoning Target
**Đáp án:** `76.32.97.132` / `google.com`
- **CMD:** In thẳng nội dung cấu hình DNS máy ra màn hình.
  ```cmd
  type C:\Windows\System32\drivers\etc\hosts
  ```

### Câu 14: What was the extension name of the shell uploaded?
**Đáp án:** `.jsp`
- **PowerShell:** Tìm 5 file mới bị upload lên Web Root.
  ```powershell
  Get-ChildItem C:\inetpub\wwwroot -Recurse | Select-Object Name, Extension, LastWriteTime | Sort-Object LastWriteTime -Descending | Select-Object -First 5
  ```

### Câu 15: What was the last port the attacker opened?
**Đáp án:** `1337`
- **CMD:** Liệt kê các rule có chữ "port" trong tường lửa.
  ```cmd
  netsh advfirewall firewall show rule name=all | findstr /i "port"
  ```

---

## PHẦN 3: CHEAT SHEET VÀ NHỮNG NỘI DUNG CẦN NHỚ

### 1. Script Tự Động Thu Thập Chứng Cứ (Evidence Collection)
Nếu bạn không muốn làm thủ công, hãy mở **PowerShell** bằng quyền Administrator và chạy cục script này. Nó sẽ gom mọi dấu vết (Tasks, Network, Event Logs, IIS Logs) và xuất ra Desktop:

```powershell
New-Item -ItemType Directory -Path C:\Users\Administrator\Desktop\forensics -Force
systeminfo > C:\Users\Administrator\Desktop\forensics\systeminfo.txt
net user > C:\Users\Administrator\Desktop\forensics\users.txt
net localgroup administrators > C:\Users\Administrator\Desktop\forensics\admins.txt
schtasks /query /fo LIST /v > C:\Users\Administrator\Desktop\forensics\tasks.txt
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" > C:\Users\Administrator\Desktop\forensics\hklm_run.txt
netstat -ano > C:\Users\Administrator\Desktop\forensics\netstat.txt
type C:\Windows\System32\drivers\etc\hosts > C:\Users\Administrator\Desktop\forensics\hosts.txt

# Security Timeline
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624,4625,4672,4720,4722,4732,4698,1102} | Select-Object TimeCreated, Id, Message | Sort-Object TimeCreated | Out-File C:\Users\Administrator\Desktop\forensics\security_timeline.txt
```

### 2. Tổng Hợp Tọa Độ "Điểm Nóng" Trong Lab:
- **`C:\TMP`:** Nơi chứa toàn bộ đồ nghề của hacker bao gồm `mim.exe` (Mimikatz để dump pass) và `nc.ps1` (Mở cổng 1348 chờ kết nối).
- **`C:\Windows\System32\drivers\etc\hosts`:** Nơi hacker thực hiện DNS Poisoning, ép `google.com` phải trỏ về IP của máy chủ C2 (`76.32.97.132`).
- **`C:\inetpub\wwwroot`:** Nơi chứa mã nguồn Website. Kẻ gian đã lợi dụng lỗ hổng web để tải file Webshell `.jsp` lên đây.
- **Task Scheduler (`Clean file system`) & Registry Run (`10.34.2.3`):** Cách hacker cấu hình Persistence (bám rễ) để mã độc tự khởi chạy lại sau khi máy khởi động.

### 3. Các Event ID (Security Log) Quan Trọng Để Forensics:
| Event ID | Ý nghĩa |
|---|---|
| **4624** | Logon thành công (Logon Type 2: Trực tiếp, Type 10: RDP). |
| **4625** | Logon thất bại (Dấu hiệu Brute-force). |
| **4672** | Được gán quyền đặc biệt (Dấu hiệu Leo thang đặc quyền). |
| **4720** | User mới được tạo ra (Dấu hiệu tạo Backdoor account). |
| **4732** | User được add vào Group Admin. |
