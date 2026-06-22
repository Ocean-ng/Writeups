đây là trang chủ web, mới đăng kí tài khoản nó đã cho khóa rồi, mình lại tiếp tục mò từng trang
<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/1fb05656-a93f-47f8-bf3c-0b01520f615b" />
<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/618ad881-a318-48a6-ac39-6904825b964b" />
tìm được trang ẩn debug, mình bắt đầu phân tích từng dòng log
<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/986cb22e-de8e-4dff-b635-1c717ede2be9" />
<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/ea604eb6-a8bb-49f7-b35f-c6abe1229f7e" />

có câu "Using deterministic key generation" 2 số nguyên tố p và q là ngẫu nhiên và bí mật trong mật mã rsa nhưng ở đây nó lại cho mình một công thức cố định là Seed pattern: {username}_lovenote_2024_valentine; mình sẽ thử admin_lovenote_2026_valentine, tạo ra p và q thông qua hash SHA256 + số nguyên tố (đối với p thêm chuỗi pki) => n = p x q là được privatekey của admin

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/5e688a33-d9fb-457b-bd2b-b5f5be508d43" />

vào trang verify để xác thực mã nhưng message content là gì?

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/5a3180eb-6a3b-41af-bad2-28cf0f7891b4" />

xem trang message có dòng tin nhắn của admin với "Send encrypted love message" có nghĩa là gửi tin nhắn đã được mã hóa
=> tìm được private key xong ký tin nhắn lên rồi chuyển sang dạng hex. Mình sẽ nhờ Gemini tạo cho mình script để tự động hóa quá trình này

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/addcc564-89fa-43be-80d8-eea8ce6f0c65" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/842312a3-2e05-46ca-ac5d-c64d0ef221d1" />

vậy là mình đã tìm được flag




