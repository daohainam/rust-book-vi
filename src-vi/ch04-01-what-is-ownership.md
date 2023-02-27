<a id="the-stack-and-the-heap"></a>
## Ownership (tính sở hữu) là gì?

*Ownership* là một tập các quy tắc phối hợp với nhau, định hình cách Rust quản lý
bộ nhớ. Tất cả các chương trình đều phải quản lý cách chúng sử dụng bộ nhớ máy
tính khi chạy. Một số ngôn ngữ có bộ dọn rác chạy định kỳ để giải phóng các phần
bộ nhớ không còn được dùng tới; trong các ngôn ngữ khác, chương trình phải xin 
cấp phát hoặc giải phóng bộ nhớ một cách cụ thể. Rust dùng cách tiếp cận thứ ba: 
bộ nhớ được quản lý thông qua một hệ thống sở hữu với một tập các quy tắc sẽ được
kiểm tra bởi trình biên dịch. Nếu bất kỳ quy tắc nào bị vi phạm, chương trình sẽ
không được dịch. Không có bất kỳ tính năng nào của ownership làm chậm chương trình 
của bạn khi chạy.

Vì ownership là một khái niệm mới với nhiều lập trình viên, nó sẽ mất một thời 
gian để làm quen. Tin tốt là càng có nhiều kinh nghiệm với Rust và các quy tắc 
của hệ thống ownership, bạn càng thấy việc lập trình các code an toàn và hiệu quả
dễ dàng hơn. Vậy nên hãy cố lên!

Khi đã hiểu ownership, bạn sẽ có một nền tảng chắc chắn để hiểu các tính năng
vốn làm cho Rust trở nên độc nhất. Trong chương này bạn sẽ học về ownership
thông qua một số ví dụ tập trung vào một cấu trúc dữ liệu rất phổ biến: string.

