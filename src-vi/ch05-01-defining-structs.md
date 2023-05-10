## Định nghĩa và khởi tạo các cấu trúc

Struct (cấu trúc) tương tự như tuple, được thảo luận trong phần [“The Tuple Type”][tuples]<!--
ignore -->, trong đó cả hai đều chứa những giá trị liên quan. Giống như các tuple, các
các phần của một cấu trúc có thể là các kiểu khác nhau. Không giống như với tuple, trong một cấu trúc
bạn sẽ đặt tên cho từng phần dữ liệu để hiểu rõ ý nghĩa của các giá trị. Việc thêm tên 
cho những thành phần bên trong giúp nó có cấu trúc linh hoạt hơn tuple: bạn không cần phải dựa vào
thứ tự của dữ liệu để truy cập vào các giá trị của một instance (thể hiện).

Để định nghĩa một struct, chúng ta nhập từ khóa `struct` và đặt tên cho toàn bộ struct. 
Tên của một cấu trúc phải mô tả mục đích quan trọng nhất của phần dữ liệu nó chứa. Sau đó, bên trong dấu 
ngoặc nhọn, chúng ta xác định tên và kiểu của các phần dữ liệu mà chúng tôi gọi là *field* (trường). 
Ví dụ, Listing 5-1 cho thấy một struct lưu trữ thông tin về tài khoản người dùng.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-01/src/main.rs:here}}
```

<span class="caption">Listing 5-1: Định nghĩa cấu trúc `User`</span>

Để sử dụng một struct sau khi đã định nghĩa, chúng ta tạo một *instance* của cấu trúc đó
bằng cách chỉ định các giá trị cụ thể cho từng trường. Chúng tôi tạo một ví dụ bằng cách
nêu tên của cấu trúc và sau đó thêm dấu ngoặc nhọn chứa các cặp *key:value*, 
trong đó các key là tên của các field và các value là dữ liệu chúng ta muốn 
lưu trong các trường đó. Chúng ta không phải chỉ định các field theo
cùng thứ tự mà chúng ta đã khai báo chúng trong cấu trúc. Nói cách khác, một
định nghĩa cấu trúc giống như một hình mẫu chung cho một kiểu và các instance 
gán vào trong hình mẫu đó các dữ liệu cụ thể để tạo các giá trị của kiểu. 
Ví dụ, chúng ta có thể khai báo một user cụ thể như trong Listing 5-2.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-02/src/main.rs:here}}
```

<span class="caption">Listing 5-2: Tạo một instance của cấu trúc `User`</span>

Để lấy một giá trị cụ thể từ một struct, chúng ta sử dụng ký hiệu dấu chấm. Ví dụ, để
truy cập địa chỉ email của user này, chúng ta sử dụng `user1.email`. Nếu instance là
mutable (có thể thay đổi), chúng ta có thể thay đổi một giá trị bằng cách sử dụng 
ký hiệu dấu chấm và gán vào một field cụ thể. Listing kê 5-3 cho thấy cách 
thay đổi giá trị trong field `email` của một mutable instance `User`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-03/src/main.rs:here}}
```

<span class="caption">Listing 5-3: Thay đổi giá trị trường `email` của một instance `User`</span>

Lưu ý rằng toàn bộ instance phải mutable; Rust không cho phép chúng ta đánh dấu 
chỉ một số trường nhất định là mutable. Như với bất kỳ biểu thức nào, chúng ta 
có thể tạo một instance mới của cấu trúc với biểu thức cuối cùng trong thân hàm 
để trả lại một cách rõ ràng instance mới đó.

Listing 5-4 biểu diễn một hàm `build_user` trả về một instance kiểu `User` với
email và tên người dùng đã cho. Trường `active` nhận giá trị `true` và
`sign_in_count` nhận giá trị `1`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-04/src/main.rs:here}}
```

<span class="caption">Listing 5-4: Một hàm `build_user` nhận vào một email và
username và trả về một `User` instance</span>

Sẽ là có nghĩa khi đặt tên các tham số của function với tên trùng với tên các 
trường của struct, nhưng việc lặp lại các tên như `email` và `username` cũng sẽ
gây nhàm chán. Nếu struct có thêm nhiều field nữa, việc lặp đi lặp lại chúng sẽ 
còn gây khó chịu hơn. May thay, Rust có một cách viết ngắn gọn!

<!-- Old heading. Do not remove or links may break. -->
<a id="using-the-field-init-shorthand-when-variables-and-fields-have-the-same-name"></a>

### Sử dụng cách viết khởi tạo trường một cách ngắn gọn

