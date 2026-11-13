Q1: lúc đầu mình cũng bị lừa bởi mấy file PDF nhưng mà thật ra file cần chú ý chính là .rar

<img width="727" height="541" alt="image" src="https://github.com/user-attachments/assets/439cc765-e52c-4422-866a-06b28535818d" />

có nghĩa là nó sẽ giải nén theo link của hình trên, nằm trong mục StartUp

Q2: Alternate Data Streams là cho thêm luồng bên cạnh data của file, sài `dir /r` để xem ADS

Q3,4: 

<img width="1350" height="400" alt="image" src="https://github.com/user-attachments/assets/2974b834-86a3-4836-b73b-fff4ee94772c" />


Q5: theo hình ở Q1, thì `EdgeUpdate.lnk` là fake 

<img width="2115" height="1053" alt="image" src="https://github.com/user-attachments/assets/34619bcb-1ff2-4ddc-acef-8f0fc2ff1f50" />

Q6: legit `C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe`

Q7,8: XOR 0x5A

<img width="1642" height="73" alt="image" src="https://github.com/user-attachments/assets/1330fc19-0888-40a7-a394-23288f4e267a" />

nhờ con chatgpt giải mã ra url

<img width="1120" height="430" alt="image" src="https://github.com/user-attachments/assets/151a2809-c84a-4514-b684-657a3eda8af0" />

Q9: không chỉ xem trong powershell mà còn trong powershell operator

<img width="1770" height="850" alt="image" src="https://github.com/user-attachments/assets/a6aa96c8-7ed4-49be-b098-92a56f329788" />


Q10: quay lại câu 7,8

<img width="1117" height="144" alt="image" src="https://github.com/user-attachments/assets/4cfe6193-4f14-4836-a45b-ae740ae7ad97" />

Q11: kỹ thuật Squiblydoo: lạm dụng regsvr32.exe để tải và chạy scriptlet .sct từ xa qua scrobj.dll (MITRE ATT&CK T1218.010)

Q12: url của payload được tải xuống tại các file dưới đây

<img width="1098" height="307" alt="image" src="https://github.com/user-attachments/assets/fedea5d2-d4f2-4f14-a612-edbd26b53d54" />

Q13: tìm thấy mã sha1 thông qua 2026-08-14T10_35_43_4774408_CopyLog.csv, dựa vào mã file tìm `C\Users\someone\AppData\LocalLow\Microsoft\CryptnetUrlCache\Content\71344B6AF035A29EF8737533CE450492`

<img width="1191" height="169" alt="image" src="https://github.com/user-attachments/assets/f9dc4135-c441-4628-a47d-939309e58425" />

Q14: đưa file dô dịch ngược nè, mình sẽ cho chatgpt xem thì nó bảo là được nén bằng UPX 4.2.4 nên ta sẽ sài command `upx -d -o .\msedgeupdate.unpacked.dll .\71344B6AF035A29EF8737533CE450492` để giải nén ra file có khóa XOR "0xDEAD1899"

Q15: cũng trong file .dll mới được nén đó `102.157.64.59:30404`

Q16: 

<img width="1117" height="186" alt="image" src="https://github.com/user-attachments/assets/cdc71087-76ae-4b57-ae52-b30fe96c9182" />







