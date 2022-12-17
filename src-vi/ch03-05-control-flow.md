## Các khối điều khiển {#control-flow}

Khả năng chạy một số đoạn lệnh dựa khi một điều kiện nào đó là `true`, hoặc chạy lặp lại một
lệnh khi một điều kiện nào đó là `true`, là các khối điều khiển cơ bản có trong hầu 
hết các ngôn ngữ lập trình. Khối điều khiển phổ biến nhất cho phép bạn kiểm soát việc
thực thi các đoạn code trong Rust là các biểu thức `if` và các lệnh lặp.

### Biểu thức `if` 

Một biểu thức `if` cho phép bạn rẽ nhánh thực thi dựa trên các điều kiện nào đó.
Bạn cung cấp một điều kiện và phát biểu: "Nếu điều kiện này đúng, hãy chạy đoạn lệnh
này. Nếu không đạt, đừng chạy nó".

Tạo một dự án mới được gọi là *branches* trong thư mục *projects* để khảo sát biểu
thức `if`. Trong file *src/main.rs*, hãy nhập vào nội dung sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/src/main.rs}}
```

Tất cả các phát biểu `if` sẽ bắt đầu bằng từ khóa `if`, theo sau bởi một điều kiện.
Trong trường hợp này, điều kiện là kiểm tra xem liệu biến `number` có giá trị nhỏ
hơn 5 hay không. Chúng ta đặt khối lệnh để thực thi nếu điều kiện là đúng ngay sau
dấu ngoặc đóng của điều kiện. Các khối lệnh kết hợp với `if` đôi khi được gọi là 
*arm*, giống như *arms* trong phát biểu `match` mà ta đã thảo luận trong phần 
[“Comparing the Guess to the Secret Number”][comparing-the-guess-to-the-secret-number]<!--
ignore --> section of Chapter 2.

Chúng ta cũng có thể tùy chọn thêm một phát biểu `else`, giống như chúng ta đã
làm ở đây, để cung cấp một đoạn code mà sẽ được thực thi nếu điều kiện trả về
giá trị false. Nếu bạn không cung cấp `else`, chương trình sẽ chỉ đơn giản bỏ qua
khối lệnh `if` và tiếp tục thực thi các lệnh tiếp sau đó.

Thử chạy đoạn lệnh này, bạn sẽ thấy kết xuất như sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/output.txt}}
```

Hãy thử thay đổi `number` sang một giá trị làm cho điều kiện trả về kết quả false
để xem điều gì xảy ra:

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/src/main.rs:here}}
```

Chạy lại chương trình, và xem kết xuất:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/output.txt}}
```

Một điểm quan trọng cần lưu ý là điều kiện trong đoạn code này bắt buộc *phải* có 
kiểu `bool`. Nếu điều kiện này không phải là bool, bạn sẽ nhận một lỗi. Ví dụ, thử 
chạy đoạn lệnh sau:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/src/main.rs}}
```

Trong trường hợp này biểu thức trong `if` trả về giá trị `3`, và Rust phát ra một
lỗi:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/output.txt}}
```

Lỗi này chỉ ra Rust cần một `bool` nhưng lại nhận được một integer. Không như
các ngôn ngữ như Ruby hay JavaScript, Rust không tự động thử chuyển các giá trị
không phải boolean sang boolean. Nếu bạn muốn đoạn `if` chỉ chạy khi một số bằng
với `0`, bạn có thể viết lại như sau:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-29-if-not-equal-0/src/main.rs}}
```

Chạy đoạn code này sẽ trả về kết xuất sau: `number was something other than zero`.

#### Xử lý nhiều điều kiện với `else if`

Bạn có thể dùng nhiều điều kiện khác nhau bằng cách kết hợp `if` với `else` trong
một biểu thức `else if`. Ví dụ:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/src/main.rs}}
```

Chương trình này có bốn nhánh có thể thực thi. Sau khi chạy bạn sẽ thấy kết xuất sau:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/output.txt}}
```

Khi chạy chương trình này, nó sẽ kiểm tra lần lượt mỗi biểu thức `if` và thực
thi thân `if` đầu tiên mà biểu thức trả về `true`. Lưu ý rằng tuy 6 chia hết cho 2, 
chúng ta vẫn không thấy câu `number is divisible by 2` được in ra, cũng như không
thấy câu `number is not divisible by 4, 3, or 2` trong khối `else`. Đó là vì Rust 
chỉ thực thi đoạn lệnh trong thân `if` đầu tiên mà nó thấy trả về true, và một khi
tìm thấy, nó sẽ thậm chí không kiểm tra các biểu thức phía sau.

Sử dụng quá nhiều `else if` làm cho code của bạn lộn xộn, do vậy nếu bạn có nhiều 
hơn một, bạn có thể sẽ cần refactor code. Chương 6 mô tả một cấu trúc rẽ nhánh mạnh
mẽ trong Rust được gọi là `match` phù hợp với trường hợp này.

#### Sử dụng `if` bên trong phát biểu `let`

Vì `if` là một biểu thức, vậy nên ta có thể dùng nó bên phải của một phát biểu `let`
để gán giá trị trả về cho một biến, như trong Listing 3-2.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-02/src/main.rs}}
```

