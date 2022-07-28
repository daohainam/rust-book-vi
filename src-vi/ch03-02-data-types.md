## Các kiểu dữ liệu

Mọi giá trị trong Rust đều thuộc một *kiểu dữ liệu* cụ thể, điều này cho Rust biết loại
dữ liệu đang được chỉ định để nó biết cách làm việc với dữ liệu đó. Chúng ta sẽ xem xét
hai tập con kiểu dữ liệu: scalar (vô hướng) và compound (phức).

Hãy nhớ rằng Rust là một ngôn ngữ *xác định kiểu*, có nghĩa là nó
phải biết kiểu dữ liệu của tất cả các biến tại thời điểm biên dịch. Trình biên dịch cũng có thể
suy ra kiểu dữ liệu ta muốn sử dụng dựa trên giá trị và cách chúng ta sử dụng nó. Trong các trường hợp
khi có thể có nhiều kiểu, chẳng hạn như khi chúng ta chuyển đổi `String` thành một số bằng 
cách sử dụng `parse` trong [“Comparing the Guess to the Secret
Number”][comparing-the-guess-to-the-secret-number]<!-- ignore --> như trong
Chương 2, chúng ta phải thêm một chú thích kiểu giống như sau:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

Nếu chúng ta không thêm chú thích về kiểu `: u32` như ở trên, Rust sẽ hiển thị
lỗi sau, có nghĩa là trình biên dịch cần thêm thông tin từ chúng ta để
biết chính xác kiểu dữ liệu nào chúng ta muốn sử dụng:

```console
{{#include ../listings/ch03-common-programming-concepts/output-only-01-no-type-annotations/output.txt}}
```

Bạn sẽ thấy các chú thích kiểu khác nhau cho những kiểu dữ liệu khác nhau.

### Các kiểu vô hướng (Scalar type)

Một kiểu vô hướng biểu diễn một giá trị đơn. Rust có bốn kiểu vô hướng chính: số nguyên (integer), 
các kiểu số dấu chấm động, boolean và kiểu ký tự. Bạn có thể thấy chúng cũng tương tự
trong các ngôn ngữ lập trình khác. Hãy cùng xem thử trong Rust chúng hoạt động thế nào.

#### Các kiểu số nguyên (integer)

Một *integer* là một số không có phần thập phân. Chúng ta đã từng dùng một kiểu integer 
trong chương 2, kiểu `u32`. Việc khai báo kiểu này chỉ ra giá trị mà nó kết hợp phải 
là một số nguyên không dấu (các kiểu có dấu sẽ bắt đầu bằng chữ `i` thay vì `u`), và nó 
chiếm 32 bit không gian nhớ. Bảng 3-1 trình bày các kiểu integer được hỗ trợ sẵn bởi Rust.
Chúng ta có thể dùng bất kỳ biến thể nào trong danh sách để khai báo một kiểu nguyên.

<span class="caption">Table 3-1: Các kiểu số nguyên trong Rust</span>

| Length  | Signed  | Unsigned |
|---------|---------|----------|
| 8-bit   | `i8`    | `u8`     |
| 16-bit  | `i16`   | `u16`    |
| 32-bit  | `i32`   | `u32`    |
| 64-bit  | `i64`   | `u64`    |
| 128-bit | `i128`  | `u128`   |
| arch    | `isize` | `usize`  |

