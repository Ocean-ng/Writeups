## kịch bản: có héc cơ lỏ đang tấn công mạng trường học, hắn dùng C2 để điều khiển và giao tiếp

https://cyberdefenders.org/blueteam-ctf-challenges/nerisbot/

Q1: thèn héc cơ nì gửi request tới với file mã độc cho nạn nhân nè

<img width="2559" height="387" alt="image" src="https://github.com/user-attachments/assets/bbeeba77-427c-4ce9-bb47-91159d6e635c" />

Q2: sau khi tìm được ip của héc cơ lỏ thì mình có thể dựa vào đó để truy vấn url mà hắn đã truy cập

<img width="2553" height="630" alt="image" src="https://github.com/user-attachments/assets/9f8faa1d-489b-4031-8721-d9aa1683b42e" />

Q3: cùng với hình trên là đều nhắm tới địa chỉ `147.32.84.165`

Q4: xem hình của Q2 tiếp thì có 5 file, mình nghĩ thèn héc cơ nì sẽ áp mã độc dô các file để có cơ hội xác suất hack máy nạn nhân thành công nhe

Q5: truy vấn md5 của file `kx4.txt` rồi đem lên virustotal quét tìm SHA256 nhoa

<img width="2559" height="456" alt="image" src="https://github.com/user-attachments/assets/b248a722-2d35-42db-948f-9896a65f547a" />

<img width="2092" height="1388" alt="image" src="https://github.com/user-attachments/assets/a71e704b-2f19-41e7-8158-5a1093c70a66" />
