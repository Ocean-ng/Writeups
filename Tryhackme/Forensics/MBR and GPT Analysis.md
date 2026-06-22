MBR và GPT đóng vai trò giúp máy tính nhận diện được phần cứng, sector để từ đó khởi động lên OS -> Hacker lợi dụng phần quan trọng này dùng bootkits để ngăn máy tính khởi động
Chữ ký MBR được biểu diễn bởi `55 AA` để đánh dấu điểm kết thúc của mã MBR
2 số thập lục phân là 1 byte. 512 byte của MBR được chia làm 3 phần
<img width="1266" height="1080" alt="image" src="https://github.com/user-attachments/assets/ca80a82c-0a39-4214-8b60-5a8c6b8eca9e" />

Mỗi cụm 16 bytes của một phân vùng này chứa toàn bộ thông tin của một ổ đĩa (ví dụ ổ C, D...). Các thông số quan trọng nhất bao gồm:
- Boot Indicator (1 byte): Cờ đánh dấu khởi động.
Nếu giá trị là 80: Đây là ổ chứa Hệ điều hành (thường là ổ C), máy tính có thể boot từ đây.
Nếu giá trị là 00: Đây là ổ lưu dữ liệu bình thường, không thể boot.
- Partition Type (1 byte): Cho biết hệ thống tập tin (Filesystem) đang dùng là gì. Ví dụ: giá trị 07 có nghĩa là định dạng NTFS.
- Starting LBA Address (4 bytes): Địa chỉ logic bắt đầu của phân vùng. Đây là thông số cực kỳ quan trọng trong điều tra số. Nó giúp bạn xác định chính xác vị trí phân vùng trên ổ cứng để trích xuất hoặc khôi phục dữ liệu bị ẩn/xóa.
- Number of Sectors (4 bytes): Tổng số lượng sector của phân vùng. Lấy con số này nhân với 512 (kích thước 1 sector), bạn sẽ tính ra được dung lượng thực tế của ổ đĩa đó.

# Công thức 3 bước giải mã (Nguyên tắc Little-Endian)
Vì máy tính lưu trữ thông số địa chỉ theo kiểu "viết ngược từ dưới lên" (gọi là Little-endian), nên để hiểu được nó, bạn luôn phải làm theo 3 bước:

- Đảo ngược thứ tự các cặp số Hex.
- Đổi mã Hex đó sang số Thập phân (Decimal).
- Nhân với 512 (vì kích thước chuẩn của 1 sector luôn là 512 bytes).

## Tìm vị trí bắt đầu của ổ đĩa (Từ trường Starting LBA): Giúp bạn biết chính xác ổ đĩa nằm ở byte thứ bao nhiêu trên phần cứng. Rất hữu ích trong điều tra số để khôi phục dữ liệu bị xóa hoặc giấu kín.

*Ví dụ:* Mã hex là 00 08 00 00 → Đảo ngược thành 00 00 08 00 → Đổi ra số thập phân là 2048 → Lấy 2048×512=1,048,576. Vị trí bắt đầu chính là byte thứ 1,048,576.

## Tính dung lượng của ổ đĩa (Từ trường Number of Sectors):

*Ví dụ:* Mã hex là 00 B0 23 03 → Đảo thành 03 23 B0 00 → Đổi ra số thập phân là 52,670,464 → Lấy 52,670,464×512=26,967,277,568 bytes (tương đương khoảng 26.9 GB).

## Chữ ký MBR (Magic Number)

- Khúc này nằm ở cuối cùng của MBR (byte 510 và 511), bắt buộc phải chứa chuỗi 55 AA.
- Nó hoạt động như một "con dấu chứng nhận" báo cho máy tính biết đây là một bản đồ MBR hợp lệ và nguyên vẹn.
- Nếu ổ cứng bị lỗi, hoặc bị mã độc (malware) cố tình sửa đổi 2 ký tự này, máy tính sẽ lập tức tê liệt và không thể khởi động được nữa.

---

<img width="1130" height="893" alt="image" src="https://github.com/user-attachments/assets/79f3e316-e15a-405b-8042-17e7a6b5b4db" />


## Bước 1: Xác định vị trí 16 bytes của Phân vùng 1

    - Bảng phân vùng (Partition Table) luôn bắt đầu từ byte thứ 446 (địa chỉ Hex là 01BE).

    - Phân vùng 1 sẽ chiếm 16 bytes đầu tiên (từ 01BE đến 01CD).

    - Nhìn vào hình ảnh, chúng ta lấy 2 ô cuối của dòng 000001B0 và 14 ô đầu của dòng 000001C0, ta thu được chuỗi 16 bytes sau:
    80 20 21 00 07 FE FF FF 00 08 00 00 00 F0 BF 03

## Bước 2: Trích xuất trường "Number of Sectors" (Số lượng Sector)

    - Thông số về tổng số sector nằm ở 4 bytes cuối cùng của chuỗi trên.

    - Lấy 4 bytes đó ra, ta có: 00 F0 BF 03

## Bước 3: Thực hiện phép tính (Chuẩn Little-Endian)

    - Đảo ngược mã: 00 F0 BF 03 trở thành 03 BF F0 00.

    - Đổi sang hệ Thập phân (Decimal): Giá trị Hex 03BFF000 khi quy đổi ra hệ thập phân là 62,910,464 (Sectors).

    - Nhân với kích thước Sector: 62,910,464 sectors × 512 bytes = 32,210,157,568 bytes.

---




