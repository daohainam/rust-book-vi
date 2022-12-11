## Unrecoverable Errors with `panic!`

Thỉnh thoảng, những điều tồi tệ xảy ra trong code của bạn, và không có gì bạn
có thể làm với nó. Trong những trường hợp này, Rust có macro `panic!`. Trong
thực tế có hai cách để gây ra một panic: bằng cách thực hiện một hành động mà
code của bạn gây ra một panic (như truy cập một mảng vượt quá phạm vi) hoặc bằng
cách gọi trực tiếp macro `panic!`. Trong cả hai trường hợp, chúng ta gây ra một
panic trong chương trình của mình. Mặc định, những panic này sẽ in ra một thông
báo lỗi, unwind, dọn dẹp stack, và thoát. Bằng một biến môi trường, bạn cũng có
thể cho Rust hiển thị call stack khi một panic xảy ra để dễ dàng theo dõi nguồn
gốc của panic.

> ### Unwinding the Stack or Aborting in Response to a Panic
>
> Mặc định, khi một panic xảy ra, chương trình bắt đầu *unwinding*, nghĩa là
> Rust đi lên stack và dọn dẹp dữ liệu từ mỗi hàm nó gặp. Tuy nhiên, đi ngược
> lại và dọn dẹp dữ liệu trong từng hàm, việc đi nguoc lại và dọn dẹp là một
> công việc rất nặng. Do đó, Rust cho phép bạn chọn cách thay thế là *aborting*
> ngưng ngay lập tức, nghĩa là kết thúc chương trình mà không dọn dẹp.
>
> Bộ nhớ mà chương trình đang sử dụng sẽ cần được dọn dẹp bởi hệ điều hành. Nếu
> trong dự án của bạn cần tạo ra một binary nhỏ nhất có thể, bạn có thể chuyển
> từ unwinding sang aborting khi một panic xảy ra bằng cách thêm
> `panic = 'abort'` vào các phần `[profile]` phù hợp trong file *Cargo.toml*
> của bạn. Ví dụ, nếu bạn muốn abort khi một panic xảy ra trong chế độ release,
> thêm vào đoạn sau:
>
> ```toml
> [profile.release]
> panic = 'abort'
> ```
>

Hãy thử gọi `panic!` trong một chương trình đơn giản:

<span class="filename">Tên file: src/main.rs</span>

```rust,should_panic,panics
{{#rustdoc_include ../listings/ch09-error-handling/no-listing-01-panic/src/main.rs}}
```

Khi bạn chạy chương trình, bạn sẽ thấy giống như thế này:

```console
{{#include ../listings/ch09-error-handling/no-listing-01-panic/output.txt}}
```

Gọi `panic!` sẽ gây ra thông báo lỗi được chứa trong 2 dòng cuối cùng. Dòng
đầu tiên hiển thị thông báo panic của chúng ta và nơi xảy ra panic trong code
: *src/main.rs:2:5* cho thấy đó là dòng thứ hai, ký tự thứ năm của file
*src/main.rs* của chúng ta.

Trong trường hợp này, dòng được chỉ định là một phần của code của chúng ta,
và nếu chúng ta đi đến dòng đó, chúng ta sẽ thấy gọi macro `panic!`. Trong
trường hợp khác, gọi `panic!` có thể nằm trong code mà chúng ta dùng,
và tên file và số dòng được báo bởi thông báo lỗi sẽ là code của người khác
nơi `panic!` macro được gọi. Chúng ta có thể sử dụng backtrace của các
hàm mà `panic!` được gọi để tìm ra phần của code của chúng ta gây ra vấn đề.
Chúng ta sẽ thảo luận về backtrace chi tiết hơn ở phần tiếp theo.

### Using a `panic!` Backtrace

Cùng nhìn vào ví dụ khác để xem nó trông như thế nào khi một `panic!` được gọi
từ một thư viện vì một lỗi trong code của chúng ta thay vì từ code của chúng
ta gọi macro trực tiếp. Listing 9-1 có một số code mà cố gắng truy cập một
index trong một vector ngoài phạm vi của các index hợp lệ.

<span class="filename">Tên file: src/main.rs</span>

```rust,should_panic,panics
{{#rustdoc_include ../listings/ch09-error-handling/listing-09-01/src/main.rs:here}}
```

<span class="caption">Listing 9-1: Cố gắng truy cập một phần tử ngoài phạm vi
của một vector, điều này sẽ gây ra một gọi đến `panic!`</span>

Tại đây, chúng ta cố gắng truy cập phần tử thứ 100 của vector của chúng ta
(phần tử này ở index 99 vì index bắt đầu từ 0), nhưng vector chỉ có 3 phần tử.
Trong trường hợp này, Rust sẽ panic. Sử dụng `[]` được dự kiến sẽ trả về một
phần tử, nhưng nếu chúng ta truyền một index không hợp lệ, không có phần tử
nào mà Rust có thể trả về.

Trong C, cố gắng đọc ngoài phạm vi của một cấu trúc dữ liệu là hành vi không
xác định. Bạn có thể nhận được bất cứ thứ gì ở vị trí trong bộ nhớ tương ứng
với phần tử đó trong cấu trúc dữ liệu, ngay cả khi bộ nhớ không thuộc về cấu
trúc đó. Điều này được gọi là một *buffer overread* và có thể dẫn đến các
lỗ hổng bảo mật nếu một kẻ tấn công có thể điều khiển index một cách nào đó để
đọc dữ liệu mà họ không nên được phép đọc được lưu trữ sau cấu trúc dữ liệu.