> ### Stack và Heap
>
> Nhiều ngôn ngữ lập trình không yêu cầu bạn phải nghĩ về stack và heap thường
> xuyên. Nhưng với một ngôn ngữ lập trình hệ thống như Rust, việc một giá trị sẽ
> được lưu trên stack hay trên heap ảnh hưởng tới cách ngôn ngữ xử lý cũng như 
> lý do bạn quyết định. Các phần của ownership sẽ được mô tả trong mối quan hệ 
> với stack và heap ở phần sau của chương này, ở đây chỉ là một giải thích
> ngắn gọn để chuẩn bị.
>
> Cả hai stack và heap đều là các phần của bộ nhớ mà chương trình của bạn có thể
> sử dụng khi chạy, nhưng chúng được tổ chức theo những cách khác nhau. Stack lưu 
> trữ các giá trị theo thứ tự bạn đưa vào và lấy các giá trị ra theo thứ tự ngược
> lại. Ta thường gọi là *last in, first out*. Hãy tưởng tượng một chồng đĩa: khi 
> bạn thêm đĩa, bạn sẽ đặt chúng lên trên cùng, và khi cần lấy một cái đĩa, bạn 
> sẽ lấy cái trên nhất. Bạn sẽ không bỏ vào hoặc lấy ra các đĩa ở giữa hay bên 
> dưới cùng. Thêm dữ liệu vào được gọi là *đẩy (push)* và stack, và thao tác 
> lấy ra được gọi là *lấy ra khỏi stack (pop)*. Tất cả dữ liệu lưu trên stack 
> phải có một kiểu dữ liệu có kích thước cố định cho trước. Dữ liệu có kích thước 
> không thể biết trước khi biên dịch hoặc có thể thay đổi phải được lưu trên heap.
>
> Heap được tổ chức ít quy củ hơn: khi bạn đưa dữ liệu lên heap, bạn yêu cầu một
> không gian trống có kích thước cụ thể. Bộ phân phối bộ nhớ sẽ tìm một phần trống 
> trên heap đủ lớn để chứa, đánh dấu nó đã được dùng, và trả về một *con trỏ (pointer)*,
> chứa địa chỉ của vị trí phần bộ nhớ vừa được cấp phát. Quá trình này được gọi
> là *phân phối bộ nhớ trên heap* và đôi khi được gọi ngắn gọn là *phân phối bộ nhớ*
> (đẩy một giá trị vào stack không được coi là phân phối bộ nhớ). Vì con trỏ chứa 
> địa chỉ vùng nhớ có kích thước cố định và biết trước, nó có thể được lưu trong
> stack, nhưng khi bạn cần lấy dữ liệu thực sự, bạn sẽ cần đi theo địa chỉ chứa
> trong con trỏ. Hãy tưởng tượng khi đến một nhà hàng, bạn cho nhân viên biết
> số người trong nhóm, họ sẽ tìm một bàn trống đủ cho nhóm của bạn và dẫn bạn đến
> đó. Nếu một người trong nhóm đến muộn, họ có thể hỏi nơi bạn ngồi để tìm bạn.
>
> Đẩy một giá trị vào stack nhanh hơn phân phối trên heap vì trình quản lý không
> cần tìm một nơi để lưu dữ liệu; vị trí đó luôn nằm trên đỉnh của stack. Trong khi
> đó, phân phối bộ nhớ trên heap cần nhiều thao tác hơn vì trình quản lý đầu tiên
> phải tìm một không gian trống đủ lớn để chứa dữ liệu, sau đó làm các thao tác để 
> đánh dấu việc sử dụng không gian nhớ đó. 
> 
> Truy cập dữ liệu trên heap cũng chậm hơn trong stack vì bạn phải theo một con trỏ để
> đến đúng nơi. Các bộ xử lý hiện nay sẽ hoạt động nhanh hơn nếu chúng không phải 
> truy cập bộ nhớ nhiều. Tiếp tục với ví dụ ở trên, hãy tưởng tượng một người phục
> vụ ở nhà hàng phải nhận đặt món từ nhiều bàn khác nhau. Cách làm hiệu quả nhất là
> nhận tất cả yêu cầu đặt món từ một bàn trước khi di chuyển đến bàn tiếp theo. Nhận
> món từ bàn A, rồi sang bàn B, sau đó quay lại bàn A, rồi tiếp tục quay lại bàn B 
> có lẽ sẽ chậm hơn nhiều. Theo cùng cách, một bộ xử lý có thể hoàn thành công việc 
> tốt hơn nếu nó làm việc với các dữ liệu nằm gần nhau (giống như trong stack) hơn 
> là khi chúng nằm xa nhau (như với heap).
> 
> Khi code của bạn gọi một hàm, các giá trị truyền vào cho hàm đó (có thể bao gồm 
> cả các con trỏ lên heap) và các biến cục bộ của hàm đều được đẩy vào stack. Khi
> hàm kết thúc, các giá trị đó sẽ được lấy ra khỏi stack.
>
> Theo dõi phần code nào code đang dùng những phần nào của heap, tối thiểu hóa việc
> trùng lắp dữ liệu, và dọn dẹp những dữ liệu nào không được dùng tới trên heap sao
> cho chúng ta không cạn kiệt các tài nguyên bộ nhớ là những vấn đề mà ownership 
> nhắm đến. Một khi đã hiểu về ownership, bạn sẽ không cần nghĩ về stack và heap thường
> xuyên nữa, nhưng biết mục đích chính của ownership là để quản lý bộ nhớ heap có thể 
> giúp giải thích vì sao nó làm việc theo cách mà bạn sẽ thấy. 

### Các quy tắc của Ownership

Đầu tiên, hãy xem qua các quy tắc của ownership. Hãy ghi nhớ các quy tắc này khi
ta đi qua các ví đụ minh họa:

* Mỗi giá trị trong Rust có một chủ sở hữu (*owner*)
* Mỗi thời điểm chỉ có duy nhất một owner.
* Khi owner ra khỏi phạm vi (scope, tầm vực của biến) của nó, giá trị sẽ bị hủy.

### Tầm vực của biến

