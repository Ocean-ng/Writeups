<img width="1134" height="1040" alt="image" src="https://github.com/user-attachments/assets/c9066c77-900f-42bc-ab35-a2d4e3ab3e3e" />

trên hình là script malware keylogger, mình dùng AI để nó giải thích code cũng như là viết script để decrypt ra flag

sau đây là các bước để decrypt nhe

<img width="1060" height="256" alt="image" src="https://github.com/user-attachments/assets/5e391f72-b75f-4860-b81e-dfe146043a16" />


dùng script này để decode nhé (cài thư viện python `scapy`)
```
from scapy.all import rdpcap
from scapy.layers.http import HTTPRequest
import base64

# Khóa bí mật mà chúng ta phân tích được từ mã độc
SECRET_KEY = b"H0t3lSt@ff0NlyK3epS3cr3t!"

def xor_decrypt(data_bytes: bytes, key_bytes: bytes) -> bytes:
    """Hàm giải mã bằng phép toán XOR từng byte một"""
    return bytes(b ^ key_bytes[i % len(key_bytes)] for i, b in enumerate(data_bytes))

def extract_and_decode(pcap_filename):
    print(f"[*] Đang đọc file: {pcap_filename}...")
    try:
        packets = rdpcap(pcap_filename)
    except FileNotFoundError:
        print("[-] Không tìm thấy file pcap. Vui lòng kiểm tra lại đường dẫn!")
        return
    
    extracted_text = ""
    
    print("[*] Bắt đầu trích xuất và giải mã các gói tin...")
    for pkt in packets:
        # Chỉ quan tâm đến các gói tin có chứa HTTP Request
        if pkt.haslayer(HTTPRequest):
            http_layer = pkt[HTTPRequest]
            
            # Kiểm tra đúng User-Agent của Keylogger
            if http_layer.User_Agent and b"ByteLotusClient/1.1" in http_layer.User_Agent:
                cookie_header = http_layer.Cookie
                
                if cookie_header and b"hotel_sess_state=" in cookie_header:
                    # Tách lấy chuỗi Base64 ra khỏi Cookie
                    # Ví dụ: b'hotel_sess_state=SGVsbG8=; other_cookie=123' -> lấy 'SGVsbG8='
                    b64_str = cookie_header.split(b"hotel_sess_state=")[1].split(b";")[0]
                    
                    try:
                        # Bước 1: Decode Base64 thành các byte mã hóa
                        encrypted_bytes = base64.b64decode(b64_str)
                        
                        # Bước 2: Decode XOR với khóa bí mật
                        decrypted_bytes = xor_decrypt(encrypted_bytes, SECRET_KEY)
                        
                        # Bước 3: Ghép ký tự vừa giải mã vào chuỗi kết quả
                        extracted_text += decrypted_bytes.decode('utf-8')
                    except Exception as e:
                        # Bỏ qua nếu có gói tin bị lỗi định dạng
                        pass
                        
    print("-" * 40)
    print("[+] KẾT QUẢ GIẢI MÃ TOÀN BỘ PHÍM BẤM:")
    print("-" * 40)
    print(extracted_text)
    print("-" * 40)

if __name__ == "__main__":
    extract_and_decode("traffic.pcapng")
```
kết quả đây nho

<img width="1059" height="350" alt="image" src="https://github.com/user-attachments/assets/0fce8e18-7767-476b-abb5-525dffce5b0c" />

có nghĩa là các gói tin này mỗi gói chứa mỗi kí tự nên dùng script automation lấy hết một lượt đỡ mất thời gian
