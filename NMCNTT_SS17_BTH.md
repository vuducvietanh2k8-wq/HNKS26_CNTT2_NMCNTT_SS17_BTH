BÀI THỰC HÀNH: THIẾT LẬP MÔI TRƯỜNG DEVELOPER VÀ DEBUG TRÊN CHROME
Phần A – Thiết lập Profile và Bookmark chuẩn Dev
1. Nguy cơ khi dùng chung một Profile
Nếu sử dụng chung một Chrome Profile cho GitHub cá nhân và GitHub công ty, trình duyệt sẽ dùng chung vùng lưu trữ dữ liệu, đặc biệt là Cookies.
- Cookies lưu thông tin phiên đăng nhập (Session).
- Khi đăng nhập nhiều tài khoản trên cùng website GitHub, phiên đăng nhập có thể bị thay thế hoặc gây nhầm lẫn tài khoản.
- History, Bookmarks và Extensions cũng bị dùng chung.
- Có nguy cơ commit hoặc thao tác nhầm bằng tài khoản GitHub cá nhân thay vì tài khoản công ty.
Giải pháp: Tạo các Chrome Profile riêng biệt để phân tách dữ liệu và phiên đăng nhập.
2. Mô hình 3 Profile chuẩn Developer
Profile	Tên đặt	Mục đích sử dụng
1	Personal	Sử dụng mạng xã hội, Gmail cá nhân, GitHub cá nhân và học tập.
2	Company_Work	Làm việc với GitHub công ty, email công ty, Jira, tài liệu và hệ thống nội bộ.
3	Sandbox_Test	Kiểm thử website, debug, thử nghiệm API và cài đặt Extension phục vụ phát triển.

Dữ liệu được phân tách:
- Cookies: Phiên đăng nhập riêng biệt.
- History: Lịch sử truy cập riêng.
- Bookmarks: Dấu trang riêng.
- Extensions: Tiện ích và cấu hình riêng.
Nên sử dụng màu nhận diện:
- Personal: Xanh lá.
- Company_Work: Xanh dương.
- Sandbox_Test: Cam.
3. Thiết kế cấu trúc Bookmark
Sử dụng Ctrl + Shift + O để mở Bookmark Manager.
Bookmarks Bar

01_Development

- GitHub công ty

- Tài liệu MDN Web Docs

02_Productivity

- Google Drive

- Jira

03_Testing_API

- test.internal-tool.com

- JSONPlaceholder (API thử nghiệm)



Cách phân loại:
- 01_Development: Công cụ lập trình và tài liệu kỹ thuật.
- 02_Productivity: Công cụ quản lý công việc và cộng tác.
- 03_Testing_API: Website kiểm thử và tài nguyên API.
4. Khi đồng nghiệp mượn máy 5 phút
Nên sử dụng Guest Profile (Hồ sơ khách).

Lý do:
- Không sử dụng trực tiếp dữ liệu Profile cá nhân.
- Không hiển thị Bookmark và lịch sử duyệt web cá nhân.
- Dữ liệu duyệt web của phiên khách được xóa khi đóng toàn bộ cửa sổ Guest.
Không nên cho đồng nghiệp sử dụng trực tiếp Profile Personal hoặc Company_Work.

Lưu ý: Guest Profile không bảo vệ các dữ liệu đã lưu ngoài trình duyệt. Cần khóa máy hoặc đăng xuất các hệ thống nhạy cảm trước khi bàn giao.

Phần B – Chọn đúng Extension hỗ trợ
1. Chọn nhóm Extension
Lựa chọn: Dev Suite
Với đường link API trả về JSON thô, nên cài Extension:

JSON Formatter (tiện ích định dạng JSON).
Chức năng:
- Định dạng JSON thô thành cấu trúc dễ đọc.
- Hiển thị các cặp Key – Value rõ ràng.
- Hỗ trợ mở rộng và thu gọn các đối tượng JSON.
- Giúp Developer kiểm tra cấu trúc dữ liệu API nhanh hơn.
Vì sao không chọn hai nhóm còn lại?
Nhóm Extension	Mục đích	Vì sao không phù hợp?
Dev Suite	Debug, định dạng JSON, kiểm tra mã nguồn	Phù hợp trực tiếp với yêu cầu.
Productivity Suite	Quản lý công việc, ghi chú, tăng hiệu suất	Không tập trung xử lý dữ liệu JSON.
Security Suite	Kiểm tra quyền riêng tư, bảo mật, chặn theo dõi	Không phải công cụ chính để định dạng JSON.

