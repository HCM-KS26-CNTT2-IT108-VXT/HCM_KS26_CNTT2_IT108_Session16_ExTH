# BÀI TẬP TỔNG HỢP

## NGÀY ĐẦU THỰC TẬP DEV TẠI CÔNG TY CÔNG NGHỆ

## PHẦN A -- Thiết lập Profile và Bookmark chuẩn Dev

### 1. Vì sao không nên dùng chung một Chrome Profile?

Nếu dùng chung một Chrome Profile cho GitHub cá nhân và GitHub công ty,
Cookie và phiên đăng nhập có thể bị dùng chung. Khi đăng nhập hai tài
khoản trên cùng dịch vụ, phiên có thể bị ghi đè hoặc chuyển qua lại, gây
nhầm tài khoản và ảnh hưởng dữ liệu làm việc.

### 2. Mô hình 3 Profile chuẩn Developer

  -----------------------------------------------------------------------
  Profile                 Tên đặt                 Mục đích sử dụng
  ----------------------- ----------------------- -----------------------
  1                       Personal                Dùng mạng xã hội, email
                                                  và GitHub cá nhân.

  2                       Company_Work            Dùng GitHub công ty,
                                                  email công ty và các
                                                  công cụ làm việc nội
                                                  bộ.

  3                       Sandbox_Test            Dùng kiểm thử website,
                                                  đăng nhập tài khoản thử
                                                  nghiệm và cài tiện ích
                                                  phục vụ debug.
  -----------------------------------------------------------------------

### 3. Cấu trúc Bookmark gồm 3 thư mục

  -----------------------------------------------------------------------
  Thư mục                             Trang web nên lưu
  ----------------------------------- -----------------------------------
  Work                                GitHub công ty; trang quản lý công
                                      việc nội bộ.

  Dev Tools                           Chrome DevTools documentation; JSON
                                      Formatter/JSON Viewer.

  Learning                            MDN Web Docs; W3Schools.
  -----------------------------------------------------------------------

### 4. Guest Mode (Chế độ khách)

Cho đồng nghiệp dùng Guest Mode để tra cứu nhanh. Chế độ này tạo phiên
duyệt tạm thời, không truy cập dữ liệu, lịch sử và tài khoản đã lưu
trong Profile cá nhân. Sau khi đóng cửa sổ khách, dữ liệu phiên sẽ bị
xóa.

## PHẦN B -- Chọn đúng Extension hỗ trợ

### 1. Chọn nhóm Extension

Chọn nhóm Dev Suite và cài tiện ích JSON Viewer (hoặc JSON Formatter).
Tiện ích giúp định dạng JSON thô thành cấu trúc dễ đọc, có thụt dòng và
phân cấp dữ liệu. Productivity Suite chủ yếu hỗ trợ năng suất làm việc;
Security Suite tập trung bảo mật, nên không phù hợp trực tiếp với nhu
cầu đọc JSON.

### 2. Chọn Site Access

Chọn **On click**. Tiện ích chỉ được cấp quyền khi mình chủ động bấm
vào, hạn chế quyền truy cập website không cần thiết và phù hợp nguyên
tắc cấp quyền tối thiểu (Least Privilege).

## PHẦN C -- Phân tích luồng HTTP Request

### 1. Bốn giai đoạn xử lý khi truy cập test.internal-tool.com

1.  **DNS Lookup:** Trình duyệt phân giải tên miền thành địa chỉ IP.
2.  **Kết nối:** Thiết lập kết nối TCP; nếu dùng HTTPS thì thực hiện
    thêm TLS Handshake.
3.  **HTTP Request/Response:** Trình duyệt gửi HTTP Request đến máy chủ
    và nhận HTTP Response.
4.  **Browser Rendering:** Trình duyệt xử lý HTML, CSS, JavaScript và
    hiển thị trang web.

