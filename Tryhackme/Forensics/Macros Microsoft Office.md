1. Macro là gì và "Dropper Attack" hoạt động ra sao?

    Macro thực chất là những đoạn mã lập trình nhỏ (thường viết bằng Visual Basic) được tích hợp hợp pháp trong Word/Excel để tự động hóa các tác vụ lặp đi lặp lại.

    Dropper Attack (Tấn công mồi nhử/nhỏ giọt): Hacker sẽ gửi cho nạn nhân một email giả mạo (ví dụ: hóa đơn cần thanh toán gấp) đính kèm file Word. Khi nạn nhân mở file và bất cẩn bấm vào nút "Enable Content" (Cho phép chạy nội dung), đoạn Macro độc hại sẽ âm thầm chạy ngầm. Bản thân đoạn Macro này không phải là virus phá máy, nhiệm vụ của nó chỉ là kết nối Internet, tải về máy tính một phần mềm độc hại thực sự (như Ransomware - mã độc tống tiền) và kích hoạt nó.

2. Cách Hacker qua mặt phần mềm Diệt Virus (Anti-Virus)

Các phần mềm diệt virus ngày nay rất thông minh. Nếu mã độc được viết thẳng vào Macro (plaintext), nó sẽ bị tóm gọn ngay lập tức. Để lách luật, hacker sử dụng phương pháp tấn công 2 giai đoạn (two-stages):

    Giai đoạn 1: Viết một đoạn Macro cực kỳ ngắn và vô hại để đánh lừa phần mềm diệt virus.

    Giai đoạn 2: Giấu mã lệnh độc hại ở một nơi khác (chính là phần nội dung văn bản của file Word). Đoạn Macro ở Giai đoạn 1 sẽ có nhiệm vụ đọc nội dung văn bản này và thực thi nó.

3. Nghệ thuật ngụy trang (Steganography)

Sẽ rất lộ liễu nếu nạn nhân mở file Word ra và thấy toàn là những dòng code loằng ngoằng, họ sẽ tắt đi ngay. Vậy làm sao để giấu những dòng code này ngay trước mắt nạn nhân?

    Thủ thuật: Hacker sẽ cho đoạn code đó vào một Text Box (hộp văn bản), sau đó đổi màu chữ thành màu trắng (trùng với màu nền giấy) và xóa đường viền của Text Box.

    Kết quả: Nạn nhân mở file lên chỉ thấy một trang giấy trắng hoặc một vài dòng chữ bình thường. Nhưng đoạn Macro vẫn có thể "đọc" được những dòng code màu trắng tàng hình đó và tiến hành tải virus về máy.

4. Cách phòng thủ và phân tích (Thực hành với vmonkey)

Vì các file Word này rất nguy hiểm (mở lên là dính mã độc), các chuyên gia bảo mật phải dùng công cụ chuyên dụng để phân tích mà không cần mở file.

    Công cụ được nhắc đến trong bài là vmonkey (thuộc hệ điều hành REMnux chuyên dùng để phân tích mã độc).

    vmonkey có khả năng "mổ bụng" file Word, đọc các đoạn mã Visual Basic bên trong mà không hề kích hoạt chúng. Nó sẽ phân tích và cảnh báo cho người dùng biết đoạn Macro này có chứa các hành vi đáng ngờ (như tự động tải file từ internet) hay không.
