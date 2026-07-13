file được phát hiện tại thư mục /var/tmp và cố gắng kết nối ra ngoài

mình sẽ dùng tool `XELFViewer` để phân tích file elf này và tìm được mã SHA256

<img width="1995" height="746" alt="image" src="https://github.com/user-attachments/assets/2126b97a-62bc-42a9-a26a-e0df27848be2" />

ta sẽ tìm được IP mà máy nạn nhân muốn kết nối tới ở mục strings

<img width="2549" height="902" alt="image" src="https://github.com/user-attachments/assets/9adc6e3d-bba8-4432-a535-3b9d9002038f" />

để tìm cổng thì minh sẽ thử vào virustotal xem nó liệt kê ra, tìm thấy được cổng 4445 cùng với IP vừa search được 

<img width="2538" height="969" alt="image" src="https://github.com/user-attachments/assets/3f67fb2e-77a4-491a-bdf4-59a84bd2c412" />

ta sẽ dùng IDA để dịch ngược xem các code của file .elf này để biết nó kết nối lại sau bao nhiêu giây

<img width="1546" height="1145" alt="image" src="https://github.com/user-attachments/assets/6bffda08-099c-42dd-b4a4-f1e1a7449e26" />

ở hình trên như ta thấy thì mình sẽ dùng AI để phân tích xem code ý nghĩa như thế nào

<img width="883" height="632" alt="image" src="https://github.com/user-attachments/assets/29d2367c-c383-43ba-b310-5e3801b923ae" />

ta sẽ đi tìm search process_cmd để đếm xem là malware này hỗ trợ bao nhiêu lệnh được gửi từ server C2 của hacker

<img width="1650" height="809" alt="image" src="https://github.com/user-attachments/assets/502264cd-d8b3-4315-891c-9159ac8a502d" />

io_uring là một công nghệ giao diện I/O bất đồng bộ (asynchronous I/O). Thay vì gọi các Syscall truyền thống như read(), write(), connect() (những thứ mà EDR đang giám sát), mã độc ném các yêu cầu mạng vào một cái "hàng đợi" (queue) của io_uring. EDR hiện nay chưa hỗ trợ giám sát hoặc bỏ sót các hoạt động đi qua kênh này 

<img width="456" height="302" alt="image" src="https://github.com/user-attachments/assets/b4b4d760-0c85-4f3d-97fe-1f6adf337caf" />

mình phán đoán rằng url trên `Logged users` này sẽ là tệp mà nó sẽ đọc

<img width="1452" height="168" alt="image" src="https://github.com/user-attachments/assets/168e3264-5fdb-4c4d-9fcc-37030ab6a672" />

đọc tiếp strings thì nó sẽ tiến hành đọc folder /usr/bin để leo quyền

<img width="1381" height="173" alt="image" src="https://github.com/user-attachments/assets/3b59d9fc-3028-4562-aaa5-41fbe2de7ab5" />

nó đi quét tiếp phần mềm eBPF

<img width="1397" height="92" alt="image" src="https://github.com/user-attachments/assets/a841be52-1d0b-4cf0-8c91-d1463e489f84" />

tiếp đến là nó sẽ đi vô hiệu hóa tiến trình đang chạy

<img width="1558" height="200" alt="image" src="https://github.com/user-attachments/assets/a3df5dc0-9869-4ce2-bde1-87cd3b9a11e8" />

khi một tiến trình muốn biết chính xác file gốc của nó đang nằm ở đâu trên ổ cứng (ví dụ để nó tự xóa file đó đi - tự hủy), nó sẽ gọi để đọc file /proc/self/exe. Trong trường hợp này thì nó nhận biết đã bị phát hiện nên sẽ tự xóa chính bản thân mình để không bị tra cứu

<img width="1297" height="39" alt="image" src="https://github.com/user-attachments/assets/0a5e7467-227b-479f-a378-96cbd94e2adf" />

quay lại hàm process_cmd ta sẽ tìm thấy lệnh tự hủy được setup từ hacker

<img width="947" height="747" alt="image" src="https://github.com/user-attachments/assets/6b3fb01c-0388-48ad-8c79-50dd15db20b7" />








