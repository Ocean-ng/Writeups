đầu tiên bài này ta sẽ dùng vol3 để truy vấn OS

<img width="688" height="502" alt="image" src="https://github.com/user-attachments/assets/c524956f-f5f1-48c9-a452-34226f7fec32" />

dùng vol2 để truy vấn PID của `SearchIndexer` với profile vừa tìm được `Win7SP1x64` và plugin `pslist`

<img width="1395" height="710" alt="image" src="https://github.com/user-attachments/assets/fb0c119a-08a2-489d-be86-b07b1e90c375" />

tiếp đến ta dùng plugin `shellbags` dùng để lưu lại kích thước, vị trí và thời gian cuối cùng mà người dùng truy cập vào một thư mục (directory) trên giao diện Windows Explorer (có thể output ra .txt để dễ truy vấn hơn)

<img width="1441" height="811" alt="image" src="https://github.com/user-attachments/assets/da30a778-cfd2-43a9-b4aa-4b6ebfb223c6" />

sau một hồi lượn vòng thì ta sẽ tìm được folder mở lần cuối trên máy

<img width="1707" height="178" alt="image" src="https://github.com/user-attachments/assets/a966521b-497f-49d8-98f9-ff4882d8de1d" />

dùng plugin `netscan` để search các kết nối mạng

<img width="1849" height="1161" alt="image" src="https://github.com/user-attachments/assets/9a0b99ca-6120-4c95-bbf5-e4b00c6da336" />

để tìm ra các ps mã độc ta sẽ sài `malfind` và output ra file .txt. Thông thường các tệp chỉ có quyền READ nhưng ở đây 3 services đã bị nhiễm mã độc cho quyền WRITE

<img width="1016" height="109" alt="image" src="https://github.com/user-attachments/assets/c4e976cc-0dc0-4699-8ce7-46666a22b5b7" />


<img width="1014" height="139" alt="image" src="https://github.com/user-attachments/assets/ee62fe36-c27d-4f9f-99b5-79362e891604" />


<img width="1016" height="142" alt="image" src="https://github.com/user-attachments/assets/fcf83bfa-f6c8-4b4e-b987-6f80d37f9f32" />

tiếp tục ta đi tìm biến môi trường của pid 2464 trước với plugin `envars -p <PID>`. Ta sẽ bắt gặp 1 biến lạ `OANOCACHE` ít xuất hiện

<img width="2160" height="949" alt="image" src="https://github.com/user-attachments/assets/3b29ae64-542c-457c-9fde-0998ee1cfb55" />

để tìm được các ip, domain ta sẽ dùng `Select-String` để lọc file **.\strings.exe "C:\victim.raw" | Select-String "www\.go"**

<img width="1408" height="844" alt="image" src="https://github.com/user-attachments/assets/0044f9e6-4440-444d-b4c5-af0240a89f01" />

**.\strings.exe "C:\Đường_dẫn_của_bạn\victim.raw" | Select-String "www\.i"**

<img width="271" height="234" alt="image" src="https://github.com/user-attachments/assets/f1e2c35b-808d-48cb-909d-a93e5728b206" />

**.\strings.exe "C:\Đường_dẫn_của_bạn\victim.raw" | Select-String -Pattern "202\.\d{1,3}\.233\.\d{1,3}"**

<img width="845" height="84" alt="image" src="https://github.com/user-attachments/assets/59b2ae04-3b67-4f87-9960-384a50a11b12" />

**.\strings.exe "C:\Đường_dẫn_của_bạn\victim.raw" | Select-String -Pattern "\d{1,3}\.200\.\d{1,3}\.164"**

<img width="842" height="83" alt="image" src="https://github.com/user-attachments/assets/17ccbc38-a81c-47ec-ab18-93057337798a" />

**.\strings.exe "C:\Đường_dẫn_của_bạn\victim.raw" | Select-String -Pattern "202\.\d{1,3}\.233\.\d{1,3}"**

<img width="617" height="54" alt="image" src="https://github.com/user-attachments/assets/33cd7576-5445-4bd8-9346-b02bc72dc2b9" />


`trời ơi trời cuối cùng cũng xong, đọc logs muốn hoa mắt, ù tai, đầu quay chong chóng. Cảnh báo anh em nào làm bài này cần một ly matcha bên cạnh :>`


