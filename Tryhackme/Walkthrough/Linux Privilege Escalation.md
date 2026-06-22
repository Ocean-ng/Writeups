# Enumeration
## 1. Thông tin Hệ thống & Môi trường (System & Environment)

- `hostname`: Trả về tên của máy mục tiêu (có thể tiết lộ vai trò của máy trong mạng, ví dụ: SQL-PROD-01).

- `uname -a`: Hiển thị thông tin chi tiết về hệ thống và phiên bản Kernel (hữu ích để tìm lỗ hổng leo thang đặc quyền qua Kernel).

- `cat /proc/version`: Xem thông tin tiến trình hệ thống, phiên bản kernel và các trình biên dịch (như GCC) đã cài đặt.

- `cat /etc/issue`: Xem thông tin về hệ điều hành (có thể bị tùy chỉnh/thay đổi).

- `env`: Hiển thị các biến môi trường (đặc biệt lưu ý biến PATH để tìm các ngôn ngữ kịch bản hoặc trình biên dịch có thể lợi dụng).

## 2. Quản lý Tiến trình (Processes)

- `ps`: Xem các tiến trình đang chạy trong shell hiện tại.
   - `ps -A`: Xem tất cả các tiến trình đang chạy.
   - `ps axjf`: Xem tiến trình dưới dạng sơ đồ cây (process tree).
   - `ps aux`: Xem tiến trình của tất cả người dùng (a), hiển thị người dùng đã khởi chạy tiến trình (u) và các tiến trình không gắn với terminal (x).

## 3. Người dùng & Phân quyền (Users & Privileges)

- `id`: Xem tổng quan về cấp độ đặc quyền và nhóm (group) của người dùng hiện tại hoặc người dùng khác.

- `sudo -l`: Liệt kê tất cả các lệnh mà người dùng hiện tại có thể chạy với quyền root (thông qua sudo).

- `cat /etc/passwd`: Khám phá danh sách tất cả người dùng trên hệ thống (có thể dùng `grep "home"` để lọc ra người dùng thực).

- `history`: Xem lịch sử các lệnh đã gõ (đôi khi có thể rò rỉ mật khẩu hoặc username).

## 4. Mạng & Kết nối (Network)

- `ifconfig`: Xem thông tin về các interface mạng (card mạng) của hệ thống.

- `ip route`: Kiểm tra các tuyến mạng (network routes) hiện có.

- `netstat`: Thu thập thông tin về các kết nối mạng.
   - `netstat -a`: Hiển thị tất cả các port đang listening và các kết nối đã thiết lập.
   - `netstat -at` / `netstat -au`: Chỉ liệt kê giao thức TCP hoặc UDP.
   - `netstat -l`: Liệt kê các port đang ở trạng thái "listening".
   - `netstat -s`: Thống kê mức sử dụng mạng theo từng giao thức.
   - `netstat -tp`: Liệt kê các kết nối kèm tên dịch vụ và PID.
   - `netstat -i`: Thống kê hoạt động của các interface mạng.
   - `netstat -ano`: Hiển thị tất cả socket (`-a`), không phân giải tên miền (`-n`) và hiển thị timers (`-o`).

## 5. Tìm kiếm & Tệp tin (Files & Searching)

- `ls -la`: Liệt kê tất cả tệp tin và thư mục (bao gồm cả file ẩn) cùng với quyền của chúng.

- `find`: Công cụ tìm kiếm tệp tin và thư mục cực kỳ mạnh mẽ.
   - Tìm theo tên/loại:
      - `find . -name flag1.txt`: Tìm file tên "flag1.txt" ở thư mục hiện tại.
      - `find / -type d -name config`: Tìm thư mục (`-type d`) tên "config" trên toàn hệ thống.
   - Tìm theo quyền (Permissions):
      - `find / -type f -perm 0777`: Tìm file có quyền 777 (ai cũng có thể đọc/ghi/thực thi).
      - `find / -perm a=x`: Tìm các file có thể thực thi.
      - `find / -perm -u=s -type f 2>/dev/null`: Tìm các file có bit SUID (cho phép chạy file với đặc quyền của chủ sở hữu file - rất quan trọng để leo thang đặc quyền).
   - Tìm theo người dùng:
      - `find /home -user frank`: Tìm tất cả file của user "frank".
   - Tìm theo thời gian:
      - `find / -mtime 10` / `find / -atime 10`: File được sửa/truy cập trong 10 ngày qua.
      - `find / -cmin -60` / `find / -amin -60`: File được thay đổi/truy cập trong 60 phút qua.
   - Tìm theo kích thước:
      - `find / -size 50M`: Tìm file có kích thước 50MB (có thể dùng `+50M` hoặc `-50M`).
   - Tìm thư mục có quyền ghi/thực thi chung (World-writable/executable):
      - `find / -writable -type d 2>/dev/null`
      - `find / -perm -222 -type d 2>/dev/null`
      - `find / -perm -o w -type d 2>/dev/null`
      - `find / -perm -o x -type d 2>/dev/null` (World-executable)
   - Tìm công cụ dev/ngôn ngữ:
      - `find / -name perl*` (hoặc `python*`, `gcc*`).

# Automation Tools

LinPeas: https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS

LinEnum: https://github.com/rebootuser/LinEnum

LES ( Exploit Suggester): https://github.com/mzet-/-exploit-suggester

Smart Enumeration: https://github.com/diego-treitos/-smart-enumeration

Priv Checker: https://github.com/linted/linuxprivchecker
