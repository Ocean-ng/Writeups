ở bài này thì mình sẽ đi kiểm tra tập tin máy tính của một đồng nghiệp bị nghi là hacker cài backdoor kết nối với server C2, trong file người ta cho mình sẽ nhận được mã hash của phần mềm độc hại đó

<img width="2373" height="1281" alt="image" src="https://github.com/user-attachments/assets/a8ba281a-7604-4724-b4fe-e977105bd6de" />

đây là con trojan, phần mềm này được tải lên với tên là `WEXTRACT.EXE .MUI`

<img width="2359" height="1290" alt="image" src="https://github.com/user-attachments/assets/3e6e2824-6d51-4f8d-9669-de62b7ab98e9" />

vào trang details sẽ tìm được thời gian mà con trojan này được đem lên virustotal quét `2023-10-06 04:41:50 UTC`

<img width="2402" height="1306" alt="image" src="https://github.com/user-attachments/assets/50b3630c-180a-414f-af86-dedd53d8997f" />

nó sẽ đi đánh cắp dữ liệu hệ thống bằng mã định danh MITRE T1005

<img width="2354" height="1323" alt="image" src="https://github.com/user-attachments/assets/bf4461cf-79a9-4aed-a4ab-655be8439852" />

truy cập phần relations, thì mạng xã hội có liên quan là facebook

<img width="1105" height="947" alt="image" src="https://github.com/user-attachments/assets/05f53def-1022-4e2d-863b-a32fa143e288" />

<img width="2132" height="1014" alt="image" src="https://github.com/user-attachments/assets/4ab4a875-9225-416a-99dc-7e95af906bae" />

ngoài ra còn có thể tìm thấy địa chỉ và cổng độc hại mà hacker đã sử dụng

<img width="1898" height="1366" alt="image" src="https://github.com/user-attachments/assets/9f23ecf0-208b-41ef-80cf-192d46537fa3" />

ta thấy các quy tắc yara được các SOC analyis tạo ra

<img width="1859" height="1268" alt="image" src="https://github.com/user-attachments/assets/30a8907f-bb52-4186-886a-296b2a10cc3d" />

bí danh của con trojan này là `RECORDSTEALER`

<img width="2111" height="734" alt="image" src="https://github.com/user-attachments/assets/3bd099dc-e2d5-467a-8f58-dedb31f231e3" />


trên hình ta thấy con trojan này đang dùng api dll, vậy thì tên thư viện chính của nó là `ADVAPI32.dll`