<span class="caption">Listing 3-2: Gán kết quả của một biểu thức `if` vào một biến</span>

Biến `number` sẽ được gán một giá trị dựa trên kết quả của một biểu thức `if`. Chạy 
đoạn code sau để xem điều gì xảy ra:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-02/output.txt}}
```

Hãy nhớ là các khối lệnh sẽ được định giá trị bằng với giá trị của biểu thức cuối
cùng bên trong nó, và bản thân các con số cũng là các biểu thức. Trong trường hợp này,
giá trị của toàn bộ biểu thức `if` phụ thuộc vào nhánh nào được thực thi. Có nghĩa là
các nhánh của `if` phải có cùng kiểu; trong Listing 3-2, kết quả của cả hai nhánh 
của `if` đều có kiểu số nguyên `i32`. Nếu các kiểu không khớp nhau, như trong ví dụ 
dưới đây, chúng ta sẽ nhận về một lỗi.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/src/main.rs}}
```

Khi chúng ta thử dịch đoạn code này, chúng ta sẽ nhận về một lỗi. Các nhánh `if`
và `else` có các kiểu không tương thích, và Rust chỉ ra chính xác vị trí nơi phát 
sinh lỗi trong chương trình:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/output.txt}}
```
Biểu thức trong khối `if` trả về một giá trị integer, trong khi biểu thức trong khối `else` 
trả về một string. Điều này không thể hoạt động được vì các biến chỉ có thể có một kiểu
duy nhất, và Rust cần biết kiểu của biến `number` là gì ngay khi dịch. Việc biết 
kiểu của biến `number` cho phép trình dịch xác định tính hợp lệ về kiểu bất cứ khi nào
ta truy xuất đến nó. Rust sẽ không thể làm được điều này nếu nó chỉ có thể xác định kiểu
của `number` vào lúc chạy chương trình; trình dịch có lẽ sẽ phức tạp hơn nhiều cũng như
khó đảm bảo về đoạn code hơn nếu nó phải lưu giữ thông tin về tất cả các kiểu dữ liệu 
`giả tưởng` cho bất kỳ biến nào.

### Sử dụng vòng lặp (loops)

Chúng ta thường xuyên phải thực thi một đoạn code nào đó nhiều lần. Để làm điều này,
Rust cung cấp một số dạng *vòng lặp*, nó cho phép chạy đến cuối đoạn code bên trong 
thân vòng lặp , sau đó quay trở lại vị trí bắt đầu. Để trải nghiệm thử các vòng lặp,
chúng ta sẽ cùng tạo một dự án mới có tên *loops*.

Rust có ba dạng lặp: `loop`, `while`, và `for`. Hãy cùng thử qua từng cái.

#### Lặp lại một đoạn code sử dụng `loop`

Từ khóa `loop` sẽ yêu cầu Rust thực thi một đoạn code lặp đi lặp lại cho đến 
khi bạn yêu cầu nó kết thúc.

Để ví dụ, hãy thay đổi file *src/main.rs* trong thư mục *loops* của bạn để nó trông
như sau:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-loop/src/main.rs}}
```

Khi chạy chương trình này, chúng ta sẽ thấy dòng `again!` được in lên màn hình
liên tục cho đến khi bạn ngừng chạy chương trình. Hầu hết các terminal hỗ trợ 
tổ hợp phím <span class="keystroke">ctrl-c</span> để ngắt một chương trình đang 
bị kẹt trong một vòng lặp. Hãy cùng chạy thử:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-32-loop
cargo run
CTRL-C
-->

```console
$ cargo run
   Compiling loops v0.1.0 (file:///projects/loops)
    Finished dev [unoptimized + debuginfo] target(s) in 0.29s
     Running `target/debug/loops`
again!
again!
again!
again!
^Cagain!
```

Ký hiệu `^C` đại diện cho vị trí bạn nhấn <span class="keystroke">ctrl-c
</span>. Bạn có thể nhìn thấy dòng `again!` được in phía sau `^C` hoặc không,
phụ thuộc vào nơi đoạn code đang thực thi bên trong vòng lặp khi nó nhận được
tín hiệu ngắt.

