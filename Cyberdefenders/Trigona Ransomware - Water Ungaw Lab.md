Q1: địa chỉ IP của hacker có thể tìm thấy ở Security.evtx, nếu RDP thì chọn event 4924 (đăng nhập thành công) với logon type là 10 (RDP)

Q3: dựa theo giờ giấc mà hacker đăng nhập thì mình có thể truy vết nó sử dụng lệnh gì trong windows powershell.evtx

Q4: biết tài khoản thằng hecker rùi đóa thì mình vào NTUSER.DAT với đường dẫn sau: Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs để xem hắn từng truy cập file gì

Q5: muốn biết nó sài tool nào thì mình sẽ đi coi file MFT xem app nào được dùng

<img width="1264" height="43" alt="image" src="https://github.com/user-attachments/assets/3c903168-56e7-4304-9e78-5440353fb9c9" />


Q6: coi tiếp trong file .csv thì sẽ tìm được app Rclone

Q7: SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces để tìm ip máy 

TerminalServices-LocalSessionManager/Operational.evtx để tìm ip bị hacker di chuyển ngang

Q8: SYSTEM\CurrentControlSet\Services\LanmanServer\Shares tìm được folder đang được share 


Q9: trước tiên là xem MFT có khởi động app gì lạ rồi các app còn lại bị mã hóa hay gì đó không. Sau đó mình sẽ vào Amcache.hve để kiểm tra mã hash của nó bằng cách dùng lệnh sau: .\AmcacheParser.exe -f "E:\Amcache.hve" --csv "E:\Output\Amcache" -i 

Q10: có được mã hash ròi thì lên virustotal đọc hoi 

<img width="694" height="42" alt="image" src="https://github.com/user-attachments/assets/3c11dda3-24ab-4d28-a352-1bc5fcfb8f75" />

Q11: hacker thay đổi registry nào thì mình sẽ vào Software\Microsoft\Windows\CurrentVersion\Run trong NTUSER.dat

