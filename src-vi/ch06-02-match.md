<a id="the-match-control-flow-operator"></a>
## The `match` Control Flow Construct

Rust có một cấu trúc điều khiển rất mạnh gọi là `match` cho phép bạn so sánh
một giá trị với một chuỗi các mẫu (pattern) và sau đó thực thi mã dựa trên mẫu
nào khớp. Các pattern có thể được tạo thành từ các giá trị literal, tên biến,
wildcard và nhiều thứ khác; Chương 18 bao gồm tất cả các loại pattern khác nhau
và những gì chúng làm. Sức mạnh của `match` đến từ sự biểu diễn của các mẫu và
sự chắc chắn của trình biên dịch rằng tất cả các trường hợp có thể được xử lý.

Hãy nghĩ về `match` giống như một máy sắp xếp tiền xu: các đồng xu sẽ trượt
xuống một đường với các lỗ có kích thước khác nhau dọc theo đường, và mỗi tiền
xu sẽ rơi vào lỗ đầu tiên nó gặp mà nó vừa vặn vào. Tương tự như vậy, các giá
trị đi qua lần lượt các pattern trong `match`, và ở pattern đầu tiên giá trị
“vừa vặn”, giá trị sẽ rơi vào code liên quan để được sử dụng trong quá trình
thực thi.

