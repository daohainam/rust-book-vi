## Recoverable Errors with `Result`

Hầu hết các lỗi không đủ nghiêm trọng để yêu cầu chương trình dừng hoàn toàn.
Đôi khi, khi một hàm thất bại, nó là vì một lý do mà bạn có thể dễ dàng diễn
giải và phản ứng. Ví dụ, nếu bạn cố gắng mở một tệp và hoạt động thất bại bởi
vì tệp không tồn tại, bạn có thể muốn tạo tệp thay vì kết thúc quá trình.

Nhắc lại từ [“Handling Potential Failure with the `Result`
Type”][handle_failure]<!-- ignore --> trong Chương 2 rằng `Result` enum được
định nghĩa là có hai biến thể, `Ok` và `Err`, như sau:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

`T` và `E` là các tham số kiểu generic: chúng ta sẽ thảo luận về generic kỹ
hơn trong Chương 10. Bạn cần biết ngay bây giờ là `T` biểu diễn kiểu của giá
trị sẽ được trả về trong trường hợp thành công trong biến thể `Ok`, và `E`
biểu diễn kiểu của lỗi sẽ được trả về trong trường hợp thất bại trong biến
thể `Err`. Bởi vì `Result` có các tham số kiểu generic này, chúng ta có thể sử
dụng kiểu `Result` và các hàm được định nghĩa trên nó trong nhiều tình huống
khác nhau mà giá trị thành công và giá trị lỗi mà chúng ta muốn trả về có thể
khác nhau.