May mắn là Rust cũng cung cấp một cách để thoát khỏi vòng lặp bằng code. Bạn có 
thể đặt một từ khóa `break` bên trong vòng lặp để nói với chương trình khi nào 
cần thoát khỏi vòng lặp. Hãy nhớ lại chúng ta đã làm điều này trong [“Quitting 
After a Correct Guess”][quitting-after-a-correct-guess]<!-- ignore --> trong 
chương 2 để thoát khỏi chương trình khi người dùng chiến thắng bằng cách đoán đúng
con số.

Chúng ta cũng dùng `continue` trong trò chơi đoán số, để yêu cầu chương trình bỏ
qua phần còn lại trong thân vòng lặp hiện tại và bắt đầu một vòng lặp mới.

#### Trả kết quả về từ vòng lặp

Một trong những lý do dùng `loop` là để thực thi lại một tác vụ nào đó bạn biết
có thể sẽ thất bại, kiểu như khi kiểm tra xem một thread đã hoàn thành công việc
hay chưa. Bạn cũng cần trả về kết quả của tác vụ đó cho phần còn lại của chương 
trình khi kết thúc vòng lặp. Để làm điểu này bạn có thể thêm giá trị muốn trả về 
sau phát biểu `break` mà bạn dùng để kết thúc vòng lặp; giá trị đó sẽ được trả 
về ra ngoài vòng lặp và bạn có thể dùng được nó như trong ví dụ dưới đây:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-33-return-value-from-loop/src/main.rs}}
```
Trước khi lặp, bạn khai báo một biến tên `counter` và khởi tạo giá trị của nó là
`0`. Sau đó bạn khai báo tiếp một biến tên là `result` để lưu lại giá trị trả về 
từ vòng lặp. Cứ mỗi lần lặp ta lại cộng thêm `1` vào biến `counter` và kiểm tra 
giá trị của `counter` với `10`, khi điểu này xảy ra, ta dùng từ khóa `break` với
giá trị trả về là `counter `* 2`. Sau vòng lặp, ta dùng một dấu chấm phẩy để kết 
thúc phát biểu gán giá trị vào cho `result`. Cuối cùng, ta in ra giá trị của `result`,
trong trường hợp này sẽ là 20.

#### Gán nhãn để phân biệt giữa các vòng lặp

Nếu bạn có nhiều vòng lặp lồng nhau, `break` và `continue` được áp dụng cho vòng 
lặp bên trong nhất tại nơi bạn gọi. Bạn cũng có thể đặt *nhãn* cho một vòng lặp
để sau đó khi gọi `break` hoặc `continue`, ta có thể chỉ ra chính xác ta muốn
áp dụng những từ khóa đó cho vòng lặp đã được gán nhãn thay vì vòng lặp trong cùng.
Các nhãn vòng lặp phải bắt đầu với một dấu nháy đơn. Sau đây là một ví dụ về hai
vòng lặp lồng nhau:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/src/main.rs}}
```

Vòng lặp bên ngoài có nhãn `'counting_up`, và nó sẽ đếm từ 0 đến 2. Vòng lặp bên trong
không có nhãn và đếm ngược từ 10 xuống 9. Phát biểu `break` đầu tiên không chỉ ra nhãn 
nên chỉ thoát ra khỏi vòng lặp bên trong. Trong khi đó, phát biểu `break 'counting_up;`
sẽ thoát ra vòng lặp bên ngoài. Đoạn code này sẽ in ra:

```console
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/output.txt}}
```

#### Lặp theo điều kiện với `while`

Một chương trình sẽ thường phải kiểm tra điều kiện bên trong một vòng lặp. Khi điều
kiện là true, tiếp tục vòng lặp. Khi điều kiện không còn là `true`, chương trình sẽ
gọi `break` và ngưng vòng lặp. Bạn hoàn toàn có thể làm những điều trên bằng việc
kết hợp `loop`, `if`, `else` và `break`; bạn có thể thử ngay nếu muốn. Tuy nhiên,
cấu trúc này rất phổ biến, do vậy Rust tạo ra một cấu trúc lặp riêng cho nó, gọi 
vòng lặp `while`. Trong Listing 3-3, chúng ta sẽ dùng `while` để lặp chương trình
3 lần, đếm ngược mỗi lần lặp, in ra một thông báo và kết thúc sau khi hoàn thành 
vòng lặp.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-03/src/main.rs}}
```

<span class="caption">Listing 3-3: Dùng một vòng lặp `while` để chạy code trong khi một
điều kiện vẫn còn đúng</span>

Vòng lặp này cho phép loại bỏ nhiều cấu trúc lồng nhau như khi bạn kết hợp
`loop`, `if`, `else`, và `break`, giúp code của bạn sáng sủa hơn. Trong khi một
điều kiện vẫn là `true`, chạy vòng lặp; ngược lại, kết thúc vòng lặp.

#### Lặp qua một tập hợp với `for` {#looping-through-a-collection-with-for}

Bạn có thể chọn dùng `while` để duyệt qua các thành phần của một tập hợp, kiểu 
như một mảng. Ví dụ, vòng lặp trong Listing 3-4 in ra các phần tử có trong mảng
`a`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-04/src/main.rs}}
```

