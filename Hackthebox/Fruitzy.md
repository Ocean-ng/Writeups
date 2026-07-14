okay trong trường hợp này là bạn của CEO nhờ ông ấy phân tích file mã độc mà bản thân anh ta đã tải xuống, qua đó CEO nhờ mình điều tra con malware này

mình sẽ dùng PhishTool để phân tích mail được gửi chi tiết hơn, phần header của mail là `Special Party Invitation from JANET CARNAHAN`

<img width="2079" height="979" alt="image" src="https://github.com/user-attachments/assets/76d71f6a-5677-4133-9597-abba4fdc316e" />

sang phần URLS, ta tìm được 1 đường link nhưng đây là link của cloudflare, có thể thay đổi mỗi lần nên ta phải dùng tool `urlscan.io` để tìm url cuối

<img width="1138" height="124" alt="image" src="https://github.com/user-attachments/assets/8e4f0773-0c04-411b-909f-43e3c60970ea" />

ở đây thì những link redirect không thể hiện được thư mục của web thật, ta tìm những link liên quan mà người khác đã scan rồi

<img width="2036" height="1086" alt="image" src="https://github.com/user-attachments/assets/143a3c2c-c3cb-4426-b423-7e75afd1054a" />

qua đó tìm được đường link tới thư mục thật của web 

Thêm 1 cách khác nữa là tìm trong history của browser nạn nhân nhé

<img width="614" height="58" alt="image" src="https://github.com/user-attachments/assets/e2f297ce-d148-44dc-93e0-e9418af2e23d" />

<img width="2206" height="1056" alt="image" src="https://github.com/user-attachments/assets/be23e49e-c54f-4cc8-8d67-ec3aea5438be" />


để mà biết là nạn nhân đã tải file mã độc gì về thì mình sẽ chú ý đế `History` của các trình duyệt mà CyberJunkie đang sử dụng

<img width="1006" height="510" alt="image" src="https://github.com/user-attachments/assets/3b8348c0-0a86-4f1d-9f50-fab597e8baef" />

export về và dùng tool browser sqlite, anh ta đã tải `premium.exe` về máy

<img width="2533" height="422" alt="image" src="https://github.com/user-attachments/assets/8ccd1fd4-1b16-4fd2-b4d6-f7131d049740" />

muốn biết file này thực thi lần đầu khi nào thì ta xem Amcache.hve ở chỗ `last write timestamp`

<img width="685" height="86" alt="image" src="https://github.com/user-attachments/assets/33d0945f-989d-4db0-8638-337f64cbf349" />

tiếp theo là chú ý dòng `Fileid` đây là mã SHA1 của premium.exe trừ đi 4 số 0 đầu

<img width="540" height="19" alt="image" src="https://github.com/user-attachments/assets/9a6a07c6-7a65-4464-9367-4ec6162b21c7" />

mình chỉ cần đưa mã SHA1 này lên virustotal thì sẽ tìm được SHA256 do là đây là một con malware nên người ta sẽ up lên để scan

<img width="2001" height="1127" alt="image" src="https://github.com/user-attachments/assets/9983e372-33d7-4b3d-b59d-da3c400847f6" />

windows defender quét file này lần đầu vào win event của windows để biết thời điểm

<img width="1717" height="536" alt="image" src="https://github.com/user-attachments/assets/5846ad43-c765-4952-98ef-987dde464620" />

xem ID 1000 là khi windows defender bắt đầu scan file, nhưng lưu ý là khi tìm được thời gian bắt đầu quét rồi thì phải nhìn lại giờ của máy ảo, ta sẽ chuyển sang tab xml view để có một múi giờ đúng nhất cho flag

<img width="932" height="817" alt="image" src="https://github.com/user-attachments/assets/a553cfc4-7a73-4ed3-b604-4c72cdab7033" />

mã độc này nó cài một phần mềm điều khiển từ xa gọi nôm na là backdoor đóa, vậy thì nó sẽ chạy ngầm mỗi khi mà nạn nhân bật máy tính lên. Cùng với thư mục vừa xem được cuộn xuống là thấy System.evtx

<img width="1728" height="575" alt="image" src="https://github.com/user-attachments/assets/88cc1e88-2bcc-4ce2-aa8c-eda8d962a903" />

thì cũng vào ngày 3 tháng 4 năm 2026 thì có 1 dịch vụ được đăng kí, search gu gồ thì nó cũng cho ra kết quả RMM

<img width="1682" height="140" alt="image" src="https://github.com/user-attachments/assets/73aac6f8-5398-4712-88cd-68a0c57b4e0f" />

<img width="1718" height="373" alt="image" src="https://github.com/user-attachments/assets/78eda882-04f4-412a-9016-0d4cc278b2c0" />

hacker dùng kỹ thuật timestomping để làm file malware nằm ẩn trong những file windows chính thống lâu đời có nghĩa là khi mà thực thi 1 file thì hệ thống ghi lại thời gian đó nhưng hacker can thiệp vào thời gian lùi lại 1 một mức nào đó 

để tìm được thời gian mà hacker đã chình thì mình sẽ dùng file $MFT -> mở powershell và dùng command `MFTECmd.exe -f "đường_dẫn_file_$MFT" --csv "thư_mục_lưu_kết_quả"`, dùng các phần mềm như excel hoặc wps excel để xem file .csv này, search service `CagService.exe`
và thấy được mốc thời gian modified mà hacker đã chỉnh sửa (lưu ý sửa lại form thời gian)

<img width="2452" height="146" alt="image" src="https://github.com/user-attachments/assets/5feb5755-f3ab-4381-956c-7bf35b9e5336" />

sợt chị gu gồ thì công ty sáng lập ra tool RMM này là `Datto`

<img width="1936" height="1070" alt="image" src="https://github.com/user-attachments/assets/6fde2f4a-9469-4e74-aec8-53f0426f934f" />

tên miền mình đã tìm được từ câu hỏi trước đó `pomi.digital`, mình lên trang `lookup.icann.org` để osint xem nó được đăng kí từ ngày tháng năm nào

<img width="1436" height="667" alt="image" src="https://github.com/user-attachments/assets/dfdd9d4c-2f44-4ff8-98d3-285171719487" />

quay lại với virustotal, do đã có mã hash của nó mình dễ dàng tìm được tên gọi khác

<img width="232" height="271" alt="image" src="https://github.com/user-attachments/assets/fa8c4997-7311-4596-aae4-f6cc964c6109" />


trời đất quo cuối cùng xong rùiiii












