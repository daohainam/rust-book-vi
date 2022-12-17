## Lưu trữ văn bản được mã hóa UTF-8 bằng chuỗi (String) {#storing-utf-8-encoded-text-with-strings}

Chúng ta đã nói về chuỗi trong Chương 4, nhưng bây giờ chúng ta sẽ xem xét chúng sâu hơn. Những tín đồ mới của Rust thường bị mắc kẹt khi làm việc với string vì ba lý do: Rust có xu hướng tìm ra các lỗi có khả năng xảy ra, chuỗi là một cấu trúc dữ liệu phức tạp hơn nhiều lập trình viên nghĩ và thứ ba là về UTF-8. Những yếu tố này kết hợp theo cách có vẻ khó khăn khi bạn hiểu theo cách các ngôn ngữ lập trình khác.

Chúng ta sẽ thảo luận về string theo khía cạnh các tập hợp bởi vì string được triển khai dưới dạng tập hợp các byte, được thêm vào một số phương thức để cung cấp chức năng hữu ích khi các byte đó được diễn dịch thành văn bản. Trong phần này, chúng ta sẽ nói về các thao tác trên `String` mà mọi loại tập hợp đều có, chẳng hạn như tạo, cập nhật và truy xuất. Chúng ta cũng sẽ thảo luận về những cách mà một `String` khác với các tập hợp khác, cụ thể là cách lập chỉ mục index trong `String` là phức tạp bởi sự khác biệt giữa cách con người và máy tính diễn giải dữ liệu `String`.

### String là gì?

Trước tiên, chúng ta sẽ xác định những gì chúng ta hiểu về khái niệm *string*. Rust chỉ có một kiểu chuỗi trong phần lõi của ngôn ngữ, đó là đoạn chuỗi `str` thường được thấy ở dạng mượn tham chiếu `&str`. Trong Chương 4, chúng ta đã nói về *string slices*, là các tham chiếu đến một số dữ liệu chuỗi mã hóa UTF-8 được lưu trữ ở một nơi nào đó. Ví dụ: các chuỗi ký tự được lưu trữ cố định trong tập tin nhị phân của chương trình và do đó được gọi là các đoạn chuỗi (string slices).

Loại `String`, được cung cấp bởi thư viện tiêu chuẩn của Rust thay vì được lập trình như là thành phần lõi của ngôn ngữ, là loại chuỗi được mã hóa UTF-8 có thể tăng kích cỡ, có thể thay đổi, có thể sở hữu. Khi các tín đồ Rust đề cập đến “strings” trong Rust, họ có thể đang đề cập đến cả 2 loại `String` hoặc `&str`, không chỉ là một loại cụ thể trong 2 loại đó. Mặc dù phần này chủ yếu nói về `String`, nhưng cả hai loại đều được sử dụng nhiều trong thư viện tiêu chuẩn của Rust và cả `String` và `&str` đều được mã hóa UTF-8.

### Tạo mới một String

Nhiều thao tác tương tự có sẵn trong `Vec<T>` cũng có sẵn với `String`, bởi vì `String` thực sự bọc lại một vector kiểu byte và có bổ sung thêm sự đảm bảo, sự hạn chế và khả năng lưu trữ. Ví dụ về một hàm hoạt động theo cùng một cách với `Vec<T>` và `String` là hàm `new` để tạo một đối tượng mới, được thể hiện trong mục 8-11.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-11/src/main.rs:here}}
```

<span class="caption">Mục 8-11: Tạo mới một `String` rỗng.</span>

Dòng này tạo một mới một chuỗi rỗng có tên là `s`, sau đó chúng ta có thể tải dữ liệu vào. Thông thường, chúng ta sẽ có dữ liệu khởi tạo mà chúng ta muốn bắt đầu. Để làm điều này, chúng ta sử dụng phương thức `to_string`, có sẵn trên bất kỳ kiểu nào triển khai đối tượng đặc tả `Display`, như các ký tự chuỗi thực hiện. Mục 8-12 cho thấy hai ví dụ.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-12/src/main.rs:here}}
```

<span class="caption">Mục 8-12: Dùng phương thức `to_string` để tạo mới `String` từ một chuỗi ký tự</span>

Đoạn mã này tạo một string chứa `initial contents`.

