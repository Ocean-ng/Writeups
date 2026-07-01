# Danh sách các Sự kiện ID của Sysmon (Sysmon Event IDs)

## Sự kiện ID 1: Tạo tiến trình (Process creation)
Sự kiện tạo tiến trình cung cấp thông tin mở rộng về một tiến trình mới được tạo. Dòng lệnh đầy đủ cung cấp ngữ cảnh về việc thực thi tiến trình. Trường `ProcessGUID` là một giá trị duy nhất cho tiến trình này trên toàn bộ miền (domain) để giúp việc tương quan sự kiện dễ dàng hơn. Mã băm (hash) là mã băm đầy đủ của tệp với các thuật toán trong trường `HashType`.

## Sự kiện ID 2: Một tiến trình đã thay đổi thời gian tạo tệp (A process changed a file creation time)
Sự kiện thay đổi thời gian tạo tệp được ghi lại khi thời gian tạo tệp bị một tiến trình sửa đổi một cách rõ ràng. Sự kiện này giúp theo dõi thời gian tạo thực sự của một tệp. Những kẻ tấn công có thể thay đổi thời gian tạo tệp của một backdoor để làm cho nó trông giống như được cài đặt cùng với hệ điều hành. Lưu ý rằng nhiều tiến trình thay đổi thời gian tạo tệp một cách hợp pháp; điều đó không nhất thiết chỉ ra hoạt động độc hại.

## Sự kiện ID 3: Kết nối mạng (Network connection)
Sự kiện kết nối mạng ghi nhật ký các kết nối TCP/UDP trên máy. Nó bị vô hiệu hóa theo mặc định. Mỗi kết nối được liên kết với một tiến trình thông qua các trường `ProcessId` và `ProcessGuid`. Sự kiện cũng chứa tên máy chủ nguồn và đích, địa chỉ IP, số cổng và trạng thái IPv6.

## Sự kiện ID 4: Trạng thái dịch vụ Sysmon thay đổi (Sysmon service state changed)
Sự kiện thay đổi trạng thái dịch vụ báo cáo trạng thái của dịch vụ Sysmon (đã bắt đầu hoặc đã dừng).

## Sự kiện ID 5: Tiến trình đã kết thúc (Process terminated)
Sự kiện kết thúc tiến trình báo cáo khi một tiến trình kết thúc. Nó cung cấp `UtcTime` (Thời gian UTC), `ProcessGuid` và `ProcessId` của tiến trình.

## Sự kiện ID 6: Đã tải driver (Driver loaded)
Sự kiện đã tải driver cung cấp thông tin về một driver đang được tải trên hệ thống. Các mã băm được cấu hình sẽ được cung cấp cũng như thông tin chữ ký. Chữ ký được tạo không đồng bộ vì lý do hiệu suất và cho biết liệu tệp có bị xóa sau khi tải hay không.

## Sự kiện ID 7: Đã tải Image/Mô-đun (Image loaded)
Sự kiện đã tải image ghi nhật ký khi một mô-đun được tải trong một tiến trình cụ thể. Sự kiện này bị vô hiệu hóa theo mặc định và cần được cấu hình với tùy chọn `-l`. Nó chỉ ra tiến trình mà mô-đun được tải vào, các mã băm và thông tin chữ ký. Chữ ký được tạo không đồng bộ vì lý do hiệu suất và cho biết liệu tệp có bị xóa sau khi tải hay không. Sự kiện này nên được cấu hình cẩn thận, vì việc giám sát tất cả các sự kiện tải image sẽ tạo ra một lượng lớn nhật ký (log).

## Sự kiện ID 8: Tạo luồng từ xa (CreateRemoteThread)
Sự kiện CreateRemoteThread phát hiện khi một tiến trình tạo một luồng (thread) trong một tiến trình khác. Kỹ thuật này được phần mềm độc hại (malware) sử dụng để tiêm mã (inject code) và ẩn náu trong các tiến trình khác. Sự kiện chỉ ra tiến trình nguồn và tiến trình đích. Nó cung cấp thông tin về mã sẽ được chạy trong luồng mới: `StartAddress`, `StartModule` và `StartFunction`. Lưu ý rằng các trường `StartModule` và `StartFunction` được suy luận, chúng có thể trống nếu địa chỉ bắt đầu nằm ngoài các mô-đun đã tải hoặc các hàm được xuất (exported functions) đã biết.

