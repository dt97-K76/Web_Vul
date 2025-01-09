# SSTI Server Side template injection

- Ngôn ngữ template bao gồm back-end rendering và front-end rendering:
    - Render trên back-end bao gồm việc dịch các ngôn ngữ template theo một tiêu chuẩn và chuyển chúng thành HTML, JavaScript hoặc CSS, từ đó trả về cho phía front-end.
    - Sau đó, quá trình front-end rendering tiếp nhận, thực thi và gửi toàn bộ mã nguồn trên đến client, cho phép client tạo ra giao diện người dùng.
- Tác dụng: Để thêm, sửa, xóa chức năng, dữ liệu, thay đổi bố cục giao diện mà không cần chỉnh sửa toàn bộ source code.

## Example

### PHP

| Template Name  | Payload Format | 
| :--- | :--- | 
| Laravel Blade | {{ }} |
| Latte | {var $X=""}{$X} | 
| Mustache | {{ }} | 
| Plates | <?= ?> |
| Smarty | { } |
| Twig | {{ }} |

### Python

| Template Name | Payload Format |
| :--- | :--- | 
| Bottle | {{ }} |
| Chameleon | ${ } |
| Cheetah | ${ } |
| Django | {{ }} |
| Jinja2 | {{ }} |
| Mako | ${ } |
| Pystache | {{ }} |
| Tornado | {{ }} |

### JavaScript

| Template Name | 	Payload Format
| :--- | :--- | 
| DotJS 	| {{= }} 
| DustJS 	| {}
| EJS 	| <% %>
| HandlebarsJS 	| {{ }}
| HoganJS 	 | {{ }}
| Lodash 	 | {{= }}
| MustacheJS 	| {{ }}
| NunjucksJS 	| {{ }}
| PugJS 	| #{}
| TwigJS 	| {{ }}
| UnderscoreJS 	| <% %>
| VelocityJS 	| #=set($X="")$X
| VueJS 	| {{ }}






