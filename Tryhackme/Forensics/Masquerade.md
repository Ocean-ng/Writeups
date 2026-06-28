ta tìm những packet có các header như là GET, POST để biết đang kết nối với domain nào. Cùng với đó ta sẽ tìm đuọc thời gian phản hồi của máy chủ

<img width="2553" height="631" alt="image" src="https://github.com/user-attachments/assets/3c52ce5f-db7c-40bc-ac2c-816cfb5cdc6e" />

vào xem file window events ta sẽ thấy được các đoạn code thuật toán ở đây là RC4

<img width="2077" height="847" alt="image" src="https://github.com/user-attachments/assets/b4cd0e06-f88b-483f-b29e-9efaf04ee04e" />

`UTF8.GetBytes(('X9vT3pL'+'2QwE'+'8xR6'+'ZkYhC4'+'s'))` -> `X9vT3pL2QwE8xR6ZkYhC4s` chính là passphrase của script malware này

tiếp theo ta lấy đoạn mã trong file vào cyberchef decrypt payload này

<img width="2550" height="828" alt="image" src="https://github.com/user-attachments/assets/5f52ff81-bdb0-47d3-8cc3-a803081e6394" />

khi đã có được mã hash256 rồi thì vào virustotal để xem payload này được đánh giá như thế nào

<img width="2556" height="1086" alt="image" src="https://github.com/user-attachments/assets/25a8947a-2791-4711-9982-c63951d9c46c" />

export file ra trên cyberchef vừa nãy (xóa thuật toán sha256) 

<img width="2049" height="1069" alt="image" src="https://github.com/user-attachments/assets/b6f9cfa6-3997-491a-acb5-027c47ab4f87" />

ta thấy rõ entrypoint của malware này

<img width="1241" height="401" alt="image" src="https://github.com/user-attachments/assets/73ad9e7e-e0d4-4e3e-b434-31e2fb191221" />

mã độc ta đang phân tích là một biến thể của TrevorC2. Theo mặc định, TrevorC2 sử dụng thuật toán AES để mã hóa giao tiếp và tìm được chuỗi bị mã hóa

<img width="1547" height="1076" alt="image" src="https://github.com/user-attachments/assets/bf0db3d2-a742-48a6-aa34-998877960b2e" />

ta sẽ dùng chuỗi bị mã hóa chuyển sang sha256 bằng cyberchef, đó chính là key cho AES decrypt

<img width="2064" height="1125" alt="image" src="https://github.com/user-attachments/assets/ddfc8a68-b3bb-43b4-b727-329aa383e0ee" />

vậy là ta đã tìm được flag

<img width="2042" height="975" alt="image" src="https://github.com/user-attachments/assets/f7344dca-fe1f-468c-9485-60a9d70cf36f" />






