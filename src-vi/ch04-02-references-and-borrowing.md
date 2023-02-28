<a id="references-and-borrowing"></a>
## References and Borrowing (tham chiếu và mượn)

Vấn đề với tuple code trong Liệt kê 4-5 là chúng ta phải trả về
`String` cho hàm gọi để vẫn có thể sử dụng `String` sau khi gọi 
tới `calculate_length`, vì `String` đã được chuyển vào
`calculate_length`. Để làm điều đó, chúng ta có thể cung cấp một reference (tham chiếu)
đến giá trị `String`.
Một *tham chiếu* giống như một con trỏ ở chỗ nó là một địa chỉ mà chúng ta có thể theo dõi để truy cập
dữ liệu được lưu trữ tại địa chỉ đó; dữ liệu đó được sở hữu bởi một số biến khác.
Nhưng không giống con trỏ, một reference được đảm bảo trỏ đến một giá trị hợp lệ của một
kiểu cụ thể trong suốt vòng đời của reference đó.

Đây là cách bạn định nghĩa và sử dụng hàm `calculate_length` để
có tham chiếu đến một đối tượng dưới dạng tham số thay vì lấy ownership của đối tượng
sở hữu giá trị:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-07-reference/src/main.rs:all}}
```

Đầu tiên, lưu ý rằng tất cả tuple code trong khai báo biến và
giá trị trả về của hàm đã biến mất. Thứ hai, lưu ý rằng chúng ta chuyển `&s1` vào
`calculate_length` và, theo định nghĩa của nó, chúng ta lấy `&String` thay vì
`String`. Các dấu & này đại diện cho *reference* và chúng cho phép bạn tham chiếu
đến một giá trị nào đó mà không sở hữu nó. Hình 4-5 mô tả khái niệm này.

<img alt="&amp;Chuỗi s chỉ vào Chuỗi s1" src="img/trpl04-05.svg" class="center" />

<span class="caption">Hình 4-5: Sơ đồ `&String s` chỉ vào `String
s1`</span>

> Lưu ý: Ngược lại với reference bằng cách sử dụng `&` là *dereferencing*, bằng cách
> dùng toán tử `*`. Chúng ta sẽ thấy một số công dụng của
> toán tử dereference trong Chương 8 và thảo luận chi tiết về dereference trong
> Chương 15.

Hãy cùng xem kỹ hơn về lệnh gọi hàm ở đây:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-07-reference/src/main.rs:here}}
```

Cú pháp `&s1` cho phép chúng ta tạo một reference *tham chiếu* đến giá trị của `s1`
nhưng không sở hữu nó. Vì không sở hữu nó, giá trị mà nó trỏ tới sẽ
không bị drop khi tham chiếu ngừng được sử dụng.

Tương tự như vậy, chữ ký của hàm sử dụng `&` để chỉ ra rằng kiểu của
tham số `s` là một tham chiếu. Hãy thêm một số annotation:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-08-reference-with-annotations/src/main.rs:here}}
```

Phạm vi (scope) mà biến `s` hợp lệ giống như bất kỳ phạm vi của tham số nào, 
nhưng giá trị được trỏ đến bởi tham chiếu không bị drop khi `s` ngừng được sử dụng 
vì `s` không có ownership. Khi hàm sử dụng tham số dưới dạng tham chiếu thay vì 
biến thực, chúng ta sẽ không cần trả lại các giá trị để trả lại ownership, vì chúng ta 
chưa bao giờ sở hữu chúng.

Chúng ta gọi hành động tạo một reference là *borrowing* (mượn). Giống như 
trong cuộc sống thực, nếu một người sở hữu một cái gì đó, bạn có thể mượn nó từ họ. 
Khi bạn hoàn thành, bạn có để trả lại. Bạn không sở hữu nó.

Vậy điều gì sẽ xảy ra nếu chúng ta cố gắng sửa đổi thứ gì đó mà chúng ta đang mượn? 
Hãy thử mã trong Liệt kê 4-6. 

Spoiler alert: nó không hoạt động!

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-06/src/main.rs}}
```

<span class="caption">Listing 4-6: Cố gắng sửa đổi một giá trị được borrow</span>

Đây là lỗi:

```console
{{#include ../listings/ch04-understanding-ownership/listing-04-06/output.txt}}
```