Vì tên tham số và tên trường của cấu trúc hoàn toàn giống nhau trong
Listing 5-4, chúng ta có thể sử dụng cú pháp tốc ký *field init* để viết lại
`build_user` giúp nó hoạt động giống hệt nhưng không có sự lặp lại của
`username` và `email`, như trong Listing 5-5.

<span class="filename">Tên tệp: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-05/src/main.rs:here}}
```

<span class="caption">Listing 5-5: Hàm `build_user` sử dụng cách viết tắt để khởi tạo 
vì tham số `username` và `email` có cùng tên với field</span>

Ở đây, chúng ta tạo một instance mới của cấu trúc `User`, có field
tên là `email`. Chúng ta muốn đặt giá trị của `email` thành giá trị trong
tham số `email` của hàm `build_user`. Vì trường `email` và
tham số `email` trùng tên ta chỉ cần viết `email` là được
hơn `email: email`.

### Tạo instance mới từ một instance khác với cú pháp cập nhật cấu trúc 

Một thao tác thường làm là tạo mới một instance của Struct với hầu hết
các giá trị từ một phiên bản khác, với một số thay đổi. Bạn có thể làm điều này bằng cách sử dụng
*cú pháp cập nhật cấu trúc* (struct update syntax).

Đầu tiên, trong Listing 5-6, chúng tôi trình bày cách tạo một instance `User` mới trong `user2`
theo cách thông thường, không dùng cú pháp cập nhật. Chúng tôi đặt một giá trị mới cho `email` nhưng
sử dụng cùng các giá trị khác từ `user1` mà chúng ta đã tạo trong Listing 5-2.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-06/src/main.rs:here}}
```

<span class="caption">Listing 5-6: Tạo một instance `User` mới dùng giá trị từ  `user1`</span>

Sử dụng struct update syntax, chúng ta có thể đạt được cùng mục đích với ít code hơn, như thể
hiện trong Listing 5-7. Cú pháp `..` chỉ định rằng các trường còn lại không
được đặt rõ ràng phải có cùng giá trị với các trường trong instance đã cho.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-07/src/main.rs:here}}
```

<span class="caption">Listing 5-7: Sử dụng struct update syntax để đặt mới
giá trị `email` cho instance `User` nhưng sử dụng phần còn lại của các giá trị từ
`user1`</span>

Đoạn code trong Listing 5-7 cũng tạo một instance trong `user2` với một giá trị khác trong 
`email` nhưng có cùng giá trị trong `username`, `active`, và `sign_in_count` từ`user1`. 
`..user1` phải được viết cuối cùng để chỉ định rằng mọi trường còn lại sẽ nhận giá trị của chúng từ
các trường tương ứng trong `user1`, nhưng chúng ta có thể chọn chỉ định giá trị cho
nhiều trường như chúng ta muốn theo bất kỳ thứ tự nào, bất kể thứ tự của chúng trong
định nghĩa của cấu trúc.

Lưu ý rằng cú pháp cập nhật cấu trúc sử dụng `=` như một phép gán; điều này là bởi vì
nó di chuyển dữ liệu, giống như chúng ta đã thấy trong phần [“Variables and Data Interacting with
Move”][move]<!-- ignore -->. Trong ví dụ này, chúng ta không còn có thể sử dụng
`user1` sau khi tạo `user2` vì `String` trong Trường `username` của `user1` đã 
được chuyển vào `user2`. Nếu chúng ta gán `user2` một giá trị `String` mới cho cả trường `email`
và `username`, có nghĩa là ta chỉ lấy `active` và `sign_in_count` từ `user1`, khi đó 
`user1` vẫn hợp lệ sau khi tạo `user2`. Đó là vì cả hai `active` và `sign_in_count` 
đều implement `Copy` trait, do vậy các hành vi như ta đã thảo luậ trong phần 
[“Stack-Only Data: Copy”][copy]<!-- ignore --> sẽ được áp dụng.

### Sử dụng Tuple struct mà không dùng các trường được đặt tên để tạo các kiểu khác

Rust also supports structs that look similar to tuples, called *tuple structs*.
Tuple structs have the added meaning the struct name provides but don’t have
names associated with their fields; rather, they just have the types of the
fields. Tuple structs are useful when you want to give the whole tuple a name
and make the tuple a different type from other tuples, and when naming each
field as in a regular struct would be verbose or redundant.

To define a tuple struct, start with the `struct` keyword and the struct name
followed by the types in the tuple. For example, here we define and use two
tuple structs named `Color` and `Point`:

Rust cũng hỗ trợ các cấu trúc trông tương tự như các bộ dữ liệu, được gọi là *tuple structs*.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-01-tuple-structs/src/main.rs}}
```