Giờ ta đã hoàn thành cú pháp cơ bản của Rust, chúng ta sẽ không cần thêm `fn main() {`
vào các ví dụ, do vậy nếu muốn chạy thử hãy tự thêm chúng vào trong hàm `main`. Khi viết 
các ví dụ theo cách ngắn gọn như vậy, chúng ta có thể tập trung vào chi tiết muốn
nói hơn là các đoạn code mẫu.

Ví dụ đầu tiên về ownership, chúng ta sẽ xem qua *tầm vực (scope)* của một số biến.
Một tầm vực là một đoạn trong một chương trình mà trong đó một thành phần nào đó
là hợp lệ. Hãy xem qua ví dụ sau:

```rust
let s = "hello";
```

Biến `s` tham chiếu đến một giá trị chuỗi, giá trị này được hard code vào trong 
phần text của chương trình. Các biến là hợp lệ tại thời điểm chúng được khai báo
cho đến hết *tầm vực* hiện tại. Listing 4-1 trình bày một chương trình với các 
ghi chú chỉ ra nơi biến `s` là hợp lệ.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-01/src/main.rs:here}}
```

<span class="caption">Listing 4-1: Một biến và tầm vực (phạm vi) của nó</span>

Nói cách khác, có hai thời điểm quan trọng ở đây:

* Khi `s` đi vào trong tầm vực, nó trở nên hợp lệ.
* `s` sẽ vẫn hợp lệ cho đến khi nó đi ra khỏi *tầm vực*.

Tại điểm này, mối quan hệ giữa các tầm vực và khi các biến là hợp lệ hoàn toàn tương 
tự trong các ngôn ngữ lập trình khác. Giờ chúng ta sẽ áp dụng điều này với kiểu 
dữ liệu `String` để tìm hiểu sâu hơn.

### Kiểu `String`

Để minh họa các quy tắc của ownership, chúng ta sẽ cần một kiểu dữ liệu phức tạp
hơn những kiểu đã được nói đến trong phần [“Các kiểu dữ liệu”][data-types]<!-- ignore --> 
ở chương 3. Những kiểu được nói đến trong phần đó có kích thước cố định, có thể 
được lưu trong stack và dễ dàng lấy ra khi đi ra khỏi phạm vi của nó, cũng như 
có thể tạo một bản sao mới độc lập với bản gốc nếu một phần khác của chương trình 
muốn đọc giá trị của nó. Nhưng chúng ta muốn xem cách dữ liệu được lưu trên heap 
và khám phá cách Rust biết khi nào cần giải phóng dữ liệu đó, trong trường hợp
này `String` là một ví dụ tuyệt vời.

Chúng ta sẽ tập trung trên các phần của `String` có liên quan đến tính sở hữu (ownership).
Nhưng những phần này cũng có thể áp dụng lên các kiểu dữ liệu phức tạp khác,
bao gồm cả các kiểu trong thư viện chuẩn và các kiểu do bạn tự tạo. Chúng ta sẽ 
nói sâu hơn về `String` trong [Chương 8][ch8]<!-- ignore -->.

Chúng ta đã xem các giá trị chuỗi, khi mà các chuỗi được hard code vào thẳng
trong chương trình. Các giá trị chuỗi rất có ích, nhưng chúng lại không phù hợp
cho nhiều trường hợp. Một lý do là chúng không thể thay đổi. Một lý do khác là 
trong nhiều trường hợp ta không biết giá trị thực sự của nó lúc viết code: ví
dụ nếu bạn muốn nhận dữ liệu từ người dùng và lưu lại? Với những trường hợp như 
vậy, Rust có một kiểu dữ liệu chuỗi nữa, `String`. Kiểu dữ liệu này quản lý dữ liệu 
được phân bố trên heap và nó có khả năng lưu trữ một khối văn bản ta không biết
vào thời điểm biên dịch. Bạn có thể tạo một `String` từ một giá trị chuỗi bằng cách 
dùng hàm `from`, giống như sau:

```rust
let s = String::from("hello");
```

Cặp dấu hai chấm `::` cho phép chúng ta đặt các thành phần trong Rust vào các 
namespace khác nhau. Chúng ta có thể chỉ ra hàm `from` nằm trong `String` thay vì 
phải viết theo kiểu `string_from`. Chúng ta sẽ thảo luận thêm về cú pháp này trong phần
[“Cú pháp của phương thức”][method-syntax]<!-- ignore --> ở chương 5. Và khi chúng 
ta nói về namespace với các module trong [“Paths for Referring to an Item in the
Module Tree”][paths-module-tree]<!-- ignore --> ở chương 7.

Kiểu chuỗi này *có thể* thay đổi.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-01-can-mutate-string/src/main.rs:here}}
```

