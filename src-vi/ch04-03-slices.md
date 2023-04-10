<a id="the-slice-type"></a>
## Kiểu Slice

*Slice* cho phép bạn tham chiếu một chuỗi các phần tử liền kề trong một collection
thay vì toàn bộ collection. Một slide là một dạng reference, do đó, nó
không có ownership.

Đây là một vấn đề lập trình nhỏ: viết một hàm nhận vào một chuỗi
các từ được phân tách bằng khoảng trắng và trả về từ đầu tiên nó tìm thấy trong chuỗi đó.
Nếu hàm không tìm thấy khoảng trắng trong chuỗi, toàn bộ chuỗi phải là
một từ, vì vậy toàn bộ chuỗi sẽ được trả về.

Hãy tìm hiểu cách chúng ta viết khai báo hàm này mà không cần sử dụng
slice, để hiểu vấn đề mà slice sẽ giải quyết:

```rust,ignore
fn first_word(s: &String) -> ?
```

Hàm `first_word` có `&String` làm tham số. chúng tôi không muốn
ownership, vì vậy điều này là tốt. Nhưng chúng ta nên trả về những gì? Chúng ta 
không thực sự có một cách để nói về *phần* của một chuỗi. Tuy nhiên, chúng ta 
có thể trả về index của vị trí cuối từ, được biểu thị bằng khoảng trắng. Hãy 
thử điều đó, như trong Liệt kê 4-7.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:here}}
```

<span class="caption">Liệt kê 4-7: Hàm `first_word` trả về một
giá trị byte index vào tham số `String`</span>

Vì chúng ta cần đi qua từng phần tử `String` và kiểm tra xem
một giá trị có là khoảng trắng hay không, chúng ta sẽ chuyển 
đổi `String` của mình thành một mảng byte bằng cách sử dụng
phương thức `as_bytes`:

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:as_bytes}}
```

Tiếp theo, chúng ta tạo một iterator trên mảng byte bằng phương thức `iter`:

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:iter}}
```

Chúng ta sẽ thảo luận chi tiết hơn về các iterator trong [Chương 13][ch13]<!-- ignore -->.
Hiện tại, hãy biết rằng `iter` là một phương thức trả về từng phần tử trong một collection
và `enumerate` bao bọc kết quả của `iter` và trả về mỗi phần tử dưới dạng tuple. 
Phần tử đầu tiên của bộ dữ liệu được trả về từ
`enumerate` là chỉ mục và phần tử thứ hai là tham chiếu đến phần tử.
Điều này thuận tiện hơn một chút so với việc tự tính toán index.

Vì phương thức `enumerate` trả về một tuple, nên chúng ta có thể sử dụng các pattern (mẫu) để
hủy tuple đó. Chúng ta sẽ thảo luận nhiều hơn về các pattern trong [Chương
6][ch6]<!-- ignore -->. Trong vòng lặp `for`, chúng ta chỉ định một pattern có `i`
cho chỉ mục trong tuple và `&item` cho byte đơn trong tuple.
Bởi vì chúng tôi nhận được tham chiếu đến phần tử từ `.iter().enumerate()`, nên chúng ta sử dụng
`&` trong pattern.

Bên trong vòng lặp `for`, chúng ta tìm kiếm byte đại diện cho khoảng trắng bằng cách
sử dụng cú pháp ký tự byte. Nếu tìm thấy một khoảng trống, chúng ta sẽ trả lại vị trí.
Ngược lại, chúng tôi trả về độ dài của chuỗi bằng cách sử dụng `s.len()`:

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-07/src/main.rs:inside_for}}
```

