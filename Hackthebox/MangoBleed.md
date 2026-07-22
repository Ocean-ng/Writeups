theo kịch bản được mô tả thì khi mà mình dùng chị gu gồ cho ra kết quả dưới ảnh sau

<img width="2062" height="860" alt="image" src="https://github.com/user-attachments/assets/1517ae2e-5439-4921-82cf-c509edd7aae6" />

để coi version của mongoDB thì vào /var/log/mongodb.log

<img width="1135" height="30" alt="image" src="https://github.com/user-attachments/assets/c107b848-02b9-4845-9b98-4a37c1a568aa" />

tiếp tục đọc logs sẽ tìm được các dòng chứa keyword `remote`, đó là biểu hiện của thèn héc cơ đang remote lên server

<img width="2531" height="908" alt="image" src="https://github.com/user-attachments/assets/0c761dd1-d890-4424-8d9d-a8e445b6b678" />

vào lúc thời gian ở dưới, sau khi remote, hacker bắt đầu nghe lén thông tin 

<img width="2523" height="834" alt="image" src="https://github.com/user-attachments/assets/824c4996-2515-4f11-bd02-da8dc214fa9a" />

đếm những dòng kết nối được chấp thuận và hủy bỏ 

<img width="965" height="40" alt="image" src="https://github.com/user-attachments/assets/f3e9ce82-225f-4dae-b085-645886327198" />

xem trong /var/log/auth.log, hacker khi brute-force thành công sẽ đăng nhập vào server với biểu hiện là terminal sẽ được mở lên

<img width="1746" height="25" alt="image" src="https://github.com/user-attachments/assets/d84dcc0d-01be-45e3-a6d5-44a739475504" />

mình sẽ vào mục /home/mongoadmin để xem bash_history

<img width="1102" height="550" alt="image" src="https://github.com/user-attachments/assets/b7080a26-6a55-421a-b576-b63c7a8b5106" />

héc cơ nì vào /var/lib/mongodb/ và mở server python, mìn nghĩ là sẽ up file lên server và quét tự động bằng linpeas đey này

<img width="253" height="26" alt="image" src="https://github.com/user-attachments/assets/3d28235c-00bb-4823-ae3f-48c962321273" />