Chúng ta cũng có thể dùng hàm `String::from` để tạo một `String` từ chuỗi ký tự. Đoạn code ở mục 8-13 tương đương với đoạn code của mục 8-12 sử dụng `to_string`.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-13/src/main.rs:here}}
```

<span class="caption">Mục 8-13: Dùng hàm `String::from` để tạo một `String` từ một chuỗi ký tự</span>

Bởi vì chuỗi được sử dụng cho rất nhiều thứ, chúng ta có thể sử dụng nhiều API chung khác nhau cho chuỗi, cung cấp cho chúng ta rất nhiều tùy chọn. Một số trong đó có vẻ thừa, nhưng chúng đều có vị trí của chúng! Trong trường hợp này, `String::from` và `to_string` làm tương tự nhau, vì vậy bạn chọn cái nào là do phong cách và tính dễ đọc của chương trình.

Hãy nhớ rằng chuỗi cơ bản được mã hóa bằng UTF-8, vì vậy chúng có thể bao gồm bất kỳ dữ liệu nào được mã hóa đúng cách, như được hiển thị trong mục 8-14.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-14/src/main.rs:here}}
```

<span class="caption">Mục 8-14: Lưu các lời chào bằng nhiều ngôn ngữ khác nhau trong chuỗi</span>

Tất cả đều là `String` hợp lệ.

### Cập nhật một String

Một `String` có khả năng tăng kích thước và nội dung của nó có thể thay đổi, nó giống như nội dung của `Vec<T>` nếu bạn đưa nhiều dữ liệu hơn vào đó. Ngoài ra, bạn có thể sử dụng toán tử `+` hoặc macro `format!` để nối các giá trị `String` một cách thuận tiện.

#### Thêm vào cuối (append) chuỗi bằng `push_str` và `push`

Chúng ta có thể tăng kích thước một `String` bằng cách sử dụng phương thức `push_str` để nối thêm một đoạn chuỗi, như được hiển thị trong mục 8-15.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-15/src/main.rs:here}}
```

<span class="caption">Mục 8-15: Nối chuỗi vào một `String` sử dụng phương thức `push_str`</span>

Sau hai dòng này, `s` sẽ chứa `foobar`. Phương thức `push_str` có tham số là một đoạn chuỗi (`&str`) vì chúng ta không nhất thiết chiếm quyền sở hữu tham số đó. Ví dụ, trong mã trong mục 8-16, chúng ta muốn có thể sử dụng lại `s2` sau khi thêm nội dung của nó vào `s1`.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-16/src/main.rs:here}}
```

<span class="caption">Mục 8-16: Sử dụng lại một đoạn chuỗi sau khi thêm nội dung của nó vào một `String` khác</span>

Nếu phương thức `push_str` chiếm quyền sở hữu của `s2`, chúng ta sẽ không thể in giá trị của nó tại dòng cuối. Tuy nhiên, đoạn mã này hoạt động như mong đợi!

Phương thức `push` nhận ký tự đơn làm tham số và thêm nó vào `String`. Mục 8-17 thêm ký tự “l” vào một `String` bằng phương thức `push`

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-17/src/main.rs:here}}
```

<span class="caption">Mục 8-17: Thêm một ký tự vào giá trị `String` bằng cách dùng phương thức `push`</span>

Kết quả là, `s` sẽ chứa `lol`.

#### Nối chuỗi với toán tử `+` hoặc macro `format!` {#concatenation-with-the--operator-or-the-format-macro}

Thông thường, bạn sẽ muốn kết hợp hai chuỗi hiện có. Có một cách để làm điều này là dùng toán tử `+`, được thể hiện ở mục 8-18

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-18/src/main.rs:here}}
```

<span class="caption">Mục 8-18: dùng toán tử `+` để kết hợp hai chuỗi thành một chuỗi mới</span>

Chuỗi `s3` sẽ chứa `Hello, world!`. Lý do `s1` không còn hợp lệ nữa sau khi thêm và lý do chúng ta sử dụng tham chiếu đến `s2` có liên quan đến phương thức bên dưới thực sự của toán tử `+`. Toán tử `+` sử dụng phương thức `add`, có cách biểu diễn trông giống như sau:

```rust,ignore
fn add(self, s: &str) -> String {
```

Trong thư viện chuẩn, bạn sẽ thấy `add` được định nghĩa bằng cách sử dụng generic và các loại liên quan. Ở đây, chúng ta đã thay thế bằng các loại cụ thể, đó là điều xảy ra khi chúng ta gọi phương thức này bằng các giá trị `String`. Chúng ta sẽ thảo luận về generic trong Chương 10. Thông qua phương thức bên dưới này, sẽ cho chúng ta những manh mối để hiểu về những điểm phức tạp của toán tử `+`.

