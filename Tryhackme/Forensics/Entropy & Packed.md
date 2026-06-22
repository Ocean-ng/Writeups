1. Entropy là gì? (Độ hỗn loạn của dữ liệu)

Entropy là một thang điểm (từ 0 đến 8) dùng để đo mức độ "ngẫu nhiên" hoặc "hỗn loạn" của dữ liệu bên trong một tệp.

    Điểm thấp (gần 0): Dữ liệu có quy luật, lặp đi lặp lại. (Ví dụ: Một file chứa toàn số 1, hoặc toàn chữ A).

    Điểm cao (gần 8): Dữ liệu cực kỳ ngẫu nhiên, lộn xộn và không có quy luật.

Tại sao điều này lại quan trọng đối với nhà phân tích?
Các tác giả viết mã độc thường cố gắng giấu mã nguồn của họ bằng cách mã hóa (encryption) hoặc đóng gói (packing) để qua mặt các phần mềm diệt virus. Các thuật toán mã hóa này xào trộn dữ liệu, làm cho cấu trúc file trở nên cực kỳ hỗn loạn, dẫn đến điểm Entropy rất cao.
Nếu bạn đứng trước 1.000 file khả nghi, bạn không thể xem từng file một. Bạn chỉ cần quét điểm Entropy, file nào điểm cao (từ 7 trở lên), bạn ưu tiên mổ xẻ file đó trước!
2. Packing và Unpacking (Đóng gói và Bóc tách)

Packing (Đóng gói) giống như việc bạn nén một tệp .exe bằng WinRAR, nhưng kết quả đầu ra không phải là file .zip hay .rar, mà vẫn là một file .exe có thể click đúp vào để chạy.

    Các lập trình viên dùng nó để chống đánh cắp bản quyền và giảm dung lượng phần mềm.

    Hacker dùng nó để "băm vò" mã độc, khiến mã độc biến dạng để Anti-Virus không nhận diện ra.

Cơ chế hoạt động:
Mọi tệp thực thi đều có một Entry Point (Điểm bắt đầu) – nơi CPU sẽ chạy dòng lệnh đầu tiên.

    File bình thường: Entry Point trỏ thẳng vào đoạn code chính của phần mềm.

    File bị Packed: Mã độc gốc đã bị nén lại. Entry Point lúc này bị bẻ lái trỏ vào một đoạn code rất nhỏ gọi là "Unpacking Stub" (Mã mồi giải nén).
    Khi nạn nhân mở file, đoạn Unpacking Stub này chạy đầu tiên. Nhiệm vụ của nó là âm thầm giải nén (unpack) toàn bộ mã độc gốc vào thẳng bộ nhớ RAM. Khi giải nén xong, nó trả Entry Point về lại đoạn mã độc nguyên thủy để bắt đầu quá trình phá hoại. Ở trạng thái này, file độc hại mới hiện nguyên hình.

3. Làm sao để biết một file đã bị Packed?

Nhà phân tích sẽ nhìn vào 3 dấu hiệu đặc trưng:

    Entropy cao: Như đã giải thích ở trên.

    Rất ít hàm Imports: Một phần mềm bình thường cần gọi vài chục hàm của Windows (như mở file, in ra màn hình). Nhưng một file bị packed thường chỉ có 1 đến 2 hàm (phổ biến nhất là GetProcAddress và LoadLibrary) để nó tự động tải các hàm khác vào bộ nhớ sau khi giải nén xong.

    Tên Section kỳ lạ: Thay vì các vùng chứa tiêu chuẩn (như .text, .data), file sẽ xuất hiện các section mang tên của chính công cụ nén đó, ví dụ như .UPX0, .UPX1.