Mỗi biến thể sẽ là có hoặc không có dấu, đồng thời sẽ có một kích thước cụ thể.
*Signed* và *unsigned* chỉ ra liệu một kiểu có thể chứa số âm hay không, hay nói cách khác
ta có cần viết dấu cho nó hay không (khi nó chứa một giá trị âm). Nó cũng hoàn toàn tương
tự khi bạn viết ra giấy: Nếu dấu là quan trọng, bạn cần viết rõ con số với dấu cộng hoặc trừ;
tuy nhiên khi nó an toàn để xác định đây là một số dương, bạn có thể bỏ qua và không cần viết
dấu.
Các số âm được lưu trữ dưới dạng biểu diễn [two’s
complement](https://en.wikipedia.org/wiki/Two%27s_complement)<!-- ignore -->.

Mỗi một biến thể có dấu có thể lưu các con số từ -(2<sup>n - 1</sup>) to 2<sup>n -
1</sup> - 1, với *n* là số bit mà biến thể đó dùng. Như vậy một biến `i8` có thể 
lưu các giá trị từ -(2<sup>7</sup>) đến 2<sup>7</sup> - 1, tương ứng với -128 đến 127.
Các biến thể không dấu có thể lưu các giá trị từ 0 đến 2<sup>n</sup> - 1, do vậy 
một biến `u8` có thể lưu được từ 0 đến 2<sup>8</sup> - 1, tương ứng với 0 đến 255.

Thêm vào đó, các kiểu `isize` và `usize` sẽ phụ thuộc vào kiến trúc hệ thống mà 
chương trình chạy trên đó (được ghi trong bảng với tên “arch”):  64 bit nếu đang 
chạy trên kiến trúc 64 bit và 32 bit nếu chạy trên kiến trúc 32 bit.

Bạn có thể viết các giá trị kiểu số nguyên theo bất kỳ dạng nào trong bảng 3-2. Lưu ý 
là nếu một giá trị số nguyên có thể thuộc nhiều kiểu dữ liệu khác nhau thì bạn cũng có
thể chỉ rõ ra một kiểu cụ thể nào đó, ví dụ `57u8`. Các giá trị số cũng có thể dùng 
dấu gạch dưới như một ký hiệu phân cách dễ dễ nhìn hơn, như là `1_000`, sẽ có cùng 
giá trị với `1000`.

<span class="caption">Table 3-2: Các giá trị số nguyên trong Rust</span>

| Các giá trị      | Ví dụ         |
|------------------|---------------|
| Decimal          | `98_222`      |
| Hex              | `0xff`        |
| Octal            | `0o77`        |
| Binary           | `0b1111_0000` |
| Byte (`u8` only) | `b'A'`        |

Vậy làm sao bạn biết được kiểu số nguyên nào để dùng? Nếu không chắc thì hãy nhớ, 
mặc nhiên trong Rust các kiểu số nguyên sẽ là `i32`. Một trường hợp mà bạn có lẽ 
sẽ dùng tới `isize` hay `usize` là khi sử dụng chỉ số trong các tập hợp.

> ##### Tràn số
>
> Lấy ví dụ bạn có một biến kiểu `u8` có thể chứa được giá trị từ 0 đến 255. Nếu 
> bạn cố thay đổi giá trị của biến thành một giá trị ngoài phạm vi đó, chẳng hạn 
> 256, *integer overflow* (lỗi tràn số) sẽ xảy ra, và nó sẽ dẫn đến một trong hai
> trạng thái. Nếu bạn dịch code ở chế độ debug, Rust sẽ thêm vào các phép kiểm tra
> tràn số và khiến chương trình của bạn bị dừng lại và trả về một lỗi nghiêm trọng, 
> Rust gọi việc dừng lại này là panicking; chúng ta sẽ thảo luận thêm về trong phần 
> [“Unrecoverable Errors with `panic!`”][unrecoverable-errors-with-panic]<!-- ignore --> 
> ở chương 9.
>
> - Wrap in all modes with the `wrapping_*` methods, such as `wrapping_add`
> - Return the `None` value if there is overflow with the `checked_*` methods
> - Return the value and a boolean indicating whether there was overflow with
>   the `overflowing_*` methods
> - Saturate at the value’s minimum or maximum values with `saturating_*`
>   methods


> Khi dịch chương trình ở chế độ release với tùy chọn `--release`, Rust *không* 
> thêm vào các phép kiểm tra tràn số gây dừng chương trình. Thay vào đó, nếu xảy ra
> tràn số, Rust sẽ thực hiện *two’s complement wrapping*. Nói một cách ngắn gọn, các 
> giá trị lớn hơn giá trị lớn nhất mà kiểu dữ liệu có thể chứa được sẽ bị "xoay vòng"
> lại từ giá trị nhỏ nhất. Trong trường hợp của `u8`, giá trị 256 sẽ quay vòng lại 0, 
> 257 trở thành 1, và tiếp tục như vậy. Chương trình sẽ không dừng do lỗi, nhưng biến
> có thể chứa một giá trị mà bạn có thể không mong muốn. Khi xảy ra "xoay vòng" lại 
> giá trị, ta có thể coi như một lỗi.
>
> Bạn có thể dùng các nhóm phương thức sau nếu muốn xử lý việc tràn số, các nhóm phương 
> thức này hỗ trợ các kiểu dữ liêu số nguyên thủy và được cung cấp bởi thư viện chuẩn.

#### Các kiểu số dấu chấm động

Rust cũng có hai kiểu nguyên thủy cho các *số dấu chấm động* (floating-point numbers),
là các kiểu số có phần thập phân. Các kiểu số dấu chấm động của Rust gồm có `f32` và `f64`,
tương ứng với các kích cỡ 32 bit và 64 bit. Kiểu mặc nhiên là `f64`, vì trên các bộ xử lý 
hiện đại tốc độ xử lý của nó tương đương với `f32` nhưng có độ chính xác cao hơn. Tất cả 
các kiểu dấu chấm động đều là có dấu.

Đây là một ví dụ về việc dùng các dấu chấm động:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-06-floating-point/src/main.rs}}
```

Các kiểu dấu chấm động được biểu diễn dựa trên tiêu chuẩn IEEE-754. Kiểu `f32` là 
kiểu dấu chấm động chính xác đơn, và `f64` có độ chính xác kép.

#### Các toán tử số

Rust hỗ trợ các phép toán toán học cơ bản: cộng, trừ, nhân, chia và lấy phần dư. 
Các kiểu số nguyên khi chia sẽ trả về một số nguyên gần nhất với thương. Đoạn code
sau đây biểu diễn cách bạn sẽ dùng các toán tử trong một phát biểu `let`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-07-numeric-operations/src/main.rs}}
```