Bây giờ chúng ta có một cách để tìm ra chỉ số kết thúc của từ đầu tiên trong
chuỗi, nhưng có một vấn đề. Chúng ta đang trả về một `usize`, nhưng nó
chỉ có ý nghĩa trong ngữ cảnh của `&String`. Nói cách khác,
bởi vì đó là một giá trị riêng biệt từ `String`, nên không có gì đảm bảo rằng nó
vẫn sẽ hợp lệ trong tương lai. Hãy xem xét chương trình trong Listing 4-8 
sử dụng hàm `first_word` từ Listing 4-7.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-08/src/main.rs:here}}
```

<span class="caption">Listing 4-8: Lưu trữ kết quả từ việc gọi hàm
hàm `first_word` rồi thay đổi nội dung `String`</span>

Chương trình này biên dịch mà không có bất kỳ lỗi nào và cũng sẽ vậy nếu chúng ta sử dụng `word`
sau khi gọi `s.clear()`. Bởi vì `word` không được kết nối với trạng thái của `s`
hoàn toàn, `word` vẫn chứa giá trị `5`. Chúng ta có thể sử dụng giá trị `5` đó với
biến `s` để trích xuất từ đầu tiên, nhưng đây sẽ là một lỗi bởi vì nội dung 
của `s` đã thay đổi kể từ khi chúng ta lưu `5` trong `word`.

Việc phải lo lắng về index trong `word` không đồng bộ với dữ liệu trong
`s` thật chán và dễ bị lỗi! Việc quản lý các chỉ số này thậm chí còn khó khăn hơn nếu
chúng tôi viết một hàm `second_word`. Khai báo của nó sẽ phải trông như thế này:

```rust,ignore
fn second_word(s: &String) -> (sử dụng, sử dụng) {
```

Bây giờ chúng ta đang theo dõi chỉ mục bắt đầu *và* kết thúc, và chúng ta thậm chí còn có
các giá trị được tính toán từ dữ liệu ở một trạng thái cụ thể nhưng không bị ràng buộc với
trạng thái đó. Chúng ta có ba biến không liên quan trôi nổi xung quanh 
cần được giữ đồng bộ với nhau.

May mắn thay, Rust có một giải pháp cho vấn đề này: slide.

### String Slices

Một *string slice* là một tham chiếu đến một phần của `String`, và nó trông như thế này:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-17-slice/src/main.rs:here}}
```

Thay vì reference đến toàn bộ `String`, `hello` là một reference đến một
phần của `String`, được chỉ định trong phần `[0..5]`. Chúng ta tạo ra các slice
sử dụng toán tử phạm vi trong ngoặc bằng cách viết `[starting_index..ending_index]`,
trong đó `starting_index` là vị trí đầu tiên trong slice và `ending_index` là
vị trí cuối cùng + 1 trong slice. Bên trong, cấu trúc dữ liệu slice
lưu trữ vị trí bắt đầu và độ dài của slice, tương ứng với `ending_index` trừ `starting_index`. 
Vì vậy, trong trường hợp `let world = &s[6..11];`, `world` sẽ là một slice chứa con trỏ tới
byte tại index 6 của `s` với giá trị độ dài là 5.

Hình 4-6 cho thấy điều này trong một diagram.

<img alt="world chứa con trỏ tới byte ở chỉ số 6 của String s và độ dài 5" src="img/trpl04-06.svg" class="center" style="width: 50%;" />

<span class="caption">Hình 4-6: String slice tham chiếu đến một phần của
`String`</span>

Với cú pháp phạm vi `..` của Rust, nếu bạn muốn bắt đầu từ chỉ số 0, bạn có thể bỏ
giá trị trước hai dấu chấm. Nói cách khác, chúng bằng nhau:

```rust
let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2];
```

Tương tự như vậy, nếu slice của bạn bao gồm byte cuối cùng của `String`, thì bạn
có thể chỉ số cuối. Điều đó có nghĩa là các lệnh sau tương tự nhau:

```rust
let s = String::from("hello");

let len = s.len();

let slice = &s[3..len];
let slice = &s[3..];
```

Bạn cũng có thể bỏ cả hai giá trị để lấy một phần của toàn bộ chuỗi:

```rust
let s = String::from("hello");

let len = s.len();

let slice = &s[0..len];
let slice = &s[..];
```

> Lưu ý: Chỉ số phạm vi string slice phải xuất hiện ở vị trí là ranh giới một ký tự UTF-8 hợp lệ.
> Nếu bạn cố gắng tạo một lát cắt chuỗi ở giữa một
> ký tự multibyte, chương trình của bạn sẽ thoát với một lỗi. Với mục đích
> giới thiệu các string slice, chúng ta giả sử chỉ dùng ASCII trong phần này; Một
> thảo luận kỹ lưỡng hơn về xử lý UTF-8 có trong [“Lưu trữ văn bản UTF-8 Encoded
> với String”][strings]<!-- ignore --> phần của Chương 8.

Với tất cả những thông tin đã có, hãy viết lại `first_word` để trả về một
slice. Loại biểu thị “lát cắt chuỗi” được viết là `&str`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-18-first-word-slice/src/main.rs:here}}
```

Chúng ta lấy chỉ mục cho phần cuối của từ giống như cách chúng ta đã làm trong Liệt kê
4-7, bằng cách tìm kiếm sự xuất hiện đầu tiên của khoảng trắng. Khi tìm thấy một space, ta
trả về một string slice bằng cách sử dụng phần đầu của chuỗi và index của space
như các chỉ số bắt đầu và kết thúc.

Bây giờ, khi gọi `first_word`, chúng ta nhận lại một giá trị duy nhất được gắn với
dữ liệu bên dưới. Giá trị được tạo thành từ một tham chiếu đến điểm bắt đầu của
slice và số phần tử trong slice.

Trả về một slice cũng sẽ hoạt động đối với hàm `second_word`:

```rust,ignore
fn second_word(s: &String) -> &str {
```

Bây giờ chúng tôi có một API đơn giản, khó gây ra rắc rối hơn nhiều, vì
trình biên dịch sẽ đảm bảo các tham chiếu vào `String` vẫn hợp lệ. Nhớ
lỗi trong chương trình trong Liệt kê 4-8, khi chúng ta lấy index đến cuối
từ đầu tiên nhưng sau đó xóa chuỗi để index đó trở nên không hợp lệ? Đoạn code đó 
không chính xác về mặt logic nhưng không hiển thị bất kỳ lỗi ngay lập tức nào. 
Các vấn đề sẽ xuất hiện sau nếu chúng ta tiếp tục cố gắng sử dụng chỉ mục từ đầu tiên 
với một chuỗi rỗng. Các slide khiến lỗi này không thể xảy ra và cho ta biết 
về các vấn đề của code sớm hơn nhiều. Sử dụng phiên bản slide của `first_word` sẽ tạo ra một
lỗi biên dịch:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-19-slice-error/src/main.rs:here}}
```

Đây là lỗi biên dịch:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-19-slice-error/output.txt}}
```

Hãy nhớ lại từ các quy tắc borrowing rằng nếu ta có một immutable reference đến
một cái gì đó, ta cũng không thể lấy một mutable reference. Bởi vì `clear` cần 
cắt bớt `String`, nó cần lấy mutable reference. Lệnh `println!` sau lệnh gọi `clear` 
sử dụng tham chiếu trong `word`, vì vậy giá trị immutable reference vẫn đang 
được sử dụng tại thời điểm đó. Rust không cho phép thay đổi
tham chiếu trong `clear` và immutable reference trong `word` cùng lúc 
và việc biên dịch sẽ không thành công. Rust không chỉ làm cho API của chúng 
ta dễ sử dụng hơn, mà nó còn loại bỏ toàn bộ các lỗi tại thời điểm biên dịch!

#### String Literals Are Slices

Nhớ lại rằng chúng ta đã nói về string literal được lưu trữ bên trong dữ liệu nhị phân. Hiện nay
mà chúng ta biết về slice, chúng ta có thể hiểu đúng về string literal:

```rust
let s = "Hello, world!";
```

Kiểu của `s` ở đây là `&str`: đó là một slice trỏ đến một điểm cụ thể của giá trị
nhị phân. Đây cũng là lý do tại sao string literal là bất biến; `&str` là một
immutable reference.

#### String Slices as Parameters

Khi đã biết rằng bạn có thể lấy slice của các literal và giá trị của `String`, ta sẽ thấy thêm 
một cải tiến nữa trên `first_word`, và đây là chữ ký của nó:

```rust,ignore
fn first_word(s: &String) -> &str {
```

Một Rustacean có kinh nghiệm hơn sẽ viết chữ ký như trong Listing 4-9
vì nó cho phép chúng ta sử dụng cùng một chức năng trên cả hai giá trị `&String`
và các giá trị `&str`.

```rust,ignore
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-09/src/main.rs:here}}
```

<span class="caption">Listing 4-9: Cải thiện chức năng `first_word` bằng cách sử dụng
một slice cho kiểu của tham số `s`</span>

Nếu chúng ta có một string slice, chúng ta có thể truyền nó trực tiếp. Nếu chúng ta có `String`, 
chúng ta có thể chuyển một slice của `String` hoặc tham chiếu đến `String`. Sự linh hoạt này
cho phép ta tận dụng *deref coercions*, một tính năng mà chúng tôi sẽ đề cập trong
phần [“Implicit Deref Coercions with Functions and
Methods”][deref-coercions]<!--ignore--> trong chương 15. 
Việc xác định một function lấy một string slice thay vì tham chiếu đến `String` giúp cho 
API của chúng ta trở nên tổng quát và hữu ích hơn mà không làm mất bất kỳ tính năng nào:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-09/src/main.rs:usage}}
```

