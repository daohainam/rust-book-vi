## To `panic!` or Not to `panic!`

Khi nào bạn nên dùng `panic` và khi nào nên return về `Result`? Khi code panic,
thì không có cách nào để khôi phục lại. Bạn có thể dùng `panic` cho bất kỳ
trường hợp lỗi nào, dù có thể khôi phục lại hay không, nhưng bạn sẽ phải đưa ra
quyết định rằng một trường hợp là không thể khôi phục lại được, đối với code gọi
hàm. Khi bạn chọn trả về một giá trị `Result`, bạn sẽ cho phép code gọi hàm có
các lựa chọn. Code gọi hàm có thể chọn cách khôi phục lại một cách phù hợp với
trường hợp của nó, hoặc nó có thể quyết định rằng một giá trị `Err` trong trường
hợp này là không thể khôi phục lại được, vì vậy nó có thể gọi `panic` và chuyển
một lỗi có thể khôi phục lại thành một lỗi không thể khôi phục lại. Do đó, trả
về một giá trị `Result` là một lựa chọn mặc định tốt khi bạn định nghĩa một hàm
có thể gây lỗi.

Trong các trường hợp như ví dụ, code prototype, và các test code, thì viết code
dùng `panic` thay vì trả về một giá trị `Result` là phù hợp hơn. Chúng ta sẽ tìm
hiểu tại sao như vậy, sau đó thảo luận về các trường hợp mà compiler không thể
biết được rằng việc gây lỗi là không thể, nhưng bạn là một con người có thể.
Chương này sẽ kết thúc với một số hướng dẫn chung về cách quyết định có nên dùng
`panic` trong code của thư viện hay không.

### Examples, Prototype Code, and Tests

Khi bạn viết một ví dụ để minh hoạ một khái niệm nào đó, việc bao gồm code xử lý
lỗi kỹ lưỡng cũng có thể làm cho ví dụ kém rõ ràng hơn. Trong một ví dụ,
dễ hiểu rằng gọi đến một phương thức như `unwrap` có thể gây lỗi
panic giống như là một bản thay thế cho cách bạn muốn ứng dụng của bạn xử lý
lỗi, mà có thể khác nhau với các phần khác trong code của bạn.

Tương tự, phương thức `unwrap` và `expect` rất hữu ích khi đang prototype,
trước khi bạn đã sẵn sàng để quyết định cách xử lý lỗi. Chúng để lại các dấu
chỉ rõ ràng trong code của bạn khi bạn đã sẵn sàng để làm cho chương trình
của bạn mạnh cứng cáp hơn.

Nếu một phương thức gọi thất bại trong một test, bạn sẽ muốn toàn bộ test thất
bại, ngay cả khi phương thức đó không phải là chức năng đang được test. Bởi
vì `panic!` là cách để một test được đánh dấu là thất bại, việc gọi `unwrap` hoặc `expect` chính là những gì nên xảy ra.

### Cases in Which You Have More Information Than the Compiler

Sẽ là chấp nhận được nếu bạn gọi `unwrap` hoặc `expect` khi bạn có một logic
khác mà đảm bảo `Result` sẽ có một giá trị `Ok`, nhưng logic này không phải là
một thứ mà compiler hiểu được. Bạn vẫn sẽ có một giá trị `Result` mà bạn cần
phải xử lý: bất kỳ hoạt động nào bạn đang gọi vẫn có thể thất bại, ngay cả
khi nó là vô lý trong trường hợp cụ thể của bạn. Nếu bạn có thể đảm bảo bằng
cách thủ công kiểm tra code mà bạn sẽ không bao giờ có một biến thể `Err`, thì
nó là hoàn toàn chấp nhận được để gọi `unwrap`, và còn tốt hơn là viết lý do
bạn nghĩ bạn sẽ không bao giờ có một biến thể `Err` trong văn bản `expect`.
Đây là một ví dụ:

```rust
{{#rustdoc_include ../listings/ch09-error-handling/no-listing-08-unwrap-that-cant-fail/output.txt}}
```