Vậy sự khác biệt ở đây là gì? Tại sao `String` có thể thay đổi mà hằng chuỗi thì
không? Sự khác nhau nằm ở cách hai loại này thao tác với bộ nhớ.

### Bộ nhớ và phân phối bộ nhớ

Trong trường hợp của hằng chuỗi, chúng ta biết nội dung của nó vào lúc biên dịch,
so vậy nội dung văn bản của nó sẽ được biên dịch thẳng vào bên trong file thực thi.
Đây là lý do vì sao các hằng chuỗi nhanh và hiệu quả. Nhưng những tính chất đó chỉ 
có nhờ vào tính không-khả-biến (không thể thay đổi) của hằng chuỗi. Không may là,
bạn không thể nhúng một khối văn bản vào một file thực thi mà không biết kích thước
của nó vào lúc biên dịch, hoặc kích thước đó có thể thay đổi vào lúc chạy chương 
trình.

Với kiểu `String`, để cho phép thay đổi nội dung, hoặc tăng độ dài của khối văn
bản, chúng ta cần phân phối cho nó một phần bộ nhớ trên heap để lưu trữ nội dung.
Điều này có nghĩa là:

* Phần bộ nhớ này cần được cấp pháp bởi trình quản lý bộ nhớ khi chạy chương trình.
* Cần một cách để trả lại phần bộ nhớ này khi đã làm việc xong với chuỗi
`String` của chúng ta.

Phần thứ nhất đã được hoàn thành khi chúng ta gọi `String::from`, hàm `from` sẽ 
yêu cầu phần bộ nhớ mà nó cần. Những thao táo quen thuộc này khá phổ biến trong 
các ngôn ngữ lập trình.

Tuy nhiên, phần thứ hai lại khác. Trong các ngôn ngữ có *bộ dọn rác* (garbage collector
(GC)), GC sẽ theo dõi và giải phóng các phần bộ nhớ không còn được dùng đến, và 
ta không cần phải quan tâm đến chúng. Trong hầu hết các ngôn ngữ không có GC,
chúng ta phải có trách nhiệm tự quản lý các vùng nhớ để biết chúng khi nào không
còn được dùng nữa và gọi hàm giải phóng bộ nhớ. Công việc này vốn đã được lịch sử
chứng minh là rất khó để làm một các đúng đắn. Nếu ta lỡ quên, chúng ta sẽ gây lãng
phí bộ nhớ. Nếu ta làm điều đó quá sớm, chúng ta sẽ có một biến không hợp lệ. Nếu
ta làm điều đó hai lần, đó cũng là lỗi. Chúng ta phải có chính xác từng `free` cho 
mỗi `allocate`.

