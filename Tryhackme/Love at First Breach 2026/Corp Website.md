<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/edcc9273-74cc-4b7b-81f6-d01ac1549ddc" />

đây là trang chủ của bài, lướt xuống sẽ thấy được form, mình sẽ thử up script XSS lên xem nó xảy ra gì không

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/d6d694f4-5482-471b-b41e-549a54a712c2" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/3d701cd7-3890-4a39-91fa-47bd8264accb" />

nó chẳng cho mình output gì cả, mình sẽ đi quét thư mục xem có chỗ nào đăng nhập vào hay tài liệu ẩn gì đó không

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/f276183f-93c5-460d-8d67-95aa5196c949" />

để í là có một trang liên quan tới cookie nên mình sẽ up script gọi cookie lên nhưng chẳng thành công

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/a72d2ddb-a8f1-4e3e-be5f-9868875f65fb" />

qua nuclei thì mình tìm được hai lỗ hổng CV-2025-55182 React2Shell, lên git tải script về https://github.com/xalgord/React2Shell

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/f52d131d-00b2-41a9-91a4-2ae0776aafbf" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/8f7a1a9a-186f-4774-9ec8-050d9ca1d571" />

vậy là mình đã tìm được flag user, tiếp theo là mình tìm được lỗ hổng để leo thang đặc quyền lên root. Mình mò nhiều command python trên https://gtfobins.org/ mà vẫn không leo thang được

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/f06dcfe3-e20b-4468-9e99-bd05708fabe8" />

nên mình đã mở web của bản thân để từ máy của bài daniel wget lấy file reverse shell python về và chạy được 

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/eea5ff89-bf9f-4340-be3f-fc2790504a84" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/98035519-9a78-4091-9e42-340ac68f6b86" />

<img width="2558" height="1359" alt="image" src="https://github.com/user-attachments/assets/ba20f1a8-a026-481c-b7a8-cf1cb4806929" />

vậy là mình đã leo lên và tìm được flag root













