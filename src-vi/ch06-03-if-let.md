## Concise Control Flow with `if let`

Cú pháp `if let` cho phép bạn kết hợp `if` và `let` theo một cách ngắn gọn hơn
để xử lý các giá trị khớp với một pattern nhất định trong khi bỏ qua các
pattern còn lại. Hãy xem xét chương trình trong Listing 6-6 với giá trị được
khớp là kiểu `Option <u8>` trong biến `config_max` nhưng chúng ta chỉ muốn thực
thi code nếu giá trị là biến thể `Some`.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-06/src/main.rs:here}}
```

<span class="caption">Listing 6-6: Một `match` chỉ quan tâm đến việc thực thi
code khi giá trị là `Some`</span>

Nếu giá trị là `Some`, chúng ta sẽ in ra giá trị trong biến thể `Some` bằng
cách gán giá trị cho biến `max` trong pattern. Chúng ta không muốn làm gì với
giá trị `None`. Để thoả mãn biểu thức `match`, chúng ta phải thêm `_ => ()` sau
dù chỉ xử lý một biến thể, điều này thật phiền phức khi thêm vào code.

Thay vào đó, chúng ta có thể viết code ngắn gọn hơn bằng cách sử dụng `if let`.
Code dưới đây sẽ hoạt động tương tự như `match` trong Listing 6-6:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-12-if-let/src/main.rs:here}}
```

Cú pháp `if let` sẽ nhận một pattern và một biểu thức được phân tách bằng dấu
bằng. Nó hoạt động tương như `match`, trong đó biểu thức được gửi vào `match`
và pattern là nhánh đầu tiên của nó. Trong trường hợp này, pattern là `Some(max)`, và `max` được gán cho giá trị bên trong `Some`. Chúng ta sau đó có thể sử
dụng `max` trong thân của code block, `if let` cũng hoạt đọng theo cách tương
tự như chúng ta sử dụng `max` trong nhánh của `match`. Code trong block `if let` sẽ không được chạy nếu giá trị không khớp với pattern.

Sử dụng `if let` sẽ ít phải gõ, ít phải thụt lề, và ít code dài dòng kiểu mẫu
hơn. Tuy nhiên, bạn sẽ không đảm bảo tính kiểm tra đầy đủ như `match`. Lựa chọn
giữa `match` và `if let` phụ thuộc vào bạn đang làm gì trong trường hợp cụ thể
của bạn và liệu có đáng để giảm thiểu code bằng cách bỏ qua việc kiểm tra đầy
đủ hay không.

Nói cách khác, bạn có thể coi `if let` như là cú pháp bổ sung cho một `match`
có thể chạy code khi giá trị khớp với một pattern và sau đó bỏ qua tất cả các
giá trị khác.

Chúng ta có thể thêm một `else` với một `if let`. Block code của `else` sẽ
giống như block code của `_` trong `match`. Nhớ lại định nghĩa enum `Coin`
trong Listing 6-4, trong đó biến thể `Quarter` cũng có một giá trị `UsState`.
Nếu chúng ta muốn đếm tất cả các xu không phải là 25 cent mà chúng ta thấy
trong khi cũng gọi tên tiểu bang của các xu 25 cent, chúng ta có thể làm được
điều đó với một biểu thức `match` như sau:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-13-count-and-announce-match/src/main.rs:here}}
```

Hoặc chúng ta có thể sử dụng một biểu thức `if let` và `else` như sau:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-14-count-and-announce-if-let-else/src/main.rs:here}}
```

Nếu bạn có một trường hợp trong đó chương trình của bạn có logic quá dài dòng
để biểu diễn bằng một `match`, hãy nhớ đến `if let`.

## Summary

Chúng ta đã đi qua cách sử dụng enum để tạo ra một kiểu dữ liệu tùy chỉnh thể
hiện một giá trị có thể có bên trong một tập các giá trị được liệt kê. Chúng ta
đã thấy cách thư viện chuẩn `Option<T>` giúp bạn sử dụng hệ thống kiểu (type
system) để ngăn chặn lỗi. Khi các giá trị enum có dữ liệu bên trong chúng, bạn
có thể sử dụng `match` hoặc `if let` để trích xuất và sử dụng các giá trị đó,
tùy thuộc vào số lượng trường hợp bạn cần xử lý.

Chương trình Rust của bạn có thể biểu diễn các khái niệm trong lĩnh vực của bạn
bằng cách sử dụng struct và enum. Tạo kiểu dữ liệu tùy chỉnh để sử dụng trong
API và đảm bảo an toàn kiểu dữ liệu: trình biên dịch sẽ đảm bảo rằng các hàm
của bạn chỉ nhận được các giá trị của kiểu mà mỗi hàm mong đợi.

Để cung cấp một API được tổ chức tốt cho người dùng, dễ sử dụng và chỉ tiết lộ
chính xác những gì người dùng của bạn sẽ cần, chúng ta sẽ bây giờ chuyển sang
Rust module.