Chúng ta đang tạo một instance `IpAddr` bằng cách truyền vào một chuỗi cố định (hardcode). Chúng ta có thể thấy `127.0.0.1` là một địa chỉ IP hợp lệ, do đó
việc sử dụng `expect` ở đây là chấp nhận được. Tuy nhiên, việc có một chuỗi hợp
lệ cố định không thay đổi kiểu trả về của phương thức `parse`: chúng ta vẫn
nhận được một giá trị `Result`, và compiler vẫn sẽ yêu cầu chúng ta xử lý
`Result` như nó có thể có biến thể `Err` vì compiler không đủ thông minh để
nhìn thấy rằng chuỗi này luôn luôn là một địa chỉ IP hợp lệ. Nếu chuỗi địa chỉ
IP được lấy từ người dùng thay vì được cố định trong chương trình và do đó
*có thể* có một khả năng thất bại, chúng ta sẽ chắc chắn muốn xử lý `Result` một
cách linh hoạt hơn. Việc nhắc nhở rằng giả định này là địa chỉ IP được cố định
sẽ làm chúng ta thay đổi `expect` thành một cách xử lý lỗi tốt hơn nếu trong
tương lai, chúng ta cần lấy địa chỉ IP từ một nguồn khác thay vì địa chỉ IP cố
định.

### Guidelines for Error Handling

Sẽ tốt hơn nếu code của bạn gọi `panic!` khi có thể code của bạn sẽ gặp một
trạng thái không tốt. Trong ngữ cảnh này, một 
*trạng thái không tốt (bad state)* là khi một giả định (assumption),
bảo đảm (guarantee), hợp đồng (contract), hoặc bất biến (invariant) nào đó đã
bị phá vỡ, như khi các giá trị không hợp lệ, giá trị trái ngược, hoặc các giá
trị bị thiếu được truyền vào code của bạn - cộng với một hoặc nhiều trong những
điều sau:

* Trạng thái không tốt này là một điều không mong đợi, khác với điều có thể
  xảy ra thường xuyên, như người dùng nhập dữ liệu sai định dạng.
* Code của bạn sau điểm này cần phải chắc rằng nó không muốn trong trạng thái
  không tốt này, thay vì kiểm tra vấn đề này ở mỗi bước.
* Không có một cách nào để encode thông tin này trong các kiểu mà bạn sử dụng.
  Chúng ta sẽ làm ví dụ về điều này trong phần [“Encoding States and Behavior
  as Types”][encoding]<!-- ignore --> của chương 17.

Nếu ai đó gọi code của bạn và truyền vào các giá trị không có ý nghĩa, tốt nhất
là trả về một lỗi nếu bạn có thể để người dùng thư viện có thể quyết định họ
muốn làm gì trong trường hợp đó. Tuy nhiên, trong trường hợp tiếp tục có thể
không an toàn hoặc có hại, lựa chọn tốt nhất có thể là gọi `panic!` và thông
báo cho người sử dụng thư viện của bạn về lỗi trong code của họ để họ có thể
sửa chữa trong quá trình phát triển. Tương tự, `panic!` thường được sử dụng nếu
bạn đang gọi code bên ngoài mà bạn không kiểm soát được và nó trả về một trạng
thái không hợp lệ mà bạn không thể sửa được.

Tuy nhiên, khi thất bại là có thể dự đoán được, sẽ thích hợp hơn để trả về một
`Result` hơn là gọi một `panic!`. Ví dụ bao gồm một bộ phân tích được cung cấp
dữ liệu bị lỗi hoặc một yêu cầu HTTP trả về một trạng thái cho thấy bạn đã đạt
giới hạn tốc độ. Trong những trường hợp này, trả về một `Result` cho thấy thất
bại là một khả năng được dự đoán rằng code gọi phải quyết định cách xử lý.

Khi code của bạn thực hiện một hoạt động có thể  tạo ra rủi ro cho người dùng
nếu nó được gọi bằng cách sử dụng giá trị không hợp lệ, code của bạn nên kiểm
tra giá trị hợp lệ trước và gọi `panic!` nếu giá trị không hợp lệ. Điều này
chủ yếu là vì lý do an toàn: cố gắng thực hiện các hoạt động trên dữ liệu không
hợp lệ có thể tiếp cận code của bạn với các lỗ hổng bảo mật. Đây là lý do chính
tại sao thư viện chuẩn sẽ gọi `panic!` nếu bạn cố gắng truy cập bộ nhớ ngoài
phạm vi: cố gắng truy cập bộ nhớ không thuộc cấu trúc dữ liệu hiện tại là một
vấn đề bảo mật phổ biến. Các hàm thường có *hợp đồng (contract)*: hành vi của
chúng chỉ được đảm bảo nếu đầu vào đáp ứng các yêu cầu nhất định. Gọi `panic!`
khi vi phạm hợp đồng là hợp lý bởi vì vi phạm hợp đồng luôn luôn chỉ ra lỗi ở
phía gọi và nó không phải là một loại lỗi bạn muốn code gọi phải xử lý một cách
rõ ràng. Thực ra, không có cách nào cho code gọi phục hồi; người lập trình viên
gọi cần phải sửa code. Hợp đồng cho một hàm, đặc biệt là khi một vi phạm sẽ gây
ra `panic!`, nên được giải thích trong tài liệu API cho hàm đó.