Giống như các biến là bất biến theo mặc định, các tham chiếu cũng vậy. Ta không
được phép sửa đổi thứ gì mà ta có một reference đến.

### Mutable References (Các tham chiếu có thể thay đổi)

Chúng tôi có thể sửa code từ Liệt kê 4-6 để cho phép sửa đổi giá trị được mượn, 
chỉ với một vài điều chỉnh nhỏ sử dụng * mutable reference *:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-09-fixes-listing-04-06/src/main.rs}}
```

Đầu tiên, chúng ta đổi `s` thành `mut`. Sau đó, chúng ta tạo một * mutable reference * 
với `&mut s` nơi ta gọi hàm `change` và cập nhật function để chấp nhận mutable reference 
với `some_string: &mut String`. Điều này giúp chỉ ra rõ ràng rằng hàm `change` sẽ thay đổi 
giá trị mà nó mượn.

Các mutable reference có một hạn chế lớn: nếu bạn có một mutable reference tới
một giá trị, bạn không thể có thêm reference nào khác đến giá trị đó. Đoạn code này
dùng để nỗ lực tạo hai tham chiếu có thể thay đổi đến `s` sẽ không thành công:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-10-multiple-mut-not-allowed/src/main.rs:here}}
```

Và đây là lỗi:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-10-multiple-mut-not-allowed/output.txt}}
```

Lỗi này nói rằng code không hợp lệ vì chúng ta không thể mượn (borrow) `s` như 
một mutable references nhiều hơn một lần tại một thời điểm. Mutable reference đầu tiên 
nằm trong `r1` và phải kéo dài cho đến khi nó được sử dụng trong `println!`, 
nhưng giữa việc tạo ra mutable reference và cách sử dụng nó, chúng ta đã cố gắng tạo một 
mutable reference khác trong `r2` và mượn cùng dữ liệu với `r1`.

Hạn chế ngăn nhiều tham chiếu có thể thay đổi đến cùng một dữ liệu tại
đồng thời cho phép thay nhưng theo một cách có kiểm soát. Đó có thể là thứ
mà những Rustacean mới cảm thấy khó khăn, vì hầu hết các ngôn ngữ cho phép bạn 
thay đổi dữ liệu bất cứ khi nào bạn muốn. Lợi ích của việc hạn chế này là Rust 
có thể ngăn chặn các *data race* tại thời gian biên dịch. Một *data race* 
tương tự như một race condition và xảy ra khi ba hành vi này xảy ra:

* Hai hoặc nhiều con trỏ truy cập cùng một dữ liệu tại cùng một thời điểm.
* Ít nhất một trong các con trỏ đang được sử dụng để ghi vào dữ liệu.
* Không có cơ chế nào được sử dụng để đồng bộ hóa quyền truy cập vào dữ liệu.

Các data race gây ra hành vi không xác định và có thể khó chẩn đoán cũng như khắc phục
khi bạn đang cố theo dõi chúng khi chạy chương trình; Rust ngăn chặn vấn đề này
bằng cách từ chối biên dịch mã với các data race!

Như mọi khi, ta có thể sử dụng dấu ngoặc nhọn để tạo một scope mới, cho phép
nhiều tham chiếu có thể thay đổi, chỉ là không phải *đồng thời*:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-11-muts-in-separate-scopes/src/main.rs:here}}
```

Rust thực thi một quy tắc tương tự cho việc kết hợp các mutable reference 
và immutable reference.
Mã này dẫn đến một lỗi:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-12-immutable-and-mutable-not-allowed/src/main.rs:here}}
```

Đây là lỗi:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-12-immutable-and-mutable-not-allowed/output.txt}}
```

Whew! Chúng ta *cũng* không thể có một mutable reference trong khi có một 
immutable reference chỉ đến cùng giá trị.

Người dùng của một immutable reference không mong đợi giá trị đột ngột thay đổi
từ đâu đó bên dưới! Tuy nhiên, nhiều immutable reference được cho phép vì 
mọi người chỉ đang đọc dữ liệu và không ai có khả năng ảnh hưởng đến bất kỳ ai khác.

