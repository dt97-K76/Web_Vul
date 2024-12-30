# XXE: XML external entity

- XML: extensible markup language -- được thiết kế với mục đích lưu trữ và truyền dữ liệu và cả người và máy đều có thế đọc được
- External entity
    - DTD (document type definitions): xác định cấu trúc, format hợp lệ của các elements và attributes trong file xml. Có 2 loại internal DTD (nằm trong file xml) và external DTD (nằm ngoài file xml).
    - DTD entity:
        - Internal: `<!ENTITY entity-name "entity-value">`
        - External: `<!ENTITY name SYSTEM "URL/URI">`

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY bar SYSTEM "file:///c:/boot.ini" >]>
<foo>&bar;</foo>
```

- `<!DOCTYPE foo [ ... ]>`: Xác định Document Type Definition (DTD) cho tài liệu XML. Trong đó:
  - <!ELEMENT foo ANY>: Khai báo phần tử foo có thể chứa bất kỳ nội dung nào.
  - <!ENTITY bar SYSTEM "file:///c:/boot.ini">: Tạo một thực thể (entity) tên là bar, liên kết với file trên hệ thống cục bộ (file:///c:/boot.ini).

## Example

