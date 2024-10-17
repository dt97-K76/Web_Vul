## Example

### Example 1

```
$sql = "SELECT username FROM users WHERE username='$username' AND password='$password'";
$query = $database->query($sql);
```

C1: Truyền dữ liệu `admin' -- ` vào biến username.

C2: Truyền dữ liệu `admin' or 1 = '2`.

Giair thichs: toán tử AND sẽ được ưu tiên thực hiện trước toán tử OR. Nên ta có logic của câu truy vấn như sau: TRUE OR (FALSE AND FALSE) --> TRUE OR FALSE = TRUE --> login thành công.

 ### Example 2

 ```
$sql = "SELECT username FROM users WHERE username=\"$username\" AND password=\"$password\"";
$query = $database->query($sql);
```
