## Hàm - Functions

Các hàm khá phổ biến trong Rust. Bạn đã gặp một trong những hàm quan quan trọng
nhất của ngôn ngữ: hàm `main`, vốn là điểm bắt đầu cho nhiều chương trình. Bạn cũng đã thấy 
từ khóa `fn`, vốn cho phép bạn khai báo các hàm mới.

Mã Rust dùng *snake case* như quy ước đặt tên hàm và biến, trong đó tất cả các ký tự
được viết chữ thường, các từ cách nhau bởi dấu gạch dưới. Đây là một chương trình 
chứa một ví dụ về cách khai báo hàm:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-16-functions/src/main.rs}}
```

Chúng ta định nghĩa một hàm trong Rust bằng cách dùng từ khóa `fn` theo sau bởi 
tên hàm và một tập các dấu ngoặc tròn. Cặp dấu ngoặc nhọn theo sau đó chỉ ra vị 
trí bắt đầu và kết thúc của thân hàm.

Chúng ta có thể gọi bất kỳ hàm nào ta đã định nghĩa bằng cách gọi tên hàm, theo 
sau bởi cặp dấu ngoặc tròn. Vì `another_function` được định nghĩa trong chương 
trình, nó có thể được gọi từ hàm `main`. Lưu ý là chúng ta định nghĩa `another_function`
*sau* hàm `main` trong file mã nguồn; nhưng tất nhiên ta cũng có thể định nghĩa 
nó trước hàm `main`. Rust không quan tâm bạn định nghĩa hàm ở chỗ nào, miễn sao
bạn viết nó trong phạm vi mà từ nơi gọi có thể truy cập đến được.

Hãy tạo một dự án có tên *functions* để khám phá thêm về các hàm. Đặt hàm ví dụ 
`another_function` trong file *src/main.rs* và chạy nó. Bạn sẽ thấy nội dung sau
được xuất ra:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-16-functions/output.txt}}
```

Các dòng được thực thi theo thứ tự chúng xuất hiện trong hàm `main`. Đầu tiên,
chuỗi "Hello, world!" được xuất ra màn hình, tiếp theo đó `another_function` 
được gọi và nội dung của nó được in ra.

### Tham số

Chúng ta có thể định nghĩa hàm với các *parameters* (tham số), là những biến đặc biệt như là 
một phần của chữ ký của hàm. Nếu một hàm có tham số, bạn có thể cung cấp cho nó
các giá trị cụ thể khi gọi. Về mặt kỹ thuật, những giá trị cụ thể mà bạn cung cấp
sẽ được gọi là các *argument* (đối số), tuy nhiên trong thực tế sử dụng, chúng 
ta thường dùng hai từ này lẫn lộn với nhau.

Trong phiên bản này của `another_function` chúng ta thêm một tham số:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/src/main.rs}}
```

Khi thử chạy chương trình; bạn sẽ nhận được kết xuất sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/output.txt}}
```

Trong phần khai báo hàm `another_function` ta có thêm một tham số được đặt tên `x`. 
Kiểu của `x` là `i32`. Khi truyền giá trị `5` cho `another_function`, macro 
`println!` sẽ in ra giá trị `5` ngay tại vị trí chứa `x` được bao bởi cặp dấu ngoặc
nhọn trong chuỗi định dạng.

Trong chữ ký của hàm (thành phần phân biệt các hàm với nhau), bạn *phải* khai báo
kiểu của mỗi tham số. Đây là một quyết định cẩn trọng trong thiết kế của Rust:
yêu cầu khai báo kiểu khi định nghĩa hàm đồng nghĩa với việc trình biên dịch sẽ 
không bao giờ cần bạn phải dùng một tham số để có thể xác định được kiểu dữ liệu 
của tham số đó. Trình dịch cũng sẽ có thể đưa ra các thông báo hữu ích hơn khi nó 
biết chính xác kiểu dữ liệu mà hàm đó cần.

Khi định nghĩa nhiều tham số, bạn cần phân cách chúng bằng dấu phẩy, giống như 
sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/src/main.rs}}
```

Ví dụ này tạo ra một hàm có tên `print_labeled_measurement` với hai tham số.
Tham số đầu tiên được đặt tên `value` và là một `i32`. Tham số thứ hai có tên 
`unit_label` và có kiểu `char`. Hàm này sau đó in ra một dòng văn bản chứa cả hai
`value` và `unit_label`.

Hãy thử chạy đoạn code này. Thay thế chương trình *src/main.rs* hiện có trong 
dự án *functions* với ví dụ phía trên và chạy dùng `cargo run`:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/output.txt}}
```

Vì chúng ta gọi hàm với `5` như là giá trị của `value` và `'h'` là giá trị của 
`unit_label`, chương trình sẽ xuất ra các giá trị đó.

### Các phát biểu (statement) và biểu thức (expression)

Thân hàm được tạo bởi một chuỗi các phát biểu, và có thể kết thúc bằng một biểu thức.
Cho tới hiện tại, các hàm chúng ta đã làm qua chưa bao gồm biểu thức kết thúc, 
nhưng bạn cũng đã gặp các biểu thức là một phần của một phát biểu. Vì Rust là một
ngôn ngữ dựa trên các phát biểu, đây là một sự phân biệt quan trọng để hiểu. Các ngôn
ngữ khác không có sự phân biệt tương tự như vậy, do vậy ta hãy cùng xem qua các phát 
biểu và biểu thức cũng như sự khác biệt giữa chúng đã ảnh hưởng đến thân các hàm như
thế nào.