Nói về những đồng xu, hãy sử dụng chúng làm ví dụ với `match`! Chúng ta có thể
viết một hàm mà nhận vào một đồng xu của Hoa Kỳ bất kỳ, tương tự như máy đếm
tiền xu, hàm này sẽ xác định đồng xu đó có mệnh giá là gì và trả về mệnh giá
của đồng xu, với đơn vị là cent, như được hiển thị ở đây trong Listing 6-3.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-03/src/main.rs:here}}
```
<span class="caption">Listing 6-3: Một enum và một biểu thức `match` nhận các
biến thể của enum như là các pattern của nó</span>

Cùng làm rõ `match` trong hàm `value_in_cents`. Đầu tiên, chúng ta sẽ liệt kê
từ khóa (keyword) `match` theo sau là một biểu thức, trong trường hợp này là
giá trị `coin`. Điều này có vẻ rất giống với biểu thức được sử dụng với `if`,
nhưng có một sự khác biệt lớn: với `if`, biểu thức cần trả về một giá trị
Boolean, nhưng ở đây, nó có thể trả về bất kỳ kiểu dữ liệu nào. Kiểu dữ liệu
của `coin` trong ví dụ này là enum `Coin` mà chúng ta đã định nghĩa ở dòng đầu
tiên.

Tiếp theo đó là các nhánh (arm) của `match`. Mỗi nhánh này đều có hai thành
phần: pattern và code. Thành phần đầu tiên ở đây là pattern có giá trị là
`Coin::Penny` và sau đó là toán tử `=>` phân tách giữa pattern và code để chạy.
Code trong trường hợp này chỉ là giá trị `1`. Mỗi nhánh được phân tách với
nhánh tiếp theo bằng dấu phẩy.

Khi biểu thức `match` được thực thi, nó sẽ so sánh giá trị kết quả với pattern
của mỗi nhánh, theo thứ tự. Nếu một pattern khớp với giá trị, code liên quan
đến pattern đó sẽ được thực thi. Nếu pattern đó không khớp với giá trị, thực
thi sẽ tiếp tục đến nhánh tiếp theo, giống như trong một máy sắp xếp tiền.
Chúng ta có thể có bao nhiêu nhánh cần thiết: trong Listing 6-3, `match` của
chúng ta có 4 nhánh.

Đoạn code liên quan với mỗi nhánh là một biểu thức (expression), và giá trị kết
quả của biểu thức trong nhánh được khớp sẽ là giá trị được trả về cho toàn bộ
biểu thức `match`.

Chúng ta thường không sử dụng dấu ngoặc nhọn nếu nhánh của match có đoạn code
ngắn, như trong Listing 6-3, mỗi nhánh chỉ trả về một giá trị. Nếu bạn muốn
chạy nhiều dòng code trong một nhánh của match, bạn phải sử dụng dấu ngoặc
nhọn, và dấu phẩy sau nhánh là tùy chọn. Ví dụ, đoạn code sau in ra “Lucky
penny!” mỗi khi phương thức được gọi với một `Coin::Penny`, nhưng vẫn trả về
giá trị cuối cùng của khối, `1`:
  
  ```rust
  {{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-08-match-arm-multiple-lines/src/main.rs:here}}
  ```

---

### Patterns that Bind to Values

Một tính năng hữu ích khác của các nhánh của match là chúng có thể gắn với giá
trị khi khớp của pattern. Đây là cách chúng ta có thể trích xuất các giá trị từ
các biến thể của enum.

Lấy ví dụ, hãy thay đổi một trong các biến thể của enum để chứa dữ liệu bên
trong nó. Từ năm 1999 đến năm 2008, Hoa Kỳ đúc các đồng 25 cent (Quater) cho 50
tiều bang với các thiết kết mặt bên khác nhau. Các loại xu khác không có thiết
kế riêng theo tiểu bang, vì vậy chỉ có bội đồng 25 cent mới có giá trị bổ sung
này. Chúng ta có thể thêm thông tin này vào `enum` bằng cách thay đổi biến thể
`Quarter` để bao gồm một giá trị `UsState` được lưu trữ bên trong nó, như chúng
ta đã làm ở đây trong Listing 6-4.

  ```rust
  {{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-04/src/main.rs:here}}
  ```

  <span class="caption">Listing 6-4: Một `Coin` enum trong đó biến thể `Quarter` cũng chứa một giá trị `UsState`</span>

Hãy tưởng tượng rằng một người bạn đang cố gắng thu thập tất cả 50 dạng đồng 25
cent này. Trong khi chúng ta chỉ sắp xếp theo loại đồng, chúng ta cũng sẽ gọi
tên của tiểu bang liên quan trên đồng 25 cent để nếu đó là một trong những đồng
25 cent mà người bạn của chúng ta không có, anh ấy có thể thêm vào bộ sưu tập
của mình.

Bên trong biểu thức `match` cho đoạn code này, chúng ta thêm một biến gọi là
`state` vào pattern khớp với các giá trị của biến thể `Coin::Quarter`. Khi một
`Coin::Quarter` khớp, biến `state` sẽ được gắn với giá trị của tiểu bang của
đồng 25 cent đó. Sau đó, chúng ta có thể sử dụng `state` trong code strong
nhánh này, như sau:

  ```rust
  {{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-09-variable-in-pattern/src/main.rs:here}}
  ```

Nếu chung ta gọi hàm `value_in_cents(Coin::Quarter(UsState::Alaska))`, `coin`
sẽ là `Coin::Quarter(UsState::Alaska)`. Khi chúng ta so sánh giá trị này với
mỗi nhánh trong biểu thức `match`, không có nhánh nào khớp cho đến khi chúng ta
đến `Coin::Quarter(state)`. Tại thời điểm đó, biến `state` sẽ là giá trị
`UsState::Alaska`. Chúng ta có thể sử dụng biến này trong biểu thức `println!`,
vì vậy chúng ta có thể lấy ra giá trị của tiểu bang bên trong biến thể
`Quarter` của `Coin` enum.

### Matching with `Option<T>`

Trong phần trước, chúng ta muốn lấy ra giá trị `T` bên trong `Some` khi sử dụng
`Option<T>`; chúng ta cũng có thể xử lý `Option<T>` bằng cách sử dụng `match`
như chúng ta đã làm với `Coin` enum! Thay vì so sánh các đồng, chúng ta sẽ so
sánh các biến thể của `Option<T>`, nhưng cách thức hoạt động của biểu thức
`match` vẫn giữ nguyên.

Giả sử chúng ta muốn viết một hàm mà nhận vào một `Option<i32>` và nếu có một
giá trị bên trong, nó sẽ cộng thêm 1 vào giá trị đó. Nếu không có giá trị bên
trong, hàm sẽ trả về giá trị `None` và không thực hiện bất kỳ thao tác nào.

Hàm này rất dễ để viết, cảm ơn `match`, và sẽ trông như trong Listing 6-5.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:here}}
```