2. Cấu hình Site Access
Chọn: On click
Lý do:
- Extension chỉ hoạt động khi người dùng chủ động nhấn vào.
- Hạn chế quyền truy cập không cần thiết vào các website.
- Giảm phạm vi dữ liệu mà Extension có thể đọc.
- Tuân thủ nguyên tắc Least Privilege (đặc quyền tối thiểu).
Không nên chọn On all sites nếu Extension không cần hoạt động trên tất cả website.
Phần C – Phân tích luồng HTTP Request
Phần C – Phân tích luồng HTTP Request
1. Bốn giai đoạn xử lý khi truy cập website
Khi nhập test.internal-tool.com và nhấn Enter, quá trình cơ bản gồm:
1. DNS Lookup

Phân giải tên miền thành địa chỉ IP của máy chủ.


2. Establish Connection

Thiết lập kết nối TCP và TLS nếu website sử dụng HTTPS.


3. HTTP Request / Response

Trình duyệt gửi HTTP Request, máy chủ xử lý và trả HTTP Response.


4. Browser Rendering

Trình duyệt xử lý HTML, CSS, JavaScript và hiển thị giao diện.


2. So sánh HTTP 404 và HTTP 500
Mã trạng thái	Ý nghĩa	Lỗi nằm ở đâu?	Hướng debug ban đầu
404 Not Found	Không tìm thấy tài nguyên được yêu cầu.	Đường dẫn URL, endpoint hoặc tài nguyên không tồn tại.	Kiểm tra URL, route, endpoint và tên tài nguyên.
500 Internal Server Error	Máy chủ gặp lỗi nội bộ khi xử lý yêu cầu.	Thường nằm ở Backend hoặc cấu hình máy chủ.	Kiểm tra Server Logs, exception, database và logic Backend.

Ghi nhớ:
- 404: Không tìm thấy tài nguyên.
- 500: Máy chủ gặp lỗi khi xử lý yêu cầu.
3. Tab DevTools dùng để xem mã trạng thái HTTP
Chọn: Network
Tab	Chức năng	Vì sao không chọn?
Elements	Kiểm tra và chỉnh sửa HTML, CSS.	Không chuyên dùng để xem HTTP Status Code.
Console	Xem lỗi JavaScript và thông báo hệ thống.	Có thể hiện lỗi mạng nhưng không phải nơi xem đầy đủ thông tin Response.
Network	Theo dõi Request, Response, Headers, Status Code.	Đây là tab phù hợp.
Application	Kiểm tra Cookies, Local Storage, Session Storage và Cache.	Không chuyên dùng để theo dõi trạng thái HTTP của Request.

Cách xem:
1. Nhấn F12.
2. Chọn tab Network.
3. Tải lại trang.
4. Chọn request cần kiểm tra.
5. Mở Headers.
6. Tìm mục Status Code.

Phần D – Chẩn đoán Cache và Cookies
1. CSS đã sửa nhưng F5 vẫn hiển thị giao diện cũ
Nguyên nhân: Trình duyệt có thể đang sử dụng CSS đã lưu trong HTTP Cache thay vì tải lại phiên bản mới từ máy chủ.

Cách khắc phục:









Cách 1: Hard Reload

Nhấn Ctrl + Shift + R trên Windows để tải lại trang và yêu cầu làm mới tài nguyên.











Cách 2: Disable Cache

Mở DevTools → Network → tích Disable cache → tải lại trang khi DevTools vẫn đang mở.



Không cần chỉnh sửa file CSS gốc của dự án. Đây chỉ là thao tác kiểm chứng trên trình duyệt.
2. Cookie session_id không có thuộc tính HttpOnly
Rủi ro bảo mật
Nguyên nhân: Cookie không có thuộc tính HttpOnly, nên JavaScript chạy trên trang có thể truy cập Cookie thông qua document.cookie.