### Other Slices

Các string slice, như bạn có thể tưởng tượng, là dành riêng cho chuỗi. Nhưng cũng có một
loại slice tổng quát hơn, quá. Hãy xem xét mảng này:

```rust
let a = [1, 2, 3, 4, 5];
```

Cũng tương tự như khi ta muốn tham chiếu đến một phần của chuỗi, ta cũng có thể muốn 
tham chiếu thành một phần của mảng. Khi đó ta sẽ làm như vậy như thế này:


```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
```

Slice này có kiểu `&[i32]`. Nó hoạt động giống như các string slice, bằng cách
lưu trữ tham chiếu đến phần tử đầu tiên và độ dài. Bạn sẽ sử dụng kiểu
slice này cho tất cả các kiểu collection khác. Chúng ta sẽ thảo luận về những collection này 
chi tiết hơn khi nói về vector trong Chương 8.

## Tổng kết

Các khái niệm về ownership, borrowing và slice đảm bảo an toàn cho bộ nhớ trong 
chương trình Rust tại thời điểm biên dịch. Ngôn ngữ Rust cho phép bạn kiểm soát 
việc sử dụng bộ nhớ giống như trong các ngôn ngữ lập trình hệ thống khác, nhưng có
owner của dữ liệu sẽ tự động giải phóng dữ liệu đó khi owner vượt quá phạm vi,
có nghĩa là bạn không phải viết và debug thêm code để có quyền kiểm soát này.

Ownership ảnh hưởng đến số lượng các phần khác của Rust khi hoạt động, vì vậy 
chúng ta sẽ nói về những khái niệm này sâu hơn trong suốt phần còn lại của cuốn sách. 
Hãy chuyển sang Chương 5 và xem xét việc nhóm các phần dữ liệu lại với nhau 
trong một `struct`.

[ch13]: ch13-02-iterators.html
[ch6]: ch06-02-match.html#patterns-that-bind-to-values
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[deref-coercions]: ch15-02-deref.html#implicit-deref-coercions-with-functions-and-methods
