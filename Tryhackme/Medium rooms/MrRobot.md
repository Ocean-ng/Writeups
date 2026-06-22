Đầu tiên ta sẽ chuẩn bị một máy ảo kali linux kết nối vpn của tryhackme để vào địa chỉ trong phòng Mr Robot CTF.

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/aa394e2a-c872-4071-a271-20e31289f05d" />

Trên đây là hình ảnh đầu tiên khi kết nối với địa chỉ web. Theo như thói quen thì ta sẽ tò mò về những câu lệnh mr robot đưa ra. Nên là sẽ nhập từng câu lệnh để xem có thông tin gì. Sau khi xem là những đoạn video về phim Mr Robot với nhân vật chính là Elliot Alderson.
Tiếp theo ta sẽ sử dụng nmap để quét xem có những cổng nào đang mở

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/00c91e86-62d8-47db-88c4-0386354cbf11" />


Có thể thấy trên hình chỉ có ssh, http và https nên ta sẽ sử dụng gobuster để xem những trang đang ẩn

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/90cd2d3f-d971-4b33-8a29-b53ece85efdd" />


Ta chú ý hơn với những trang có status 200, thông thường các địa chỉ url sẽ nằm trong sitemap và robots.txt

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/aeb87151-9b9a-4bba-96aa-699756d40e9e" />


<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/4dbe62ae-1b17-4a72-ad9f-54de19843b49" />


Ta thấy được 2 file mà mr robot không muốn ta biết. 

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/48039c16-f9ff-4eb5-a66f-7cd117fac17c" />


Vậy là ta đã tìm được key1 : 073403c8a58a1f80d943455fb30724b9

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/8a1c2ade-0df0-42ab-ba33-57760ceacdfb" />


Quay lại với file fsocity.dic, đây là một file rất dài và nhiều từ lặp lại nên để tấn công bruteforce thì sẽ rất mất thời gian và tài nguyên nên ta sẽ sắp xếp và xóa những kí tự trùng lặp file này với câu lệnh: sort fsocity.dic | uniq > list.txt . (tải file về máy bằng lệnh wget)
<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/a844c63a-bb08-44f6-8a4a-8f0944d5aeba" />


file đã nhẹ đi bớt, ta sẽ lọc những từ khóa có liên quan tới phim mr robot và những tên mang đặc quyền cao nhất như là mrrobot, admin, elliot, Alderson. (không phân biệt viết hoa viết thường)

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/1227d5a5-f963-4b72-923f-9912caa7da52" />


<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/be7400cd-a48e-4aaa-8a55-302cea85a9a5" />


lúc đầu khi quét bằng gobuster thì ta thấy được trang đăng nhập vào wordpress, với danh sách này thì ta sẽ thử điền username hợp lệ trước. Bạn có thấy sự khác biệt gì ở đây không? Đúng như vậy, ta đã điền username được đăng kí và bây giờ chỉ cần tìm mật khẩu.

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/0dbb2b1e-4393-4c3e-b709-db33fd0b46ea" />


Sử dụng tool wpscan để tìm lỗ hổng wordpress: wpscan --url http://ip-address -U Elliot -P list.txt. Vậy trên hình ảnh này thì ta thấy được username và password hợp lệ ER28-0652

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/808c6551-6730-4bd4-9075-868f8718e7a8" />


Sau một lục lọi trang web thì tìm được lỗ hổng upload file (không dùng upload media file vì file php không hợp lệ và nếu muốn bypass thì mất thêm thời gian và upload media file lạ sẽ bị admin dễ phát hiện). 

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/104ceecc-8fcd-419b-adb1-0c1c057ff994" />


Ta sử dụng kĩ thuật gọi là web shell upload và server của wordpress đc viết bằng php nên sẽ sử dụng code php để tấn công. Kĩ thuật web shell upload sẽ sử dụng reverse shell để làm server tự động gửi kết nối tới client mà không bị tường lửa chặn. (Trong kali linux có sẵn code reverse shell)

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/96e87af4-a51c-47ce-8e96-7b7f62dde161" />


Thay đổi ip bằng ip client kali linux, đổi cổng lớn hơn 1024 để tránh các dịch vụ được đăng kí riêng. Xóa hết code trong 404.php và copy code php reverse shell vào 404.php rồi nhấn update file. Sau đó chạy tool netcat: nc -lvnp 1025

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/2c3d0e73-30b2-4033-9fdc-f497def31e77" />


Sau đó kết nối trang không tồn tại để server cho 404.php chạy. Sử dụng địa chỉ url: http://10.82.188.192/wp-content/themes/twentyfifteen/404.php 
P/s: cấu hình web nào thì ghi ra web đó - ở đây ta sử dụng web twentyfifteen

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/5feb6374-dd0e-45b9-9217-0247c398d0f3" />


<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/1e5379c5-ce8d-4b0b-a4a6-784e0daba9b0" />


Vậy là ta đã kết nối được với server, tiếp theo sẽ sử dụng các lệnh như là ls -la, cd để xem thư mục và chuyển thư mục

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/01a25a48-5887-4eff-95f5-6f4cdeed99f5" />


file key-2-of-3.txt do robot sở hữu mới có thể đọc được nên quay ngược lại file password.raw-md5 để giải mã bằng web online 
password của robot là abcdefghijklmnopqrstuvwxyz. Sau đó quay lại sử dụng su robot để đăng nhập

<img width="975" height="609" alt="image" src="https://github.com/user-attachments/assets/a02efe2a-f233-4fc4-bc84-fb128b554d9d" />


<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/39671c17-1da1-47cf-b2fa-ee2a567a172d" />


do thường dumbshell của netcat sẽ không cho phép thực hiện lệnh sudo nên ta nâng cấp lên Text terminal để dễ dàng làm bài dùng lệnh: python -c 'import pty; pty.spawn("/bin/bash")'

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/41cd6651-b915-4fdc-a492-95b91f162c1c" />


Vậy là ta đã tìm được key2: 822c73956184f694993bede3eb39f959

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/ba924615-e1e9-4996-b3ed-fd82ce218153" />


Sau khi tìm hiểu vài thư mục thì mình nghĩ nên chuyển sang vai trò root để đọc và truy cập được tất cả các file, folder. Nhưng lại không có quyền được đăng nhập. Vậy để chạy dưới quyền root thì ta sẽ sử dụng kỹ thuật SUID (Set User ID) để mạo danh root. Sử dụng lệnh: find / -perm -u=s -type f 2>/dev/null

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/45d19636-2fb2-4099-9540-7ca292c43937" />


Có các file như là passwd, sudo, su nhưng chỉ làm được một việc của nó chứ không cho chạy lung tung. Đối với nmap, tìm hiểu được rằng ở phiên bản cũ thì nhà phát triển đã thêm interactive mode. Khi mà scan bằng nmap thì người ta đa số sẽ dùng root, nên khi ta là user thường kết nối vào nmap (interactive mode) thì cũng sẽ hưởng quyền của root luôn. (tham khảo https://gtfobins.org/ để tìm những command giúp leo thang đặc quyền)

<img width="975" height="518" alt="image" src="https://github.com/user-attachments/assets/253a7b78-fd38-4f8a-99d5-4fb08f6a6c4d" />


Sử dụng lệnh: /usr/local/bin/nmap –interactive để mở nmap. Sử dụng !sh để nmap mở terminal root
 
Vậy là ta đã tìm được key3: 04787ddef27c3dee1ee161b21670b4e4