Tuy nhiên, có nhiều kiểm tra lỗi trong tất cả các hàm của bạn sẽ rất dài dòng
và phiền phức. May mắn thay, bạn có thể sử dụng hệ thống kiểu của Rust (và vì
vậy là kiểm tra kiểu được thực hiện bởi trình biên dịch) để thực hiện nhiều
kiểm tra cho bạn. Nếu hàm của bạn có một kiểu cụ thể làm tham số, bạn có thể
tiếp tục với logic code của mình biết rằng trình biên dịch đã đảm bảo bạn có một
giá trị hợp lệ. Ví dụ, nếu bạn có một kiểu thay vì một `Option`, chương trình
của bạn mong đợi sẽ *có gì đó* thay vì *không có gì*. Code của bạn sau đó không
cần phải xử lý hai trường hợp cho các biến thể `Some` và `None`: nó sẽ chỉ có
một trường hợp cho việc chắc chắn có một giá trị. Code cố gắng truyền không có
gì cho hàm của bạn sẽ không thể biên dịch, vì vậy hàm của bạn không cần phải
kiểm tra trường hợp đó khi chạy. Một ví dụ khác là sử dụng một kiểu số nguyên
không dấu như `u32`, đảm bảo tham số không bao giờ âm.

### Creating Custom Types for Validation

Lấy ý tưởng sử dụng hệ thống kiểu Rust để đảm bảo chúng ta có một giá trị hợp
lệ, hơn nữa là tạo một kiểu tùy chỉnh cho việc xác thực. Nhớ lại trò chơi đoán
số trong Chương 2 trong đó code của chúng ta yêu cầu người dùng đoán một số
giữa 1 và 100. Chúng ta không bao giờ xác thực rằng đoán của người dùng nằm
giữa hai số này trước khi kiểm tra nó với số bí mật của chúng ta; chúng ta chỉ
xác thực rằng đoán là dương. Trong trường hợp này, hậu quả không quá nghiêm trọng: output của chúng ta có thể "Quá cao" hoặc "Quá thấp" vẫn sẽ chính xác. Nhưng nó sẽ là một sự cải thiện có ích để hướng dẫn người dùng đến các đoán hợp
lệ và có hành vi khác nhau khi người dùng đoán một số nằm ngoài phạm vi so với
khi người dùng nhập, ví dụ, các chữ cái thay vì số.

Một cách để làm điều này là phân tích đoán dưới dạng `i32` thay vì chỉ `u32` để
cho phép các số âm, và sau đó thêm một kiểm tra cho số nằm trong phạm vi, như
sau:

```rust,ignore
{{#rustdoc_include ../listings/ch09-error-handling/no-listing-09-guess-out-of-range/src/main.rs:here}}
```

Dòng `if` kiểm tra xem giá trị của chúng ta có nằm ngoài phạm vi hay không, nói
với người dùng về vấn đề này, và gọi `continue` để bắt đầu vòng lặp tiếp theo
và yêu cầu một đoán khác. Sau dòng `if`, chúng ta có thể tiếp tục với các
sự so sánh giữa `guess` và số bí mật biết rằng `guess` nằm giữa 1 và 100.

Tuy nhiên, đây không phải là một giải pháp tối ưu: nếu nó là rất quan trọng
cho chương trình chỉ hoạt động trên các giá trị giữa 1 và 100, và nó có nhiều
hàm với yêu cầu này, có một kiểm tra như thế này trong mỗi hàm sẽ là một việc
nhàm chán (và có thể ảnh hưởng đến hiệu suất (performance)).

Thay vào đó, chúng ta có thể tạo một kiểu mới và đặt các xác thực trong một
hàm để tạo một thể hiện của kiểu thay vì lặp lại các xác thực ở mọi nơi. Đó
làm cho an toàn cho các hàm sử dụng kiểu mới trong chữ ký của chúng và tin tưởng
vào các giá trị mà chúng nhận được. Listing 9-13 cho thấy một cách để định nghĩa
một kiểu `Guess` sẽ chỉ tạo một thể hiện của `Guess` nếu hàm `new` nhận được một
giá trị giữa 1 và 100.

