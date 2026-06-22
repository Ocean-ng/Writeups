mới vô nó sẽ hiện tên CMS, sau đó mình sẽ đi tìm phiên bản để biết nó có lỗ hổng nào hay không

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/ffb71d26-8b64-4b67-8868-cbfae55a7a07" />

ở đây mình đã biết là CMS Cockpit version 0.11.1, lên search google sẽ ra script trên exploitdb

<img width="2558" height="1194" alt="image" src="https://github.com/user-attachments/assets/8d50f311-0bc9-466c-b92f-c3e9a3c988d6"/>

đọc kỹ script thì nó sử dụng hàm var_dump để truy vấn ra các databases và lợi dụng `/auth/resetpassword` để đổi mật khẩu.

<img width="743" height="234" alt="image" src="https://github.com/user-attachments/assets/4d35bb9a-070f-4344-ac2d-3960a431e005" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/79589757-0481-40d0-a7c2-70a1496cb072" />

thì ở đây là tìm thấy 4 user hợp lệ và biết được email của skidy, sau đó đi đổi mật khẩu để đăng nhập vào web

<img width="919" height="230" alt="image" src="https://github.com/user-attachments/assets/59dc140e-85bb-4a03-b8ed-31564577b513" />

có chỗ upload thì biết rằng mình sẽ rce lên server, sau khi lên được shell rồi có thể sài `/usr/bin/script -qc /bin/bash /dev/null` để làm shell ổn định hơn

<img width="1350" height="360" alt="image" src="https://github.com/user-attachments/assets/ac9cb5ac-f186-437e-b31e-1e6d31b74b56" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/b310f3be-6f6e-4f3e-8f51-f58826e55f69" />

nhưng không may phải đăng nhập vào stux thì mới có thể đọc user flag được. Thì quay lại script trên exploitdb hồi nãy dùng nosql injection, thường là sẽ dùng mongodb, thật may là ở folder stux đang có tên quản lí database đó

<img width="2559" height="1193" alt="image" src="https://github.com/user-attachments/assets/f023fe48-acef-4e85-a87e-bda09446fa47" />

<img width="2436" height="664" alt="image" src="https://github.com/user-attachments/assets/367180eb-332c-44a5-9689-4b8b0bb2b5dd" />

stux được chạy dưới quyền root bằng công cụ `exiftool`, kiểm tra `version 12.05` 

<img width="2340" height="1027" alt="image" src="https://github.com/user-attachments/assets/76907815-c57d-4778-8388-19f17fe74c9c" />

đầu tiên là mình sẽ tạo một file chứa mã độc bằng script đã cho, kiểu như là `/bin/bash` chẳng hạn để leo quyền. Theo như nguyên lí hoạt động của exiftool thì nó sẽ phân tích 1 bức ảnh nên là mình sẽ gán mã mở root shell vào trong bức ảnh đó. Sau đó upload lên server và phân tích ảnh thì mã độc đã được thực thi ngay lúc đó

<img width="1768" height="546" alt="image" src="https://github.com/user-attachments/assets/d3df45d1-1fc5-40c1-a63d-b6c44e82cdbc" />

up image.jpg lên thì cấp quyền thực thi cho nó, sau đó sử dụng đường dẫn exiftool để thực thi file ảnh và cuối cùng mở shell mới để đọc root flag

<img width="1309" height="501" alt="image" src="https://github.com/user-attachments/assets/b68767f2-71ef-4379-a228-d604fba5f6b6" />
























