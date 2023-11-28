## Một Chương Trình Minh Họa Sử Dụng Struct

Để hiểu khi nào chúng ta nên sử dụng struct, hãy viết một chương trình tính 
diện tích của một hình chữ nhật. Chúng ta sẽ bắt đầu bằng cách sử dụng các 
biến đơn, sau đó làm lại chương trình cho đến khi sử dụng struct.

Hãy tạo một dự án binary mới với Cargo gọi là *rectangles* để nhận chiều rộng 
và chiều cao của một hình chữ nhật được chỉ định bằng pixel và tính diện tích 
của hình chữ nhật đó. Code ở Listing 5-8 hiển thị một chương trình ngắn có một 
cách làm như vậy trong file *src/main.rs* của dự án của chúng ta.

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-08/src/main.rs:all}}
```

<span class="caption">Listing 5-8: Tính diện tích của hình chữ nhật được chỉ 
định bởi các biến chiều rộng và chiều cao riêng lẻ</span>

Giờ chạy chương trình này bằng cách sử dụng `cargo run`:

```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-08/output.txt}}
```

Mã này thành công trong việc tính diện tích của hình chữ nhật bằng cách gọi hàm `area` với mỗi chiều, nhưng chúng ta có thể làm thêm để làm cho mã này rõ ràng và dễ đọc hơn.

Vấn đề của mã này rõ ràng trong chữ ký của `area`:

```rust,ignore
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-08/src/main.rs:here}}
```

Hàm `area` được thiết kế để tính diện tích của một hình chữ nhật, nhưng hàm 
mà chúng ta đã viết có hai tham số và không rõ ở đâu trong chương trình 
của chúng ta rằng các tham số này có liên quan. Việc nhóm chiều rộng và 
chiều cao lại với nhau sẽ làm cho mã này dễ đọc và quản lý hơn. Chúng ta 
đã thảo luận một cách làm như vậy trong [“The Tuple Type”][the-tuple-type]<!-- ignore --> 
trong Chương 3: sử dụng tuples.

### Tái Cấu Trúc với Tuple

Listing 5-9 hiển thị một phiên bản khác của chương trình của chúng ta sử dụng tuple.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-09/src/main.rs}}
```

<span class="caption">Listing 5-9: Xác định chiều rộng và chiều cao của hình chữ nhật bằng tuple</span>

Ở một cách, chương trình này tốt hơn. Tuple cho phép chúng ta thêm một chút 
cấu trúc và giờ đây chúng ta chỉ đang truyền một đối số. Nhưng theo một cách khác, 
phiên bản này lại ít rõ ràng: tuples không đặt tên cho các phần tử của chúng, 
vì vậy chúng ta phải sử dụng chỉ mục vào các phần của tuple, làm cho phép tính 
toán của chúng ta ít rõ ràng hơn.

Việc nhầm lẫn giữa chiều rộng và chiều cao có thể không quan trọng đối 
với việc tính diện tích, nhưng nếu chúng ta muốn vẽ hình chữ nhật 
lên màn hình, điều đó sẽ quan trọng! Chúng ta sẽ phải nhớ rằng width 
là chỉ mục 0 của tuple và `height` là chỉ mục `1` của tuple. Điều này sẽ làm 
cho người khác khó khăn hơn khi cố gắng hiểu và giữ mã của chúng ta. 
Bởi vì chúng ta chưa truyền đạt ý nghĩa của dữ liệu trong mã của chúng ta, 
việc giới thiệu lỗi giờ đây dễ dàng hơn.

### Tái Cấu Trúc với Structs: Thêm Nhiều Ý Nghĩa Hơn

