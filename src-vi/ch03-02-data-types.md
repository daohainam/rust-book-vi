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

Lưu ý là chúng ta chỉ ra một dữ liệu nào đó là kiểu `char` bằng cách dùng dấu nháy đơn, trong khi đó 
với string thì ta dùng dấu nháy kép. Kiểu `char` trong Rust có kích cỡ 4 byte và biểu diễn một ký tự
Unicode, do nó nó có thể biểu diễn nhiều hơn nhiều so với ASCII. Các ký tự cổ, các ký tự tiếng Trung, 
tiếng Nhật, tiếng Hàn; các biểu tượng cảm xúc (emoji); và cả các khoảng trống có chiều rộng bằng 0 
đều là các ký tự kiểu `char` hợp lệ. Các ký tự Unicode cũng bao gồm cả từ `U+0000` đến `U+D7FF` 
và từ `U+E000` đến `U+10FFFF`. Tuy nhiên, "ký tự" là một khái niệm không thực sự được dùng trong 
Unicode, do vậy cách mà bạn nghĩ về "ký tự" có thể không hoàn toàn giống với `char` trong Rust.
Chúng ta sẽ thảo luận thêm về chủ đề này trong phần [“Storing UTF-8 Encoded Text with
Strings”][strings]<!-- ignore --> in Chương 8.

### Các kiểu phức

*Kiểu phức* có thể nhóm nhiều giá trị vào chung một kiểu. Rust có hai kiểu phức chính: tuple và array.

#### Kiểu tuple

Một kiểu tuple là một cách chung để nhóm các giá trị khác nhau vào làm một. Các tupble có kích thước cố
định: một khi đã khai báo, chúng sẽ không thể tăng hay giảm kích cỡ. Chúng ta tạo một tuble bằng cách viết 
một danh sách các giá trị cách nhau bởi dấu phẩy, bao bọc lại bởi một cặp ngoặc tròn. Mỗi vị trí trong tuble 
có một kiểu, và các giá trị khác nhau trong một tuble không nhất thiết phải cùng kiểu. Chúng ta cũng đã 
thêm một số phụ chú kiểu trong ví dụ sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-10-tuples/src/main.rs}}
```

Biến `tup` đại diện cho toàn bộ tuple, vì mỗi một tuple được coi như một biến đơn. Để lấy giá trị của từng
thành phần riêng lẻ bên trong một tuple, chúng ta có thể dùng cách khớp mẫu để phân tách một giá trị kiểu 
tuple, giống trong ví dụ sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-11-destructuring-tuples/src/main.rs}}
```

Chương trình này đầu tiên sẽ tạo ra một tuple và gắn kết nó với biến `tup`. 
Sau đó nó dùng một mẫu với `let` để lấy ra ba giá trị riêng lẻ từ các thành phần 
của `tup`, `x`, `y` và `z`. Ta gọi quá trình này là phá hủy (*destructuring*), vì nó sẽ tách một
tuple đơn ra thành ba phần riêng biệt. Cuối cùng, chương trình in ra giá trị của `y` la `6.4`.

Ta cũng có thể truy cập trực tiếp vào một thành phần bên trong tuple bằng cách dùng 
dấu chấm (`.`), theo sau bởi chỉ mục của giá trị mà bạn muốn đọc hay ghi. Ví dụ:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-12-tuple-indexing/src/main.rs}}
```

Chương trình này tạo một tuple `x` và sau đó truy cập vào từng thành phần của tuple 
thông qua các giá trị chỉ mục tương ứng. Tương tự với hầu hết ngôn ngữ lập trình khác,
chỉ mục đầu tiên sẽ mang giá trị 0.

Một tuple mà không có giá trị nào có một cái tên đặc biệt, *unit*. Giá trị này và kiểu 
tương ứng của nó được viết là `()` và đại diện cho một giá trị rỗng hay một kiểu trả 
về rỗng. Các biểu thức được ngầm hiểu là trả về *unit* nếu chúng không trả về một giá
trị nào khác.

#### Kiểu mảng (array)

Một cách khác để khai báo một biến chứa được nhiều giá trị là *array* (mảng). Không như
tuple, các thành phần của mảng phải có cùng kiểu. Cũng không giống mảng trong nhiều ngôn
ngữ khác, mảng trong Rust có chiều dài cố định.

Chúng ta viết các giá trị của mảng như một danh sách các giá trị cách nhau bởi dấu phẩy, 
được bao bọc bởi cặp dấu ngoặc vuông:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-13-arrays/src/main.rs}}
```

Các mảng sẽ hữu ích hơn khi chúng ta phân bố chúng trên stack thay vì heap (chúng ta sẽ 
thảo luận về stack và heap trong [Chương 4][stack-and-heap]<!-- ignore -->) hoặc 
khi chúng ta muốn đảm bảo luôn có một số phần tử cố định. Dù vậy kiểu array không 
được mềm dẻo như vector. Một vector là một kiểu tập hợp tương tự được cung cấp 
bởi thư viện chuẩn, và nó cho phép thay đổi kích thước. Nếu bạn không chắc nên dùng
array hay vector, vậy thì hãy dùng vector. [Chương 8][vectors]<!-- ignore --> sẽ
thảo luận kỹ hơn về vector.