Rust chọn một con đường khác: phần bộ nhớ sẽ được tự động trả lại một khi biến đi
ra khỏi tầm vực của nó. Đây là một phiên bản của ví dụ về tầm vực từ Listing 4-1 
nhưng sử dụng `String` thay vì một hằng chuỗi:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-02-string-scope/src/main.rs:here}}
```

Có một thời điểm tự nhiên mà chúng ta có thể trả lại phần bộ nhớ mà biến `String`
cần: khi `s` đi ra khỏi tầm vực của nó. Khi một biến đi ra khỏi scope, Rust gọi một 
hàm đặc biệt cho chúng ta. Hàm này được gọi là [`drop`][drop]<!-- ignore -->, và
nó là nơi tác giả của `String` có thể viết code để trả lại phần bộ nhớ đã cấp phát
trước đó. Rust gọi `drop` một cách tự động ngay tại vị trí dấu ngoặc nhọn đóng.

> Ghi chú: trong C++, mẫu thiết kế cho phép tự động giải phóng tài nguyên vào 
> thời điểm một phần tử nào đó kết thúc vòng đời đôi khi được gọi là: *Resource 
> Acquisition Is Initialization (RAII)*. Hàm `drop` trong Rust sẽ là quen thuộc 
> nếu bạn đã từng sử dụng mẫu RAII.

Mẫu thiết kế này có một sự ảnh hưởng sâu sắc đến cách viết code Rust. Nó trông
có vẻ đơn giản, nhưng trong những trường hợp phức tạp code có thể trở nên khó dự 
đoán, như khi ta có nhiều biến dùng dữ liệu được phân bố trên heap. Hãy cùng khảo 
sát một vài trường hợp:

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-move"></a>

#### Các biến và việc tương tác dữ liệu với Move

Nhiều biến có thể tương tác với cùng dữ liệu theo những cách khác nhau trong Rust.
Hãy cùng xem qua một ví dụ sử dụng biến kiểu integer trong Listing 4-2.

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-02/src/main.rs:here}}
```

<span class="caption">Listing 4-2: Gán một số nguyên từ biến `x` sang biến `y`</span>

Chúng ta có thể đoán xem đoạn code này làm gì: "gán giá trị `5` vào `x`; sau đó
tạo một bản sao của giá trị trong `x` và gán nó cho `y`". Chúng ta sẽ có hai biến, 
`x` và `y`, và cả hai đều bằng `5`. Đây thực sự là những gì đã diễn ra, vì số nguyên
là những giá trị đơn giản với kích thước cố định biết trước, và hai giá trị `5` đó
sẽ được lưu trữ trên stack.