## Sự kiện ID 9: Đọc truy cập thô (RawAccessRead)
Sự kiện RawAccessRead phát hiện khi một tiến trình tiến hành các thao tác đọc từ ổ đĩa bằng cách sử dụng ký hiệu `\\.\`. Kỹ thuật này thường được phần mềm độc hại sử dụng để lấy cắp dữ liệu của các tệp bị khóa đọc, cũng như để tránh các công cụ kiểm toán truy cập tệp. Sự kiện chỉ ra tiến trình nguồn và thiết bị đích.

## Sự kiện ID 10: Truy cập tiến trình (ProcessAccess)
Sự kiện truy cập tiến trình báo cáo khi một tiến trình mở một tiến trình khác, một thao tác thường được theo sau bởi các truy vấn thông tin hoặc đọc và ghi không gian địa chỉ của tiến trình đích. Điều này cho phép phát hiện các công cụ hack đọc nội dung bộ nhớ của các tiến trình như Cơ quan bảo mật cục bộ (Lsass.exe) nhằm đánh cắp thông tin xác thực để sử dụng trong các cuộc tấn công Pass-the-Hash. Việc bật sự kiện này có thể tạo ra lượng lớn nhật ký nếu có các tiện ích chẩn đoán đang hoạt động liên tục mở các tiến trình để truy vấn trạng thái của chúng, vì vậy nó thường chỉ nên được thực hiện với các bộ lọc loại bỏ các truy cập dự kiến.

## Sự kiện ID 11: Tạo tệp (FileCreate)
Thao tác tạo tệp được ghi nhật ký khi một tệp được tạo hoặc bị ghi đè. Sự kiện này hữu ích cho việc giám sát các vị trí tự khởi động (autostart), như thư mục Startup, cũng như các thư mục tạm thời và thư mục tải xuống, những nơi phổ biến mà phần mềm độc hại thường thả tệp vào trong quá trình lây nhiễm ban đầu.

## Sự kiện ID 12: Sự kiện Registry - Tạo và xóa đối tượng (RegistryEvent - Object create and delete)
Các thao tác tạo và xóa khóa (key) và giá trị (value) của Registry được ánh xạ tới loại sự kiện này, có thể hữu ích cho việc giám sát các thay đổi đối với các vị trí tự khởi động trong Registry hoặc các sửa đổi Registry cụ thể của phần mềm độc hại.

Sysmon sử dụng các phiên bản viết tắt của tên khóa gốc Registry, với các ánh xạ sau:

| Tên khóa (Key name) | Viết tắt (Abbreviation) |
|---|---|
| `HKEY_LOCAL_MACHINE` | `HKLM` |
| `HKEY_USERS` | `HKU` |
| `HKEY_LOCAL_MACHINE\System\ControlSet00x` | `HKLM\System\CurrentControlSet` |
| `HKEY_LOCAL_MACHINE\Classes` | `HKCR` |

## Sự kiện ID 13: Sự kiện Registry - Đặt giá trị (RegistryEvent - Value Set)
Loại sự kiện Registry này xác định các sửa đổi giá trị Registry. Sự kiện ghi lại giá trị được ghi cho các giá trị Registry thuộc loại `DWORD` và `QWORD`.

## Sự kiện ID 14: Sự kiện Registry - Đổi tên khóa và giá trị (RegistryEvent - Key and Value Rename)
Các thao tác đổi tên khóa và giá trị Registry được ánh xạ tới loại sự kiện này, ghi lại tên mới của khóa hoặc giá trị đã được đổi tên.

## Sự kiện ID 15: Mã băm luồng tạo tệp (FileCreateStreamHash)
Sự kiện này ghi nhật ký khi một luồng tệp được đặt tên (named file stream) được tạo và nó tạo ra các sự kiện ghi lại mã băm của nội dung tệp mà luồng được gán cho (luồng không tên), cũng như nội dung của luồng được đặt tên. Có các biến thể phần mềm độc hại thả các tệp thực thi hoặc cài đặt cấu hình của chúng thông qua các bản tải xuống của trình duyệt và sự kiện này nhằm mục đích nắm bắt điều đó dựa trên việc trình duyệt đính kèm một luồng "mark of the web" (dấu hiệu của web) `Zone.Identifier`.

## Sự kiện ID 16: Thay đổi cấu hình dịch vụ (ServiceConfigurationChange)
Sự kiện này ghi lại các thay đổi trong cấu hình Sysmon - ví dụ: khi các quy tắc lọc được cập nhật.

## Sự kiện ID 17: Sự kiện Pipe - Pipe được tạo (PipeEvent - Pipe Created)
Sự kiện này được tạo khi một named pipe (đường ống có tên) được tạo. Phần mềm độc hại thường sử dụng named pipe để giao tiếp giữa các tiến trình (interprocess communication).

## Sự kiện ID 18: Sự kiện Pipe - Pipe đã kết nối (PipeEvent - Pipe Connected)
Sự kiện này ghi nhật ký khi một kết nối named pipe được thực hiện giữa client (máy khách) và server (máy chủ).

## Sự kiện ID 19: Sự kiện WMI - Phát hiện hoạt động WmiEventFilter (WmiEvent - WmiEventFilter activity detected)
Khi một bộ lọc sự kiện WMI được đăng ký, đây là một phương pháp được phần mềm độc hại sử dụng để thực thi, sự kiện này sẽ ghi lại không gian tên WMI, tên bộ lọc và biểu thức bộ lọc.

## Sự kiện ID 20: Sự kiện WMI - Phát hiện hoạt động WmiEventConsumer (WmiEvent - WmiEventConsumer activity detected)
Sự kiện này ghi lại việc đăng ký của các đối tượng tiêu thụ (consumer) WMI, ghi lại tên consumer, nhật ký và đích đến.

## Sự kiện ID 21: Sự kiện WMI - Phát hiện hoạt động WmiEventConsumerToFilter (WmiEvent - WmiEventConsumerToFilter activity detected)
Khi một consumer liên kết với một bộ lọc, sự kiện này sẽ ghi lại tên consumer và đường dẫn bộ lọc.

## Sự kiện ID 22: Sự kiện DNS - Truy vấn DNS (DNSEvent - DNS query)
Sự kiện này được tạo khi một tiến trình thực thi một truy vấn DNS, cho dù kết quả thành công hay thất bại, có được lưu vào bộ nhớ cache hay không. Đo lường từ xa (telemetry) cho sự kiện này đã được thêm vào cho Windows 8.1, do đó nó không có sẵn trên Windows 7 và các phiên bản trước đó.

## Sự kiện ID 23: Xóa tệp - Đã lưu trữ tệp bị xóa (FileDelete - File Delete archived)
Một tệp đã bị xóa. Ngoài việc ghi nhật ký sự kiện, tệp bị xóa cũng được lưu trong `ArchiveDirectory` (mặc định là `C:\Sysmon`). Trong điều kiện hoạt động bình thường, thư mục này có thể phát triển đến một kích thước bất hợp lý - xem sự kiện ID 26: `FileDeleteDetected` cho hành vi tương tự nhưng không lưu các tệp bị xóa.

## Sự kiện ID 24: Thay đổi khay nhớ tạm - Nội dung mới trong khay nhớ tạm (ClipboardChange - New content in the clipboard)
Sự kiện này được tạo khi nội dung khay nhớ tạm (clipboard) của hệ thống thay đổi.

## Sự kiện ID 25: Can thiệp tiến trình - Thay đổi hình ảnh tiến trình (ProcessTampering - Process image change)
Sự kiện này được tạo ra khi các kỹ thuật ẩn tiến trình như "hollow" hoặc "herpaderp" bị phát hiện.

## Sự kiện ID 26: Phát hiện xóa tệp - Đã ghi nhật ký xóa tệp (FileDeleteDetected - File Delete logged)
Một tệp đã bị xóa.

## Sự kiện ID 27: Chặn tệp thực thi (FileBlockExecutable)
Sự kiện này được tạo khi Sysmon phát hiện và chặn việc tạo các tệp thực thi (định dạng PE).

## Sự kiện ID 28: Chặn hủy tệp (FileBlockShredding)
Sự kiện này được tạo khi Sysmon phát hiện và chặn việc hủy tệp (file shredding) từ các công cụ như SDelete.

## Sự kiện ID 29: Phát hiện tệp thực thi (FileExecutableDetected)
Sự kiện này được tạo khi Sysmon phát hiện việc tạo một tệp thực thi mới (định dạng PE).

## Sự kiện ID 255: Lỗi (Error)
Sự kiện này được tạo ra khi có lỗi xảy ra trong Sysmon. Chúng có thể xảy ra nếu hệ thống đang chịu tải nặng và một số tác vụ không thể được thực hiện hoặc có lỗi (bug) tồn tại trong dịch vụ Sysmon, hoặc thậm chí nếu một số điều kiện bảo mật và tính toàn vẹn không được đáp ứng. Bạn có thể báo cáo bất kỳ lỗi nào trên diễn đàn Sysinternals.
