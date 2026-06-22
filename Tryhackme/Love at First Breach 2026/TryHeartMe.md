<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/7db96316-c66d-4cb1-8895-42ab5fda1256" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/eff30ca1-3c85-43fe-9276-b1c0f365e100" />

đăng kí tài khoản vào trang chính, định mua gửi tặng người thân mà không có đồng nào hết. Não mình lóe lên một suy nghĩ "hay là mình hack credit lên mấy ka đô la để mua nhỉ??"; "nhưng mà hack sao trùi, hmm, có ai nhiều credit hơn mình hong ta".
Thế là mình đi coi source code, quét thư mục thì tìm được trang của admin "á à thèn admin nì cấp cho người dùng nhiều credit lắm nè" nhưng status là 302, phải có mật khẩu hay token mới dô được

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/d6170957-9225-4202-86c3-f054522a1b93" />

mở f12 lên, nhận diện được api là chuỗi jwt gồm 3 phần cắt nhau bởi dấu chấm được mã hóa bằng base64 (Header.Payload.Signature)

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/e9d9c5dd-f984-40ee-9761-494e910a44f8" />

mình lên trang jwt.io để giải mã chuỗi này và có được thông tin quan trọng

để í role đang là user, mình chỉnh lại thành admin để leo thang đặc quyền

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/beaaa152-3d24-4a98-a822-938fbd62730c" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/9981ee8f-423a-463e-85a1-f936ca126239" />

rồi giờ thay chuỗi này vào value thôi

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/98e327ad-6acb-4858-9186-34b5e3ff47b9" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/c4a13f41-acf1-46bd-a5dd-77e99280ed9d" />

gòi gòi vừa lên được admin, vừa được tặng ngay 5000 credit lại còn được phát flag miễn phí nữa...