Đầu tiên, `s2` có dấu `&`, nghĩa là chúng ta đang thêm một *tham chiếu* của chuỗi thứ hai vào chuỗi đầu tiên. Điều này là do tham số `s` trong hàm `add`: chúng ta chỉ có thể thêm một kiểu `&str` vào một `String`; chúng ta không thể thêm hai giá trị `String` lại với nhau. Nhưng chờ đã - kiểu của `&s2` là `&String`, không phải là `&str` như được chỉ định trong tham số thứ hai của `add`. Vậy tại sao đoạn code của mục 8-18 biên dịch được?

Lý do chúng ta có thể sử dụng `&s2` trong lệnh gọi tới `add` là trình biên dịch có thể *ép kiểu* đối số `&String` thành một `&str`. Khi chúng ta gọi phương thức `add`, Rust sử dụng một *ép kiểu tự động*, mà cụ thể ở đây sẽ biến`&s2` thành `&s2[..]`. Chúng ta sẽ thảo luận sâu hơn về ép kiểu tự động trong Chương 15. Bởi vì `add` không chiếm quyền sở hữu tham số `s` nên `s2` sẽ vẫn là một `String` hợp lệ sau thao tác này.

Thứ hai, chúng ta có thể thấy trong phương thức đại diện rằng, `add` chiếm quyền sở hữu `self` bởi vì `self` *không* có dấu `&`. Điều này có nghĩa là quyền sở hữu của `s1` trong mục 8-18 sẽ được chuyển vào lệnh gọi `add` và sẽ không còn hợp lệ sau lời gọi đó nữa. Vì vậy, mặc dù `let s3 = s1 + & s2;` có vẻ như nó sẽ sao chép cả hai chuỗi và tạo một chuỗi mới, nhưng câu lệnh này thực sự chiếm quyền sở hữu của biến `s1`, thêm vào cuối nội dung bản sao của `s2` và sau đó trả về tham chiếu quyền sở hữu của kết quả. Nói cách khác, có vẻ như nó đang tạo ra rất nhiều bản sao nhưng không phải vậy; việc thực hiện hiệu quả hơn sao chép.

Nếu chúng ta cần nối nhiều chuỗi, hành vi của toán tử `+` sẽ khó sử dụng:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/no-listing-01-concat-multiple-strings/src/main.rs:here}}
```

Lúc này, `s` sẽ là `tic-tac-toe`. Với các ký tự `+` và `"`, rất khó để biết điều gì đang xảy ra. Thay vào đó, để kết hợp những chuỗi phức tạp hơn, chúng ta có thể sử dụng macro `format!`:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/no-listing-02-format/src/main.rs:here}}
```

Mã này cũng thiết lập `s` thành `tic-tac-toe`. Macro `format!` hoạt động giống như `println!`, nhưng thay vì in kết quả ra màn hình, nó trả về một `String` kèm với nội dung. Phiên bản của đoạn mã sử dụng `format!` dễ đọc hơn nhiều và mã được tạo bởi macro `format!` sử dụng các tham chiếu để lệnh gọi này không chiếm quyền sở hữu bất kỳ tham số nào của nó.

### Chỉ mục index đến String

Trong nhiều ngôn ngữ lập trình khác, truy cập đến các ký tự riêng lẻ trong một chuỗi bằng cách tham chiếu theo chỉ mục là một thao tác hợp lệ và phổ biến. Tuy nhiên, nếu bạn cố truy cập các phần của `String` bằng cú pháp chỉ mục index trong Rust, bạn sẽ gặp lỗi. Xem xét mã không hợp lệ trong mục 8-19.

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-19/src/main.rs:here}}
```

<span class="caption">Mục 8-19: Cố dùng cú pháp index trong String</span>

Đoạn mã này sẽ trả về kết quả lỗi sau:

```console
{{#include ../listings/ch08-common-collections/listing-08-19/output.txt}}
```

Lỗi và ghi chú về việc lỗi này là: chuỗi trong Rust không hỗ trợ chỉ mục index. Nhưng tại sao không? Để trả lời câu hỏi đó, chúng ta cần thảo luận về cách Rust lưu trữ các chuỗi trong bộ nhớ.

#### Mô tả cách hoạt động bên trong