Giờ hãy xem qua phiên bản với `String`:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-03-string-move/src/main.rs:here}}
```

Đoạn này trông khá tương tự, do vậy ta có thể cho là chúng hoạt động theo cùng cách:
đó là, dòng thứ hai sẽ tạo một bản sao của giá trị trong `s1` và gán nó vào cho 
`s2`. Nhưng đây không thực sự là điều diễn ra.

Hãy xem qua Figure 4-1 để xem điều gì diễn ra đằng sau đối với `String`. Một 
`String` được tạo nên từ ba phần, được hiển thị phía bên trái: một con trỏ
đến phần bộ nhớ lưu giữ nội dung của chuỗi, một biến chứa chiều dài và một chứa 
khả năng lưu trữ. Nhóm dữ liệu này được lưu trên stack. Bên phía phải là phần bộ
nhớ chứa nội dung của chuỗi.

<img alt="Hai bảng: Bảng đầu tiên chứa biểu diễn của s1 trên stack, bao gồm chiều 
dài (5), dung lượng khả dụng (5) và một con trỏ đến giá trị đầu tiên trong bảng thứ
hai. Bảng thứ hai chứa biểu diễn dữ liệu của chuỗi trên heap, theo từng byte."
src="img/trpl04-01.svg" class="center"
style="width: 50%;" />

<span class="caption">Figure 4-1: Biểu diễn bên trong bộ nhớ của một `String`
chứa giá trị `"hello"` gán cho biến `s1`</span>

Chiều dài là bao nhiêu bộ nhớ, tính theo byte, mà nội dung của `String` hiện đang
dùng. Dung lượng khả dụng là tổng số bộ nhớ, tính theo byte, mà `String` đã nhận
từ trình quản lý bộ nhớ. Sự khác nhau giữa chiều dài và dung lượng khả dụng, vì 
không có trong ví dụ này, nên hiện tại ta tạm thời bỏ qua không nói đến.

Khi ta gán `s1` vào `s2`, nội dung của `String` sẽ được sao chép, có nghĩa là ta
chỉ sao chép các thông tin con trỏ, chiều dài, và dung lượng khả dụng vốn được 
lưu trên stack. Ta không sao chép dữ liệu trên heap mà con trỏ trỏ đến. Nói cách 
khác, biểu diễn dữ liệu trong bộ nhớ sẽ giống như trong hình minh họa 4-2.

<img alt="Ba bảng: bảng s1 và s2 biểu diễn các string tương ứng trên stack, và 
cả hai cùng trỏ vào cùng một vùng dữ liệu trên heap."
src="img/trpl04-02.svg" class="center" style="width: 50%;" />

<span class="caption">Hình minh họa 4-2: Biểu diễn trong bộ nhớ của biến `s2`
chứa bản sao con trỏ, chiều dài, và dung lượng khả dụng `s1`</span>

Biểu diễn *không* trông giống như Hình 4-3, là tổ chức bộ nhớ trong trường hợp Rust 
cũng sao chép dữ liệu heap. Nếu Rust đã làm điều này, phép gán `s2 = s1` có thể 
rất tốn kém về hiệu suất thời gian chạy nếu dữ liệu trên heap lớn.

<img alt="Bốn bảng: hai bảng đại diện cho dữ liệu stack cho s1 và s2,
và mỗi con trỏ trỏ đến bản sao dữ liệu chuỗi của riêng nó trên heap."
src="img/trpl04-03.svg" class="center" style="width: 50%;" />

<span class="caption">Hình 4-3: Biểu diễn bộ nhớ sau phép gán `s2 = s1` nếu Rust 
sao chép cả dữ liệu trên heap</span>

Trước đó, chúng tôi đã nói rằng khi một biến ra ngoài phạm vi, Rust sẽ tự động
gọi hàm `drop` và dọn sạch bộ nhớ heap cho biến đó. Nhưng Hình 4-2 cho thấy cả 
hai con trỏ dữ liệu trỏ đến cùng một vị trí. Đây là một vấn đề: khi `s2` và `s1` 
vượt ra ngoài phạm vi, cả hai sẽ cố gắng giải phóng cùng một phần bộ nhớ. Đây 
được gọi là lỗi *double free* (giải phóng hai lần) và là một trong những lỗi 
lỗi an toàn bộ nhớ mà chúng ta đã đề cập đến trước đây. Giải phóng bộ nhớ 
hai lần có thể dẫn đến sai sót trong tổ chức bộ nhớ, và có khả năng dẫn đến 
các lỗ hổng bảo mật.

Để đảm bảo an toàn cho bộ nhớ, sau dòng `let s2 = s1;`, Rust coi `s1` là
không còn giá trị. Do đó, Rust không cần giải phóng bất cứ thứ gì khi `s1` ra
ra khỏi phạm vi. Xem điều gì sẽ xảy ra khi bạn cố gắng sử dụng `s1` sau khi `s2` 
được tạo; nó sẽ không hoạt động:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/src/main.rs:here}}
```

Bạn sẽ gặp lỗi như thế này vì Rust ngăn bạn sử dụng tham chiếu không hợp lệ:

```console
{{#include ../listings/ch04-understanding-ownership/no-listing-04-cant-use-after-move/output.txt}}
```

Nếu bạn đã nghe các thuật ngữ *shallow copy* (sao chép cạn) và *deep copy* 
(sao chép sâu) khi làm việc với các ngôn ngữ khác, việc sao chép con trỏ, 
độ dài và dung lượng mà không sao chép dữ liệu có thể giống như tạo một 
shallow copy. Nhưng vì Rust cũng vô hiệu hóa biến đầu tiên, thay vì được gọi là
bản sao nông, nó được gọi là *move* (di chuyển). Trong ví dụ này, chúng ta 
sẽ nói rằng `s1` đã được *move* vào `s2`. Vì vậy, những gì thực sự xảy ra 
được thể hiện trong Hình 4-4.

<img alt="Ba bảng: bảng s1 và s2 đại diện cho các chuỗi đó trên
stack tương ứng và cả hai đều trỏ đến cùng một dữ liệu chuỗi trên heap.
Bảng s1 chuyển sang màu xám vì s1 không còn giá trị; chỉ s2 có thể được sử dụng để
truy cập dữ liệu heap." src="img/trpl04-04.svg" class="center" style="width:
50%;" />