Note that the `black` and `origin` values are different types because they’re
instances of different tuple structs. Each struct you define is its own type,
even though the fields within the struct might have the same types. For
example, a function that takes a parameter of type `Color` cannot take a
`Point` as an argument, even though both types are made up of three `i32`
values. Otherwise, tuple struct instances are similar to tuples in that you can
destructure them into their individual pieces, and you can use a `.` followed
by the index to access an individual value.

### Unit-Like Structs Without Any Fields

You can also define structs that don’t have any fields! These are called
*unit-like structs* because they behave similarly to `()`, the unit type that
we mentioned in [“The Tuple Type”][tuples]<!-- ignore --> section. Unit-like
structs can be useful when you need to implement a trait on some type but don’t
have any data that you want to store in the type itself. We’ll discuss traits
in Chapter 10. Here’s an example of declaring and instantiating a unit struct
named `AlwaysEqual`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-04-unit-like-structs/src/main.rs}}
```

To define `AlwaysEqual`, we use the `struct` keyword, the name we want, and
then a semicolon. No need for curly brackets or parentheses! Then we can get an
instance of `AlwaysEqual` in the `subject` variable in a similar way: using the
name we defined, without any curly brackets or parentheses. Imagine that later
we’ll implement behavior for this type such that every instance of
`AlwaysEqual` is always equal to every instance of any other type, perhaps to
have a known result for testing purposes. We wouldn’t need any data to
implement that behavior! You’ll see in Chapter 10 how to define traits and
implement them on any type, including unit-like structs.

> ### Ownership of Struct Data
>
> In the `User` struct definition in Listing 5-1, we used the owned `String`
> type rather than the `&str` string slice type. This is a deliberate choice
> because we want each instance of this struct to own all of its data and for
> that data to be valid for as long as the entire struct is valid.
>
> It’s also possible for structs to store references to data owned by something
> else, but to do so requires the use of *lifetimes*, a Rust feature that we’ll
> discuss in Chapter 10. Lifetimes ensure that the data referenced by a struct
> is valid for as long as the struct is. Let’s say you try to store a reference
> in a struct without specifying lifetimes, like the following; this won’t work:
>
> <span class="filename">Filename: src/main.rs</span>
>
> <!-- CAN'T EXTRACT SEE https://github.com/rust-lang/mdBook/issues/1127 -->
>
> ```rust,ignore,does_not_compile
> struct User {
>     active: bool,
>     username: &str,
>     email: &str,
>     sign_in_count: u64,
> }
>
> fn main() {
>     let user1 = User {
>         active: true,
>         username: "someusername123",
>         email: "someone@example.com",
>         sign_in_count: 1,
>     };
> }
> ```
>
> The compiler will complain that it needs lifetime specifiers:
>
> ```console
> $ cargo run
>    Compiling structs v0.1.0 (file:///projects/structs)
> error[E0106]: missing lifetime specifier
>  --> src/main.rs:3:15
>   |
> 3 |     username: &str,
>   |               ^ expected named lifetime parameter
>   |
> help: consider introducing a named lifetime parameter
>   |
> 1 ~ struct User<'a> {
> 2 |     active: bool,
> 3 ~     username: &'a str,
>   |
>
> error[E0106]: missing lifetime specifier
>  --> src/main.rs:4:12
>   |
> 4 |     email: &str,
>   |            ^ expected named lifetime parameter
>   |
> help: consider introducing a named lifetime parameter
>   |
> 1 ~ struct User<'a> {
> 2 |     active: bool,
> 3 |     username: &str,
> 4 ~     email: &'a str,
>   |
>
> For more information about this error, try `rustc --explain E0106`.
> error: could not compile `structs` due to 2 previous errors
> ```
>
> In Chapter 10, we’ll discuss how to fix these errors so you can store
> references in structs, but for now, we’ll fix errors like these using owned
> types like `String` instead of references like `&str`.

<!-- manual-regeneration
for the error above
after running update-rustc.sh:
pbcopy < listings/ch05-using-structs-to-structure-related-data/no-listing-02-reference-in-struct/output.txt
paste above
add `> ` before every line -->

[tuples]: ch03-02-data-types.html#the-tuple-type
[move]: ch04-01-what-is-ownership.html#variables-and-data-interacting-with-move
[copy]: ch04-01-what-is-ownership.html#stack-only-data-copy
