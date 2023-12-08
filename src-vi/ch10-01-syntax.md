## Các kiểu dữ liệu Generic

Chúng ta sử dụng generics để tạo định nghĩa cho các mục như chữ ký hàm hoặc 
các struct, mà chúng ta sau đó có thể sử dụng với nhiều loại dữ liệu cụ thể 
khác nhau. Hãy trước hết xem cách định nghĩa hàm, struct, enum và các phương 
thức bằng generics. Sau đó, chúng ta sẽ thảo luận về cách generics ảnh hưởng 
đến hiệu suất của code.

### Trong Định nghĩa Hàm

Khi định nghĩa một hàm sử dụng generics, chúng ta đặt generics trong chữ ký 
của hàm nơi chúng ta thường chỉ định loại dữ liệu của các tham số và giá trị 
trả về. Việc này làm cho code của chúng ta linh hoạt hơn và cung cấp thêm chức 
năng cho người gọi hàm của chúng ta trong khi ngăn chặn việc trùng lặp code.

Tiếp tục với hàm `largest` của chúng ta, Listing 10-4 hiển thị hai hàm cả hai 
đều tìm giá trị lớn nhất trong một slice. Sau đó, chúng ta sẽ kết hợp chúng 
thành một hàm duy nhất sử dụng generics.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-04/src/main.rs:here}}
```

<span class="caption">Listing 10-4: Two functions that differ only in their
names and the types in their signatures</span>

Hàm `largest_i32` là hàm chúng ta đã trích xuất ở Listing 10-3 để tìm giá trị 
lớn nhất của `i32` trong một slice. Hàm `largest_char` tìm giá trị lớn nhất của `char` 
trong một slice. Cả hai hàm có cùng mã nguồn, vì vậy hãy loại bỏ sự trùng lặp 
bằng cách giới thiệu một tham số kiểu generic trong một hàm duy nhất.

Để tham số hóa các loại trong một hàm mới, chúng ta cần đặt tên tham số kiểu, 
giống như chúng ta làm với các tham số giá trị của một hàm. Bạn có thể sử dụng 
bất kỳ định danh nào làm tên tham số kiểu. Nhưng chúng ta sẽ sử dụng `T` vì theo 
quy ước, tên tham số kiểu trong Rust ngắn gọn, thường chỉ là một chữ cái, và 
quy ước đặt tên kiểu của Rust là CamelCase. Rút gọn từ “type,” `T` là lựa chọn 
mặc định của hầu hết các lập trình viên Rust.

Khi chúng ta sử dụng một tham số trong thân của hàm, chúng ta phải khai báo tên 
tham số trong chữ ký để trình biên dịch biết nghĩa của tên đó là gì. Tương tự, khi 
chúng ta sử dụng tên tham số kiểu trong chữ ký hàm, chúng ta phải khai báo tên 
tham số kiểu trước khi sử dụng nó. Để định nghĩa hàm generic `largest`, đặt khai 
báo tên kiểu bên trong dấu ngoặc nhọn, `<>`, giữa tên hàm và danh sách tham số, như sau:

```rust,ignore
fn largest<T>(list: &[T]) -> &T {
```

Chúng ta đọc định nghĩa này như sau: hàm `largest` là generic qua một loại T 
nào đó. Hàm này có một tham số có tên là `list`, là một slice của các giá trị 
kiểu `T`. Hàm `largest` sẽ trả về một tham chiếu đến một giá trị cùng kiểu `T`.

Listing 10-5 cho thấy định nghĩa hàm `largest` kết hợp sử dụng kiểu dữ liệu 
generic trong chữ ký của nó. Listing cũng cho thấy cách chúng ta có thể gọi 
hàm với một slice của giá trị `i32` hoặc giá trị `char`. Lưu ý rằng đoạn code này 
chưa thể biên dịch được, nhưng chúng ta sẽ sửa nó sau trong chương này.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-05/src/main.rs}}
```

<span class="caption">Listing 10-5: Hàm `largest` sử dụng các tham số kiểu 
generic; code này hiện chưa thể biên dịch được</span>

Nếu chúng ta biên dịch đoạn code này ngay bây giờ, chúng ta sẽ nhận được lỗi này:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-05/output.txt}}
```

Phần trợ giúp chỉ đến `std::cmp::PartialOrd`, đó là một *trait*, và chúng ta 
sẽ nói về các traits trong phần tiếp theo. Hiện tại, hãy biết rằng lỗi này 
nói rằng thân của `largest` không hoạt động với tất cả các loại `T` có thể có. 
Vì chúng ta muốn so sánh giá trị của kiểu `T` trong thân hàm, chúng ta chỉ có 
thể sử dụng các loại mà giá trị của chúng có thể được so sánh. Để kích hoạt so 
sánh, thư viện chuẩn có trait `std::cmp::PartialOrd` mà bạn có thể triển khai cho 
các loại (xem Phụ lục C để biết thêm về trait này). Bằng cách tuân theo gợi ý 
của văn bản trợ giúp, chúng ta giới hạn các loại hợp lệ cho `T` chỉ đến những 
loại triển khai PartialOrd, và ví dụ này sẽ biên dịch được, vì thư viện chuẩn 
triển khai `PartialOrd` cho cả `i32` và `char`.

### Trong Các Định Nghĩa Struct

Chúng ta cũng có thể định nghĩa các structs để sử dụng một tham số kiểu generic 
trong một hoặc nhiều trường bằng cú pháp `<>`. Listing 10-6 định nghĩa một 
struct `Point<T>` để chứa các giá trị tọa độ `x` và `y` của bất kỳ kiểu nào.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-06/src/main.rs}}
```