Để bảo vệ chương trình của bạn khỏi loại lỗ hổng này, nếu bạn cố gắng đọc một
phần tử ở một index không tồn tại, Rust sẽ dừng thực thi và từ chối tiếp tục.
Hãy thử và xem:

```console
{{#include ../listings/ch09-error-handling/listing-09-01/output.txt}}
```

Lỗi này chỉ ra rằng tại dòng 4 của `main.rs` chúng ta cố gắng truy cập index 99.
Dòng chú thích tiếp theo cho chúng ta biết rằng chúng ta có thể thiết lập biến
môi trường `RUST_BACKTRACE` để có được một backtrace chính xác về những gì đã
xảy ra để gây ra lỗi. Một *backtrace* là một danh sách tất cả các hàm đã được
gọi để đến điểm này. Backtrace trong Rust hoạt động như các ngôn ngữ khác:
điều quan trọng để đọc backtrace là bắt đầu từ đầu và đọc cho đến khi bạn thấy
các tập tin bạn đã viết. Đó là điểm code nguốn của vấn đề. Các dòng trên điểm đó
là code của bạn đã gọi; các dòng dưới là code gọi đến code của bạn. Những dòng
trước và sau có thể bao gồm code Rust cơ bản, thư viện chuẩn hoặc các thư viện
được sử dụng. Hãy thử lấy một backtrace bằng cách thiết lập biến môi trường
`RUST_BACKTRACE` thành bất kỳ giá trị nào khác 0. Listing 9-2 hiển thị kết quả
tương tự như bạn sẽ thấy.

<!-- manual-regeneration
cd listings/ch09-error-handling/listing-09-01
RUST_BACKTRACE=1 cargo run
copy the backtrace output below
check the backtrace number mentioned in the text below the listing
-->

```console
$ RUST_BACKTRACE=1 cargo run
thread 'main' panicked at 'index out of bounds: the len is 3 but the index is 99', src/main.rs:4:5
stack backtrace:
   0: rust_begin_unwind
             at /rustc/7eac88abb2e57e752f3302f02be5f3ce3d7adfb4/library/std/src/panicking.rs:483
   1: core::panicking::panic_fmt
             at /rustc/7eac88abb2e57e752f3302f02be5f3ce3d7adfb4/library/core/src/panicking.rs:85
   2: core::panicking::panic_bounds_check
             at /rustc/7eac88abb2e57e752f3302f02be5f3ce3d7adfb4/library/core/src/panicking.rs:62
   3: <usize as core::slice::index::SliceIndex<[T]>>::index
             at /rustc/7eac88abb2e57e752f3302f02be5f3ce3d7adfb4/library/core/src/slice/index.rs:255
   4: core::slice::index::<impl core::ops::index::Index<I> for [T]>::index
             at /rustc/7eac88abb2e57e752f3302f02be5f3ce3d7adfb4/library/core/src/slice/index.rs:15
   5: <alloc::vec::Vec<T> as core::ops::index::Index<I>>::index
             at /rustc/7eac88abb2e57e752f3302f02be5f3ce3d7adfb4/library/alloc/src/vec.rs:1982
   6: panic::main
             at ./src/main.rs:4
   7: core::ops::function::FnOnce::call_once
             at /rustc/7eac88abb2e57e752f3302f02be5f3ce3d7adfb4/library/core/src/ops/function.rs:227
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

<span class="caption">Listing 9-2: Backtrace được tạo ra bởi một cuộc gọi
`panic!` được hiển thị khi biến môi trường `RUST_BACKTRACE` được thiết lập</span>

Nhiều ouput được in ra! Ouput cụ thể bạn thấy có thể khác nhau tùy thuộc vào
hệ điều hành và phiên bản Rust của bạn. Để có được backtraces với thông tin
này, các ký hiệu gỡ lỗi phải được bật. Ký hiệu gỡ lỗi được bật mặc định khi sử
dụng `cargo build` hoặc `cargo run` mà không có cờ `--release`, như chúng tôi
đã có ở đây.

Trong ouput trong Listing 9-2, dòng 6 của backtrace chỉ đến dòng trong dự án
của chúng ta gây ra vấn đề: dòng 4 của *src/main.rs*. Nếu chúng ta không muốn
chương trình của chúng ta bị panic, chúng ta nên bắt đầu điều tra tại vị trí
được chỉ đến bởi dòng đầu tiên nói về một tệp mà chúng ta đã viết. Trong
Listing 9-1, khi chúng ta có ý định viết mã sẽ bị panic, cách để sửa lỗi panic
là không yêu cầu một phần tử ngoài phạm vi của các chỉ số vector. Khi mã của
bạn bị panic trong tương lai, bạn sẽ cần phải suy nghĩ về hành động mã đang
thực hiện với giá trị nào để gây ra panic và mã nên làm gì thay thế.

Chúng ta sẽ quay lại `panic!` và khi nào chúng ta nên và không nên sử dụng
`panic!` để xử lý các điều kiện lỗi trong phần [“To `panic!` or Not to
`panic!`”][to-panic-or-not-to-panic]<!-- ignore --> sau trong chương này.
Tiếp theo, chúng ta sẽ xem cách khôi phục lỗi bằng cách sử dụng `Result`.

[to-panic-or-not-to-panic]:
ch09-03-to-panic-or-not-to-panic.html#to-panic-or-not-to-panic
