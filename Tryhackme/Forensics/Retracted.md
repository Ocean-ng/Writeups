khi vừa vào ta sẽ tìm được file để lại lời nhắn cho SOPHIE

<img width="1017" height="849" alt="image" src="https://github.com/user-attachments/assets/f708bbfb-2926-4041-9bdb-21b9af4cc75d" />

với đuôi .txt thì ta chắc rằng file đã được tạo ra bởi `notepad.exe`

để biết dòng thời gian mà file được tạo ra sẽ vào `event viewer -> applications and services logs -> microsoft -> sysmon -> operational`. Dựa vào tính chất của `event ID 1` là `process creation` ta sẽ dùng bộ lọc 

<img width="2559" height="1281" alt="image" src="https://github.com/user-attachments/assets/5cab41b7-efa8-4aba-9dce-aa3615781e0f" />

và `Find` SOPHIE.txt

<img width="2559" height="1267" alt="image" src="https://github.com/user-attachments/assets/0b243561-1241-4761-b1fe-c06f5324ef0d" />

tìm được thời gian tạo file chính xác

<img width="2559" height="1282" alt="image" src="https://github.com/user-attachments/assets/a1091646-d275-4710-a714-9b4088e96431" />

tiếp tục vào nơi `C:\Users\Sophie\download` (lưu ý ở đây có 2 file chữ cái đầu D và d) của nạn nhân sẽ tìm được 3 file cô ấy tải về, 1 trong số đó tên là `antivirus.exe`

<img width="2559" height="1284" alt="image" src="https://github.com/user-attachments/assets/8d09a590-f01b-4697-92db-071b483b6feb" />

để biết được là malware này đã làm gì thì ta tra cứu `event ID 11` với `file created` xem nó đã tạo thêm file gì vào máy

<img width="2559" height="1280" alt="image" src="https://github.com/user-attachments/assets/0969dbcf-e763-4646-88f2-725ab68d1e0d" />

vậy là ta biết rằng khi chạy `antivirus.exe` thì nó sẽ encrypt file với đuôi .dmp

để biết được hacker đã cài cắm backdoor vào và gửi thông tin nạn nhân đến IP nào thì filter `event ID 3` là network

<img width="2555" height="1352" alt="image" src="https://github.com/user-attachments/assets/56fc608f-9a7f-4d89-b636-77c8d543b632" />

hacker đã kết nối với máy thông qua RDP, tiếp tục với event id 3 này ta find `RDP` (ngay sau khi tải file mã độc về)

<img width="2559" height="1343" alt="image" src="https://github.com/user-attachments/assets/cd883718-ec72-48dc-b6d2-e38aa1279c2a" />

quay lại với `event id 1` thì hacker tải về file tiếp theo là `decryptor.exe`

<img width="2559" height="1352" alt="image" src="https://github.com/user-attachments/assets/b86c94db-4d5e-4f86-877a-5cd6196735fc" />

cuối cùng, ta sẽ tường thuật lại câu chuyện này:

Sophie downloaded the malware and ran it -> The downloaded malware encrypted the files on the computer and showed a ransomware note -> After seeing the ransomware note, Sophie ran out and reached out to you for help -> While Sophie was away, an intruder logged into Sophie's machine via RDP and started looking around 
-> The intruder realized he infected a charity organization. He then downloaded a decryptor and decrypted all the files -> After all the files are restored, the intruder left the desktop telling Sophie to check her Bitcoin -> Sophie and I arrive on the scene to investigate. At this point, the intruder was gone

qua đây thì câu chuyện khá là nhân văn í he :)))