Hãy xem xét kỹ hơn về lần đầu tiên chúng ta gọi `plus_one`. Khi chúng ta gọi
`plus_one(five)`, biến `x` trong thân hàm `plus_one` sẽ có giá trị `Some(5)`.
Chúng ta sau đó so sánh nó với mỗi nhánh trong biểu thức `match`.

```rust,ignore
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:first_arm}}
```

Giá trị `Some(5)` không khớp với mẫu `None`, vì vậy chúng ta tiếp tục đến nhánh
tiếp theo.

```rust,ignore
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:second_arm}}
```

`Some(5)` có khớp với `Some(i)` không? Vâng, nó khớp! Chúng ta có cùng một biến
thể. `i` được gắn với giá trị bên trong `Some`, vì vậy `i` có giá trị là `5`.
Sau đó, code trong nhánh `match` được thực thi, vì vậy chúng ta cộng thêm 1 vào
giá trị của `i` và tạo ra một giá trị `Some` mới với tổng `6` bên trong.

Giờ hãy xem xét lần gọi thứ hai của `plus_one` trong Listing 6-5, khi `x` là
`None`. Chúng ta vào `match` và so sánh với nhánh đầu tiên.

```rust,ignore
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-05/src/main.rs:first_arm}}
```

Nó khớp! Không có giá trị để cộng thêm, vì vậy chương trình dừng lại và trả về
giá trị `None` bên phải của `=>`. Vì nhánh đầu tiên khớp, nên các nhánh khác
không được so sánh.

Kết hợp `match` và enums rất hữu ích trong nhiều tình huống. Bạn sẽ thấy
pattern này nhiều trong mã Rust: `match` với một enum, gắn một biến với dữ liệu
bên trong, và sau đó thực thi code dựa trên nó. Đầu tiên nó có vẻ khó khăn,
nhưng một khi bạn quen với nó, bạn sẽ muốn có nó trong tất cả các ngôn ngữ. Nó
luôn được người dùng ưa thích.

### Matches Are Exhaustive