*Các phát biểu* là các câu lệnh mà nó sẽ thực hiện một số hành động nào đó và không 
trả về giá trị. *Các biểu thức* có trả về giá trị. Hãy cũng xem qua một số ví dụ.

Chúng ta đã từng thực sự dùng đến các phát biểu và biểu thức. Việc tạo ra một biến và gán một 
giá trị cho nó với từ khóa `let` là một phát biểu. Trong Listing 3-1, `let y = 6;` là một
phát biểu.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-01/src/main.rs}}
```

<span class="caption">Listing 3-1: Một hàm `main` đang khai báo một phát biểu</span>

Các khai báo hàm cũng là các phát biểu; toàn bộ ví dụ trên bản thân nó cũng là một phát biểu.

Các phát biểu không trả về giá trị. Do vậy, bạn không thể gán một phát biểu `let`
cho một biến khác, bạn sẽ gặp lỗi nếu thử làm như trong ví dụ sau:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/src/main.rs}}
```

Khi chạy chương trình này, lỗi bạn gặp sẽ giống như sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/output.txt}}
```

Phát biểu `let y = 6` không trả về một giá trị, do vậy chẳng có gì để đưa vào
biến `x`. Điều này khác với các ngôn ngữ khác như C hoặc Ruby, khi các một câu
lệnh gán sẽ trả về giá trị bằng giá trị được gán. Trong các ngôn ngữ đó, bạn có 
thể viết `x = y = 6` và làm cho cả hai `x` và `y` mang cùng giá trị `6`; điều 
này không xảy ra trong Rust.

Các biểu thức xác định giá trị và tạo nên hầu hết các đoạn lệnh bạn viết trong
Rust. Hãy xem qua một phép toán, như `5 + 6`, đó là một biểu thức và trả về 
giá trị `11`. Các biểu thức cũng có thể là một phần của các phát biểu: Trong 
Listing 3-1, giá trị `6`  trong phát biểu `let y = 6;` là một biểu thức và trả
về giá trị `6`. Việc gọi một hàm cũng là một biểu thức, gọi một macro cũng là
một biểu thức. Một khối lệnh mới được tạo bởi một cặp ngoặc nhọn cũng là một 
biểu thức, giống như trong ví dụ sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-20-blocks-are-expressions/src/main.rs}}
```

Biểu thức này:

```rust,ignore
{
    let x = 3;
    x + 1
}
```

là một khối lệnh, mà trong trường hợp này trả về giá trị `4`. Giá trị này được
gán vào cho `y` như một phần của phát biểu `let`. Lưu ý là `x + 1` line không 
có một dấu chấm phẩy (;) ở cuối, không như hầu hết các dòng lệnh bạn đã từng thấy.
Các biểu thức không có dấu chấm phẩy kết thúc. Nếu bạn thêm dấu chấm phẩy, bạn sẽ
biến nó thành một phát biểu, và nó sẽ không trả về giá trị. Hãy lưu ý điều này khi 
bạn xem đến phần kế tiếp nói về giá trị và biểu thức trả về của hàm.

### Functions with Return Values

Functions can return values to the code that calls them. We don’t name return
values, but we must declare their type after an arrow (`->`). In Rust, the
return value of the function is synonymous with the value of the final
expression in the block of the body of a function. You can return early from a
function by using the `return` keyword and specifying a value, but most
functions return the last expression implicitly. Here’s an example of a
function that returns a value:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/src/main.rs}}
```

There are no function calls, macros, or even `let` statements in the `five`
function—just the number `5` by itself. That’s a perfectly valid function in
Rust. Note that the function’s return type is specified too, as `-> i32`. Try
running this code; the output should look like this:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/output.txt}}
```

The `5` in `five` is the function’s return value, which is why the return type
is `i32`. Let’s examine this in more detail. There are two important bits:
first, the line `let x = five();` shows that we’re using the return value of a
function to initialize a variable. Because the function `five` returns a `5`,
that line is the same as the following:

```rust
let x = 5;
```

Second, the `five` function has no parameters and defines the type of the
return value, but the body of the function is a lonely `5` with no semicolon
because it’s an expression whose value we want to return.

Let’s look at another example:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-22-function-parameter-and-return/src/main.rs}}
```

Running this code will print `The value of x is: 6`. But if we place a
semicolon at the end of the line containing `x + 1`, changing it from an
expression to a statement, we’ll get an error.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/src/main.rs}}
```

Compiling this code produces an error, as follows:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/output.txt}}
```

The main error message, “mismatched types,” reveals the core issue with this
code. The definition of the function `plus_one` says that it will return an
`i32`, but statements don’t evaluate to a value, which is expressed by `()`,
the unit type. Therefore, nothing is returned, which contradicts the function
definition and results in an error. In this output, Rust provides a message to
possibly help rectify this issue: it suggests removing the semicolon, which
would fix the error.