Nguy cơ:
- Nếu website tồn tại lỗ hổng Cross-Site Scripting (XSS), kẻ tấn công có thể lợi dụng JavaScript độc hại để đọc Cookie.
- Nếu Cookie chứa mã phiên đăng nhập, mã này có thể bị đánh cắp.
- Kẻ tấn công có thể sử dụng Session ID bị đánh cắp để chiếm quyền phiên đăng nhập (Session Hijacking).
Giải pháp: Máy chủ cần thiết lập:
Set-Cookie: session_id=...; HttpOnly; Secure; SameSite=Lax


Trong đó:
- HttpOnly: Ngăn JavaScript đọc Cookie.
- Secure: Chỉ gửi Cookie qua kết nối HTTPS.
- SameSite: Hạn chế Cookie được gửi trong các yêu cầu liên trang.
3. Đăng xuất nhưng giữ nguyên Cache
Không sử dụng chức năng Clear all Browsing Data, vì thao tác này có thể xóa cả Cache, Cookies và những dữ liệu khác.

Cách phù hợp:
1. Nhấn nút Logout trên website để máy chủ kết thúc phiên đăng nhập.



Nếu cần xóa Cookie thủ công, mở DevTools → Application.



Chọn Storage → Cookies → tên miền website.



Tìm Cookie session_id.



Xóa riêng Cookie này
Phần E – Debug bằng Inspect Element và Network Tab
1. Thử sửa màu nút Đăng nhập trực tiếp trên trình duyệt
Sử dụng Inspect Element để chỉnh CSS tạm thời.
Các bước thực hiện:
1. Mở trang test.internal-tool.com.
2. Nhấn F12 hoặc Ctrl + Shift + I.
3. Chọn công cụ Select Element (biểu tượng mũi tên).
4. Nhấn vào nút Đăng nhập.
5. Chuyển sang tab Elements.
6. Trong bảng Styles, tìm thuộc tính background-color.
7. Thử thay đổi mã màu, ví dụ:
Quan sát kết quả trực tiếp trên trình duyệt.



Ghi lại mã màu đúng để báo lại Developer.
2. Quy trình 4 bước Debugging khi API Request hiện màu đỏ
Bước 1
Identify – Xác định lỗi

Mở Network, tìm dòng Request màu đỏ và xác định API nào đang gặp sự cố.



Bước 2
Inspect – Kiểm tra chi tiết

Chọn Request và kiểm tra:
- Request URL.
- Request Method (GET, POST, PUT, DELETE).
- Status Code.
- Request Headers và Payload.
- Response.



Bước 3
Analyze – Phân tích nguyên nhân

Đối chiếu mã HTTP:
- 400: Request không hợp lệ.
- 401: Chưa xác thực.
- 403: Không có quyền truy cập.
- 404: Không tìm thấy tài nguyên.
- 500: Lỗi máy chủ.
Kiểm tra thêm lỗi JavaScript trong Console.



Bước 4
Fix & Verify – Khắc phục và xác minh

Thực hiện biện pháp kiểm chứng phù hợp, gửi lại Request hoặc tải lại trang. Ghi nhận kết quả trước và sau khi xử lý để báo cáo mentor.
3. Hai thao tác cần bật trong Network
Yêu cầu	Thao tác
Chỉ xem API Call	Chọn bộ lọc Fetch/XHR trong Network.
Không lấy dữ liệu từ Cache	Tích chọn Disable cache khi DevTools đang mở.

Giải thích:
- Fetch/XHR: Lọc các yêu cầu API được gửi thông qua Fetch API hoặc XMLHttpRequest, loại bỏ phần lớn ảnh, CSS và các tài nguyên tĩnh khỏi danh sách.
- Disable cache: Ngăn trình duyệt sử dụng HTTP Cache trong khi DevTools đang mở.
Lưu ý: Disable cache không đồng nghĩa với xóa Cache đã lưu. Nó chỉ vô hiệu hóa việc sử dụng HTTP Cache trong phiên kiểm tra đó.