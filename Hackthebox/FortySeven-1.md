# Mysterious Elephant (APT-K-47)

Mysterious Elephant (APT-K-47) là một nhóm tin tặc đe dọa dai dẳng nâng cao (APT) được phát hiện lần đầu vào năm 2023, nhưng các dấu vết hoạt động thực tế đã có từ năm 2022. Đây là một nhóm gián điệp mạng có nguồn gốc từ khu vực Nam Á, liên tục nâng cấp các thủ đoạn để đánh cắp thông tin tình báo.

Dưới đây là thông tin chi tiết và chính xác về hồ sơ hoạt động của nhóm APT này:

## 1. Nguồn gốc và Sự tiến hóa

- **Mối liên hệ ban đầu:** Trong giai đoạn đầu hoạt động, Mysterious Elephant thường xuyên nâng cấp và sử dụng lại mã độc bị bỏ từ các nhóm APT Nam Á khác như SideWinder, Confucius, Bitter, và Origami Elephant.

- **Sự tiến hóa:** Kể từ các chiến dịch gần đây, nhóm này đã nâng cấp tools của mình, chuyển sang tự phát triển hệ sinh thái phần mềm độc hại (malware) tùy chỉnh riêng biệt, khó phát hiện và tinh vi hơn rất nhiều.

## 2. Mục tiêu và Động cơ

- **Quốc gia và Lĩnh vực:** Nhóm này nhắm mục tiêu chủ yếu vào các cơ quan chính phủ, tổ chức ngoại giao và thực thể quân sự tại khu vực Châu Á - Thái Bình Dương. Các quốc gia thường xuyên bị nhắm tới bao gồm Pakistan, Bangladesh, Afghanistan, Nepal, Sri Lanka và Thổ Nhĩ Kỳ.

- **Động cơ:** Thu thập thông tin tình báo và gián điệp mạng. Điểm đặc biệt nhất của Mysterious Elephant là chúng cực kỳ tập trung vào việc đánh cắp dữ liệu WhatsApp (bao gồm tài liệu, hình ảnh, tệp nén) trên máy tính của nạn nhân, bởi đây là ứng dụng được dùng phổ biến cho liên lạc chính thức tại các khu vực này.

## 3. Kỹ thuật xâm nhập (TTPs)

- **Lừa đảo có chủ đích (Spear-Phishing):** Nhóm thường gửi các email chứa tài liệu mồi nhử mang tính thời sự hoặc các chủ đề nhạy cảm (ví dụ: tài liệu giả mạo của bộ ban ngành, tài liệu về lễ hành hương Hajj).

- **Tệp đính kèm độc hại:** Chúng thường đính kèm các tệp ZIP được cài mật khẩu (mật khẩu ghi ngay trong email) để qua mặt các hệ thống quét virus tự động. Bên trong là các tệp CHM (Compiled HTML Help) hoặc tệp văn bản độc hại.

- **Khai thác lỗ hổng:** Để tự động thực thi mã độc, chúng lợi dụng các lỗ hổng phần mềm phổ biến như CVE-2023-38831 (lỗ hổng của phần mềm giải nén WinRAR) hoặc CVE-2017-11882 (lỗ hổng trong trình soạn thảo công thức của Microsoft Office).

## 4. Kho vũ khí mã độc đặc trưng

Mysterious Elephant sở hữu một bộ công cụ gián điệp rất đa dạng, nổi bật gồm:

- **BabShell:** Một công cụ dạng Reverse Shell giúp kẻ tấn công giành quyền điều khiển máy tính. Khi chạy, nó thu thập thông tin cốt lõi như tên người dùng, tên máy tính, địa chỉ MAC và dùng làm bệ phóng để tải các module khác.

- **MemLoader HidenDesk / MemLoader Edge:** Đây là các module tải mã độc trực tiếp vào bộ nhớ RAM (fileless malware) để né tránh sự kiểm tra của phần mềm diệt virus. Như bạn đã biết, chúng có cơ chế kiểm tra môi trường Sandbox (ví dụ: đếm số lượng tiến trình) và tạo ra các môi trường ẩn trên máy nạn nhân để tải các công cụ điều khiển từ xa (như Remcos RAT hoặc VRAT).

