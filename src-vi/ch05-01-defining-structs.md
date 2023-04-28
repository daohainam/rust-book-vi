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

To use a struct after we’ve defined it, we create an *instance* of that struct
by specifying concrete values for each of the fields. We create an instance by
stating the name of the struct and then add curly brackets containing *key:
value* pairs, where the keys are the names of the fields and the values are the
data we want to store in those fields. We don’t have to specify the fields in
the same order in which we declared them in the struct. In other words, the
struct definition is like a general template for the type, and instances fill
in that template with particular data to create values of the type. For
example, we can declare a particular user as shown in Listing 5-2.

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

Listing 5-4 shows a `build_user` function that returns a `User` instance with
the given email and username. The `active` field gets the value of `true`, and
the `sign_in_count` gets a value of `1`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-04/src/main.rs:here}}
```

<span class="caption">Listing 5-4: A `build_user` function that takes an email
and username and returns a `User` instance</span>

It makes sense to name the function parameters with the same name as the struct
fields, but having to repeat the `email` and `username` field names and
variables is a bit tedious. If the struct had more fields, repeating each name
would get even more annoying. Luckily, there’s a convenient shorthand!

<!-- Old heading. Do not remove or links may break. -->
<a id="using-the-field-init-shorthand-when-variables-and-fields-have-the-same-name"></a>

### Using the Field Init Shorthand

Because the parameter names and the struct field names are exactly the same in
Listing 5-4, we can use the *field init shorthand* syntax to rewrite
`build_user` so it behaves exactly the same but doesn’t have the repetition of
`username` and `email`, as shown in Listing 5-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-05/src/main.rs:here}}
```

<span class="caption">Listing 5-5: A `build_user` function that uses field init
shorthand because the `username` and `email` parameters have the same name as
struct fields</span>

Here, we’re creating a new instance of the `User` struct, which has a field
named `email`. We want to set the `email` field’s value to the value in the
`email` parameter of the `build_user` function. Because the `email` field and
the `email` parameter have the same name, we only need to write `email` rather
than `email: email`.

### Creating Instances from Other Instances with Struct Update Syntax

It’s often useful to create a new instance of a struct that includes most of
the values from another instance, but changes some. You can do this using
*struct update syntax*.

First, in Listing 5-6 we show how to create a new `User` instance in `user2`
regularly, without the update syntax. We set a new value for `email` but
otherwise use the same values from `user1` that we created in Listing 5-2.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-06/src/main.rs:here}}
```

<span class="caption">Listing 5-6: Creating a new `User` instance using one of
the values from `user1`</span>

Using struct update syntax, we can achieve the same effect with less code, as
shown in Listing 5-7. The syntax `..` specifies that the remaining fields not
explicitly set should have the same value as the fields in the given instance.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/listing-05-07/src/main.rs:here}}
```

<span class="caption">Listing 5-7: Using struct update syntax to set a new
`email` value for a `User` instance but to use the rest of the values from
`user1`</span>

The code in Listing 5-7 also creates an instance in `user2` that has a
different value for `email` but has the same values for the `username`,
`active`, and `sign_in_count` fields from `user1`. The `..user1` must come last
to specify that any remaining fields should get their values from the
corresponding fields in `user1`, but we can choose to specify values for as
many fields as we want in any order, regardless of the order of the fields in
the struct’s definition.

Note that the struct update syntax uses `=` like an assignment; this is because
it moves the data, just as we saw in the [“Variables and Data Interacting with
Move”][move]<!-- ignore --> section. In this example, we can no longer use
`user1` as a whole after creating `user2` because the `String` in the
`username` field of `user1` was moved into `user2`. If we had given `user2` new
`String` values for both `email` and `username`, and thus only used the
`active` and `sign_in_count` values from `user1`, then `user1` would still be
valid after creating `user2`. Both `active` and `sign_in_count` are types that
implement the `Copy` trait, so the behavior we discussed in the [“Stack-Only
Data: Copy”][copy]<!-- ignore --> section would apply.

### Using Tuple Structs Without Named Fields to Create Different Types

Rust also supports structs that look similar to tuples, called *tuple structs*.
Tuple structs have the added meaning the struct name provides but don’t have
names associated with their fields; rather, they just have the types of the
fields. Tuple structs are useful when you want to give the whole tuple a name
and make the tuple a different type from other tuples, and when naming each
field as in a regular struct would be verbose or redundant.

To define a tuple struct, start with the `struct` keyword and the struct name
followed by the types in the tuple. For example, here we define and use two
tuple structs named `Color` and `Point`:

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