`String` là một kiểu bọc lại kiểu `Vec<u8>`. Hãy xem xét một số chuỗi UTF-8 được mã hóa đúng cách từ mục 8-14. Đầu tiên là dòng này:

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-14/src/main.rs:spanish}}
```

Trong trường hợp này, `len` sẽ là 4, có nghĩa là vector lưu trữ chuỗi “Hola” dài 4 byte. Mỗi chữ cái này chiếm 1 byte khi được mã hóa bằng UTF-8. Tuy nhiên, dòng sau đây có thể làm bạn ngạc nhiên. (Lưu ý rằng chuỗi này bắt đầu bằng chữ cái Cyrillic viết hoa Ze, không phải số 3 trong tiếng Ả Rập)

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-14/src/main.rs:russian}}
```

Khi được hỏi chuỗi dài bao nhiêu, bạn có thể trả lời là 12. Thực tế, câu trả lời của Rust là 24: đó là số byte cần thiết để mã hóa “Здравствуйте” trong UTF-8, bởi vì mỗi giá trị vô hướng Unicode trong chuỗi đó chiếm 2 byte dung lượng lưu trữ . Do đó, một chỉ mục index trỏ đến các byte của chuỗi sẽ không phải lúc nào cũng tương ứng với một giá trị vô hướng Unicode hợp lệ. Để chứng minh, hãy xem xét mã Rust không hợp lệ này:

```rust,ignore,does_not_compile
let hello = "Здравствуйте";
let answer = &hello[0];
```

Bạn biết rằng `câu trả lời` của chữ cái đầu tiên sẽ không phải là `З`. Khi được mã hóa bằng UTF-8, byte đầu tiên của `З` là `208` và byte thứ hai là `151`, vì vậy, có vẻ như `câu trả lời` trên thực tế phải là `208`, nhưng `208` không phải là ký tự hợp lệ theo riêng nó. Trả về `208` có thể không phải là những gì người dùng muốn nếu họ yêu cầu ký tự đầu tiên của chuỗi này; tuy nhiên, đó là dữ liệu duy nhất mà Rust có ở chỉ mục byte số 0. Người dùng thường không muốn giá trị byte được trả về, ngay cả khi chuỗi chỉ chứa các chữ cái Latinh: nếu `&"hello"[0]` là mã hợp lệ trả về giá trị byte, nó sẽ trả về `104`, không phải `h`.

Vì vậy, để tránh trả về một giá trị không mong muốn và gây ra các lỗi mà có thể không được phát hiện ngay lập tức, câu trả lời là: Rust không biên dịch mã này gì hết và ngăn chặn sự hiểu lầm sớm trong quá trình phát triển.

#### Byte, giá trị vô hướng (scalar) và các cụm ký tự kết hợp (grapheme)! Ôi trời!

Một điểm khác về UTF-8 là trên thực tế có ba cách liên quan để xem xét chuỗi từ quan điểm của Rust: dưới dạng byte, giá trị vô hướng (scalar) và cụm ký tự kết hợp (grapheme) (thứ gần nhất với những gì chúng ta gọi là *chữ cái*).

Nếu chúng ta nhìn vào từ tiếng Hindi “नमस्ते” được viết bằng chữ viết Devanagari, nó được lưu trữ dưới dạng vector của các giá trị `u8` trông giống như sau:

```text
[224, 164, 168, 224, 164, 174, 224, 164, 184, 224, 165, 141, 224, 164, 164,
224, 165, 135]
```

Đó là 18 byte và là cách máy tính cuối cùng lưu trữ dữ liệu này. Nếu chúng ta xem chúng dưới dạng giá trị vô hướng Unicode, là kiểu `char` của Rust, thì những byte đó trông như thế này:

```text
['न', 'म', 'स', '्', 'त', 'े']
```

Có sáu giá trị `char` ở đây, nhưng giá trị thứ tư và thứ sáu không phải là chữ cái thực sự: chúng là những dấu phụ không có ý nghĩa nếu đứng riêng. Cuối cùng, nếu chúng ta xem chúng dưới dạng các cụm ký tự kết hợp (grapheme), chúng ta sẽ hiểu một người sẽ gọi bốn chữ cái tạo nên từ tiếng Hindi:

```text
["न", "म", "स्", "ते"]
```

Rust cung cấp những cách khác nhau để diễn giải dữ liệu chuỗi thô mà máy tính lưu trữ để mỗi chương trình có thể chọn cách diễn giải mà nó cần, bất kể dữ liệu đó bằng ngôn ngữ con người nào.

Lý do cuối cùng là Rust không cho phép chúng ta lập chỉ mục cho `String` để truy cập một ký tự là, các hoạt động lập chỉ mục được xem là luôn mất chi phí thời gian không đổi (O(1)). Nhưng không thể đảm bảo hiệu suất đó bằng `String`, vì Rust sẽ phải lặp qua hết các nội dung từ đầu chuỗi đến vị trí index chỉ định để xác định xem có bao nhiêu ký tự hợp lệ.

