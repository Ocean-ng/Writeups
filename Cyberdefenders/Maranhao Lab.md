## kịch bản: một game thủ đã tải mod của một game sinh tồn nổi tiếng về (mình đoán là minecraft), háo hức để chơi nên là khởi động file cài đặt ngay lập tức. Sau đó là các thông tin của game thủ này được gửi đến địa chỉ ip và tên miền lạ

https://cyberdefenders.org/blueteam-ctf-challenges/maranhao/

Q1: để tìm được url cuối mà game thủ này đã tải thì mình sẽ vào Users -> Levin -> AppData -> Microsoft -> Edge -> Default ->History

<img width="2559" height="320" alt="image" src="https://github.com/user-attachments/assets/29f56453-2cda-4888-ba31-d9d66a62d562" />

Q2: cũng ngay tại ô start_time là lúc mà file được tải xuống, mình sẽ dùng các con số này đổi sang định dạng UTC với chrome webkit (lưu ý là convert bằng bản thường nha)

<img width="225" height="116" alt="image" src="https://github.com/user-attachments/assets/f300c72b-745e-4b92-9853-8f2b919f7669" />

<img width="1341" height="1077" alt="image" src="https://github.com/user-attachments/assets/63c08655-6662-47f1-bf92-7544943623b6" />

Q3: xem trên ổ cứng sao chép thì thư mục Downloads của Levi đã bị xóa nên mình sẽ tiến hành vào Windows -> System32 -> winevt -> Logs -> Microsoft-Windows-Sysmon%4Operational.evtx -> filter ID 11 (file create)

dựa vào giờ mà levi tải về để truy vấn thì tìm được tên launcher của file mã độc

<img width="1775" height="992" alt="image" src="https://github.com/user-attachments/assets/e58eebd5-271a-4b24-b145-1808eac2e0c6" />

Q4: cũng cùng file .evtx này, filter id 1 (process) để xem khi mà chạy launcher lên nó có chạy ngầm gì mà người dùng không nhận biết được

<img width="1765" height="1037" alt="image" src="https://github.com/user-attachments/assets/e544df2b-9f8f-4bea-9cde-063650f4a818" />

Q5: lướt xuống sẽ thấy mã SHA1 của tiến trình ở Q4

<img width="823" height="39" alt="image" src="https://github.com/user-attachments/assets/ae9b888d-7850-4de1-8292-e94dfb6ab3ce" />

Q6: thèn héc cơ nì cũng khôn, giả danh tiến trình hợp lệ để qua mặt mình và win def nhưng cưng còn non lắm, dấu vết lộ ở parentimage lun nho

<img width="1779" height="1086" alt="image" src="https://github.com/user-attachments/assets/4e63b3cb-eecd-40cf-8a1b-759966d1d05d" />

Q7: cũng ở hình trên dòng cuối gần chữ update.exe, các ký tự ngẫu nhiên ngăn cách bởi dấu `-`, điều này cho biết thèn héc cơ đã đánh dấu ID máy này

Q8: filter id 13 (registry value) để xem nó lợi dụng Run để auto mở file mỗi khi mở máy lên

<img width="1766" height="941" alt="image" src="https://github.com/user-attachments/assets/1cf6b917-ccb8-452a-b0ef-05430e224102" />

Q9: với hình trên, ngày tạo registry cũng được hiển thị

Q10: héc cơ lỏ này dùng `attrib` để gán cho file mã độc `hidden` đồng thời là `system` làm cho file tàn hình hoàn toàn trên máy tránh bị phát hiện

<img width="1621" height="684" alt="image" src="https://github.com/user-attachments/assets/090dbaeb-f9b6-4c17-8a47-2ba0618df0bf" />

Q11: thèn héc cơ này lợi dụng tool có tên là WMIC có sẵn trên win để truy vấn thông tin phần cứng và phần mềm. filter bằng id 1 xem là có tiến trình nào chạy nữa không

<img width="1693" height="764" alt="image" src="https://github.com/user-attachments/assets/4414264c-01d4-4b32-acb6-f361feacccb5" />

Q12: héc cơ tiếp tục truy vấn tên CPU để biết rằng mã độc đang được thực thi trên môi trường máy ảo hay thật, có đủ tài nguyên như là nhân luồng của CPU để chạy con malware này hay không

<img width="858" height="30" alt="image" src="https://github.com/user-attachments/assets/0b81f4a1-600e-42b2-af16-6d1e4534749a" />

Q13: héc cơ quét thêm về card máy cũng muốn biết là đang chạy trên máy ảo hay thật

<img width="1103" height="33" alt="image" src="https://github.com/user-attachments/assets/01ad0833-8315-4454-abf4-dce6c03702a4" />

Q14: tiếp theo lấy mã nhận dạng máy để dễ dàng quản lí

<img width="903" height="30" alt="image" src="https://github.com/user-attachments/assets/2c388bb2-111d-4762-bab5-ffc230f9b7ca" />

Q15: lấy thêm thông tin về ổ cứng để héc cơ thấy có đáng để tấn công hay không (kiểu như là dung lượng ổ đĩa lớn có thể chứa database quan trọng)

<img width="1338" height="23" alt="image" src="https://github.com/user-attachments/assets/344c78ea-d67e-4a1e-b3db-2180de8c923b" />

Q16: để xem héc cơ thêm khóa gì trong registry thì dô coi Microsoft-Windows-PowerShell%4Operational.evxt (mình truy vấn bằng cách dựa theo giờ mà file được tải xuống và thực hiện các hoạt động ngầm)

<img width="1242" height="623" alt="image" src="https://github.com/user-attachments/assets/3e4c8052-d2e6-4927-8174-b2adb73900be" />

Q17: thường thì để tắt hẳn các trình duyệt mình hay vô task manager để tắt (GUI) còn héc cơ lỏ đây sẽ dùng CLI để tắt 

<img width="908" height="31" alt="image" src="https://github.com/user-attachments/assets/81a22967-d5dc-4ae4-a1b1-c34f0d563746" />

Q18: filter sysmon id 17 (pipe) xem thèn héc cơ thiết lập pipe để truyền dữ liệu ra ngoài thông qua RAM

<img width="989" height="540" alt="image" src="https://github.com/user-attachments/assets/2397da9d-3e44-4a28-9422-a23cd4cef175" />

Q19: filter id 22 (dns) héc cơ lỏ truy vấn tới domain nào để lấy ip của nạn nhân

<img width="813" height="654" alt="image" src="https://github.com/user-attachments/assets/2b119b85-d626-4749-a6c7-0fa28b9c3690" />

Q20: cùng hình trên thì query được ip, nghĩa là con mã độc chỉ tấn công 1 khu vực, nếu ở khu vực khác nó sẽ cho dô blacklist

Q21: héc cơ 1 lần nữa query tới domain lạ và có 2 địa chỉ mới được tìm thấy

<img width="802" height="598" alt="image" src="https://github.com/user-attachments/assets/9bda1e6c-b616-471c-a8ea-9c176c2a487d" />