<!-- Deliberately not using rustdoc_include here; the `main` function in the
file requires the `rand` crate. We do want to include it for reader
experimentation purposes, but don't want to include it for rustdoc testing
purposes. -->

```rust
{{#include ../listings/ch09-error-handling/listing-09-13/src/main.rs:here}}
```

<span class="caption">Listing 9-13: Một kiểu `Guess` sẽ chỉ tiếp tục với các
giá trị giữa 1 và 100</span>

Đầu tiên, chúng ta định nghĩa một struct có tên `Guess` có một trường có tên
`value` chứa một `i32`. Đây là nơi số sẽ được lưu trữ.

Sau đó chúng ta hiện thưc một hàm `new` trên `Guess` để tạo các instance
của `Guess`. Hàm `new` được định nghĩa để có một tham số có tên `value` của kiểu
`i32` và trả về một `Guess`. Code trong thân hàm `new` kiểm tra `value` để chắc
chắn nó nằm trong khoảng từ 1 đến 100. Nếu `value` không đạt được điều kiện này,
chúng ta sẽ gọi một `panic!`, đó sẽ thông báo cho người lập trình viên đang viết
code gọi hàm này rằng họ có một lỗi mà họ cần phải sửa, vì tạo một `Guess` với
`value` nằm ngoài phạm vi này sẽ vi phạm hợp đồng mà `Guess::new` đang phụ thuộc
vào. Các điều kiện mà `Guess::new` có thể gây ra `panic!` nên được thảo luận
trong tài liệu API của nó; chúng ta sẽ thảo luận về các quy ước về tài liệu
trong chương 14. Nếu `value` đạt được điều kiện này, chúng ta sẽ tạo một
`Guess` mới với trường `value` được thiết lập thành tham số `value` và trả về
`Guess`.

Tiếp theo, chúng ta hiện thực một phương thức có tên `value` mà mượn `self`,
không có bất kỳ tham số nào khác, và trả về một `i32`. Loại phương thức này
đôi khi được gọi là *getter*, vì mục đích của nó là để lấy một số dữ liệu từ
các trường của nó. Phương thức công khai này là cần thiết vì trường `value` của
`Guess` struct là riêng tư. Quan trọng là trường `value` phải là riêng tư để
code sử dụng `Guess` struct không được phép thiết lập `value` trực tiếp: code
bên ngoài module *phải* sử dụng hàm `Guess::new` để tạo một thể hiện của
`Guess`, vì vậy đảm bảo rằng không có cách nào cho một `Guess` có một `value`
mà chưa được kiểm tra bởi các điều kiện trong hàm `Guess::new`.

Một hàm có một tham số hoặc trả về chỉ các số từ 1 đến 100 có thể sau đó khai
báo trong chữ ký (signature) của nó rằng nó nhận hoặc trả về một `Guess` thay
vì một `i32` và không cần phải làm bất kỳ kiểm tra bổ sung nào trong thân hàm
của nó.

## Summary

Xử lý lỗi trong Rust được thiết kế để giúp bạn viết mã có tính ổn định cao hơn.
`panic!` macro cho biết rằng chương trình của bạn đang ở một trạng thái nó không
thể xử lý và cho phép bạn nói với quá trình dừng thay vì cố gắng tiếp tục với
giá trị không hợp lệ hoặc không chính xác. `Result` enum sử dụng hệ thống kiểu của Rust để chỉ ra rằng các hoạt động có thể thất bại một cách mà mã của bạn có
thể khôi phục được. Bạn có thể sử dụng `Result` để nói với mã gọi mã của bạn
rằng nó cần phải xử lý thành công hoặc thất bại có thể xảy ra. Sử dụng `panic!`
và `Result` trong các tình huống phù hợp sẽ làm mã của bạn ổn định hơn đối với các vấn đề không thể tránh được.

Bây giờ bạn đã thấy các cách sử dụng hữu ích mà thư viện chuẩn sử dụng với
`Option` và `Result` enum, chúng ta sẽ nói về cách hoạt động của generics và
cách bạn có thể sử dụng chúng trong mã của bạn.

[encoding]: ch17-03-oo-design-patterns.html#encoding-states-and-behavior-as-types