- **Asyncshell:** Một payload độc hại dạng dòng lệnh dựa trên lập trình không đồng bộ (asynchronous). Công cụ này đã được nhóm liên tục nâng cấp qua nhiều phiên bản khác nhau kể từ năm 2023 để mở rộng chuỗi tấn công.

- **ORPCBackdoor:** Một loại backdoor được phát hiện vào cuối năm 2023, giúp kẻ tấn công duy trì sự kiểm soát và trích xuất dữ liệu ra ngoài.

- **Uplo & Stom:** Đây là các module chuyên dụng. "Stom" nhắm thẳng vào thư mục Desktop của WhatsApp cùng các ổ đĩa phụ để gom dữ liệu, trong khi "Uplo" sẽ tự động quét các tệp giá trị (PDF, bảng tính, hình ảnh) và lén lút tải (upload) toàn bộ về máy chủ của tin tặc.







---

1. Mysterious Elephant

<img width="1237" height="732" alt="image" src="https://github.com/user-attachments/assets/84611d3a-48c1-4147-a46e-ca4f318cda6e" />

2. 2022

<img width="1253" height="503" alt="image" src="https://github.com/user-attachments/assets/73902ed9-6aa1-4fb9-86b9-dc820a40981b" />

3. GetFileVersionInfoByHandleEx(void)

<img width="1132" height="209" alt="image" src="https://github.com/user-attachments/assets/2ccb1447-d044-4316-b799-08591a7a7eee" />

4. ts.dat

<img width="1221" height="360" alt="image" src="https://github.com/user-attachments/assets/c0eca3ba-f44a-47df-802c-a8f9299af165" />

5. Bitter

<img width="1238" height="311" alt="image" src="https://github.com/user-attachments/assets/989783f2-a9d5-40a8-8304-7b666515cd26" />

6. Asyncshell-v2

<img width="1253" height="640" alt="image" src="https://github.com/user-attachments/assets/b3ceb106-c7ba-4c58-a0ac-39ecdc21fb59" />

7. 40

<img width="1209" height="180" alt="image" src="https://github.com/user-attachments/assets/f30383c4-96ed-423f-a7e6-4cb84f2808af" />

8. MalwareTech_Hidden


<img width="1181" height="140" alt="image" src="https://github.com/user-attachments/assets/3ed2db31-9f22-4909-9a30-30dcd024fbfb" />

9. T1547.001



<img width="1822" height="872" alt="image" src="https://github.com/user-attachments/assets/4a34c8da-8eaa-458e-a70e-f0a442ecc698" />
10. Stom Exfiltrator


<img width="1240" height="503" alt="image" src="https://github.com/user-attachments/assets/8b7a71eb-1ffc-4121-b26d-c1e24c3fea14" />
11. T1059.001


<img width="1837" height="897" alt="image" src="https://github.com/user-attachments/assets/c5fa6484-f81a-4d08-8781-d218850d3e5a" />
12. Vtyrei


<img width="1208" height="131" alt="image" src="https://github.com/user-attachments/assets/c2a76492-6891-4a4e-a2c7-dcc4ec73e22d" />
13. CVE-2023-38831

 
<img width="1242" height="834" alt="Screenshot 2026-07-20 002408" src="https://github.com/user-attachments/assets/a821ef63-6246-4e67-b820-b286ef9a403a" />
14. 9e50adb6107067ff0bab73307f5499b6


<img width="718" height="103" alt="Screenshot 2026-07-20 003457" src="https://github.com/user-attachments/assets/b683e6d7-51b9-433e-adfa-cf0f44478f86" />


15. T1041

<img width="1825" height="717" alt="image" src="https://github.com/user-attachments/assets/45291446-33f3-4213-b39c-06ec68d5336d" />