### 2. So sánh mã trạng thái HTTP

  -----------------------------------------------------------------------
  Mã trạng thái     Ý nghĩa           Lỗi nằm ở đâu?    Hướng debug ban
                                                        đầu
  ----------------- ----------------- ----------------- -----------------
  **404 Not Found** Không tìm thấy    Thường ở URL,     Kiểm tra URL,
                    tài nguyên được   route hoặc tài    endpoint, đường
                    yêu cầu.          nguyên phía       dẫn và route có
                                      client/yêu cầu.   chính xác không.

  **500 Internal    Máy chủ gặp lỗi   Thường ở phía     Kiểm tra log
  Server Error**    khi xử lý         Server/Backend.   server, xử lý
                    request.                            API, dữ liệu đầu
                                                        vào và truy vấn
                                                        cơ sở dữ liệu.
  -----------------------------------------------------------------------

### 3. Công dụng các tab trong DevTools

Mở tab **Network** để xem mã trạng thái HTTP. Tab này hiển thị từng
request, response và Status Code.

-   **Elements:** Xem/sửa cấu trúc HTML và CSS.
-   **Console:** Hiển thị log và lỗi JavaScript.
-   **Application:** Quản lý Cookie, Local Storage và dữ liệu lưu trên
    trình duyệt.

## PHẦN D -- Chẩn đoán Cache và Cookies

### 1. Website vẫn hiển thị giao diện cũ dù đã F5

**Nguyên nhân:** Trình duyệt vẫn tải file CSS cũ từ Cache nên F5 thông
thường chưa lấy phiên bản mới.

**Cách khắc phục:** - Nhấn `Ctrl + Shift + R` (Hard Reload). - Hoặc mở
DevTools, nhấn giữ/nhấp chuột phải nút Reload và chọn **Empty Cache and
Hard Reload**.

### 2. Rủi ro khi Cookie session_id không bật HttpOnly

Cookie `session_id` không bật `HttpOnly` có nguy cơ bị JavaScript đọc
được. Nếu website có lỗ hổng Cross-Site Scripting (XSS), mã độc có thể
lấy Cookie phiên đăng nhập và chiếm quyền phiên (Session Hijacking).

### 3. Chỉ muốn đăng xuất mà giữ Cache

Vào **DevTools \> Application \> Storage \> Cookies \> chọn domain**,
xóa Cookie `session_id` (hoặc Cookie phiên đăng nhập tương ứng). Không
chọn **Clear site data** hay xóa toàn bộ Browsing Data.

## PHẦN E -- Debug bằng Inspect Element và Network Tab

### 1. Kiểm tra và thay đổi màu nút "Đăng nhập"

Nhấp chuột phải nút **"Đăng nhập" \> Inspect**. Trong tab **Elements**,
chọn đúng phần tử và tìm thuộc tính CSS `color` hoặc `background-color`
ở khung **Styles**. Thử thay giá trị màu trực tiếp tại đó để kiểm chứng.

Đây chỉ là thay đổi tạm thời trên trình duyệt, không sửa file CSS gốc.

### 2. Quy trình 4 bước Debugging với request API màu đỏ

1.  **Identify:** Chọn request màu đỏ trong Network, xác định URL,
    Method và Status Code.
2.  **Inspect:** Kiểm tra Headers, Payload/Request và Response để tìm
    thông tin bất thường.
3.  **Diagnose:** Đối chiếu mã lỗi và dữ liệu trả về để xác định khả
    năng lỗi ở URL, request, quyền truy cập hay server.
4.  **Report/Verify:** Ghi lại request, mã trạng thái, thông tin lỗi và
    báo mentor; sau khi Dev xử lý thì gửi lại request để kiểm tra.

### 3. Lọc request API trong Network

Trên tab **Network**, chọn bộ lọc **Fetch/XHR** để chỉ xem các lời gọi
API và bật **Disable cache** để không sử dụng dữ liệu trong bộ nhớ đệm.
DevTools cần đang mở để Disable cache có hiệu lực.