<span class="caption">Hình 4-4: Biểu diễn trong bộ nhớ sau khi `s1` được
vô hiệu</span>

Điều này giúp giải quyết vấn đề của chúng ta! Với chỉ `s2` hợp lệ, khi vượt ra 
phạm vi, chỉ có nó phải giải phóng bộ nhớ, và chỉ vậy là đủ.

Ngoài ra, có một lựa chọn thiết kế được ngụ ý bởi điều này: Rust sẽ không bao giờ
tự động tạo các bản sao dữ liệu “sâu” của bạn. Do đó, bất kỳ thao tác *tự động*
sao chép đều có thể được coi là không tốn kém về hiệu suất hoạt động.

<!-- Old heading. Do not remove or links may break. -->
<a id="ways-variables-and-data-interact-clone"></a>

#### Các biến và tương tác dữ liệu với Clone

Nếu chúng tôi *thực sự* muốn sao chép cả dữ liệu trên heap của `String`, không chỉ
dữ liệu trên stack, chúng ta có thể sử dụng một phương pháp phổ biến gọi là `clone` (nhân bản). 
Chúng ta sẽ thảo luận về cú pháp trong Chương 5, nhưng bởi vì các phương thức kiểu này 
là một tính năng phổ biến trong nhiều ngôn ngữ lập trình, bạn có thể đã nhìn thấy chúng trước đây.

Đây là một ví dụ về phương thức `clone` đang hoạt động:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-05-clone/src/main.rs:here}}
```

Cách này hoạt động hoàn toàn tốt và tạo ra hành vi như trong Hình 4-3,
khi mà dữ liệu trên heap *thực sự* được sao chép.

Khi bạn thấy lệnh gọi đến `clone`, bạn sẽ biết rằng một số code tùy biến đang được
được thực hiện và code đó có thể tốn kém. Đó cũng là một chỉ dẫn trực quan cho thấy 
rằng một điều gì đó khác đang diễn ra.

#### Dữ liệu trên stack: sao chép

Có một điều thắc mắc khác mà chúng ta chưa nói đến. Mã này sử dụng
integer — là một phần được hiển thị trong trong Listing 4-2 — chạy được và 
hoàn toàn hợp lệ:

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/no-listing-06-copy/src/main.rs:here}}
```

Nhưng mã này dường như mâu thuẫn với những gì chúng ta vừa học được: chúng ta không có lời gọi đến
`clone`, nhưng `x` vẫn hợp lệ và không được chuyển vào `y`.

Rust có một annotation (chú thích) đặc biệt gọi là `Copy` mà chúng ta có thể đặt trên
các loại dữ liệu được lưu trữ trên stack, như số nguyên (chúng ta sẽ nói thêm về
annotation trong [Chương 10][traits]<!-- ignore -->). Nếu một kiểu dữ liệu áp dụng
`Copy`, các biến sử dụng nó không move mà được copy một cách bình thường,
làm cho chúng vẫn hợp lệ sau khi được gán cho một biến khác.

Rust sẽ không cho phép chúng ta đánh dấu một kiểu bằng `Copy` nếu kiểu đó hoặc 
bất kỳ thành phần nào của nó, đã được đánh dấu `Drop`. Nếu kiểu dữ liệu cần làm một 
điều gì đó đặc biệt khi giá trị vượt quá phạm vi và chúng ta thêm chú thích `Copy` 
cho loại đó, chúng tôi sẽ gặp lỗi biên dịch. Để tìm hiểu về cách thêm chú thích `Copy`
vào kiểu dữ liệu của bạn để thực hiện trait, xem phần [“Derivable Traits”][derivable-traits]<!-- ignore --> 
trong Phụ lục C.