Có một khía cạnh khác của `match` mà chúng ta cần thảo luận: các pattern thuộc
các nhánh phải bao phủ tất cả các khả năng. Hãy xem xét phiên bản lỗi của hàm
`plus_one` của chúng ta, nó sẽ không biên dịch được:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-10-non-exhaustive-match/src/main.rs:here}}
```

Chúng ta đã không xử lý trường hợp `None`, vì vậy mã này sẽ gây ra một lỗi. May
mắn thay, đó là một lỗi mà Rust biết cách phát hiện. Nếu chúng ta cố gắng biên
dịch mã này, chúng ta sẽ nhận được lỗi này:

```console
{{#include ../listings/ch06-enums-and-pattern-matching/no-listing-10-non-exhaustive-match/output.txt}}
```

Rust biết rằng chúng ta không bao phủ (cover) tất cả các trường hợp có thể và
cũng biết pattern nào chúng ta quên! Các `match` trong Rust là *đầy đủ*
(exhaustive): chúng ta phải bao phủ tất cả các trường hợp để mã có thể hợp lệ.
Đặc biệt trong trường hợp của `Option<T>`, khi Rust ngăn chúng ta quên xử lý
trường hợp `None` một cách rõ ràng, nó bảo vệ chúng ta khỏi việc giả định rằng
chúng ta có một giá trị khi chúng ta có thể có null, vì vậy lỗi tỷ đô đầu tiên
được thảo luận ở trên là không thể xảy ra.

### Catch-all Patterns and the `_` Placeholder

Khi dùng với enum, chúng ta cũng có thể thực hiện các hành động đặc biệt cho
một vài giá trị cụ thể, nhưng với tất cả các giá trị khác, chúng ta sẽ thực
hiện một hành động mặc định. Hãy tưởng tượng chúng ta đang triển khai một trò
chơi, nếu bạn tung xúc xắc và nhận được 3, người chơi của bạn sẽ không di
chuyển, mà thay vào đó sẽ nhận được một chiếc mũ đẹp. Nếu bạn tung xúc xắc và
nhận được 7, người chơi của bạn sẽ mất một chiếc mũ đẹp. Với tất cả các giá trị
khác, người chơi của bạn sẽ di chuyển số lượng ô trên bàn cờ tương ứng với giá
trị của xúc xắc. Đây là một `match` thực hiện logic này, với kết quả của xúc
xắc được cố định thay vì một giá trị ngẫu nhiên, và tất cả các logic khác được
biểu diễn bởi các hàm mà không có thân hàm vì thực sự triển khai chúng nằm
ngoài phạm vi của ví dụ này:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-15-binding-catchall/src/main.rs:here}}
```

Với hai nhánh đầu tiên, các pattern là các giá trị chính xác 3 và 7. Với nhánh
cuối cùng bao gồm tất cả các giá trị khác có thể, pattern là biến mà chúng ta
đã chọn để đặt tên là `other`. Code chạy cho nhánh `other` sử dụng biến bằng
cách truyền nó cho hàm `move_player`.

Code này sẽ được biên dịch, ngay cả khi chúng ta không liệt kê tất cả các giá
trị có thể có của một `u8`, vì nhánh cuối cùng sẽ khớp với tất cả các giá trị
không được liệt kê cụ thể. Pattern này đáp ứng yêu cầu rằng `match` phải là đầy
đủ. Lưu ý rằng chúng ta phải đặt nhánh cuối cùng ở cuối vì các pattern được
khớp theo thứ tự. Nếu chúng ta đặt nhánh cuối cùng ở trước, các nhánh khác sẽ
không bao giờ chạy, vì vậy Rust sẽ cảnh báo chúng ta nếu chúng ta thêm các
nhánh sau một nhánh cuối cùng!

Rust cũng có một pattern mà chúng ta có thể sử dụng khi chúng ta muốn một
pattern cuối cùng nhưng không muốn *sử dụng* giá trị trong pattern cuối cùng:
`_` là một pattern đặc biệt mà khớp với bất kỳ giá trị nào và không liên kết
với giá trị đó. Điều này cho Rust biết chúng ta không sẽ sử dụng giá trị, vì
vậy Rust sẽ không cảnh báo chúng ta về một biến không được sử dụng.

Cùng thay đổi quy tắc của trò chơi: bây giờ, nếu bạn tung xúc xắc và nhận được
bất kỳ giá trị nào khác ngoài 3 hoặc 7, bạn phải quay lại. Chúng ta không cần
nữa sử dụng giá trị cuối cùng, vì vậy chúng ta có thể thay đổi code của chúng
ta để sử dụng `_` thay vì biến có tên là `other`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-16-underscore-catchall/src/main.rs:here}}
```

Ví dụ này cũng đáp ứng yêu cầu đầy đủ vì chúng ta đang tường minh bỏ qua tất cả
các giá trị khác trong nhánh cuối cùng; chúng ta không bỏ sót bất kỳ thứ gì.

Cuối cùng, chúng ta sẽ thay đổi quy tắc của trò chơi một lần nữa, sẽ không có
gì khác xảy ra trong lượt của bạn nếu bạn tung xúc xắc và nhận được bất kỳ giá
trị nào khác ngoài 3 hoặc 7. Chúng ta có thể biểu diễn bằng cách sử dụng giá
trị đơn vị (kiểu tuple trống mà chúng ta đã nói ở phần [“The Tuple Type”][tuples]<!-- ignore -->) làm mã cho nhánh `_`:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-17-underscore-unit/src/main.rs:here}}
```

Ở đây, chúng ta đang nói với Rust rõ ràng rằng chúng ta sẽ không sử dụng bất kỳ
giá trị nào khác không khớp với mẫu trong nhánh trước đó, và chúng ta không
muốn chạy bất kỳ mã nào trong trường hợp này.

Để có thêm về các pattern và matching, chúng ta sẽ tìm hiểu trong [Chapter 18][ch18-00-patterns]<!-- ignore -->. Bây giờ, chúng ta sẽ tiếp tục với cú pháp
`if let`, nó có thể hữu ích trong các tình huống mà biểu thức `match` có vẻ quá
dài.

[tuples]: ch03-02-data-types.html#the-tuple-type
[ch18-00-patterns]: ch18-00-patterns.html