<span class="caption">Listing 3-4: Lặp qua từng phần tử trong một tập hợp sử dụng 
vòng lặp `while`</span>

Ở đây, đoạn code đếm qua các phần tử có trong mảng. Nó bắt đầu từ chỉ số `0`, và 
tiếp tục lặp cho đến khi gặp chỉ số cuối cùng của mảng (là khi `index < 5` không 
còn trả về true). Chạy đoạn code này sẽ in ra tất cả các phần tử trong mảng:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-04/output.txt}}
```

Tất cả năm giá trị trong mảng đều được xuất ra cửa sổ chạy chương trình. Mặc dù
`index` sẽ đạt giá trị `5` vào một thời điểm nào đó, vòng lặp sẽ ngừng thực thi 
trước khi cố lấy giá trị thứ sáu (không tồn tại) từ mảng.

Tuy nhiên, các tiếp cận này ẩn chứa lỗi; chúng ta có thể làm chương trình về trạng
thái panic nếu giá trị chỉ số hay điều kiện lặp không chính xác. Ví dụ, nếu bạn 
thay đổi định nghĩa mảng `a` thành một mảng chỉ có bốn phần tử nhưng lại quên thay
đổi điều kiện thành `while index < 4`, đoạn code sẽ bị lỗi. Và nó cũng chạy chậm
bởi trình dịch phải thêm code để kiểm tra mỗi lần lặp xem chỉ số có nằm trong 
phạm vi hợp lệ hay không.

Như một cách tiếp cận chính xác hơn, bạn có thể dùng một vòng `for` và thực thi
code cho mỗi phần tử trong tập hợp. Một vòng lặp `for` sẽ trông như đoạn code 
trong Listing 3-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-05/src/main.rs}}
```

<span class="caption">Listing 3-5: Looping through each element of a collection
using a `for` loop</span>

Khi chạy đoạn code này, bạn sẽ thấy cùng kết xuất như trong Listing 3-4. Quan trọng
hơn, giờ độ an toàn của code cao hơn và loại bỏ cơ hội phát sinh bug khi cố truy
cập một phần tử vượt ra ngoài phạm vi của mảng.

Sử dụng vòng `for`, bạn cũng không cần phải nhớ thay đổi các đoạn code khác nếu bạn thay
đổi số giá trị có trong mảng, như cách bạn cần làm nếu sử dụng cách thức trong 
Listing 3-4.

Sự an toàn và chính xác của các vòng `for` làm cho chúng trở thành cấu trúc lặp
phổ biến nhất trong Rust. Ngay cả khi bạn muốn chạy một số code một số lần nhất
định, giống trong ví dụ đếm ngược mà chúng ta dùng vòng lặp `while`trong Listing 3-3,
hầu hết Rustaceans sẽ chọn dùng `for`. Để làm điều này ta có thể dùng một `Range`,
vốn được cung cấp bởi thư viện chuẩn, và sẽ tạo ra tất cả các con số tuần tự bắt 
đầu từ một giá trị và kết thúc trước một giá trị khác.

Đây là ví dụ countdown được viết lại dùng vòng `for` và một phương thức khác ta chưa 
nhắc đến, `rev`, để đảo ngược `Range`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-34-for-range/src/main.rs}}
```

Đoạn code này trông rõ ràng sáng sủa hơn phải không?

## Tổng kết

Bạn đã làm được! Đây thật là một chương với rất nhiều thông tin: bạn học về biến,
các kiểu dữ liệu vô hướng và kết hợp, hàm, ghi chú, phát biểu `if`, và cả các
vòng lặp! Để thực hành với các khái niệm được giới thiệu trong chương này, hãy thử
viết một chương trình làm những việc sau:

* Chuyển đổi nhiệt độ giữa các hệ Fahrenheit và Celsius.
* Tạo số Fibonacci thứ *n*.
* In ra lời bài hát “The Twelve Days of Christmas”, ứng dụng các vòng lặp để in ra những đoạn lặp lại trong bài hát.

Khi bạn đã sẵn sàng để tiếp tục, chúng ta sẽ nói về một khái niệm *không* tồn tại 
trong hầu hết các ngôn ngữ khác: ownership.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[quitting-after-a-correct-guess]:
ch02-00-guessing-game-tutorial.html#quitting-after-a-correct-guess