<span class="caption">Listing 10-6: Một struct `Point<T>` chứa các giá trị x 
và y của kiểu `T`</span>

Cú pháp sử dụng generics trong định nghĩa struct tương tự như cú pháp 
được sử dụng trong định nghĩa hàm. Trước tiên, chúng ta khai báo tên 
của tham số kiểu bên trong dấu ngoặc nhọn ngay sau tên của struct. Sau 
đó, chúng ta sử dụng kiểu generic trong định nghĩa struct ở những nơi 
chúng ta thông thường sẽ chỉ định kiểu dữ liệu cụ thể.

Lưu ý rằng vì chúng ta đã sử dụng chỉ một kiểu generic để định nghĩa `Point<T>`, 
định nghĩa này nói rằng struct `Point<T>` là generic trên một loại `T`, và các 
trường `x` và `y` đều là cùng một kiểu đó, bất kể kiểu đó là gì. Nếu chúng ta 
tạo một thể hiện của `Point<T>` có giá trị của các kiểu khác nhau, như trong 
Listing 10-7, mã nguồn của chúng ta sẽ không biên dịch được.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-07/src/main.rs}}
```

<span class="caption">Listing 10-7: Các trường `x` and `y` phải có cùng kiểu vì chúng 
sử dụng cùng kiểu generic T</span>

Trong ví dụ này, khi chúng ta gán giá trị số nguyên 5 cho `x`, chúng ta thông 
báo cho trình biên dịch biết rằng kiểu generic T sẽ là một số nguyên cho 
instance này của `Point<T>`. Sau đó, khi chúng ta chỉ định giá trị 4.0 cho 
`y`, mà chúng ta đã định nghĩa có cùng kiểu với x, chúng ta sẽ nhận được một 
lỗi không khớp kiểu như sau:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-07/output.txt}}
```

To define a `Point` struct where `x` and `y` are both generics but could have
different types, we can use multiple generic type parameters. For example, in
Listing 10-8, we change the definition of `Point` to be generic over types `T`
and `U` where `x` is of type `T` and `y` is of type `U`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-08/src/main.rs}}
```

<span class="caption">Listing 10-8: Một struct `Point<T, U>` generic trên hai 
kiểu để `x` và `y` có thể là giá trị của các kiểu khác nhau</span>

Bây giờ tất cả các thể hiện của `Point` được hiển thị đều được chấp nhận! 
Bạn có thể sử dụng nhiều tham số kiểu generic trong định nghĩa càng nhiều 
càng tốt, nhưng sử dụng quá nhiều có thể làm cho mã nguồn của bạn khó đọc. 
Nếu bạn phát hiện bạn cần nhiều kiểu generic trong mã nguồn của mình, 
điều này có thể là dấu hiệu cho thấy mã nguồn của bạn cần được tổ chức 
lại thành các phần nhỏ hơn.

### In Enum Definitions

As we did with structs, we can define enums to hold generic data types in their
variants. Let’s take another look at the `Option<T>` enum that the standard
library provides, which we used in Chapter 6:

```rust
enum Option<T> {
    Some(T),
    None,
}
```

This definition should now make more sense to you. As you can see, the
`Option<T>` enum is generic over type `T` and has two variants: `Some`, which
holds one value of type `T`, and a `None` variant that doesn’t hold any value.
By using the `Option<T>` enum, we can express the abstract concept of an
optional value, and because `Option<T>` is generic, we can use this abstraction
no matter what the type of the optional value is.

Enums can use multiple generic types as well. The definition of the `Result`
enum that we used in Chapter 9 is one example:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

The `Result` enum is generic over two types, `T` and `E`, and has two variants:
`Ok`, which holds a value of type `T`, and `Err`, which holds a value of type
`E`. This definition makes it convenient to use the `Result` enum anywhere we
have an operation that might succeed (return a value of some type `T`) or fail
(return an error of some type `E`). In fact, this is what we used to open a
file in Listing 9-3, where `T` was filled in with the type `std::fs::File` when
the file was opened successfully and `E` was filled in with the type
`std::io::Error` when there were problems opening the file.

When you recognize situations in your code with multiple struct or enum
definitions that differ only in the types of the values they hold, you can
avoid duplication by using generic types instead.

### In Method Definitions

We can implement methods on structs and enums (as we did in Chapter 5) and use
generic types in their definitions, too. Listing 10-9 shows the `Point<T>`
struct we defined in Listing 10-6 with a method named `x` implemented on it.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-09/src/main.rs}}
```

