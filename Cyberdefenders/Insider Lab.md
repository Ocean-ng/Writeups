'TAAUSAI' thuê mình làm pháp y số để điều tra cái bà Karen này đang thực hiện những hành vi bất hợp pháp. Mình được điều tra ổ đĩa chứa OS linux của Karen


dưới ảnh thì mình tìm được OS mà Karen đang dùng
<img width="1998" height="532" alt="image" src="https://github.com/user-attachments/assets/828e6905-78ca-413b-9727-bd07fcbd10b8" />

vào var -> log -> apache2 sẽ tìm được MD5 của access.log

<img width="954" height="907" alt="image" src="https://github.com/user-attachments/assets/8ba3d6d6-a5d5-4616-a1e0-f150ce21a40b" />

root -> Downloads là file mà karen đã tải về

<img width="1293" height="604" alt="image" src="https://github.com/user-attachments/assets/8c4544c5-a0c1-4a1d-9433-de62a7003c92" />

root -> .bash_history file sít rịt được tạo ở /root/Desktop/SuperSecretFile.txt

<img width="798" height="1118" alt="image" src="https://github.com/user-attachments/assets/bd52560e-b6dc-4cd8-8ecc-b496016bc088" />

kéo xuống còn tìm được tool để xem didyouthinkwedmakeiteasy.jpg

<img width="514" height="31" alt="image" src="https://github.com/user-attachments/assets/6a917c82-656e-4e98-8ecf-e3c994a00bd6" />

root -> Desktop -> checklist

<img width="664" height="411" alt="image" src="https://github.com/user-attachments/assets/a9f10172-d825-40c1-b9a9-276872c012bd" />

dựa vào apache log thì ko chứa data nào (0 kb) nên là chả có apache nào chạy hết

trên hình là thấy karen đang tấn công bob, có lẽ đang muốn quét ổ đĩa do là thấy bả đang sài ftk nè

<img width="1839" height="909" alt="image" src="https://github.com/user-attachments/assets/7777277a-3408-4a79-9aaa-e881c6ba9611" />

root -> documents -> myfirsthack. Trong là karen bảo cũng viết bash được giống young

<img width="670" height="471" alt="image" src="https://github.com/user-attachments/assets/7dcf5de1-4bbf-46fe-a77f-c645f9fcc670" />

root -> var -> log -> auth.log. ká rèn hắc cơ đã dùng lệnh su với user postgres dưới quyền root

<img width="1696" height="1073" alt="image" src="https://github.com/user-attachments/assets/1762a8be-3d80-4009-bb7a-965af89a2f28" />

coi lại file .bash_history thì karen đang hack tại /root/Documents/myfirsthack/ sau đó chắc bị bắt gòi

<img width="671" height="100" alt="image" src="https://github.com/user-attachments/assets/e32f212d-27c4-41be-8042-8fb0a5cff208" />