### Cắt chuỗi

Lập chỉ mục cho một chuỗi thường là một ý tưởng tồi vì nó không biết rõ kiểu trả về của thao tác lấy giá trị theo index trong chuỗi là gì: giá trị byte, ký tự, một cụm grapheme hay một đoạn chuỗi. Do đó, nếu bạn thực sự cần sử dụng các chỉ số để tạo các đoạn chuỗi, Rust yêu cầu bạn phải cụ thể hơn nữa.

Thay vì lập chỉ mục bằng cách sử dụng `[]` với một số đơn lẻ, bạn có thể sử dụng `[]` với một dãy phạm vi để tạo một đoạn chuỗi chứa các byte cụ thể:

```rust
let hello = "Здравствуйте";

let s = &hello[0..4];
```

Ở đây, `s` sẽ là một `&str` chứa 4 byte đầu tiên của chuỗi.
Trước đó, chúng tôi đã đề cập rằng mỗi ký tự này là 2 byte, có nghĩa là `s` sẽ là `Зд`.

Nếu chúng ta cố thử cắt chuỗi chỉ một phần byte của một ký tự, đại loại một thứ gì đó như `&hello[0..1]`, Rust sẽ gây lỗi lúc chạy giống như trường hợp dùng chỉ mục index không hợp lệ truy cập phần tử trong một vector:

```console
{{#include ../listings/ch08-common-collections/output-only-01-not-char-boundary/output.txt}}
```

Bạn nên sử dụng các dãy phạm vi để tạo các đoạn chuỗi một cách thận trọng, vì làm như vậy có thể làm hỏng chương trình của bạn.

### Cách duyệt qua các phần tử của chuỗi

Cách tốt nhất để thao tác trên các phần của chuỗi là phải rõ ràng về việc bạn mong muốn: kiểu ký tự hay kiểu byte. Đối với các giá trị vô hướng Unicode riêng lẻ, hãy sử dụng phương thức `chars`. Việc gọi `chars` trên “Зд” sẽ tách chuỗi ra và trả về hai giá trị kiểu `char` và bạn có thể lặp qua kết quả này để truy cập từng phần tử:

```rust
for c in "Зд".chars() {
    println!("{c}");
}
```

Đoạn mã này sẽ in ra như sau:

```text
З
д
```

Ngoài ra, phương thức `bytes` trả về từng byte thô, có thể phù hợp với lĩnh vực của bạn:

```rust
for b in "Зд".bytes() {
    println!("{b}");
}
```

Đoạn mã này sẽ in ra bốn byte đã tạo nên chuỗi này:

```text
208
151
208
180
```

Nhưng hãy nhớ rằng các giá trị vô hướng Unicode hợp lệ có thể được tạo thành từ nhiều hơn 1 byte.

Việc lấy các cụm ký tự kết hợp (grapheme) từ chuỗi giống như với chữ viết Devanagari rất phức tạp, vì vậy chức năng này không cung cấp sẵn trong thư viện chuẩn. Có nhiều thư viện có sẵn trên [crates.io](https://crates.io/)<!-- ignore --> nếu bạn cần những chức năng này.

### Kiểu chuỗi không đơn giản như vậy

Tóm lại, kiểu chuỗi rất phức tạp. Các ngôn ngữ lập trình khác nhau tạo ra các lựa chọn khác nhau về cách thể hiện sự phức tạp cho lập trình viên. Rust đã chọn việc xử lý chính xác dữ liệu `String` làm hành vi mặc định cho tất cả các chương trình Rust, điều đó có nghĩa là các lập trình viên phải suy nghĩ nhiều hơn về việc xử lý dữ liệu UTF-8 từ trước. Sự đánh đổi này cho thấy kiểu chuỗi có tính phức tạp hơn các ngôn ngữ lập trình khác, nhưng nó lại giúp bạn khỏi phải xử lý các lỗi liên quan đến các ký tự không phải ASCII sau này trong vòng đời phát triển của mình.

Tin tốt là thư viện chuẩn cung cấp rất nhiều chức năng được xây dựng dựa trên các loại `String` và `&str` để giúp xử lý các tình huống phức tạp này một cách chính xác. Nhớ xem tài liệu để biết các phương thức hữu ích như `contains` để tìm kiếm trong một chuỗi và `replace` để thay thế các phần của một chuỗi bằng một chuỗi khác.

Hãy chuyển sang một thứ ít phức tạp hơn một chút: bản đồ băm!