Chúng ta sử dụng structs để thêm ý nghĩa bằng cách đặt tên cho dữ liệu. Chúng ta 
có thể chuyển đổi tuple chúng ta đang sử dụng thành một struct có tên cho toàn bộ 
cũng như tên cho các phần, như thể hiện trong Listing 5-10.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-10/src/main.rs}}
```

<span class="caption">Listing 5-10: Định nghĩa một struct `Rectangle`</span>

Here we’ve defined a struct and named it `Rectangle`. Inside the curly
brackets, we defined the fields as `width` and `height`, both of which have
type `u32`. Then, in `main`, we created a particular instance of `Rectangle`
that has a width of `30` and a height of `50`.

Our `area` function is now defined with one parameter, which we’ve named
`rectangle`, whose type is an immutable borrow of a struct `Rectangle`
instance. As mentioned in Chapter 4, we want to borrow the struct rather than
take ownership of it. This way, `main` retains its ownership and can continue
using `rect1`, which is the reason we use the `&` in the function signature and
where we call the function.

The `area` function accesses the `width` and `height` fields of the `Rectangle`
instance (note that accessing fields of a borrowed struct instance does not
move the field values, which is why you often see borrows of structs). Our
function signature for `area` now says exactly what we mean: calculate the area
of `Rectangle`, using its `width` and `height` fields. This conveys that the
width and height are related to each other, and it gives descriptive names to
the values rather than using the tuple index values of `0` and `1`. This is a
win for clarity.

Ở đây, chúng ta đã định nghĩa một struct và đặt tên cho nó là `Rectangle`. 
Bên trong dấu ngoặc nhọn, chúng ta đã xác định các trường là `width` và `height`, 
cả hai đều có kiểu `u32`. Sau đó, trong hàm `main`, chúng ta đã tạo một instance 
cụ thể của `Rectangle` có chiều rộng là `30` và chiều cao là `50`.

Hàm `area` của chúng ta giờ đây được định nghĩa với một tham số, mà chúng ta 
đã đặt tên là rectangle, kiểu dữ liệu của nó là một borrow không thể thay đổi 
(immutable borrow) của một thể hiện của struct `Rectangle`. Như đã đề cập 
trong Chương 4, chúng ta muốn mượn (borrow) struct thay vì sở hữu nó. Điều này 
giúp cho hàm main giữ quyền sở hữu và có thể tiếp tục sử dụng rect1, đó là lý do 
chúng ta sử dụng ký hiệu & trong chữ ký hàm và ở nơi chúng ta gọi hàm.

Hàm area truy cập các trường `width` và `height` của thể hiện `Rectangle` (lưu ý rằng 
việc truy cập các trường của một thể hiện struct đã được mượn không di chuyển 
giá trị trường, đó là lý do tại sao bạn thường thấy việc mượn struct). Chữ ký hàm 
cho area bây giờ diễn đạt đúng ý chúng ta: tính diện tích của `Rectangle`, sử dụng 
các trường `width` và `height` của nó. Điều này truyền đạt rằng chiều rộng và chiều cao 
có liên quan đến nhau, và nó đặt tên mô tả cho các giá trị thay vì sử dụng chỉ mục 
tuple là `0` và `1`. Điều này là một chiến thắng về sự rõ ràng.

### Thêm Chức Năng Hữu Ích với Các Derived Traits

Sẽ hữu ích nếu chúng ta có thể in ra một thể hiện của `Rectangle` trong khi chúng ta 
đang gỡ lỗi chương trình và xem giá trị của tất cả các trường. Đoạn mã 5-11 
thử nghiệm việc sử dụng [macro println!][println]<!-- ignore --> như chúng ta 
đã sử dụng trong các chương trước đó. Tuy nhiên, điều này sẽ không hoạt động.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-11/src/main.rs}}
```

<span class="caption">Listing 5-11: Thử in ra một instance `Rectangle`</span>

Khi chúng ta biên dịch mã này, chúng ta sẽ nhận được một lỗi với thông báo chính sau:

```text
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-11/output.txt:3}}
```

Macro `println!` có thể thực hiện nhiều loại định dạng, và theo mặc định, dấu ngoặc nhọn 
trong `println!` đều cho biết chúng ta muốn sử dụng định dạng được biết đến là `Display`: 
định dạng đầu ra dành cho việc tiêu thụ trực tiếp bởi người dùng cuối. Các kiểu dữ liệu 
nguyên thủy mà chúng ta đã thấy cho đến nay mặc định triển khai Display vì chỉ có một 
cách bạn muốn hiển thị một 1 hoặc bất kỳ loại nguyên thủy nào khác cho người dùng. Nhưng với struct, 
cách `println!` nên định dạng đầu ra là không rõ ràng hơn vì có nhiều khả năng hiển 
thị: bạn có muốn có dấu phẩy hay không? Bạn muốn in các dấu ngoặc nhọn không? Tất 
cả các trường có nên được hiển thị không? Do sự mơ hồ này, Rust không cố gắng đoán xem 
chúng ta muốn gì, và structs không có một triển khai Display được cung cấp để sử dụng 
với `println!` và `{}` placeholder.

Nếu chúng ta tiếp tục đọc lỗi, chúng ta sẽ thấy một ghi chú hữu ích này:

```text
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-11/output.txt:9:10}}
```

Hãy thử nó xem! Lời gọi macro `println!` bây giờ sẽ trông như `println!("rect1 is {:?}", rect1);`. 
Đặt specifier `:?` bên trong dấu ngoặc nhọn cho biết cho `println!` rằng chúng ta muốn 
sử dụng một định dạng đầu ra được gọi là `Debug`. Trait `Debug` cho phép chúng ta 
in ra struct của mình một cách hữu ích cho các nhà phát triển để chúng ta có thể 
thấy giá trị của nó trong khi chúng ta đang gỡ lỗi mã của mình.

Biên dịch mã với thay đổi này. Cmn :D! Chúng ta vẫn gặp một lỗi:

```text
{{#include ../listings/ch05-using-structs-to-structure-related-data/output-only-01-debug/output.txt:3}}
```

Nhưng một lần nữa, trình biên dịch lại cung cấp cho chúng ta một ghi chú hữu ích:

```text
{{#include ../listings/ch05-using-structs-to-structure-related-data/output-only-01-debug/output.txt:9:10}}
```

Rust *thực sự* bao gồm chức năng in ra thông tin gỡ lỗi, nhưng chúng ta phải chọn 
một cách tường minh (opt in) để làm cho chức năng này có sẵn cho struct của chúng ta. 
Để làm điều đó, chúng ta thêm thuộc tính bên ngoài `#[derive(Debug)]` ngay trước 
định nghĩa của struct, như thể hiện trong Đoạn mã 5-12.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-12/src/main.rs}}
```

<span class="caption">Listing 5-12: Thêm thuộc tính để tạo ra `Debug` trait và 
in ra instance của `Rectangle` sử dụng định dạng gỡ lỗi(Debug).</span>

Now when we run the program, we won’t get any errors, and we’ll see the
following output:

```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/listing-05-12/output.txt}}
```

Tuyệt vời! Đây không phải là output đẹp nhất, nhưng nó hiển thị giá trị của 
tất cả các trường cho thể hiện này, điều này chắc chắn sẽ giúp trong quá trình gỡ lỗi. 
Khi chúng ta có các struct lớn hơn, việc có output dễ đọc hơn một chút 
sẽ hữu ích; trong những trường hợp đó, chúng ta có thể sử dụng `{:#?}` 
thay vì `{:?}` trong chuỗi `println!`. Trong ví dụ này, sử dụng kiểu `{:#?}` sẽ 
xuất ra như sau:

```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/output-only-02-pretty-debug/output.txt}}
```

Một cách khác để in ra một giá trị sử dụng định dạng `Debug` là sử dụng [dbg!
macro][dbg]<!-- ignore -->, nó lấy quyền sở hữu của một biểu thức (so với
`println!`, nó lấy một tham chiếu), in ra số dòng và tên tệp của nơi cuộc gọi 
macro `dbg!` xuất hiện trong mã của bạn cùng với giá trị kết quả của biểu thức 
đó và trả lại quyền sở hữu của giá trị.

> Lưu ý: Gọi macro `dbg!` in ra luồng console lỗi tiêu chuẩn (`stderr`), khác với 
> `println!`, nó in ra luồng console đầu ra tiêu chuẩn (`stdout`). Chúng ta sẽ 
> thảo luận thêm về `stderr` và `stdout` trong phần ["Viết các Thông báo Lỗi vào 
> Luồng Lỗi Tiêu Chuẩn Thay vì Luồng Đầu Ra Tiêu Chuẩn" trong Chương 12][err]<!-- ignore -->.

Dưới đây là một ví dụ trong đó chúng ta quan tâm đến giá trị được gán cho 
trường `width`, cũng như giá trị của toàn bộ struct trong `rect1`:

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-05-dbg-macro/src/main.rs}}
```

Chúng ta có thể đặt `dbg!` xung quanh biểu thức `30 * scale` và, vì `dbg!`
trả lại quyền sở hữu của giá trị của biểu thức, trường `width` sẽ có giá trị
giống như khi chúng ta không có lời gọi `dbg!` ở đó. Chúng ta không muốn
`dbg!` sở hữu `rect1`, nên chúng ta sử dụng một tham chiếu đến `rect1` trong 
lời gọi tiếp theo. Dưới đây là đầu ra của ví dụ này:

```console
{{#include ../listings/ch05-using-structs-to-structure-related-data/no-listing-05-dbg-macro/output.txt}}
```

We can see the first bit of output came from *src/main.rs* line 10 where we’re
debugging the expression `30 * scale`, and its resultant value is `60` (the
`Debug` formatting implemented for integers is to print only their value). The
`dbg!` call on line 14 of *src/main.rs* outputs the value of `&rect1`, which is
the `Rectangle` struct. This output uses the pretty `Debug` formatting of the
`Rectangle` type. The `dbg!` macro can be really helpful when you’re trying to
figure out what your code is doing!

In addition to the `Debug` trait, Rust has provided a number of traits for us
to use with the `derive` attribute that can add useful behavior to our custom
types. Those traits and their behaviors are listed in [Appendix C][app-c]<!--
ignore -->. We’ll cover how to implement these traits with custom behavior as
well as how to create your own traits in Chapter 10. There are also many
attributes other than `derive`; for more information, see [the “Attributes”
section of the Rust Reference][attributes].

Our `area` function is very specific: it only computes the area of rectangles.
It would be helpful to tie this behavior more closely to our `Rectangle` struct
because it won’t work with any other type. Let’s look at how we can continue to
refactor this code by turning the `area` function into an `area` *method*
defined on our `Rectangle` type.

[the-tuple-type]: ch03-02-data-types.html#the-tuple-type
[app-c]: appendix-03-derivable-traits.md
[println]: ../std/macro.println.html
[dbg]: ../std/macro.dbg.html
[err]: ch12-06-writing-to-stderr-instead-of-stdout.html
[attributes]: ../reference/attributes.html
