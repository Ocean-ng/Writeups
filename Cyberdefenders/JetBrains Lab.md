## kịch bản: nạn nhân bị héc cơ RCE lên để thao túng dữ liệu

https://cyberdefenders.org/blueteam-ctf-challenges/jetbrains/

Q1: xem héc cơ này nó đã RCE lên máy như nào sẽ sài filter `http.request.method == POST`. Có thể thấy là hắn đã tạo 1 project có tên `bullshit`

<img width="1881" height="582" alt="image" src="https://github.com/user-attachments/assets/ff591fd5-b4a3-4ed3-9cb3-b06adb64d2a4" />

Q2: vào xem http stream của 1 packet bất kì được filter trước đó

<img width="1643" height="108" alt="image" src="https://github.com/user-attachments/assets/e9c2b1df-43bc-4136-a140-58ed34644dd1" />

Q3: sợt chị gu gồ với từ khóa `lastSeenSakuraUIVersion 2023.11.3 CVE`

<img width="1908" height="772" alt="image" src="https://github.com/user-attachments/assets/6cee4960-2b4d-4377-acf8-ac71f291a8ed" />

<img width="1299" height="560" alt="image" src="https://github.com/user-attachments/assets/95f2875f-da3b-4c31-9be0-ce5b8280d2ca" />

Q4: héc cơ nó tìm được lỗ hổng trong tạo user account mới, mình sẽ dùng filter `ip.src == 23.158.56.196 && http.request.method == "POST" && http contains "/users"` để tìm hiểu thêm hắn dùng gì x:y. héc cơ khai thác lỗ hổng CVE-2024-27198 (TeamCity authentication bypass) bằng cách thêm ;.jsp vào URL để bypass xác thực, rồi gọi API tạo user mới với quyền SYSTEM_ADMIN

<img width="1625" height="218" alt="image" src="https://github.com/user-attachments/assets/20738afb-fef5-4b74-81d3-e0c9336571f6" />

Q5: Find keyword `upload` để tìm tên file héc cơ up lên

<img width="1396" height="361" alt="image" src="https://github.com/user-attachments/assets/e276ada7-7b28-47a4-973e-3a6dd8666bd4" />

Q6: cuộn xuống tiếp sẽ thấy hắn truy vấn file `plugins` với tham số item

<img width="1643" height="678" alt="image" src="https://github.com/user-attachments/assets/9f0f0435-5a89-41b1-b4bd-74afff2947ae" />

Q7: sài filter `ip.src == 23.158.56.196 && http.request.method == "POST" && http contains "echo"` để biết hắn đã ghi đè username:password gì

<img width="1207" height="44" alt="image" src="https://github.com/user-attachments/assets/31cd53b9-b47e-48fe-a7f4-de9f86c9aa89" />

Q8: đây là hành vi sửa đổi dữ liệu đã lưu, sợt chị gu gồ là ra đáp án nha

<img width="1867" height="604" alt="image" src="https://github.com/user-attachments/assets/01115d27-db5a-48f8-bb13-658aad11ac4d" />

Q9: cuối cùng dùng filter `ip.src == 23.158.56.196 && http.request.method == "POST" && http contains "echo"`

<img width="820" height="37" alt="image" src="https://github.com/user-attachments/assets/9813c96b-f0ac-4e6a-a6da-1b00d78c2c2e" />

lệnh này có nghĩa như sau:

```
docker run \
  --rm \          # Tự xóa container sau khi thoát
  -it \           # Interactive + TTY
  -v /:/host \    # Mount TOÀN BỘ filesystem "/" của host vào "/host" trong container
  ubuntu \        # Image Ubuntu
  chroot /host    # Chuyển root directory sang /host → trở thành root của host
```

