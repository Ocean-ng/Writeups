## kịch bản: héc cơ dùng powershell để tấn công hệ thống

Q1: đầu tiên là dùng plugin pslist để xuất ra danh sách process hoạt động tại thời điểm nhất định -> mình tìm được PPID 4596 của powershell.exe (do PPID bị ẩn nên đành dùng plugin khác) -> dùng psscan để quét PPID tìm được -> dùng cmdline để liệt kê ra các lệnh được dùng khi tìm được tên của PPID

<img width="1926" height="34" alt="image" src="https://github.com/user-attachments/assets/95975297-0b7f-4198-95c7-cb0f9a41723f" />

<img width="2534" height="375" alt="image" src="https://github.com/user-attachments/assets/1a4eb35a-c423-49af-936c-52c7882a6035" />

Q2: sài pslist lần nữa để xem con malware này lợi dụng ps nào để duy trì tồn tại (có cùng PPID với powershell). schtasks.exe là công cụ dòng lệnh chính thức của Microsoft Windows, cho phép người dùng tạo, xóa, chạy, chỉnh sửa và quản lý các tác vụ theo lịch trình (Scheduled Tasks) trên máy tính cục bộ hoặc từ xa

<img width="2285" height="31" alt="image" src="https://github.com/user-attachments/assets/71d78150-5f6c-4574-88c8-eecc9f1b6e15" />

Q3: grep PPID 4596 show cho mình các ps khác. Regsvcs.exe là công cụ Cài đặt Dịch vụ .NET của Microsoft (được gọi chính thức là .NET Services Installation Tool). Công cụ này chạy trên dòng lệnh Windows để tải, đăng ký và cấu hình các tập hợp (assembly) .NET Component Object Model (COM+)

<img width="1925" height="27" alt="image" src="https://github.com/user-attachments/assets/3e7c9f8f-09bd-432b-a03d-4a1fa83c80a3" />

Q4: dùng cmdline với grep id 6980 của powershell. Add-MpPreference dùng để thay đổi cấu hình Windows Defender (Microsoft Defender Antivirus). -ExclusionPath là thêm đường dẫn vào danh sách loại trừ (không quét)

<img width="2525" height="57" alt="image" src="https://github.com/user-attachments/assets/9954f059-deae-4b36-95ae-f8e59803e706" />

Q5: để tìm được 2 file được miễn scan của windows defenders thì mình chỉ cần filter Add-MpPreference

<img width="2528" height="143" alt="image" src="https://github.com/user-attachments/assets/d9850434-ecfa-4061-9e39-2eae9f076466" />

Q6: T1564.012

<img width="1860" height="675" alt="image" src="https://github.com/user-attachments/assets/07f5512e-edf0-4f4b-a7cb-98e3143d4615" />

Q7: user Lee (bị chiếm quyền điều khiển) hư quá đó nho, phá bậy bạ trên máy người ta dị nè

<img width="2531" height="909" alt="image" src="https://github.com/user-attachments/assets/c7bd7a5e-df8c-49b0-8b93-15ded25ff2f9" />


