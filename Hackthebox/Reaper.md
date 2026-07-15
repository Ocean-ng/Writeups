
mới vào xem packet thì ip của Forela-Wkstn001 hiện ngay trước mặt lun nì

<img width="1238" height="134" alt="image" src="https://github.com/user-attachments/assets/93e57e0f-cd96-44a6-8fdd-8dab49af562b" />

cuộn xuống để xem thêm các hoạt động của packet thì tìm được ip của Forela-Wkstn002

<img width="1232" height="93" alt="image" src="https://github.com/user-attachments/assets/2f960b67-4600-4909-922f-7a4112d49fc1" />

dùng flag `ntlmssp` để filter username bị đăng nhập trái phép

<img width="1743" height="178" alt="image" src="https://github.com/user-attachments/assets/2ffd2a3e-2dbd-4399-add2-a44589747967" />

lấy flag `nbns` để filter những packet truy vấn tên, có nghĩa là nạn nhân bị ghi sai tên server hay server bị sập gì đó, hacker làm 1 server giả mạo để qua mặt nạn nhân gửi mật khẩu cho hắn ta

<img width="1978" height="362" alt="image" src="https://github.com/user-attachments/assets/17774e79-e733-48e4-afa9-4de7194557b5" />

lướt xuống mồi hồi thì mình sẽ tìm được file share của nạn nhân với hacker

<img width="1619" height="252" alt="image" src="https://github.com/user-attachments/assets/459becea-bcf6-451a-8f04-e277515f4107" />

vào wins events với ID `4624` (Successful Logon), xem log chứa type 3 (Network Logon: đăng nhập thông qua mạng từ xa), xem general có được cổng remote, LogonID, workstation, IP và giờ xảy ra event đó

<img width="761" height="671" alt="image" src="https://github.com/user-attachments/assets/752e7a48-7cb3-4e31-9507-5449a28554fb" />

<img width="768" height="256" alt="image" src="https://github.com/user-attachments/assets/d1d8d441-63ad-4647-83ba-5023ca59f098" />

<img width="374" height="50" alt="image" src="https://github.com/user-attachments/assets/d5b3d6c0-0904-4510-bb5d-99e9e275276d" />

filter ID `5140` để xem nạn nhân đã share thư mục tổng nào với hacker, mạng ngoài

<img width="752" height="520" alt="image" src="https://github.com/user-attachments/assets/ffcda5b0-10ac-4cff-aa24-437ecb1975e3" />















