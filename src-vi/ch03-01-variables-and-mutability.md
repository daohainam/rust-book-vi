## Biến và tính khả biến {#ch03-01-variables-and-mutability}

Như đã nhắc đến trong phần [“Storing Values with
Variables”][storing-values-with-variables]<!-- ignore -->, mặc nhiên các biến
là không thể thay đổi giá trị (immutable). Đây là một trong nhiều cách Rust mang đến để giúp viết 
ra những đoạn code an toàn và dễ dàng hoạt động song song. Tuy nhiên, bạn vẫn có các 
tùy chọn để cho phép thay đổi giá trị các biến. Hãy cùng chúng tôi xem qua như thế 
nào và vì sao Rust khuyến khích việc chặn thay đổi giá trị biến, và vì sao đôi khi 
chúng ta vẫn phải bỏ chặn.

Khi một biến là bất biến (immutable), một khi đã gán cho nó một giá trị, bạn sẽ không
thể thay đổi giá trị đó nữa. Để minh họa điều này, hãy tạo ra một dự án mới
được gọi là *variables* trong thư mục *projects* bằng cách chạy câu lệnh `cargo new variables`.

Sau đó, trong thư mục *variables* mới tạo, mở file *src/main.rs* và thay thế code của nó với
đoạn sau (đoạn code này sẽ không thể dịch được ngay):

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
```

Lưu lại và chạy chương trình dùng `cargo run`. Bạn sẽ phải thấy một thông báo 
liên quan đến lỗi bất biến, như được hiển thị dưới đây:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
```

Ví dụ này cho thấy cách trình biên dịch giúp bạn tìm lỗi trong chương trình.
Các lỗi biên dịch có thể làm nản lòng, nhưng chúng thực sự mang ý nghĩa là chương
trình của bạn không an toàn khi thực hiện cái mà bạn yêu cầu nó thực hiện.
Chúng *không* có nghĩa bạn không phải là một lập trình viên tốt! Những Rustaceans
nhiều kinh nghiệm vẫn gặp các lỗi biên dịch.

Bạn nhận được thông báo `` cannot assign twice to immutable variable `x` ``, 
vì bạn đã thử gán giá trị cho biến immutable `x` thêm một lần nữa.

Việc nhận được các lỗi biên dịch khi ta cố gắng thay đổi giá trị của một biến được 
chỉ định là bất biến là rất quan trọng, nó là một trong những nguyên nhân hàng 
đầu dẫn đến bug. Nếu một phần trong chương trình cho là biến đó sẽ không thay đổi
nhưng ở một phần khác lại gán một giá trị mới cho nó, rất có thể phần đầu tiên sẽ
không còn hoạt động như nó được thiết kế. Nguyên nhân gây lỗi này đôi khi rất khó 
dò ra, đặc biệt nếu phần code thay đổi giá trị của biến chỉ *đôi khi* được thực hiện.
Trình dịch Rust bảo đảm khi bạn đã phát biểu rằng một biến sẽ không thể thay đổi, nó 
sẽ thực sự không thay đổi, và bạn không cần phải tự kiểm soát việc đó. Như vậy 
code của bạn sẽ dễ lý giải hơn.

Nhưng khả năng thay đổi giá trị cũng rất cần thiết, và làm cho việc viết code dễ dàng
hơn. Các biến mặc nhiên sẽ không thay đổi được; như bạn đã thấy trong [Chương
2][storing-values-with-variables]<!-- ignore -->, 
bạn có thể làm cho chúng thay đổi được giá trị bằng cách thêm `mut` vào phía trước 
tên biến. Việc thêm `mut` cũng truyền đạt ý định cho những người đọc code trong tương lai 
bằng cách chỉ ra rằng các phần khác của code sẽ thay đổi giá trị của biến này.

Lấy ví dụ, hãy thay đổi *src/main.rs* thành như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
```

Giờ chạy chương trình, bạn sẽ nhận được nội dung sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/output.txt}}
```

Chúng ta được phép thay đổi giá trị gán cho `x` từ `5` thành `6` khi dùng `mut`.
Hơn hết, việc quyết định liệu có cho phép thay đổi hay không là do bạn và phụ thuộc 
vào việc bạn nghĩ cách nào là rõ ràng nhất trong một trường hợp cụ thể.

### Hằng {#constants}

Tương tự với các biến không đổi, các hằng cũng là các giá trị được đặt tên và không được 
phép thay đổi, tuy nhiên có một số điểm khác nhau.

Đầu tiên, bạn không được phép dùng `mut` với hằng. Các hằng không chỉ là mặc nhiên
bất biến - mà là luôn luôn bất biến. Bạn khai báo các hằng dùng từ khóa `const`
thay vì từ khóa `let`, và *phải* chỉ ra kiểu của hằng. Chúng ta sẽ nó thêm về 
cover type và type annotation trong phần tiếp theo, [“Các kiểu dữ liệu,”][data-types]<!-- ignore -->
do vậy đừng lo về chi tiết bây giờ. Chỉ cần nhớ là bạn luôn phải chỉ ra kiểu.

Các hằng có thể được khai báo trong bất kỳ tầm vực nào, bao gồm phạm vi toàn cục (global),
điều này hữu ích với các giá trị mà bạn muốn truy cập từ nhiều đoạn code khác nhau.

