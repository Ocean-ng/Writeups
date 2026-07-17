bài này là có 1 tổ chức bên ngoài đăng nhập vào wifi nội bộ với quyền là khách và đi ăn cắp database. Đầu tiên ta vào `phpbb_users` để check user nào lạm dụng lỗ hổng web

<img width="1923" height="948" alt="image" src="https://github.com/user-attachments/assets/39bbc946-63bd-43a8-b693-89dbfce59bbf" />

<img width="309" height="947" alt="image" src="https://github.com/user-attachments/assets/dba12721-2dbd-4fd8-87a3-0beba6c582b7" />

<img width="230" height="227" alt="image" src="https://github.com/user-attachments/assets/c37ea95d-750e-4228-a7b8-ec298541c94f" />


có 2 IP đc thể hiện trên hình có sự khác biệt, `10.10.0.78` cho user guest và `10.255.254.2` cho admin . trang xem cuối của apoole1 là có vẻ đang lợi dụng lỗ hổng LFI để xem thông tin mà quản trị viên đã ẩn, múi giờ đăng nhập cũng khác nhau, admin đang ở UTC nhưng lại chuyển sang Africa
-> user `apoole1` đang bị lợi dụng để đăng nhập phpadmin


<img width="2486" height="391" alt="image" src="https://github.com/user-attachments/assets/73852dc8-4080-4e8a-9c27-03d1271834fe" />

<img width="2208" height="302" alt="image" src="https://github.com/user-attachments/assets/1942d81d-002f-4092-89a2-8a2413eaf4d8" />

mã post_id là `9`
Hacker dùng account `apoole1` trên để đăng 1 bài viết chứa nội dung cụ thể là Stored XSS. Khi mà nạn nhân nhấp vô bài viết thì nó sẽ ra trang đăng nhập giả mạo y chang trang chính chủ, mục đích là để lừa admin đăng nhập lấy thông tin tài khoản mật khẩu sau đó là gửi về url  `http://10.10.0.78/update.php`.

<img width="1367" height="795" alt="image" src="https://github.com/user-attachments/assets/032d91e6-b8e9-4a03-b849-13f77923015d" />

<img width="1223" height="510" alt="image" src="https://github.com/user-attachments/assets/89f83385-eba2-4eed-8dc2-8a86373cff15" />

Vào `phpbb_log` để xem hacker đăng nhập tài khoản admin khi nào và quy đổi thời gian từ những con số thành ngày tháng năm

<img width="1089" height="778" alt="image" src="https://github.com/user-attachments/assets/6ad0fcbe-1177-44e3-937b-5704d135bfba" />

xem trong bảng `phpbb_config` để tìm được cấu hình của ldap

<img width="1622" height="37" alt="image" src="https://github.com/user-attachments/assets/8a9c2e5d-7658-4dd8-a64a-3630b802337f" />

đọc trong access logs để biết user-agent của admin


<img width="1040" height="43" alt="image" src="https://github.com/user-attachments/assets/7649786c-2b1d-4476-b1aa-8e43939262ea" />

quay lại với `phpbb_log` ngay dưới thời gian đăng nhập là thời gian hacker add user guest của bản thân vào Administrators

<img width="693" height="53" alt="image" src="https://github.com/user-attachments/assets/706408a3-01f0-4712-b92a-3afb958f17ee" />

đọc trong access log để xem lúc hacker backup database và đổi thành giờ UTC

<img width="469" height="78" alt="image" src="https://github.com/user-attachments/assets/fa1ba229-f810-4113-8f70-a3d1c73e91cf" />


<img width="1382" height="25" alt="image" src="https://github.com/user-attachments/assets/3b541dcf-03f1-442b-85be-22f199249de2" />

dữ liệu hacker tải về là `34707` bytes
