Lưu ý rằng phạm vi của tham chiếu bắt đầu từ nơi nó được giới thiệu và tiếp tục
cho đến lần cuối cùng tham chiếu đó được sử dụng. Ví dụ, mã này sẽ
biên dịch bởi vì lần sử dụng cuối cùng của immutable reference, `println!`,
xảy ra trước khi mutable reference được giới thiệu:

```rust,edition2021
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-13-reference-scope-ends/src/main.rs:here}}
```

Phạm vi của các tham chiếu bất biến (mutable reference) `r1` và `r2` kết thúc sau `println!`
nơi chúng được sử dụng lần cuối, trước mutable reference `r3` được tạo. 
Các phạm vi này không trùng nhau, vì vậy code này được phép. khả năng của
trình biên dịch để báo rằng một tham chiếu không còn được sử dụng tại một thời điểm trước 
khi kết thúc scope được gọi là *Non-Lexical Lifetimes* (viết tắt là NLL) và bạn
có thể đọc thêm về nó trong [The Edition Guide][nll].

Mặc dù đôi khi lỗi borrow có thể gây khó chịu, hãy nhớ rằng đó là
trình biên dịch Rust đã sớm chỉ ra một lỗi tiềm ẩn (tại thời điểm biên dịch
so với thời gian chạy) và cho bạn biết chính xác vấn đề nằm ở đâu. Sau đó, bạn không
phải tìm ra lý do tại sao dữ liệu của bạn không giống như bạn nghĩ.

### Dangling References

Trong các ngôn ngữ có con trỏ, rất dễ tạo nhầm *dangling pointer*--con trỏ 
tham chiếu đến một vị trí trong bộ nhớ có thể đã được được cấp cho một code 
khác--bằng cách giải phóng bộ nhớ trong khi vẫn giữ một con trỏ tới
phần bộ nhớ đó. Ngược lại, trong Rust, trình biên dịch đảm bảo rằng các tham chiếu sẽ
không bao giờ là dangling reference: nếu bạn có một reference đến một dữ liệu,
trình biên dịch sẽ đảm bảo rằng dữ liệu sẽ không đi ra khỏi scope trước reference đó.

Hãy thử tạo một dangling reference để xem cách Rust ngăn chặn chúng bằng một
lỗi biên dịch:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-14-dangling-reference/src/main.rs}}
```

Đây là lỗi:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-14-dangling-reference/output.txt}}
```

Thông báo lỗi này đề cập đến một tính năng mà chúng ta chưa đề cập đến: lifetime 
(thời gian sống). Ta sẽ thảo luận chi tiết về lifetime trong Chương 10. 
Nhưng, nếu bạn bỏ qua các phần về lifetime, thông báo chứa chìa khóa giải 
thích tại sao mã này lại có vấn đề:

```text
this function's return type contains a borrowed value, but there is no value
for it to be borrowed from
```

Chúng ta hãy xem xét kỹ hơn chính xác những gì đang xảy ra vào mỗi bước trong
code `dangle` của chúng ta:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-15-dangling-reference-annotated/src/main.rs:here}}
```

Vì `s` được tạo bên trong `dangle`, khi code của `dangle` kết thúc,
`s` sẽ được giải phóng. Nhưng chúng ta đã cố gắng trả lại một tham chiếu đến nó. 
Điều đó có nghĩa là tham chiếu này sẽ trỏ đến một `String` không hợp lệ. 
Điều đó hoàn toàn không tốt! Rust sẽ không cho phép chúng ta làm điều này.

Giải pháp ở đây là trả về `String` trực tiếp:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-16-no-dangle/src/main.rs:here}}
```

Điều này hoạt động mà không có bất kỳ vấn đề gì. Ownership được chuyển ra ngoài, và 
không có biến nào được giải phóng.

### Các quy tắc về tham chiếu

Hãy tóm tắt lại những gì chúng ta đã thảo luận về tham chiếu:

* Tại bất kỳ thời điểm nào, bạn có thể có *hoặc* chỉ một mutable reference *hoặc* 
nhiều mutable reference.
* Reference phải luôn hợp lệ.

Tiếp theo, chúng ta sẽ xem xét một loại tham chiếu khác: slice.

[nll]: https://doc.rust-lang.org/edition-guide/rust-2018/ownership-and-lifetimes/non-lexical-lifetimes.html
