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

<a id="creating-instances-from-other-instances-with-struct-update-syntax"></a>
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

<a id="using-tuple-structs-without-named-fields-to-create-different-types"></a>
### Sử dụng tuple struct không dùng các trường được đặt tên để tạo các kiểu khác

Rust cũng hỗ trợ các cấu trúc trông tương tự như các tuple, được gọi là *tuple structs*.
Các tuple struct cho phép cung cấp tên cho cấu trúc nhưng không có các tên được kết hợp với từng 
trường bên trong struct; thay vì vậy, chúng chỉ có các kiểu cho các field đó. 
Các tuple struct rất hữu ích khi bạn muốn đặt tên cho toàn bộ tuple và biến nó thành 
một kiểu khác với các tuple khác, và khi đặt tên cho từng field bên trong struct
như trong một cấu trúc thông thường sẽ trở nên dài dòng hoặc dư thừa.

Để định nghĩa tuple struct, hãy bắt đầu với từ khóa `struct` và tên cấu trúc
theo sau là các kiểu trong tuple. Ví dụ, ở đây chúng ta định nghĩa và sử dụng hai
tuple struct có tên `Color` và `Point`:

Rust cũng hỗ trợ các cấu trúc trông giống như các bộ dữ liệu, được gọi là *tuple structs*.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-01-tuple-structs/src/main.rs}}
```

Lưu ý rằng các giá trị `black` và `origin` có các kiểu khác nhau vì chúng
là instance của các tuple struct khác nhau. Mỗi struct bạn định nghĩa mang kiểu riêng của nó,
mặc dù các trường trong struct có thể có cùng loại. Ví dụ, một hàm nhận tham số kiểu `Color` không thể nhận
`Point` làm đối số, mặc dù cả hai loại đều được tạo thành từ ba giá trị `i32`. 
Mặt khác, các instance tuple struct tương tự như tuple ở chỗ bạn có thể
hủy struct chúng thành các phần riêng lẻ và bạn có thể sử dụng dấu `.` theo sau
theo chỉ mục để truy cập từng giá trị riêng.

### Các cấu trúc Unit-Like không có bất kỳ trường nào

Bạn cũng có thể định nghĩa các cấu trúc không có bất kỳ trường nào! Chúng được gọi là
*unit-like struct* vì chúng hoạt động tương tự như `()`, kiểu đơn vị
chúng ta đã đề cập trong phần [“The Tuple Type”][tuples]<!-- ignore -->. Unit-like
struct có thể hữu ích khi bạn cần triển khai một trait trên một số kiểu nhưng không
có bất kỳ dữ liệu nào mà bạn muốn lưu trữ trong chính kiểu đó. Chúng ta sẽ thảo luận về trait
trong Chương 10. Đây là một ví dụ về khai báo và khởi tạo một unit struct
được đặt tên là `AlwaysEqual`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch05-using-structs-to-structure-related-data/no-listing-04-unit-like-structs/src/main.rs}}
```

Để định nghĩa `AlwaysEqual`, chúng ta sử dụng từ khóa `struct`, tên chúng ta muốn và
sau đó là dấu chấm phẩy. Không cần dấu ngoặc nhọn hoặc dấu ngoặc đơn! 
Sau đó, chúng ta có thể nhận được một instance về `AlwaysEqual` trong biến `subject` 
theo cách tương tự: sử dụng tên chúng ta đã xác định, không có bất kỳ dấu ngoặc 
nhọn hoặc dấu ngoặc đơn nào. Hãy tưởng tượng rằng sau này chúng tôi sẽ triển khai 
hành vi cho kiểu này sao cho mọi instance của `AlwaysEqual` luôn bằng với mọi instance 
của bất kỳ kiểu nào khác, có lẽ để có một kết quả xác định nhằm mục đích thử nghiệm. 
Chúng ta sẽ không cần bất kỳ dữ liệu nào để thực hiện hành vi đó! Bạn sẽ thấy trong 
Chương 10 cách định nghĩa các trait và triển khai chúng trên bất kỳ kiểu dữ liệu nào, 
kể cả các unit-like struct.

> ### Sở hữu của các cấu trúc dữ liệu
>
> Trong định nghĩa cấu trúc `User` trong Listing 5-1, chúng ta đã sử dụng kiểu `String` 
> type thay vì kiểu string slice `&str`. Đây là một sự lựa chọn có chủ ý
> bởi vì chúng ta muốn mỗi instance của cấu trúc này sở hữu tất cả dữ liệu của nó và cho
> dữ liệu đó là hợp lệ miễn sao toàn bộ cấu trúc là hợp lệ.
> 
>Các cấu trúc cũng có thể lưu trữ các tham chiếu đến dữ liệu thuộc sở hữu của một thứ gì đó
> khác, nhưng để làm như vậy yêu cầu sử dụng *lifetimes*, một tính năng của Rust mà chúng ta sẽ
> thảo luận trong Chương 10. *lifetimes* đảm bảo rằng dữ liệu được tham chiếu bởi một cấu trúc
> sẽ là hợp lệ cùng với chính cấu trúc đó . Giả sử bạn cố gắng lưu trữ một tham chiếu
> trong một cấu trúc mà không chỉ định thời gian tồn tại giống như sau; chúng sẽ không hoạt động:
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
> Trình duyệt sẽ báo lỗi rằng bạn cần các khai báo về lifetime:
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
> Trong chương 10, chúng ta sẽ thảo luận cách sửa các lỗi trên, cho phép bạn
> lưu các tham chiếu bên trong các struct, nhưng hiện tại, chúng ta sẽ sửa các 
> lỗi này bằng cách dùng các kiểu được sở hữu như `String` thay vì dùng tham
> chiếu như `&str`.

<!-- manual-regeneration
for the error above
after running update-rustc.sh:
pbcopy < listings/ch05-using-structs-to-structure-related-data/no-listing-02-reference-in-struct/output.txt
paste above
add `> ` before every line -->

[tuples]: ch03-02-data-types.html#the-tuple-type
[move]: ch04-01-what-is-ownership.html#variables-and-data-interacting-with-move
[copy]: ch04-01-what-is-ownership.html#stack-only-data-copy