<span class="caption">Listing 10-9: Implementing a method named `x` on the
`Point<T>` struct that will return a reference to the `x` field of type
`T`</span>

Here, we’ve defined a method named `x` on `Point<T>` that returns a reference
to the data in the field `x`.

Note that we have to declare `T` just after `impl` so we can use `T` to specify
that we’re implementing methods on the type `Point<T>`. By declaring `T` as a
generic type after `impl`, Rust can identify that the type in the angle
brackets in `Point` is a generic type rather than a concrete type. We could
have chosen a different name for this generic parameter than the generic
parameter declared in the struct definition, but using the same name is
conventional. Methods written within an `impl` that declares the generic type
will be defined on any instance of the type, no matter what concrete type ends
up substituting for the generic type.

We can also specify constraints on generic types when defining methods on the
type. We could, for example, implement methods only on `Point<f32>` instances
rather than on `Point<T>` instances with any generic type. In Listing 10-10 we
use the concrete type `f32`, meaning we don’t declare any types after `impl`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-10/src/main.rs:here}}
```

<span class="caption">Listing 10-10: An `impl` block that only applies to a
struct with a particular concrete type for the generic type parameter `T`</span>

This code means the type `Point<f32>` will have a `distance_from_origin`
method; other instances of `Point<T>` where `T` is not of type `f32` will not
have this method defined. The method measures how far our point is from the
point at coordinates (0.0, 0.0) and uses mathematical operations that are
available only for floating point types.

Generic type parameters in a struct definition aren’t always the same as those
you use in that same struct’s method signatures. Listing 10-11 uses the generic
types `X1` and `Y1` for the `Point` struct and `X2` `Y2` for the `mixup` method
signature to make the example clearer. The method creates a new `Point`
instance with the `x` value from the `self` `Point` (of type `X1`) and the `y`
value from the passed-in `Point` (of type `Y2`).

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-11/src/main.rs}}
```

<span class="caption">Listing 10-11: A method that uses generic types different
from its struct’s definition</span>

In `main`, we’ve defined a `Point` that has an `i32` for `x` (with value `5`)
and an `f64` for `y` (with value `10.4`). The `p2` variable is a `Point` struct
that has a string slice for `x` (with value `"Hello"`) and a `char` for `y`
(with value `c`). Calling `mixup` on `p1` with the argument `p2` gives us `p3`,
which will have an `i32` for `x`, because `x` came from `p1`. The `p3` variable
will have a `char` for `y`, because `y` came from `p2`. The `println!` macro
call will print `p3.x = 5, p3.y = c`.

The purpose of this example is to demonstrate a situation in which some generic
parameters are declared with `impl` and some are declared with the method
definition. Here, the generic parameters `X1` and `Y1` are declared after
`impl` because they go with the struct definition. The generic parameters `X2`
and `Y2` are declared after `fn mixup`, because they’re only relevant to the
method.

### Performance of Code Using Generics

You might be wondering whether there is a runtime cost when using generic type
parameters. The good news is that using generic types won't make your program run 
any slower than it would with concrete types.

Rust accomplishes this by performing monomorphization of the code using
generics at compile time. *Monomorphization* is the process of turning generic
code into specific code by filling in the concrete types that are used when
compiled. In this process, the compiler does the opposite of the steps we used
to create the generic function in Listing 10-5: the compiler looks at all the
places where generic code is called and generates code for the concrete types
the generic code is called with.

Let’s look at how this works by using the standard library’s generic
`Option<T>` enum:

```rust
let integer = Some(5);
let float = Some(5.0);
```

When Rust compiles this code, it performs monomorphization. During that
process, the compiler reads the values that have been used in `Option<T>`
instances and identifies two kinds of `Option<T>`: one is `i32` and the other
is `f64`. As such, it expands the generic definition of `Option<T>` into two
definitions specialized to `i32` and `f64`, thereby replacing the generic
definition with the specific ones.

The monomorphized version of the code looks similar to the following (the
compiler uses different names than what we’re using here for illustration):

<span class="filename">Filename: src/main.rs</span>

```rust
enum Option_i32 {
    Some(i32),
    None,
}

enum Option_f64 {
    Some(f64),
    None,
}

fn main() {
    let integer = Option_i32::Some(5);
    let float = Option_f64::Some(5.0);
}
```

The generic `Option<T>` is replaced with the specific definitions created by
the compiler. Because Rust compiles generic code into code that specifies the
type in each instance, we pay no runtime cost for using generics. When the code
runs, it performs just as it would if we had duplicated each definition by
hand. The process of monomorphization makes Rust’s generics extremely efficient
at runtime.
