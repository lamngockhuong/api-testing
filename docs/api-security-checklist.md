# API Security Checklist
Danh sách các lưu ý quan trọng và giải pháp nên sử dụng khi thiết kế, kiểm thử và phát hành API.
---

## Xác thực (Authentication)
- [ ] Không sử dụng `Basic Auth`.
- [ ] Không tự thiết kế lại các giải pháp `Authentication`, `token generation`, `password storage`. Hãy sử dụng các giải pháp tiêu chuẩn.
- [ ] Mã hóa các dữ liệu nhạy cảm.

### JWT (JSON Web Token)
- [ ] Không lưu các thông tin nhạy cảm trong JWT, nó có thể [dễ dàng](https://jwt.io/#debugger-io) được giải mã.

### OAuth
- [ ] Luôn xác nhận `redirect_uri` phía server để chỉ cho phép redirect đến các URL tin cậy.

## Access
- [ ] Giới hạn request (Throttling) để phòng tránh các tấn công DDoS / brute-force.
- [ ] Với private API, chỉ cho phép access từ whitelisted IPs/hosts.

## Input
- [ ] Sử dụng HTTP method phù hợp với từng công việc: `GET (đọc)`, `POST (tạo mới)`, `PUT/PATCH (cập nhật/sửa)`, `DELETE (để xóa bản ghi)`, và phản hồi `405 Method Not Allowed` nếu HTTP method không phù hợp với resource được request.
- [ ] Xác nhận dữ liệu `content-type` được chấp nhận khi gửi lên (chẳng hạn như. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`...).
- [ ] Kiểm tra dữ liệu truyền lên từ người dùng để tránh các lỗ hổng phổ biến (chẳng hạn như `XSS`, `SQL-Injection`, `Remote Code Execution`...).
- [ ] Không sử dụng các dữ liệu nhạy cảm như (`credentials`, `Passwords`, `security tokens`, or `API keys`) tại URL, sử dụng header Authorization để xác thực.

## Processing
- [ ] Đảm bảo rằng các endpoint chỉ xử lý dữ liệu sau khi đã qua bước xác thực.
- [ ] Không nên thiết kế ID dạng tự động tăng. Sử dụng UUID để thay thế.
- [ ] Nếu bạn đang cần xử lý với lượng dữ liệu lớn, sử dụng các kỹ thuật Workers và Queues để xử lý tác vụ ngầm càng nhiều càng tốt và giúp phản hồi nhanh để tránh bị timeout HTTP.
- [ ] Đừng quên tắt chế độ DEBUG.

## Output
- [ ] Loại bỏ các header chứa thông tin nhạy cảm như phiên bản web server, ví dụ: `X-Powered-By`, `Server`, `X-AspNet-Version`, V.v...
- [ ] Bắt buộc có `content-type` trong response headers, nếu bạn trả về `application/json` thì header `content-type` sẽ có giá trị `application/json`.
- [ ] Không trả về client các thông tin nhạy cảm như credentials, Passwords, security tokens.
- [ ] Trả về status code tương ứng với kết quả xử lý (Chẳng hạn: `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`...).
## CI & CD
- [ ] Kiểm tra thiết kế và thực hiện đầy đủ unit/integration test.
- [ ] Áp dụng quy trình review code và bỏ qua việc tự phê duyệt.
- [ ] Đảm bảo các component của service được quét virus trước khi release production (bao gồm các library và dependency)
- [ ] Thiết kế giải pháp rollback cho việc deploy

## Tham khảo
+ https://github.com/shieldfy/API-Security-Checklist