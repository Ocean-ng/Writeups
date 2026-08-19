## kịch bản: khách hàng của công ty bị dính ransomware mã hóa data, bị đòi tiền chuộc mới lấy lại được dữ liệu

Q1: ở đây mình sẽ query để tìm file mà ransomware để lại `index=* "README" OR "HELP" OR "DECRYPT" OR "RESTORE"| head 20` -> do file được count nhiều nhất là bị ransomware encrypt data

<img width="859" height="391" alt="image" src="https://github.com/user-attachments/assets/2dd5c3ad-6e5b-42fd-a623-67849a986bd9" />

Q2: cùng với hình ở Q1, PID được hiển thị 

Q3: cùng với hình ở Q1, dễ dàng thấy héc cơ đã thực thi file .exe rồi để lại file .txt như trên hình. file mà hắn đã thực thì `C:\Users\Administrator\Downloads\facebook assistant.exe`

Q4: dùng câu này query file .exe đã thực thi các ps nào `index=* "facebook assistant" ("-enc" OR "-e " OR "encodedcommand" OR "Base64")`. Phải dùng UTF-16LE vì PowerShell -EncodedCommand luôn encode bằng UTF-16LE

<img width="1801" height="64" alt="image" src="https://github.com/user-attachments/assets/1ad4ecd9-c7a3-417b-9310-0c7d06342cb8" />

<img width="2050" height="735" alt="image" src="https://github.com/user-attachments/assets/11abb7e5-de4f-4279-ae27-7e8827ce1f30" />

Q5: query filter `index=* "facebook assistant" "SHA256" | head 5` để tìm thấy file .exe thực thi có mã SHA256

<img width="1652" height="438" alt="image" src="https://github.com/user-attachments/assets/700e3ac8-4de7-4d9a-abc7-f868d6908b3f" />

Q6: giờ có mã hash rồi mình sẽ ưu tiên osint trên cj gu gồ bằng key `B8D7FB4488C0556385498271AB9FFFDF0EB38BB2A330265D9852E3A6288092AA onion domain` và vào page any.run xem bản report 

<img width="329" height="67" alt="image" src="https://github.com/user-attachments/assets/9352eda8-412e-452d-9df0-977dcb9c961e" />


