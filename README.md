# CNTT1-nh-p-m-n-c-ng-ngh-th-ng-tin-Session18_Ex01

## 1. Phân tích lỗi & Trạng thái DNS

* **Sơ đồ luồng:** Tên miền nhập vào (`Input`) Browser Cache -> OS Cache/Hosts -> DNS Resolver (ISP) -> Root / TLD (`.vn`) / Authoritative DNS -> Địa chỉ IP (`Output`).
* **Sự cố:** Do gõ nhầm thành `grenlog.vn` (thiếu "e"), máy chủ DNS tra cứu danh bạ không tìm thấy bản ghi nào khớp nên trả về lỗi **`NXDOMAIN`** (Domain không tồn tại).
* **Đánh giá khẳng định của Nam:** Nam nói "máy chủ bị sập" là sai. Thực tế kết nối chưa từng tới được máy chủ do sai địa chỉ ngay từ bước phân giải mạng.

---

## 2. Chẩn đoán & Quy trình kiểm tra

* **Phân biệt lỗi:**
* **DNS_PROBE_FINISHED_NXDOMAIN:** Lỗi phía Client/Mạng. Chưa gửi được dữ liệu tới máy chủ.
* **500 Internal Server Error:** Lỗi phía Server. Máy chủ vẫn sống, đã nhận request nhưng code/hệ thống bên trong bị hỏng.


* **Quy trình xác minh:**
1. Mở Terminal/CMD, chạy `nslookup phong-dieu-phoi.greenlog.vn` xem tên miền có trỏ về IP nào không.
2. Bấm **F12** trên trình duyệt chọn tab **Network** $\rightarrow$ tải lại trang. Nếu có mã HTTP (200, 404, 500...) nghĩa là gói tin đã chạm tới máy chủ. Nếu báo `ERR_NAME_NOT_RESOLVED`, kết nối bị ngắt ngay tại Client.



---

## 3. Kiểm thử case 404 Not Found

Nếu gõ đúng tên miền mà nhận mã **`404 Not Found`**:

* **Tình trạng:** Kết nối đã tới đúng máy chủ công ty (DNS và Server đều hoạt động).
* **Nguyên nhân:** Máy chủ không tìm thấy tài nguyên hoặc đường dẫn (URL/route) mà Nam đang truy cập. Có thể Nam gõ sai phần đuôi đường dẫn, hoặc trang quản trị lộ trình đã bị xóa/đổi tên trên hệ thống.