Cùng gọi một hàm mà trả về một giá trị `Result` bởi vì hàm có thể thất bại.
Trong Listing 9-3 chúng ta cố gắng mở một tệp.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch09-error-handling/listing-09-03/src/main.rs}}
```

<span class="caption">Listing 9-3: Mở một tệp</span>

Kiểu trả về của `File::open` là `Result<T, E>`. Tham số kiểu generic `T`
được điền vào bởi hiện thực `File::open` với kiểu của giá trị khi gọi thành
công là `std::fs::File`, đó là một file handle. Kiểu của `E` được sử dụng trong
giá và giá trị lỗi là `std::io::Error`. Kiểu trả về này có nghĩa là lời gọi đến
`File::open` có thể thành công và trả về một file handle mà chúng ta có thể
đọc hoặc ghi. Lời gọi hàm cũng có thể thất bại: ví dụ, tệp có thể không tồn tại,
hoặc chúng ta có thể không có quyền truy cập tệp. Hàm `File::open` cần phải có
một cách để cho chúng ta biết nó đã thành công hay thất bại và cùng lúc cho
chúng ta một file handle hoặc thông tin về lỗi. Thông tin này chính là những
gì enum `Result` truyền tải.

Trong trường hợp `File::open` thành công, giá trị trong biến
`greeting_file_result` sẽ là một thể hiện của `Ok` chứa một file handle. Trong
trường hợp nó thất bại, giá trị trong `greeting_file_result` sẽ là một thể hiện
của `Err` chứa thêm thông tin về loại lỗi đã xảy ra.

Chúng ta cần thêm vào code trong Listing 9-3 để thực hiện các hành động khác
nhau tùy thuộc vào giá trị mà `File::open` trả về. Listing 9-4 cho thấy một
cách để xử lý `Result` sử dụng một công cụ cơ bản, biểu thức `match` mà chúng
ta đã thảo luận trong Chương 6.

<span class="filename">Filename: src/main.rs</span>

```rust,should_panic
{{#rustdoc_include ../listings/ch09-error-handling/listing-09-04/src/main.rs}}
```

<span class="caption">Listing 9-4: Sử dụng biểu thức `match` để xử lý các
thể hiện `Result` có thể được trả về</span>

Chú ý rằng, giống như enum `Option`, enum `Result` và các biến thể của nó đã
được đưa vào scope bởi prelude, vì vậy chúng ta không cần chỉ định `Result::`
trước các biến thể `Ok` và `Err` trong các nhánh của `match`.

Khi kết quả là `Ok`, code này sẽ trả về giá trị nội bộ `file` bên trong biến
thể `Ok`, và chúng ta sau đó gán giá trị file handle đó cho biến
`greeting_file`. Sau `match`, chúng ta có thể sử dụng file handle để đọc hoặc
ghi.

Nhánh còn lại của `match` xử lý trường hợp chúng ta nhận được một giá trị `Err`
từ `File::open`. Trong ví dụ này, chúng ta đã chọn gọi macro `panic!`. Nếu
không có tệp nào có tên *hello.txt* trong thư mục hiện tại của chúng ta và chúng
ta chạy mã này, chúng ta sẽ thấy đầu ra sau từ macro `panic!`:

```console
{{#include ../listings/ch09-error-handling/listing-09-04/output.txt}}
```

Như thường lệ, output này cho chúng ta biết chính xác lỗi là gì.

### Matching on Different Errors

Code trong Listing 9-4 sẽ `panic!` bất kể lý do nào mà `File::open` thất bại.
Tuy nhiên, chúng ta muốn thực hiện các hành động khác nhau cho các lý do thất
bại khác nhau: nếu `File::open` thất bại vì tệp không tồn tại, chúng ta muốn
tạo tệp và trả về handle đến tệp mới. Nếu `File::open` thất bại vì bất kỳ lý
do nào khác - ví dụ, vì chúng ta không có quyền mở tệp - chúng ta vẫn muốn mã
`panic!` theo cách giống như trong Listing 9-4. Để làm điều này, chúng ta thêm
một biểu thức `match` bên trong, được hiển thị trong Listing 9-5.

<span class="filename">Filename: src/main.rs</span>

<!-- ignore this test because otherwise it creates hello.txt which causes other
tests to fail lol -->

```rust,ignore
{{#rustdoc_include ../listings/ch09-error-handling/listing-09-05/src/main.rs}}
```

<span class="caption">Listing 9-5: Xử lý các loại lỗi khác nhau theo các cách
khác nhau</span>

Kiểu của giá trị mà `File::open` trả về bên trong biến `Err` là `io::Error`,
một struct được cung cấp bởi thư viện chuẩn. Struct này có một phương thức
`kind` mà chúng ta có thể gọi để lấy một giá trị `io::ErrorKind`. Enum
`io::ErrorKind` được cung cấp bởi thư viện chuẩn và có các biến thể biểu
thị các loại lỗi khác nhau có thể xảy ra từ một hoạt động `io`. Biến thể mà
chúng ta muốn sử dụng là `ErrorKind::NotFound`, biểu thị tệp mà chúng ta đang
cố gắng mở không tồn tại. Do đó, chúng ta `match` trên `greeting_file_result`,
nhưng chúng ta cũng có một `match` bên trong trên `error.kind()`.

Điều kiện mà chúng ta muốn kiểm tra trong `match` bên trong là liệu giá trị
trả về bởi `error.kind()` có phải là biến thể `NotFound` của enum `ErrorKind`.
Nếu có, chúng ta cố gắng tạo tệp với `File::create`. Tuy nhiên, vì
`File::create` cũng có thể thất bại, chúng ta cần một biến thể thứ hai trong
biểu thức `match` bên trong. Khi tệp không thể được tạo, một thông báo lỗi
khác được in ra. Biến thể thứ hai của `match` bên ngoài vẫn giữ nguyên, vì vậy
chương trình sẽ bị panic với bất kỳ lỗi nào ngoài lỗi tệp thiếu.

> ### Alternatives to Using `match` with `Result<T, E>`
>
> Có quá nhiều `match`! Biểu thức `match` rất hữu ích nhưng vẫn là một biểu thức
> cơ bản. Trong Chương 13, bạn sẽ tìm hiểu về closures, được sử dụng với nhiều
> phương thức được định nghĩa trên `Result<T, E>`. Những phương thức này có thể
> ngắn gọn hơn so với việc sử dụng `match` khi xử lý giá trị `Result<T, E>`
> trong code của bạn
>
> Ví dụ, đây là một cách khác để viết cùng một logic như trong Listing 9-5,
> lần này sử dụng closures và phương thức `unwrap_or_else`:
>
> <!-- CAN'T EXTRACT SEE https://github.com/rust-lang/mdBook/issues/1127 -->
>
> ```rust,ignore
> use std::fs::File;
> use std::io::ErrorKind;
> 
> fn main() {
>     let greeting_file = File::open("hello.txt").unwrap_or_else(|error| {
>         if error.kind() == ErrorKind::NotFound {
>             File::create("hello.txt").unwrap_or_else(|error| {
>                 panic!("Problem creating the file: {:?}", error);
>             })
>         } else {
>             panic!("Problem opening the file: {:?}", error);
>         }
>     });
> }
> ```
>
> Mặc dù code này có cùng hành vi như Listing 9-5, nó không chứa bất kỳ biểu
> thức `match` nào và dễ đọc hơn. Quay lại ví dụ này sau khi bạn đã đọc Chương
> 13, và tìm kiếm phương thức `unwrap_or_else` trong tài liệu thư viện chuẩn.
> Nhiều phương thức khác có thể  làm ngắn gọn các biểu thức `match` lồng nhau
> lớn khi bạn đang xử lý các lỗi.

---

### Shortcuts for Panic on Error: `unwrap` and `expect`

Sử dụng `match` thường tốt, nhưng nó có thể rất dài dòng và khó hiểu.
Kiểu `Result<T, E>` có nhiều phương thức trợ giúp được định nghĩa trên nó để
thực hiện các tác vụ cụ thể hơn. Phương thức `unwrap` là một phương thức tắt
được thực hiện giống như biểu thức `match` mà chúng ta đã viết trong Listing
9-4. Nếu giá trị `Result` là biến `Ok`, `unwrap` sẽ trả về giá trị bên trong
biến `Ok`. Nếu `Result` là biến `Err`, `unwrap` sẽ gọi macro `panic!` giúp
chúng ta. Đây là một ví dụ về `unwrap` hoạt động:

<span class="filename">Filename: src/main.rs</span>

```rust,should_panic
{{#rustdoc_include ../listings/ch09-error-handling/no-listing-04-unwrap/src/main.rs}}
```

Nếu chúng ta chạy code này mà không có file *hello.txt*, chúng ta sẽ thấy một
thông báo lỗi từ lệnh `panic!` mà phương thức `unwrap` gọi:

<!-- manual-regeneration
cd listings/ch09-error-handling/no-listing-04-unwrap
cargo run
copy and paste relevant text
-->

```text
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Error {
repr: Os { code: 2, message: "No such file or directory" } }',
src/libcore/result.rs:906:4
```

Tương tự, phương thức `expect` cho phép chúng ta chọn thông báo lỗi `panic!`.
Sử dụng `expect` thay vì `unwrap` và cung cấp các thông báo lỗi tốt có thể
truyền đạt ý định của bạn và làm cho việc tìm nguyên nhân của một lỗi dễ dàng
hơn. Cú pháp của `expect` như sau:

<span class="filename">Filename: src/main.rs</span>

```rust,should_panic
{{#rustdoc_include ../listings/ch09-error-handling/no-listing-05-expect/src/main.rs}}
```

Chúng ta sử dụng `expect` theo cách giống như `unwrap`: để trả về file handle
hoặc gọi macro `panic!`. Thông báo lỗi được sử dụng bởi `expect` trong lệnh
`panic!` sẽ là tham số mà chúng ta truyền cho `expect`, thay vì thông báo lỗi
mặc định của `panic!` mà `unwrap` sử dụng. Đây là cách nó hoạt động:

<!-- manual-regeneration
cd listings/ch09-error-handling/no-listing-05-expect
cargo run
copy and paste relevant text
-->

```text
thread 'main' panicked at 'hello.txt should be included in this project: Error
{ repr: Os { code: 2, message: "No such file or directory" } }',
src/libcore/result.rs:906:4
```

Trong code ở production, hầu hết Rustaceans (người dùng Rust) chọn
`expect` thay vì `unwrap` và cung cấp thêm thông tin về tại sao các hoạt động
được mong đợi là luôn thành công. Điều đó cho phép bạn có thêm thông tin để
sử dụng trong quá trình debug nếu như các giả định của bạn bị chứng minh sai.

---

### Propagating Errors

Khi một hàm thực thi gọi một thứ gì đó có thể gây ra lỗi, thay vì xử lý lỗi
trong hàm thực thi, bạn có thể trả về lỗi cho code gọi hàm để nó có thể quyết
định làm gì. Điều này được gọi là *propagating (lan truyền)* lỗi và cho phép
code gọi đến hàm có thể quyết định xử lý lỗi theo cách nào mà nó thích hơn.

Ví dụ, Listing 9-6 cho thấy một hàm đọc username từ một file. Nếu file không
tồn tại hoặc không thể đọc được, hàm này sẽ trả về lỗi đó cho code gọi hàm.

<span class="filename">Filename: src/main.rs</span>

<!-- Deliberately not using rustdoc_include here; the `main` function in the
file panics. We do want to include it for reader experimentation purposes, but
don't want to include it for rustdoc testing purposes. -->

```rust
{{#include ../listings/ch09-error-handling/listing-09-06/src/main.rs:here}}
```

<span class="caption">Listing 9-6: Một hàm trả về lỗi cho code gọi hàm sử dụng
`match`</span>

Hàm này có thể được viết ngắn gọn hơn, nhưng chúng ta sẽ bắt đầu bằng cách
viết nó một cách thủ công để khám phá về xử lý lỗi; ở cuối, chúng ta sẽ cho
thấy cách viết ngắn gọn hơn. Hãy xem kiểu trả về của hàm trước tiên:
`Result<String, io::Error>`. Điều này có nghĩa là hàm trả về một giá trị của
kiểu `Result<T, E>` với tham số `T` được điền vào với kiểu cụ thể `String`,
và kiểu `E` được điền vào với kiểu cụ thể `io::Error`.

Nếu hàm này thành công mà không có vấn đề gì, code gọi hàm này sẽ nhận được
một giá trị `Ok` chứa một `String` - username mà hàm này đọc từ file. Nếu hàm
này gặp bất kỳ vấn đề nào, code gọi hàm sẽ nhận được một giá trị `Err` chứa một
thể hiện của `io::Error` chứa thông tin chi tiết về vấn đề. Chúng ta chọn
`io::Error` là kiểu trả về của hàm này vì đó chính là kiểu của giá trị lỗi
trả về từ cả hai hoạt động mà chúng ta gọi trong thân hàm này có thể gặp lỗi:
hàm `File::open` và phương thức `read_to_string`.

Thân hàm bắt đầu bằng cách gọi hàm `File::open`. Sau đó chúng ta xử lý giá trị
`Result` với một `match` giống như `match` trong Listing 9-4. Nếu `File::open`
thành công, file handle trong biến mẫu `file` trở thành giá trị trong biến
`username_file` và hàm tiếp tục. Trong trường hợp `Err`, thay vì gọi `panic!`,
chúng ta sử dụng từ khóa `return` để trả về sớm và trả về giá trị lỗi từ
`File::open`, giờ đây trong biến mẫu `e`, về code gọi hàm như là giá trị lỗi
của hàm này.

Nên nếu chúng ta có một file handle trong `username_file`,
chúng ta sẽ tạo một biến kiêu `String` mới với tên `username`
và gọi phương thức `read_to_string` của
file handle trong `username_file` để đọc nội dung của file vào `username`.
Phương thức `read_to_string` cũng trả về một `Result` vì nó có thể gặp lỗi,
dù `File::open` đã thành công. Vì vậy chúng ta cần một `match` khác để xử lý
`Result`: nếu `read_to_string` thành công, thì hàm của chúng ta đã thành công,
và chúng ta trả về username từ file hiện tại trong `username` được bọc trong
một `Ok`. Nếu `read_to_string` gặp lỗi, chúng ta trả về giá trị lỗi theo cách
giống như chúng ta trả về giá trị lỗi trong `match` xử lý giá trị trả về của
`File::open`. Tuy nhiên, chúng ta không cần phải nói rõ `return`, vì đây là
biểu thức cuối cùng trong hàm.

Đoạn code gọi hàm này sẽ xử lý việc nhận được một giá trị `Ok` chứa một username
hoặc một giá trị `Err` chứa một `io::Error`. Nó phụ thuộc vào code gọi hàm để
quyết định làm gì với những giá trị này. Nếu code gọi hàm nhận được một giá trị
`Err`, nó có thể gọi `panic!` và crash chương trình, sử dụng một username mặc
định, hoặc tìm kiếm username từ một nơi khác ngoài file, ví dụ. Chúng ta không
có đủ thông tin về việc code gọi hàm thực sự muốn làm gì, vì vậy chúng ta truyền
tất cả thông tin thành công hoặc lỗi lên trên để code gọi hàm xử lý phù hợp.

Phương pháp truyền lỗi này rất phổ biến trong Rust, vì vậy Rust cung cấp
phép toán dấu hỏi `?` để làm cho việc này dễ dàng hơn.

#### A Shortcut for Propagating Errors: the `?` Operator

Listing 9-7 chỉ ra một cách code của `read_username_from_file` có cùng chức
năng như trong Listing 9-6, nhưng cách code này sử dụng phép toán `?`.

<span class="filename">Filename: src/main.rs</span>

<!-- Deliberately not using rustdoc_include here; the `main` function in the
file panics. We do want to include it for reader experimentation purposes, but
don't want to include it for rustdoc testing purposes. -->

```rust
{{#include ../listings/ch09-error-handling/listing-09-07/src/main.rs:here}}
```

<span class="caption">Listing 9-7: Một hàm trả về lỗi cho code gọi hàm sử dụng
phép toán `?`</span>

Phép toán `?` được định nghĩa để hoạt động gần như giống như các biểu thức
`match` mà chúng ta định nghĩa để xử lý các giá trị `Result` trong Listing
9-6. Nếu giá trị của `Result` là `Ok`, giá trị bên trong `Ok` sẽ được trả về
từ biểu thức này, và chương trình sẽ tiếp tục. Nếu giá trị là `Err`, `Err` sẽ
được trả về trả về sớm và kết thúc hàm, giống cách chúng ta dùng `return`.

Có một sự khác biệt giữa việc biểu thức `match` từ Listing 9-6 làm gì và việc
phép toán `?` làm gì: các giá trị lỗi mà có phép toán `?` được gọi sẽ đi qua
hàm `from`, được định nghĩa trong trait `From` của thư viện chuẩn, được sử
dụng để chuyển đổi giá trị từ một kiểu thành một kiểu khác. Khi phép toán `?`
gọi hàm `from`, kiểu lỗi nhận được sẽ được chuyển đổi thành kiểu lỗi được định
nghĩa trong kiểu trả về của hàm hiện tại. Điều này rất hữu ích khi một hàm trả
về một kiểu lỗi để biểu diễn tất cả các cách mà một hàm có thể thất bại, ngay cả
khi các phần có thể thất bại vì nhiều lý do khác nhau.

Ví dụ, chúng ta có thể thay đổi hàm `read_username_from_file` trong Listing
9-7 để trả về một kiểu lỗi tùy chỉnh được đặt tên là `OurError` mà chúng ta
định nghĩa. Nếu chúng ta cũng định nghĩa `impl From<io::Error> for OurError`
để tạo một thể hiện của `OurError` từ một `io::Error`, thì phép toán `?` sẽ
gọi `from` và chuyển đổi kiểu lỗi mà không cần thêm bất kì đoạn mã nào vào
hàm.

Trong ngữ cảnh của Listing 9-7, phép toán `?` ở cuối lời gọi `File::open` sẽ
trả về giá trị bên trong một `Ok` cho biến `username_file`. Nếu xảy ra lỗi,
phép toán `?` sẽ trả về sớm và trả bất kì giá trị `Err` nào
cho code gọi hàm. Điều đó cũng được áp dụng cho phép toán `?` ở cuối lời gọi
`read_to_string`.

Phép toán `?` loại bỏ rất nhiều đoạn mã lặp đi và làm cho việc triển khai hàm
trở nên đơn giản hơn. Chúng ta có thể rút ngắn đoạn mã này thêm bằng cách
liên tiếp gọi các phương thức ngay sau phép toán `?`, như trong Listing 9-8.

<span class="filename">Filename: src/main.rs</span>

<!-- Deliberately not using rustdoc_include here; the `main` function in the
file panics. We do want to include it for reader experimentation purposes, but
don't want to include it for rustdoc testing purposes. -->

```rust
{{#include ../listings/ch09-error-handling/listing-09-08/src/main.rs:here}}
```

<span class="caption">Listing 9-8: Liên tiếp gọi các phương thức sau phép toán
`?`</span>

Chúng ta tạo một biến `String` tên `username` ở đầu hàm; phần đó không
thay đổi. Thay vì tạo một biến `username_file`, chúng ta liên tiếp gọi phương
thức `read_to_string` trực tiếp trên kết quả của `File::open("hello.txt")?`.
Chúng ta vẫn có một `?` ở cuối lời gọi `read_to_string` và chúng ta vẫn trả về
một giá trị `Ok` chứa `username` khi cả `File::open` và `read_to_string`
thành công thay vì trả về lỗi. Chức năng vẫn giống như trong Listing 9-6 và
Listing 9-7; đây chỉ là một cách khác, dễ dùng hơn để viết nó.

Listing 9-9 cho thấy một cách để làm cho đoạn mã này ngắn hơn bằng cách sử
dụng `fs::read_to_string`.

<span class="filename">Filename: src/main.rs</span>

<!-- Deliberately not using rustdoc_include here; the `main` function in the
file panics. We do want to include it for reader experimentation purposes, but
don't want to include it for rustdoc testing purposes. -->

```rust
{{#include ../listings/ch09-error-handling/listing-09-09/src/main.rs:here}}
```

<span class="caption">Listing 9-9: Sử dụng `fs::read_to_string` thay vì mở và
sau đó đọc file</span>

Đọc một file vào một chuỗi là một thao tác rất phổ biến, vì vậy thư viện chuẩn
cung cấp một hàm tiện lợi `fs::read_to_string` để mở file, tạo một `String`
mới, đọc nội dung của file, đặt nội dung vào `String` đó và trả về nó. Đương
nhiên, sử dụng `fs::read_to_string` không cho chúng ta cơ hội để giải thích
tất cả các xử lý lỗi, vì vậy chúng ta đã làm theo cách dài hơn trước.

---

#### Where The `?` Operator Can Be Used

Toán tử `?` chỉ có thể được sử dụng trong các hàm mà kiểu trả về là tương
thích với giá trị mà `?` được sử dụng. Điều này là bởi vì toán tử `?` được định
nghĩa để thực hiện một yêu cầu trả về sớm của một giá trị ra khỏi hàm,
cùng một cách như biểu thức `match` mà chúng ta định nghĩa trong Listing 9-6.
Trong Listing 9-6, `match` sử dụng một giá trị `Result`, và trả về sớm một giá
trị `Err(e)`. Kiểu trả về của hàm phải là một `Result` để nó tương thích với
trả về sớm của lệnh `return`.

Trong Listing 9-10, hãy xem lỗi mà chúng ta sẽ nhận được nếu chúng ta sử dụng
toán tử `?` trong một hàm `main` với kiểu trả về không tương thích với kiểu của
giá trị mà chúng ta sử dụng `?`:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch09-error-handling/listing-09-10/src/main.rs}}
```

<span class="caption">Listing 9-10: Cố gắng sử dụng `?` trong hàm `main` trả
về `()` sẽ không biên dịch được</span>

Đoạn code này mở một file, có thể sẽ thất bại. Toán tử `?` theo sau giá trị
`Result` được trả về bởi `File::open`, nhưng hàm `main` này có kiểu trả về là
`()`, không phải `Result`. Khi chúng ta biên dịch đoạn code này, chúng ta sẽ
nhận được lỗi như sau:

```console
{{#include ../listings/ch09-error-handling/listing-09-10/output.txt}}
```

Lỗi này chỉ ra rằng chúng ta chỉ được phép sử dụng toán tử `?` trong một hàm
trả về `Result`, `Option`, hoặc một kiểu khác mà nó triển khai `FromResidual`.

Để sửa lỗi, chúng ta có hai lựa chọn. Lựa chọn đầu tiên là thay đổi kiểu trả về
của hàm để tương thích với giá trị mà chúng ta đang sử dụng toán tử `?`
trong khi không có giới hạn nào ngăn cản điều đó. Cách khác là sử dụng một
`match` hoặc một trong các phương thức của `Result<T, E>` để xử lý 
`Result<T, E>` theo cách phù hợp.

Lỗi còn nói rằng `?` có thể được sử dụng với giá trị `Option<T>` cũng như vậy.
Giống như sử dụng `?` trên `Result`, chúng ta chỉ có thể sử dụng `?` trên
`Option` trong một hàm trả về `Option`. Hành vi của toán tử `?` khi được gọi
trên một `Option<T>` tương tự như hành vi của nó khi được gọi trên một
`Result<T, E>`: nếu giá trị là `None`, `None` sẽ được trả về sớm từ hàm tại
điểm đó. Nếu giá trị là `Some`, giá trị bên trong `Some`
sẽ là giá trị kết quả của biểu thức và hàm sẽ tiếp tục. Listing 9-11 có một
ví dụ về một hàm tìm ký tự cuối cùng của dòng đầu tiên trong văn bản cho trước:

```rust
{{#rustdoc_include ../listings/ch09-error-handling/listing-09-11/src/main.rs:here}}
```

<span class="caption">Listing 9-11: Sử dụng toán tử `?` trên một giá trị
`Option<T>`</span>

Hàm này trả về `Option<char>` vì có thể có một ký tự ở đó, nhưng cũng có thể
không có. Code này lấy đoạn chuỗi `text` và gọi phương thức `lines` trên nó,
nó sẽ trả về một iterator (vòng lặp) trên các dòng trong chuỗi. Vì hàm này muốn
xem xét dòng đầu tiên, nó gọi `next` trên iterator để lấy giá trị đầu tiên từ
iterator. Nếu `text` là một chuỗi rỗng, cuộc gọi `next` này sẽ trả về `None`,
trong trường hợp này chúng ta sử dụng `?` để dừng và trả về `None` từ
`last_char_of_first_line`. Nếu `text` không phải là một chuỗi rỗng, `next` sẽ
trả về một giá trị `Some` chứa một đoạn chuỗi của dòng đầu tiên trong `text`.

`?` trích xuất đoạn chuỗi, và chúng ta có thể gọi `chars` trên đoạn chuỗi đó để
lấy một iterator (vòng lặp) của các ký tự trong đoạn chuỗi. Chúng ta quan tâm
đến ký tự cuối cùng trong dòng đầu tiên này, vì vậy chúng ta gọi `last` để
trả về item cuối cùng trong iterator. Đây là một `Option` vì có thể dòng đầu
tiên là một chuỗi rỗng, ví dụ nếu `text` bắt đầu với một dòng trắng nhưng có
các ký tự trên các dòng khác, như trong `"\nhi"`. Tuy nhiên, nếu có một ký tự
cuối cùng trên dòng đầu tiên, nó sẽ được trả về trong biến `Some`. Toán tử `?`
ở giữa cung cấp cho chúng ta một cách ngắn gọn để biểu thị logic này, cho phép
chúng ta thực hiện hàm trong một dòng. Nếu chúng ta không thể sử dụng toán tử
`?` trên `Option`, chúng ta sẽ phải thực hiện logic này bằng cách sử dụng nhiều
phương thức gọi hơn hoặc một biểu thức `match`.

Lưu ý rằng bạn có thể sử dụng toán tử `?` trên một `Result` trong một hàm trả
về `Result`, và bạn có thể sử dụng toán tử `?` trên một `Option` trong một hàm
trả về `Option`, nhưng bạn không thể kết hợp và phối hợp. Toán tử `?` sẽ không
tự động chuyển đổi một `Result` thành một `Option` hoặc ngược lại; trong những
trường hợp đó, bạn có thể sử dụng các phương thức như phương thức `ok` trên
`Result` hoặc phương thức `ok_or` trên `Option` để thực hiện chuyển đổi một cách
rõ ràng.

Hiện tại, tất cả các hàm `main` mà chúng ta đã sử dụng trả về `()`. Hàm `main`
đặc biệt bởi vì nó là điểm vào (entry point) và ra của các chương trình thực
thi, và có những hạn chế về kiểu trả về của nó để các chương trình hoạt động
như mong đợi.

May mắn thay, `main` cũng có thể trả về một `Result<(), E>`. Listing 9-12 có
code từ Listing 9-10 nhưng đã thay đổi kiểu trả về của `main` thành
`Result<(), Box<dyn Error>>` và thêm một giá trị trả về `Ok(())` vào cuối.
Code này sẽ bây giờ được biên dịch:

```rust,ignore
{{#rustdoc_include ../listings/ch09-error-handling/listing-09-12/output.txt}}
```

<span class="caption">Listing 9-12: Đổi `main` để trả về `Result<(), E>` cho
phép sử dụng toán tử `?` trên các giá trị `Result`</span>

Kiểu `Box<dyn Error>` là một *trait object*, mà chúng ta sẽ nói về nó trong
phần [“Using Trait Objects that Allow for Values of Different
Types”][trait-objects]<!-- ignore --> trong Chương 17. Hiện tại, bạn có thể
đọc `Box<dyn Error>` là “bất kỳ loại lỗi nào”. Sử dụng `?` trên một giá trị
`Result` trong một hàm `main` với kiểu lỗi `Box<dyn Error>` được cho phép,
bởi vì nó cho phép bất kỳ giá trị `Err` nào được trả về sớm. Dù cho phần thân
của hàm `main` này sẽ chỉ trả về lỗi của kiểu `std::io::Error`, bằng cách
xác định `Box<dyn Error>`, kiểu này sẽ tiếp tục đúng ngay cả khi thêm code
mà trả về các lỗi khác vào phần thân của `main`.

Khi hàm `main` trả về `Result<(), E>`, chương trình sẽ thoát với một giá
trị `0` nếu `main` trả về `Ok(())` và sẽ thoát với một giá trị khác không
phải `0` nếu `main` trả về một giá trị `Err`. Các chương trình được viết bằng
C trả về các số nguyên khi thoát: các chương trình thoát thành công trả về
số nguyên `0`, và các chương trình bị lỗi trả về một số nguyên khác không
phải `0`. Rust cũng trả về các số nguyên từ các chương trình để tương thích
với quy ước này.

Hàm `main` có thể trả về bất kỳ kiểu nào thực thi [the
`std::process::Termination` trait][termination]<!-- ignore -->, mà chứa một
hàm `report` trả về một `ExitCode`. Hãy xem tài liệu thư viện chuẩn để biết
thêm thông tin về việc thực thi trait `Termination` cho các kiểu của bạn.

Bây giờ chúng ta đã thảo luận về chi tiết về việc gọi `panic!` hoặc trả về
`Result`, hãy trở lại về chủ đề làm thế nào để quyết định sử dụng cái nào
trong các trường hợp cụ thể.

[handle_failure]: ch02-00-guessing-game-tutorial.html#handling-potential-failure-with-the-result-type
[trait-objects]: ch17-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types
[termination]: ../std/process/trait.Termination.html