Tuy nhiên, các mảng sẽ hữu ích hơn nếu bạn biết số phần tử sẽ không thay đổi. Ví dụ,
nếu bạn muốn lưu danh sách tên các tháng trong năm, sẽ tốt hơn khi dùng array thay vì
vector vì bạn biết nó luôn có 12 phần tử:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

Bạn viết một kiểu của mảng sử dụng cặp dấu ngoặc vuông với kiểu của mỗi phần tử,
một dấu chấm phẩy, và số phần tử của mảng, giống như sau:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Ở đây, `i32` là kiểu của mỗi phần tử. Sau dấu chấm phẩy, số `5` chỉ ra mang này có 5
phần tử.

Bạn cũng có thể khởi tạo một mảng để chứa các phần tử cùng giá trị bằng cách chỉ ra
giá trị ban đầu, theo sau bởi một dấu chấm phẩy, và sau đó là chiều dai của mảng, tất
cả bao trong cặp dấu ngoặc vuông, như ví dụ dưới đây:

```rust
let a = [3; 5];
```

Mảng tên `a` sẽ chứa `5` phần tử với giá trị ban đầu là `3`. Điều này hoàn toàn giống
với khi bạn viết `let a = [3, 3, 3, 3, 3];` nhưng theo một cách ngắn gọn hơn.

##### Truy cập các phần tử của mảng

Một mảng là một đoạn bộ nhớ có kích thước cố định, xác định trước và có thể được cấp
phát trên stack. Bạn có thể truy cập các phần tử của mảng dùng chỉ số, giống như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-14-array-indexing/src/main.rs}}
```

Trong ví dụ này, một biến có tên `first` sẽ có giá trị `1`, vì nó là giá trị tại vị trí 
`[0]` trong mảng. Biến có tên `second` sẽ có giá trị `2` từ phần tử `[1]` trong mảng.

##### Việc truy cập mảng không hợp lệ

Hãy xem điều gì sẽ xảy ra nếu bạn thử truy cập vào một phần tử vượt quá phần tử cuối của
mảng. Chúng ta sẽ chạy đoạn code sau, tương tự trong trò chơi đoán chữ trong chương 2, để 
lấy một chỉ mục từ người dùng:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,panics
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access/src/main.rs}}
```

Đoạn code này được dịch thành công. Nếu bạn chạy nó bằng cách dùng `cargo run` và nhập
vào 0,1, 2, 3, hay 4, chương trình sẽ in ra giá trị tương ứng tại vị trí trong mảng. Nếu
bạn nhập một giá trị vượt quá kích thước mảng, chẳng hạn 10, bạn sẽ thấy xuất ra như sau:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access
cargo run
10
-->

```console
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Chương trình sinh ra một lỗi *runtime* ngay tại nơi mà chúng ta dùng một giá trị không 
hợp lệ khi dùng chỉ số mảng. Chương trình kết thúc với một thông báo lỗi và đã không
thực thi đến câu lệnh `println!` cuối cùng. Khi bạn thử truy cập một phần tử bằng cách 
dùng chỉ số, Rust sẽ kiểm tra xem liệu chỉ số đó có nhỏ hơn chiều dài của mảng hay không.
Nếu chỉ số này lớn hơn hay bằng chiều dài mảng, chương trình sẽ bị lỗi và kết thúc 
ngay lập tức (panic). Việc kiểm tra này phải được thực hiện khi chạy chương trình, đặc
biệt trong trường hợp này, vì trình dịch không thể biết giá trị mà người dùng sẽ nhập
vào khi chạy chương trình sau này.

Đây là một ví dụ về các nguyên tắc an toàn của Rust khi hoạt động. Trong nhiều ngôn ngữ 
cấp thấp, việc kiểm tra này không được thực hiện, và khi bạn cung cấp một chỉ số không 
hợp lệ, vùng bộ nhớ không hợp lệ sẽ bị truy cập. Rust bảo vệ bạn khỏi loại lỗi này bằng 
cách thoát ra ngay lập tức thay vì cho phép truy cập vào vùng nhớ và tiếp tục chạy. 
Chương 9 sẽ thảo luận thêm về việc xử lý lỗi trong Rust, cách viết code dễ đọc, an toàn
và tránh các lỗi panic hay truy cập vùng nhớ không hợp lệ.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[control-flow]: ch03-05-control-flow.html#control-flow
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[stack-and-heap]: ch04-01-what-is-ownership.html#the-stack-and-the-heap
[vectors]: ch08-01-vectors.html
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[wrapping]: ../std/num/struct.Wrapping.html
[appendix_b]: appendix-02-operators.md
