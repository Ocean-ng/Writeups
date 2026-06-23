bài Lab này đã sử dụng một framework C2 (Command & Control) là Cobalt Strike. Khi Cobalt Strike thực hiện kỹ thuật tiêm mã độc từ file update.exe sang tiến trình notepad.exe, hai tiến trình này cần một đường dây liên lạc bí mật với nhau. Để làm điều này, chúng mở một kênh giao tiếp ngầm trong bộ nhớ gọi là Named Pipe.

ta sẽ dùng windbg để đọc 2 file này. Trước tiên ta đã có được thông tin version OS và url của malware

<img width="2559" height="1196" alt="image" src="https://github.com/user-attachments/assets/768dcb13-1a9f-4dc8-8294-e3450490fbc8" />

<img width="2559" height="1200" alt="image" src="https://github.com/user-attachments/assets/dad42150-0c71-4a60-a87a-77110879c405" />

giờ ở thanh command thì ta sẽ bấm `~` hiện ra các threads mà malware dùng

<img width="875" height="229" alt="image" src="https://github.com/user-attachments/assets/3e4cda89-eb7e-4c58-aa91-640a0baad7df" />

giải thích cho command truy vấn
- s -a: Tìm kiếm chuỗi ký tự dạng văn bản (ASCII strings)

- 0 L?ffffffff: Quét toàn bộ dải bộ nhớ khả dụng từ địa chỉ 0

- "MSSE-": Từ khóa đặc trưng của Named Pipe mà framework độc hại này tạo ra

<img width="1184" height="103" alt="image" src="https://github.com/user-attachments/assets/3667c718-1608-4a82-960e-c6ef89a9ccc3" />

dùng | biết id ở hệ 16 sau đó dùng ? <id> để chuyển sang hệ 10

<img width="868" height="120" alt="image" src="https://github.com/user-attachments/assets/fb032d66-aca5-47f1-a55e-728a0bdbefb8" />

chú ý là giờ khi dump ra trên máy của mình khác với máy của người trong bài này, ta phải dựa vào múi giờ được liệt kê ở dưới để cộng hoặc trừ mới cho ra kết quả đúng

<img width="728" height="382" alt="image" src="https://github.com/user-attachments/assets/da017d58-050f-449b-9843-268f8f560644" />

liệt kê tất cả các vùng nhớ có quyền RWX

<img width="2011" height="521" alt="image" src="https://github.com/user-attachments/assets/bea073fd-1315-4661-8da3-c772f3586a57" />

ta có thể dùng strings của sysinternals để truy vấn ip bằng command sau: `strings.exe C:notepad.DMP | findstr /i "http"`

<img width="876" height="672" alt="image" src="https://github.com/user-attachments/assets/9b98bffd-bf86-4698-a322-63ca37ae333a" />

khi mà search địa chỉ ip vừa tìm được trên google thì sẽ ra các bản report về con malware này -> Cobalt Strike









