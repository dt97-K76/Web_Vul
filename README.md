# Vulnerability

Sau khi phát hiện và khai thác được một trong các lỗ hỏng, những hành động nên làm tiếp theo nhằm tối đa sức công phá, khai thác được nhiều tài nguyên hơn để phục vụ quá trình tiếp theo.

## SQL Injection 
	
- Trích xuất chi tiết phiên bản cơ sở dữ liệu để thực hiện khai thác có chủ đích.
- Liệt kê các cấu trúc cơ sở dữ kiệu và bảng.
- Tập trung vào các bảng lưu trữ thông tin xác thực người dùng và chi tiết thanh toán.
- Đọc/ghi hệ thống tệp tin (Optional: Writing webshell to PHP/ASP/JSP webapp)
- Kiểm tra khả năng thực thi lệnh của hệ điều hành thông qua môi trường SQL.
	
## Command Injection 
	
- Kiểm tra giới hạn của các lệnh injection (e.g., kí tự bị hạn chế).
- Truy xuất dữ liệu hệ thống (OS version, users, network configuration).
- Download hoặc upload tệp tin lên hệ thống bị xâm nhập.
- Thiết lập reverse/bind shell để mở rộng truy cập.
	
## Broken Authentication 
	
- Liệt kê vai trò của người dùng.
- Tự động hóa các nổ lực bỏ qua hoặc vô hiệu hóa cơ chế xác thực.
- Hijack sessions của người dùng đặc quyền.
- Khám phá hệ thống sâu hơn hoặc truy cập dữ liệu sau khi xác thực.
	
## Misconfigured Cloud Resources 
	
- Quét các nhóm S3 đang mở hoặc cơ sở dữ liệu bị định cấu hình sai.
- Tải xuống dữ liệu hoặc cấu hình nhạy cảm được tìm thấy trong các tài nguyên bị lộ.
- Cố gắng leo thang đặc quyền bằng cách thao tác cấu hình dịch vụ cloud.
- Khai thác tích hợp dịch vụ để di chuyển ngang trong môi trường cloud.
	
## Insecure Deserialization 
	
- Tạo các đối tượng được tuần tự hóa độc hại cho ngôn ngữ/khuôn khổ cụ thể.
- Kiểm tra tải trọng để thực thi mã hoặc bỏ qua ứng dụng.
- Sử dụng lỗ hổng bảo mật để bỏ qua xác thực hoặc đạt được leo thang đặc quyền.
- Trích xuất hoặc thao tác dữ liệu ứng dụng bằng cách sử dụng tải trọng được giải tuần tự hóa.
	
## SSRF 
	
- Xác định tên miền nội bộ hoặc dải IP.
- Thăm dò các dịch vụ nội bộ (ví dụ: cơ sở dữ liệu, máy chủ lưu trữ).
- Cố gắng lọc dữ liệu từ các điểm cuối nội bộ.
- Sử dụng lỗ hổng để vượt qua các biện pháp kiểm soát truy cập dựa trên IP.
	
## LFI 
	
- Tập trung vào việc đọc các tệp quan trọng (ví dụ: /etc/passwd, cấu hình ứng dụng).
- Cố gắng bao gồm các tệp có thể thực thi mã (ví dụ: phiên PHP).
- Tìm kiếm các tập tin sao lưu hoặc tạm thời có thể chứa dữ liệu nhạy cảm.
- Tận dụng LFI để leo thang lên RCE, nếu có thể.
	
## RFI
	
- Nguồn tải trọng bên ngoài từ các miền được kiểm soát.
- Giới thiệu các tập lệnh thiết lập cửa sau hoặc shell.
- Giám sát nguồn RFI để lọc dữ liệu.
- Sử dụng RFI để xâm phạm sâu hơn các hệ thống mạng nội bộ.
	
## Insecured APIs 
	
- Ghi lại tất cả các điểm cuối và phương thức API (GET, POST, PUT, DELETE).
- Kiểm tra điểm cuối xem có rò rỉ dữ liệu hoặc hành động trái phép không.
- Cố gắng vượt qua giới hạn tốc độ API hoặc kiểm tra xác thực.
- Khai thác API để thao tác trạng thái hoặc dữ liệu ứng dụng.
	
## IDOR 
	
- Ánh xạ luồng cấu trúc dữ liệu và đối tượng của ứng dụng.
- Liệt kê các đối tượng có thể truy cập cho các mục tiêu có giá trị cao (ví dụ: dữ liệu quản trị viên).
- Sửa đổi dữ liệu để kiểm tra tính toàn vẹn của logic ứng dụng.
- Cố gắng nâng cao đặc quyền hoặc quyền truy cập bằng cách thao túng các tham chiếu đối tượng.
	
## XXE 
	
- Thiết lập `blind XXE shell` nếu có thể, lọc các chia sẻ SMB thông qua ứng dụng XXE ngoài băng tần, làm mờ và liệt kê để phát hiện dữ liệu nhạy cảm thông qua các thông báo lỗi, chuyển sang máy chủ mạng nội bộ.
  
## XSS 
	
- Phát hiện tính tồn tại của lỗ hỏng XSS (stored vs. reflected).
- Đưa tải trọng để báo hiệu đến các miền bên ngoài.
- Cố gắng đánh cắp cookie hoặc dữ liệu lưu trữ cục bộ.
- Nâng cao đặc quyền trong ứng dụng bằng cách sử dụng các tập lệnh được chèn.
