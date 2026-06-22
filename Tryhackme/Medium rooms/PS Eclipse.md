Để xem được logs thì trước tiên bạn nhớ tạo data view và tìm kiếm nội dung theo thanh search. Bạn có thể hỏi AI để truy vấn các thông tin nhanh nhất

thường là malware nó sẽ ẩn mình dưới dạng exe để khi người dùng ấn vào thì nó sẽ thực thi nhanh chóng: `index=main sourcetype="*Sysmon*" EventCode=11 TargetFilename="*.exe"`

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/ccbe1405-0431-4367-ac74-bd0816e5adab" />

Đọc kỹ thì nó sử dụng powershell để tải trên internet về, nên sẽ truy vấn powershell đã kết nối với web nào? cụ thể hơn là IP nào?: `index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1 CommandLine="*powershell*"
| table _time, CommandLine`

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/32be3305-cfe9-4d54-b7ff-a39c19ade5b2" />

do đoạn mã đã bị mã hóa nên ta sẽ lên (https://www.base64decode.org/) để giải mã sau đó defang

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/00b48a1f-26c0-48cb-8311-00a1723dc131" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/c65646de-c00e-4db2-934c-62bdcf33ee31" />

Xem lại chỗ **Image** thì hacker đã chiếm quyền điều khiển powershell của máy nạn nhân để mà tải file về

hacker leo quyền do thao túng được điểm yếu của windows là windows task scheduler, đây là một app giúp tự động hóa các tác vụ nên thường sẽ do admin đảm nhiệm giao quyền cho: `index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1 CommandLine="*schtasks*" CommandLine="*/Create*"
| table _time, CommandLine, ParentCommandLine`

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/94a6485d-f485-460f-95d5-fcca02b7ec2a" />

để mà file thực thi được hacker sẽ ép file đó chạy trong khi chưa tới lịch lặp của WTS và thường sẽ chạy dưới quyền NT AUTHORITY\SYSTEM: `index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1 CommandLine="*schtasks*" CommandLine="*/Run*"
| table _time, User, CommandLine`

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/adba2d0b-1d9c-4de0-abad-9b6178f97f86" />

sau khi mã độc chạy thì nó kết nối với hacker bên ngoài mạng nên giờ mình sẽ đi truy vấn mạng: `index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=3 Image="*OUTSTANDING_GUTTER.exe"
| table _time, DestinationIp, DestinationPort`

nhưng câu này chỉ dừng ở mức tìm IP nên mình sẽ truy vấn thêm tên miền của nó: `index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=22 Image="*OUTSTANDING_GUTTER.exe*"
| table _time, Image, QueryName, QueryResults` sau đó defang url thôi

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/013dc96c-2734-458e-9a71-48a25e0ff24f" />

không chỉ upload lên malware mà hacker còn up lên 1 file powershell: `index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11 TargetFilename="*\\Temp\\*.ps1"
| table _time, TargetFilename`

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/c07e82c3-47c7-4647-8818-20d837828e6b" />

ta sẽ lên virustotal để xem mã hash sha265 của script.ps1 nó nguy hiểm cỡ nào và tên thật của nó là gì?: `index=main "script.ps1" (Hashes=* OR MD5=* OR SHA256=*)
| table _time, sourcetype, TargetFilename, Hashes`

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/72c88632-6c67-47d6-b537-88ca7af1e6a6" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/853eaae2-bd29-4074-8849-854adfe56804" />

ransomware nó sẽ để lại thư tống tiền: `index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11 (TargetFilename="*note*.txt" OR TargetFilename="*readme*.txt")
| stats count by TargetFilename`

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/31cba57a-3d23-4bbe-8d8f-24c109c4ed29" />

và thường nó sẽ đổi hình nền để nạn nhân biết mình đang bị mã độc tống tiền: `index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11 (TargetFilename="*.jpg" OR TargetFilename="*.png" OR TargetFilename="*.bmp")
| stats count by TargetFilename`

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/06045dff-7864-4ba4-8f5c-2065c6c5d9dd" />






































