# 🗂️ Windows Registry Cheat Sheet

> Windows Registry là một tập hợp các cơ sở dữ liệu chứa dữ liệu cấu hình của hệ thống. Dữ liệu cấu hình này có thể liên quan đến phần cứng, phần mềm hoặc thông tin người dùng. Nó cũng bao gồm dữ liệu về các tệp, chương trình được sử dụng gần đây hoặc các thiết bị được kết nối với hệ thống.

🔧 Mở `regedit.exe` để xem các khóa

📁 Phần lớn các nhánh nằm trong: `C:\Windows\System32\Config`

---

## 📂 Các cụm chứa thông tin người dùng

| Tệp | Đường dẫn |
|------|-----------|
| `USRCLASS.DAT` | `C:\Users\<username>\AppData\Local\Microsoft\Windows` |
| `NTUSER.DAT` | `C:\Users\<username>\` |
| `Amcache.hve` | `C:\Windows\AppCompat\Programs\Amcache.hve` — Windows tạo ra nhánh này để lưu trữ thông tin về các chương trình đã được chạy gần đây trên hệ thống |

---

## 🖥️ Thông tin máy tính

### 📋 Cấu hình hệ thống

| Mục | Registry Key |
|-----|-------------|
| Bản build Windows | `SOFTWARE\Microsoft\Windows NT\CurrentVersion` |
| Lệnh control | `HKLM\SYSTEM\CurrentControlSet` |
| Tên máy tính | `SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName` |
| Múi giờ | `SYSTEM\CurrentControlSet\Control\TimeZoneInformation` |

### 🌐 Thông tin mạng

```
SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces
SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged
SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Managed
```

### 🚀 Autorun

```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce
SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run
SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

### ⚙️ Dịch vụ

```
SYSTEM\CurrentControlSet\Services
```

---

## 👤 Thông tin người dùng

| Mục | Registry Key |
|-----|-------------|
| Thông tin người dùng | `<username>\Domains\Account\Users` |
| Tệp được sử dụng gần đây | `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs` |
| Phiên bản Office Microsoft | `NTUSER.DAT\Software\Microsoft\Office\VERSION` |

### 🐚 Shell Logs

```
USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags
USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU
NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU
NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags
```

### 📝 MRU (Most Recently Used)

```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU
```

### 🔍 Thanh tìm kiếm Windows Explorer

```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery
```

---

## 🔎 Evidence of Execution

| Artifact | Registry Key / Đường dẫn |
|----------|--------------------------|
| **UserAssist** | `NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count` |
| **ShimCache** | `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache` |
| **AmCache** | `C:\Windows\appcompat\Programs\Amcache.hve` → `Amcache.hve\Root\File\{Volume GUID}\` |

> **ShimCache Parser:**
> ```
> AppCompatCacheParser.exe --csv <path to save output> -f <path to SYSTEM hive for data parsing> -c <control set to parse>
> ```

### 🔄 BAM / DAM (Background Activity Monitor)

```
SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}
SYSTEM\CurrentControlSet\Services\dam\UserSettings\{SID}
```

---

## 🔌 Nhận dạng thiết bị được cắm vào máy tính

```
SYSTEM\CurrentControlSet\Enum\USBSTOR
SYSTEM\CurrentControlSet\Enum\USB
SYSTEM\CurrentControlSet\Enum\USBSTOR\Ven_Prod_Version\USBSerial#\Properties\{83da6326-97a6-4088-9453-a19231573b29}\####
SOFTWARE\Microsoft\Windows Portable Devices\Devices
```
[WindowsForensicsCheatsheet-TryHackMe-1642092762578.pdf](https://github.com/user-attachments/files/27788064/WindowsForensicsCheatsheet-TryHackMe-1642092762578.pdf)