Vậy, những kiểu dữ liệu nào thực hiện đặc điểm `Copy`? Để chắc chắn bạn có thể 
kiểm tra tài liệu của loại dữ liệu đó, nhưng theo nguyên tắc chung, bất kỳ nhóm kiểu dữ liệu 
vô hướng đơn giản nào các giá trị có thể triển khai `Copy` và không có gì yêu cầu phân bổ hoặc là một số
dạng tài nguyên có thể triển khai `Copy`. Đây là một số loại áp dụng `Copy`:

* Tất cả các loại số nguyên, chẳng hạn như `u32`.
* Kiểu Boolean, `bool`, với các giá trị `true` và `false`.
* Tất cả các loại dấu phẩy động, chẳng hạn như `f64`.
* Loại ký tự, `char`.
* Tuple, nếu chúng chỉ chứa các loại cũng áp dụng `Copy`. Ví dụ,
   `(i32, i32)` thực hiện `Copy`, nhưng `(i32, String)` thì không.  

<a id="ownership-and-functions"></a>  
### Ownership và Functions

Cơ chế chuyển một giá trị cho một hàm cũng tương tự như khi gán giá trị cho một biến. 
Truyền một biến cho một hàm sẽ move hoặc copy, giống như phép gán. Liệt kê 4-3 có một ví 
dụ với một số annotation nơi các biến vào và ra khỏi phạm vi.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-03/src/main.rs}}
```

<span class="caption">Liệt kê 4-3: Các hàm với ownership và scope
annotated</span>

Nếu chúng tôi cố sử dụng `s` sau lệnh gọi `takes_ownership`, Rust sẽ đưa ra một
lỗi biên dịch. Những kiểm tra tĩnh này bảo vệ chúng ta khỏi những sai lầm. Thử thêm
code vào `main` để sử dụng `s` và `x` và xem bạn có thể sử dụng chúng ở đâu và ở đâu
các quy tắc ownership ngăn cản bạn làm vậy.

<a id="return-values-and-scope"></a>
### Giá trị trả về và Scope

Các giá trị trả về cũng có thể chuyển ownership. Liệt kê 4-4 trình bày một ví dụ về một
hàm trả về một số giá trị, với các annotation tương tự như trong Liệt kê 4-3.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-04/src/main.rs}}
```

<span class="caption">Listing 4-4: Chuyển ownership của các giá trị trả về</span>

Ownership của một biến luôn tuân theo cùng một khuôn mẫu: việc gán giá trị cho một 
biến khác sẽ move nó. Khi một biến bao gồm dữ liệu trên heap nằm ngoài scope, giá trị 
sẽ bị `drop` trừ khi ownership đã được chuyển sang một biến khác.

Khi điều này hoạt động, việc lấy ownership và sau đó trả lại ownership với các 
hàm sẽ có một chút tẻ nhạt. Điều gì sẽ xảy ra nếu chúng ta muốn để một hàm sử dụng một 
giá trị nhưng không lấy ownership? Thật khó chịu khi bất cứ thứ gì chúng ta truyền đi 
cũng cần phải được trả lại nếu chúng ta muốn sử dụng lại, chưa kể chúng ta còn phải trả
về giá trị của hàm.

Rust cho phép chúng ta trả về nhiều giá trị bằng cách sử dụng tuple, như trong Liệt kê 4-5.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch04-understanding-ownership/listing-04-05/src/main.rs}}
```

<span class="caption">Liệt kê 4-5: Trả về ownership của các tham số</span>

Nhưng quả là có quá nhiều thứ cho một khái niệm vốn khá phổ biến. Thật may mắn cho chúng ta, 
Rust có một tính năng cho phép sử dụng một giá trị mà không cần
chuyển quyền sở hữu, được gọi là *reference* (tham chiếu).

[data-types]: ch03-02-data-types.html#data-types
[ch8]: ch08-02-strings.html
[traits]: ch10-02-traits.html
[derivable-traits]: appendix-03-derivable-traits.html
[method-syntax]: ch05-03-method-syntax.html#method-syntax
[paths-module-tree]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html
[drop]: ../std/ops/trait.Drop.html#tymethod.drop
