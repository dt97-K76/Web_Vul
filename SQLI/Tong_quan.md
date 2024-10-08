# SQL injection

Đây là lỗ hỏng bảo mật web cho phép kẻ tấn công can thiệp vào các truy vấn đối với cơ sở dữ liệu của nó. Điều này cho phép kẻ tấn công truy xuất, thay đổi, tác động đến dữ liệu ẩn.

## SQLi UNION attack**: kết hợp truy vấn

- Xác định số cột trả về từ truy vấn ban đầu.
    - Sắp xếp kết quả theo các cột: ORDER BY 1--.
    - Gửi truy vấn: UNION SELECT NULL, NULL--.
- Kiểu dữ liệu của các cột được trả về từ truy vấn ban đầu tương ứng để chứa kết quả từ truy vấn được chèn.

Oracle - nối chuỗi ||.

Tìm version:

| Database type | Query |
| --- | --- |
| Microsoft, MySQL | `SELECT @@version` |
| Oracle | `SELECT * FROM v$version` |
| PostgreSQL | `SELECT version()` |

Tìm tên bảng: table_name:

`SELECT * FROM information_schema.tables`

Tìm tên cột: column_name:

`SELECT * FROM information_schema.columns WHERE table_name = 'Users'`

Blind SQLi: lỗi SQLi nhưng phản hồi không chứa kết quả truy vấn liên quan hoặc chi tiết.

Exploit:

- triggering conditional responses (kích hoạt phản ứng có điều kiện)
