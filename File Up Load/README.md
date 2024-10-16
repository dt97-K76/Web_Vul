## Một số example

### Example 1

```
    $file = $dir . "/" . $_FILES["file"]["name"];
    move_uploaded_file($_FILES["file"]["tmp_name"], $file);
    $success = 'Successfully uploaded file at: <a href="/' . $file . '">/' . $file . ' </a><br>';
```

Không có bất kì kiểm tra nào, cho phép upload file thỏa mái.

### Example 2

```
    $filename = $_FILES["file"]["name"];
    $extension = explode(".", $filename)[1];
    if ($extension === "php") {
        die("detected");
    }
    $file = $dir . "/" . $filename;
    move_uploaded_file($_FILES["file"]["tmp_name"], $file);
    $success = 'Successfully uploaded file at: <a href="/' . $file . '">/' . $file . ' </a><br>';
```

Tương tự ex 1 nhưng ở ex 2 có sự chỉnh sửa nhằm phát hiện extension có thể tác động gây hại `php` bằng cách sử dụng câu lệnh `$extension = explode(".", $filename)[1];` để lọc extension từ `$filename`. 

Hàm explode() sẽ chia chuỗi $filename thành một mảng dựa trên ký tự "." (dấu chấm). Mỗi phần của chuỗi sau khi tách bởi dấu chấm sẽ trở thành một phần tử trong mảng.

[1]: Sau khi chuỗi được tách thành mảng, phần tử ở vị trí thứ 1 (chỉ số bắt đầu từ 0) của mảng sẽ được truy xuất. Điều này thường là phần mở rộng của tên file, ví dụ: "file.txt" -> explode(".", "file.txt")[1] sẽ trả về "txt".

Vấn đề ở đây là nếu $filename="file.txt.php" thì kết quả của câu lệnh `$extension = explode(".", $filename)[1];` là $extension = "txt". Như vậy chương trình kiểm tra extension `php` là vô hiệu hóa.

### Example 3

```
    $filename = $_FILES["file"]["name"];
    $extension = end(explode(".", $filename));
    if ($extension === "php") {
        die("detected");
        }
```

Để khắc phục vấn đề ở ex 2 thì ở tình huống này thay vì lấy chỉ số [1] của kết quả hàm explode() sẽ lấy phần tử cuối cùng sau dấu chấm tránh trường hợp filename chứa nhiều dấu chấm (.) không thể kiểm tả được extension `php`.

Hãy suy nghĩ theo hướng khác để tìm xem liệu `mod-php` còn xử lý tệp tin nào ngoài `php`. Tìm hiểu phát hiện thêm 2 file mà `mod-php` có thể xử lý `.phar, .phtml`.

### Example 4

```
    $filename = $_FILES["file"]["name"];
    $extension = end(explode(".", $filename));
    if (in_array($extension, ["php", "phtml", "phar"])) {
        die("detected");
    }
```

Ở ví dụ này, không còn kiểm tra mỗi `php` nữa mà là một blacklist `php,phtml,phar`. Kiểm tra các extension mà `mod php` sẽ xử lý ngăn chặn việc up load file.

Kiểm tra quyền upload file `.htaccess`. Nếu được hãy upload một tập tin .htaccess để tuỳ ý set handler cho filename có extension tuỳ ý.

    AddType application/x-httpdphp .txt 

Điều này cho phép chạy file .txt như code php.

### Example 5

```
    $mime_type = $_FILES["file"]["type"];
    if (!in_array($mime_type, ["image/jpeg", "image/png", "image/gif"])) {
        die("detected");
    }
```

Khác với ví dụ trước, ở ví dụ này dùng whitelist thay vì blacklist. Lần này không kiểm tra extension nữa mà chuyển qua kiểm tra Content-Type. Nhưng vì Content-Type là một header trong HTTP request nên ta có thể dễ dàng thay đổi giá trị của nó bằng Burp Suite.

### Example 6

```
    $finfo = finfo_open(FILEINFO_MIME_TYPE);
    $mime_type = finfo_file($finfo, $_FILES['file']['tmp_name']);
    $whitelist = array("image/jpeg", "image/png", "image/gif");
    if (!in_array($mime_type, $whitelist, TRUE)) {
        die("detected");
    }
```

Để khắc phục lỗ hỏng ở ex trước, lần này không kiểm tra Content-Type, mà sẽ trực tiếp kiểm tra nội dung của tệp tin được upload.

Các loại file khác nhau sẽ được xác định bằng một vài byte đầu tiên của file, gọi là file signature (chữ ký đầu tệp).

![image](https://github.com/user-attachments/assets/7ece03d3-36a8-468a-9190-49335ee8adab)

`finfo_file` sẽ so sánh chữ ký đầu tệp của các file trong `magic database` để đưa ra kết luận là tập tin gì. Magic database là nơi chứa tất cả chữ ký đầu tệp của các file tương ứng. Server lấy file signature bằng finfo_file và kiểm tra với whitelist("image/jpeg", "image/png", "image/gif").