Mỗi biểu thức trong các phát biểu đó dùng một toán tử toán học và trả về một giá trị 
đơn, giá trị này sau đó lại được gán cho một biến. [Appendix B][appendix_b]<!-- ignore --> chứa
một danh sách tất cả các toán tử có trong Rust.

#### Kiểu Boolean 

Tương tự trong các ngôn ngữ lập trình khác, một kiểu Boolean có thể chứa một trong 
hai giá trị `true` và `false`. Boolean có kích cỡ một byte. Một biếu kiểu Boolean trong Rust 
được khai báo sử dụng `bool`. Ví dụ:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-08-boolean/src/main.rs}}
```

Các chính để dùng Boolean là thông qua các điều kiện, kiểu như phát biểu `if`.
Chúng ta sẽ xem thêm về cách `if` làm việc trong Rust trong phần [“Control
Flow”][control-flow]<!-- ignore -->.

#### Kiểu ký tự

Kiểu `char` trong Rust là kiểu ký tự nguyên thủy nhất. Sau đây là một số ví dụ về 
cách khai báo các giá trị kiểu `char`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-09-char/src/main.rs}}
```

Note that we specify `char` literals with single quotes, as opposed to string
literals, which use double quotes. Rust’s `char` type is four bytes in size and
represents a Unicode Scalar Value, which means it can represent a lot more than
just ASCII. Accented letters; Chinese, Japanese, and Korean characters; emoji;
and zero-width spaces are all valid `char` values in Rust. Unicode Scalar
Values range from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF` inclusive.
However, a “character” isn’t really a concept in Unicode, so your human
intuition for what a “character” is may not match up with what a `char` is in
Rust. We’ll discuss this topic in detail in [“Storing UTF-8 Encoded Text with
Strings”][strings]<!-- ignore --> in Chapter 8.

### Compound Types

*Compound types* can group multiple values into one type. Rust has two
primitive compound types: tuples and arrays.

#### The Tuple Type

A tuple is a general way of grouping together a number of values with a variety
of types into one compound type. Tuples have a fixed length: once declared,
they cannot grow or shrink in size.

We create a tuple by writing a comma-separated list of values inside
parentheses. Each position in the tuple has a type, and the types of the
different values in the tuple don’t have to be the same. We’ve added optional
type annotations in this example:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-10-tuples/src/main.rs}}
```

