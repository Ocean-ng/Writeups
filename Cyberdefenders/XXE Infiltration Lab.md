## kịch bản: hệ thống máy chủ bị XEE, đăng nhập vào MySQL để đánh cắp dữ liệu

https://cyberdefenders.org/blueteam-ctf-challenges/xxe-infiltration/

Q1: đọc lướt các packet thì nhận biết được ip của héc cơ là `210.106.114.183` còn về phía nạn nhân là `50.239.151.185`. Dựa vào 2 ip này mình sẽ vào Statistics để xem thống kê port

<img width="402" height="39" alt="image" src="https://github.com/user-attachments/assets/3b6fc305-1eeb-4131-b595-757cdddd36ae" />

<img width="231" height="68" alt="image" src="https://github.com/user-attachments/assets/eb66b51f-5779-482a-89cc-d8c617e22e63" />

Q2: lướt tiếp tục cho đến khi hết phần bruteforce directory của héc cơ, do hắn đã tìm được thư mục mong muốn 

<img width="353" height="22" alt="image" src="https://github.com/user-attachments/assets/3c7744c0-08f2-42b6-8813-24ba7f369541" />

Q3: cùng với packet đó thì ở phía có hiện tên của file .xml chứa script mã độc

<img width="808" height="202" alt="image" src="https://github.com/user-attachments/assets/91bb5efd-6327-4c31-a4f2-c7137efe6e44" />

Q4: đọc các packet POST còn lại sẽ tìm được từng payload chứa script đọc các file khác nhau

<img width="849" height="240" alt="image" src="https://github.com/user-attachments/assets/7ca9078c-e808-4b66-a6b5-c43a22a9b1f1" />

Q5: cùng packet của Q4, server nạn nhân sẽ trả về nội dung mà héc cơ đã request

<img width="646" height="568" alt="image" src="https://github.com/user-attachments/assets/260e2bd1-8409-4154-ba63-f38d564ee380" />

Q6: tiếp tục đọc các packet thì tìm thấy thời điểm lần đầu héc cơ đăng nhập vào mysql

<img width="987" height="263" alt="image" src="https://github.com/user-attachments/assets/3b05e99b-fe21-4b9c-84c9-b57974854f3c" />

Q7: miệt mài lướt tiếp sẽ tìm thấy packet có key `upload`, bạn biết phải làm gì rùi đó, nhấn dô coi thấy được héc cơ up file shell lên nhó. Sau đó sẽ LFI để tự định danh user hay đọc file gì đó

<img width="1632" height="181" alt="image" src="https://github.com/user-attachments/assets/fdf309be-750b-4883-a9fc-ad2e1bbc48ea" />

<img width="381" height="23" alt="image" src="https://github.com/user-attachments/assets/e329aea8-8d9c-4154-b55e-922db6d34ec1" />


