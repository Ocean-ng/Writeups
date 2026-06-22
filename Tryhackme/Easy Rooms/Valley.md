mới vào trang thì có lẽ bạn sẽ đi lục lọi các thứ, ở chỗ static có các số, vậy thì ta sẽ lợi dụng đổi tham số về thư mục ẩn 

<img width="756" height="318" alt="image" src="https://github.com/user-attachments/assets/37d5631d-775d-4a40-9ae5-c369cdd223b4" />

khi mà đọc source code thì có 2 file `dev.js` và `button.js`

<img width="1467" height="1237" alt="image" src="https://github.com/user-attachments/assets/935f6c58-efb9-4e27-aa2a-5b00a9afc5a9" />

ở dev.js thì lộ username, password và thư mục ẩn tiếp theo

<img width="811" height="342" alt="image" src="https://github.com/user-attachments/assets/2348f7ee-f19e-4cd5-b908-9e10d539d83d" />

thì theo lời dev nói là `đừng dùng lại thông tin đăng nhập` và `đổi cổng ftp sang cổng thường` -> vậy là mình sẽ dùng username, password bị lộ đăng nhập vô trang nãy cùng với đó là sẽ quét thêm cổng ftp.
Đăng nhập vô thì nó cũng sẽ ra devnote thôi nên mình sẽ vào cổng ftp

<img width="2107" height="862" alt="image" src="https://github.com/user-attachments/assets/1df8e9c9-ba44-4dae-add4-c00fba507aff" />

okay vậy có cổng là `37370`, trong ftp sẽ có 3 file để phân tích

<img width="1603" height="611" alt="image" src="https://github.com/user-attachments/assets/eef6326b-e7ed-40f3-bff8-5e5606a783b6" />

trong file siemHTTP2.pcapng thì lộ thêm 1 username:password nữa, mình sẽ đăng nhập thử vô trang web lúc nãy và không hợp lệ, thế mình sẽ thử vào ftp - lại không tiếp và phương án cuối là ssh vào được thành công

<img width="1073" height="493" alt="image" src="https://github.com/user-attachments/assets/8a37f388-cf56-437d-8816-59361092ef30" />

tìm được flag user rồi

<img width="741" height="89" alt="image" src="https://github.com/user-attachments/assets/ecca0be3-bf57-4b81-8865-0c64109c54f3" />

giờ mình sẽ thử xem crontab có gì không, có 1 script py tự động

<img width="2257" height="740" alt="image" src="https://github.com/user-attachments/assets/568b853d-59ab-461c-a284-7be55c441f2a" />

bây giờ mình sẽ thêm 2 lệnh này vào file để leo quyền lên root 

```
import os
os.system('cp /bin/bash /tmp/rootbash && chmod +s /tmp/rootbash')
```

<img width="1704" height="1064" alt="image" src="https://github.com/user-attachments/assets/532f2dfb-f6a8-491d-b1ce-9197cb6748b8" />

khi lưu sẽ bị chặn do là mình chưa có quyền, nên là sẽ đi tìm user khác xem có lên root được không

<img width="1656" height="266" alt="image" src="https://github.com/user-attachments/assets/7ba45297-0c01-4ffa-a672-b2b8812dd44c" />

có file valleyAuthenticator có quyền r nhưng lại khó đọc được nên mình sẽ tải về và dùng UPX (Ultimate Packer for eXecutables) do file được nén bằng tool này.
Mình sẽ tìm đoạn khi nhập mật khẩu thành công hoặc là thông báo lỗi xem có thông tin gì

<img width="1379" height="780" alt="image" src="https://github.com/user-attachments/assets/29012085-0498-478e-9317-ef664da953e8" />

trên đây có 2 mã hash, dịch xem nó là gì nè

<img width="2001" height="65" alt="image" src="https://github.com/user-attachments/assets/0b27cee5-f04f-444a-83ec-bbc585d5eeb6" />

<img width="2006" height="68" alt="image" src="https://github.com/user-attachments/assets/4c17ef44-a2c4-4554-adb1-fa83d2520a5e" />

đăng nhập vào `valley` sử dụng file authenticator thì báo ok

<img width="1053" height="376" alt="image" src="https://github.com/user-attachments/assets/9c99b854-fb9c-4b0d-911d-9d298f18576f" />

quan sát đọc file photosEncrypt.py thì có các thư viện để hijack, mình sẽ chọn base64. Vào thư mục tmp để thoải mái dùng quyền write; tạo file `base64.py` sau đó lưu lại (code yêu cầu root copy một bản shell ra thư mục /tmp và cấp quyền SUID cho nó)

<img width="1547" height="142" alt="image" src="https://github.com/user-attachments/assets/6534dbbd-d3bc-4a90-97d1-8bf6f685ad7c" />

ngồi chờ crontab hoạt động định kỳ để mở bash, hình như có gì đó sai sai?, mình có nhầm file hong nhỉ. À thì ra là không phải tạo file ở base64.py do file photosEncrypt.py không gọi thư viện này mà sẽ gọi trong ảnh dưới.
thêm lại câu lệnh mình vừa ghi ở trên và ngồi đợi nữa hoy

<img width="1198" height="56" alt="image" src="https://github.com/user-attachments/assets/50d7371f-4c6c-4908-829a-6a66ef4da326" />

okay thế là leo và đọc được root flag òi

<img width="1327" height="263" alt="image" src="https://github.com/user-attachments/assets/71d6c1b1-0219-4049-8aa6-10c61f2f06bb" />



















