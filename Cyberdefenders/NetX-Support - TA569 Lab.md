tải file zip -> giải nén và chạy file 5645_M.js -> lợi dụng powershell tải về file index.php -> 1 file tên deploy.ps1 được thực thi và tạo ra tại `C:\Users\Alpha\AppData\Roaming\IRomvWG3\presentationhost.exe` -> thêm khóa xem trong registry 
NTUSER.dat\SOFTWARE\Microsoft\Windows\CurrentVersion\Run -> tạo thêm 1 người dùng mới xem trong security.evxt (event id 4720) -> hacker add user mới vào group administrators và Remote Desktop Users xem trong security.evxt (event id 4732) -> 
thêm lệnh chạy ssh với cấu hình chỉ định xem trong tasks của ssh server -> dùng tiện ích của windows là CryptoAPI để tải xuống 1 tập tin, dùng `.\strings.exe -s -u "C:\Users\Administrator\Desktop\MetaData\*"` để đọc unicode hợp lệ và quét mục con ->
ssh kết nối ngược với username@ip trong file ssh key exchange -> registry rdp sẽ đc lưu ở NTUser.dat\SOFTWARE\Microsoft\Terminal Server Client\Default -> truy cập <img width="1335" height="748" alt="image" src="https://github.com/user-attachments/assets/d66889f5-0c7e-4b97-bc8b-f5262f0189ab" /> -> 
-> tìm domain trong client32.ini