The variable `tup` binds to the entire tuple, because a tuple is considered a
single compound element. To get the individual values out of a tuple, we can
use pattern matching to destructure a tuple value, like this:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-11-destructuring-tuples/src/main.rs}}
```

This program first creates a tuple and binds it to the variable `tup`. It then
uses a pattern with `let` to take `tup` and turn it into three separate
variables, `x`, `y`, and `z`. This is called *destructuring*, because it breaks
the single tuple into three parts. Finally, the program prints the value of
`y`, which is `6.4`.

We can also access a tuple element directly by using a period (`.`) followed by
the index of the value we want to access. For example:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-12-tuple-indexing/src/main.rs}}
```

This program creates the tuple `x` and then accesses each element of the tuple
using their respective indices. As with most programming languages, the first
index in a tuple is 0.

The tuple without any values has a special name, *unit*. This value and its
corresponding type are both written `()` and represent an empty value or an
empty return type. Expressions implicitly return the unit value if they don’t
return any other value.

#### The Array Type

Another way to have a collection of multiple values is with an *array*. Unlike
a tuple, every element of an array must have the same type. Unlike arrays in
some other languages, arrays in Rust have a fixed length.

We write the values in an array as a comma-separated list inside square
brackets:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-13-arrays/src/main.rs}}
```

Arrays are useful when you want your data allocated on the stack rather than
the heap (we will discuss the stack and the heap more in [Chapter
4][stack-and-heap]<!-- ignore -->) or when you want to ensure you always have a
fixed number of elements. An array isn’t as flexible as the vector type,
though. A vector is a similar collection type provided by the standard library
that *is* allowed to grow or shrink in size. If you’re unsure whether to use an
array or a vector, chances are you should use a vector. [Chapter
8][vectors]<!-- ignore --> discusses vectors in more detail.

However, arrays are more useful when you know the number of elements will not
need to change. For example, if you were using the names of the month in a
program, you would probably use an array rather than a vector because you know
it will always contain 12 elements:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

You write an array’s type using square brackets with the type of each element,
a semicolon, and then the number of elements in the array, like so:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Here, `i32` is the type of each element. After the semicolon, the number `5`
indicates the array contains five elements.

You can also initialize an array to contain the same value for each element by
specifying the initial value, followed by a semicolon, and then the length of
the array in square brackets, as shown here:

```rust
let a = [3; 5];
```

The array named `a` will contain `5` elements that will all be set to the value
`3` initially. This is the same as writing `let a = [3, 3, 3, 3, 3];` but in a
more concise way.

##### Accessing Array Elements

An array is a single chunk of memory of a known, fixed size that can be
allocated on the stack. You can access elements of an array using indexing,
like this:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-14-array-indexing/src/main.rs}}
```

In this example, the variable named `first` will get the value `1`, because
that is the value at index `[0]` in the array. The variable named `second` will
get the value `2` from index `[1]` in the array.

##### Invalid Array Element Access

Let’s see what happens if you try to access an element of an array that is past
the end of the array. Say you run this code, similar to the guessing game in
Chapter 2, to get an array index from the user:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,panics
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access/src/main.rs}}
```

This code compiles successfully. If you run this code using `cargo run` and
enter 0, 1, 2, 3, or 4, the program will print out the corresponding value at
that index in the array. If you instead enter a number past the end of the
array, such as 10, you’ll see output like this:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access
cargo run
10
-->

```console
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

The program resulted in a *runtime* error at the point of using an invalid
value in the indexing operation. The program exited with an error message and
didn’t execute the final `println!` statement. When you attempt to access an
element using indexing, Rust will check that the index you’ve specified is less
than the array length. If the index is greater than or equal to the length,
Rust will panic. This check has to happen at runtime, especially in this case,
because the compiler can’t possibly know what value a user will enter when they
run the code later.

This is an example of Rust’s memory safety principles in action. In many
low-level languages, this kind of check is not done, and when you provide an
incorrect index, invalid memory can be accessed. Rust protects you against this
kind of error by immediately exiting instead of allowing the memory access and
continuing. Chapter 9 discusses more of Rust’s error handling and how you can
write readable, safe code that neither panics nor allows invalid memory access.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[control-flow]: ch03-05-control-flow.html#control-flow
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[stack-and-heap]: ch04-01-what-is-ownership.html#the-stack-and-the-heap
[vectors]: ch08-01-vectors.html
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[wrapping]: ../std/num/struct.Wrapping.html
[appendix_b]: appendix-02-operators.md