Phần khác biệt cuối cùng là các hằng chỉ có thể được gán giá trị là một biểu thức
hằng, không thể là một giá trị mà chỉ có thể tính toán được khi chạy.

Đây là một ví dụ về khai báo hằng:

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Tên của hằng là `THREE_HOURS_IN_SECONDS` và giá trị của nó là tích của 60 (số giây
trong một phút) với 60 (số phút trong một giờ) với (số giờ bạn muốn tính trong 
chương trình này). Quy ước đặt tên hằng trong Rust là sử dụng tất cả ký tự in hoa,
các từ cách nhau bằng ký tự gạch dưới. Trình dịch cho phép sử dụng một số toán tử
khi dịch, nhờ đó chúng ta có thể viết các giá trị này theo cách dễ hiểu vào kiểm 
tra hơn, thay vì viết 10,000. Xem phần [Rust Reference’s section on constant
evaluation][const-eval] để biết thêm về những toán tử nào ta có thể dùng được 
khi khai báo hằng.

Hằng có giá trị trong toàn bộ thời gian chương trình chạy, trong phạm vi chúng
được khai báo. Tính chất này làm cho các hằng trở nên hữu ích với những giá trị trong
miền ứng dụng mà nhiều phần khác của chương trình có thể cần biết tới, chẳng hạn 
như số điểm tối đa mà mà một người chơi của trò chơi có thể kiếm được hoặc
tốc độ ánh sáng.

Đặt tên cho các giá trị được mã hóa cứng được sử dụng trong suốt chương trình của bạn 
dưới dạng hằng rất hữu ích trong việc truyền đạt ý nghĩa của giá trị đó cho 
những người duy trì code trong tương lai. Nó cũng giúp chỉ có một vị trí trong code 
cần thay đổi nếu giá trị của hằng cần được cập nhật trong tương lai.

### Shadowing

Như bạn đã thấy trong phần hướng dẫn trò chơi đoán số trong phần [Chapter
2][comparing-the-guess-to-the-secret-number]<!-- ignore -->, bạn có thể khai báo một
biến mới trùng tên với biến trước đó. Rustaceans nói rằng biến đầu tiên bị 
*shadowed* bởi biến thứ hai, nghĩa là trình biên dịch sẽ chỉ thấy biến thứ hai
khi bạn sử dụng tên của biến. Trên thực tế, biến thứ hai che đi biến thứ nhất, 
sở hữu tên biến cho đến khi chính nó bị shadowed hoặc phạm vi kết thúc.
Chúng ta có thể shadow một biến bằng cách sử dụng tên của biến đó và lặp lại
sử dụng từ khóa `let` như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/src/main.rs}}
```

Đầu tiên chương trình này liên kết `x` với giá trị là` 5`. Sau đó, nó tạo ra một biến mới
`x` bằng cách lặp lại `let x = `, lấy giá trị ban đầu và thêm `1` để giá trị 
của `x` khi đó là` 6`. Sau đó, trong một phạm vi mới được tạo bằng cặp dấu ngoặc kép,
câu lệnh thứ ba `let` cũng shadow `x` và tạo ra một biến mới, nhân giá trị trước đó 
với `2` để cung cấp cho `x` giá trị là `12`.
Khi phạm vi đó kết thúc, việc shadow bên trong kết thúc và `x` trở lại là `6`.
Khi chúng ta chạy chương trình này, nó sẽ xuất ra như sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
```

Shadowing khác với việc đánh dấu một biến là `mut`, vì chúng ta sẽ nhận được một
lỗi thời gian biên dịch nếu chúng ta vô tình cố gắng gán lại biến này mà không
sử dụng từ khóa `let`. Bằng cách sử dụng `let`, chúng ta có thể thực hiện một 
số phép biến đổi trên một biến nhưng biến đó lấy lại giá trị ban đầu sau khi hoàn thành công việc.

Sự khác biệt khác giữa `mut` và shadowing là khi ta tạo một biến mới bằng cách dùng từ khóa `let`, 
chúng ta có thể thay đổi kiểu của biến nhưng sử dụng lại tên. Ví dụ, cho một chương trình 
yêu cầu người dùng hiển thị bao nhiêu khoảng trắng họ muốn giữa một số văn bản bằng cách
nhập các ký tự khoảng trắng, sau đó lưu lại dữ liệu đó đó dưới dạng số:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-04-shadowing-can-change-types/src/main.rs:here}}
```

Biến `spaces` đầu tiên là một kiểu string và biến `spaces` thứ hai là kiểu số. 
Shadowing do đó giúp chúng ta không phải nghĩ ra các tên khác nhau, chẳng hạn 
như `spaces_str` và `spaces_num`; thay vào đó, chúng ta có thể sử dụng lại
tên `spaces` đơn giản hơn. Tuy nhiên, nếu chúng ta cố gắng sử dụng `mut` cho điều này, 
như được hiển thị ở đây, chúng ta sẽ gặp lỗi biên dịch:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/src/main.rs:here}}
```

Lỗi này nó là chúng ta không được phép thay đổi kiểu của một biến:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/output.txt}}
```

Chúng ta đã xem xong cách các biến làm việc, giờ hãy cùng xem các kiểu dữ liệu chúng có thể có.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[data-types]: ch03-02-data-types.html#data-types
[storing-values-with-variables]: ch02-00-guessing-game-tutorial.html#storing-values-with-variables
[const-eval]: ../reference/const_eval.html
