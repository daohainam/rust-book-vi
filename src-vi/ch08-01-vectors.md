## Lưu danh sách giá trị bằng Vector

Loại tập hợp đầu tiên chúng ta sẽ xem xét là `Vec<T>`, còn được gọi là *vector*.
Vector cho phép bạn lưu trữ nhiều giá trị trong một cấu trúc dữ liệu đơn lẻ mà nó
đặt tất cả các giá trị nằm cạnh nhau trong bộ nhớ. Vector chỉ có thể lưu trữ các giá trị
cùng loại. Chúng hữu ích khi bạn có một danh sách các hạng mục, chẳng hạn như
những dòng văn bản trong một tập tin hoặc giá cả của các mặt hàng trong giỏ hàng.

### Tạo mới Vector

Để tạo mới một vector rỗng, chúng ta thực thi hàm `Vec::new`, như hiển thị trong mục 8-1

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-01/src/main.rs:here}}
```

<span class="caption">Mục 8-1: Tạo mới vector rỗng để lưu trữ giá trị loại `i32`</span>

Lưu ý rằng chúng ta đã có thêm chú thích kiểu dữ liệu ở đây. Bởi vì nếu không chèn bất kỳ
giá trị nào vào vector này, Rust sẽ không biết loại phần tử nào chúng ta dự định lưu vào.
Đây là điểm quan trọng. Vector được triển khai bằng cách sử dụng kiểu generic;
chúng tôi sẽ trình bày cách sử dụng generic với kiểu dữ liệu riêng của bạn trong Chương 10. 
Hiện tại, chỉ cần biết rằng kiểu `Vec<T>` được cung cấp bởi thư viện chuẩn, nó có thể chứa 
bất kỳ kiểu dữ liệu nào cũng được. Khi tạo một vector để chứa một kiểu dữ liệu cụ thể 
nào đó, chúng ta có thể chỉ định chúng bên trong dấu ngoặc nhọn (`<>`). 
Trong mục 8-1, chúng ta đã khai báo với Rust rằng `Vec<T>` trong `v` sẽ chứa các phần tử của kiểu `i32`.

Thông thường hơn, khi bạn tạo một `Vec<T>` với các giá trị khởi tạo, Rust sẽ suy ra
kiểu dữ liệu của giá trị bạn muốn lưu trữ vào, vì vậy hiếm khi bạn phải khai báo chú thích 
kiểu dữ liệu thế này. Rust cung cấp macro `vec!` một cách tiện dụng, nó sẽ tạo ra một
vector mới chứa các giá trị bạn cung cấp cho nó. Mục 8-2 tạo ra một
`Vec<i32>` chứa các giá trị `1`, `2` và `3`. Kiểu số nguyên là `i32`
bởi vì đó là kiểu số nguyên mặc định như chúng ta đã thảo luận trong [“Kiểu dữ liệu”][data-types]<!-- ignore --> của Chương 3.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-02/src/main.rs:here}}
```

<span class="caption">Mục 8-2: Tạo mới vector có chứa dữ liệu</span>

Vì chúng ta đã khởi tạo các giá trị ban đầu là kiểu dữ liệu `i32`, nên Rust có thể suy ra rằng kiểu dữ liệu của `v` là `Vec<i32>`, nên việc chú thích kiểu dữ liệu là không cần thiết nữa. Tiếp theo, chúng ta sẽ xem xét cách để sửa đổi một vector.

### Cập nhật Vector

Để tạo một vector và sau đó thêm các phần tử vào nó, chúng ta có thể sử dụng phương thức `push`,
như được hiển thị trong mục 8-3.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-03/src/main.rs:here}}
```

<span class="caption">Mục 8-3: Sử dụng phương thức `push` để thêm giá trị vào vector</span>

Như với bất kỳ biến nào, nếu chúng ta muốn có thể thay đổi giá trị của nó, chúng ta cần
làm cho nó có thể thay đổi bằng cách sử dụng từ khóa `mut`, như đã thảo luận trong Chương 3. 
Các con số chúng tôi đặt bên trong tất cả đều thuộc loại `i32` và Rust suy ra điều này 
từ dữ liệu, vì vậy chúng ta không cần chú thích `Vec<i32>`.

### Truy xuất các phần tử của Vector

Có hai cách để tham chiếu một giá trị được lưu trữ trong một vector: thông qua chỉ mục (index) hoặc
sử dụng phương thức `get`. Trong các ví dụ sau, chúng tôi đã thêm chú thích kiểu của những
giá trị được trả về từ các hàm này để làm rõ nghĩa hơn.

Mục 8-4 thể hiện cả hai phương pháp truy cập giá trị trong một vector, với cú pháp index và phương thức `get`.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-04/src/main.rs:here}}
```

<span class="caption">Mục 8-4: Sử dụng cú pháp index hoặc phương thức `get` để truy cập một phần tử trong vector</span>

Lưu ý một vài chi tiết ở đây. Chúng ta sử dụng giá trị index là `2` để lấy phần tử thứ ba
bởi vì các phần tử của vector được lập theo cách đánh số chỉ mục (index), bắt đầu từ số 0. 
Sử dụng `&` và `[]` sẽ cung cấp cho chúng ta một tham chiếu đến phần tử tại giá trị 
index chỉ định. Khi chúng ta sử dụng phương thức `get` với đối số index chỉ định, 
chúng ta nhận được một `Option<&T>` mà chúng ta có thể sử dụng với `match`.

Lý do Rust cung cấp hai cách này để tham chiếu đến một phần tử là vì bạn có thể
lựa chọn cách thức chương trình hoạt động khi bạn cố sử dụng một giá trị index bên ngoài
phạm vi của các phần tử hiện có. Ví dụ, hãy xem điều gì sẽ xảy ra khi chúng ta có
một vector gồm 5 phần tử và sau đó chúng ta cố ý truy cập một phần tử ở index 100
với mỗi cách, như được hiển thị trong mục 8-5.

```rust,should_panic,panics
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-05/src/main.rs:here}}
```

<span class="caption">Mục 8-5: Cố gắng truy cập phần tử tại index 100 trong vector chỉ có 5 phần tử</span>

Khi chúng ta chạy đoạn mã này, phương thức sử dụng index `[]` đầu tiên sẽ khiến chương trình bị lỗi
bởi vì nó tham chiếu đến một phần tử không tồn tại. Phương pháp này được sử dụng tốt nhất khi bạn
muốn chương trình của bạn gặp sự cố nếu có nỗ lực truy cập vào một phần tử sau phần tử cuối vector.

Khi phương thức `get` được truyền vào một chỉ mục index nằm ngoài vector, nó sẽ trả về `None` 
mà không bị lỗi. Bạn sẽ sử dụng phương pháp này nếu việc truy cập một phần tử nằm ngoài phạm vi của vector có thể thỉnh thoảng xảy ra trong các tình huống bình thường. Khi đó, bạn sẽ có logic để xử lý cả hai trường hợp `Some(&element)` hoặc `None`, như đã thảo luận trong Chương 6. Ví dụ, chỉ mục index có thể phát sinh từ một người nhập liệu chỉ số đó. Nếu họ vô tình nhập một số quá lớn, thì chương trình nhận giá trị trả về là `None`, khi đó bạn có thể cho người dùng biết có bao nhiêu phần tử trong vector hiện tại và cho họ một cơ hội khác để nhập lại giá trị hợp lệ. Điều đó sẽ thân thiện với người dùng hơn, hơn là làm hỏng chương trình do lỗi đánh máy!

Khi chương trình có tham chiếu hợp lệ, trình kiểm tra mượn tham chiếu (borrow) thực thi 
các quy tắc về quyền sở hữu (ownership) và những quy tắc về mượn tham chiếu (được đề cập 
trong Chương 4) để đảm bảo tham chiếu này và mọi tham chiếu khác đến nội dung của vector 
vẫn hợp lệ. Nhắc lại quy tắc về trạng thái, bạn không thể có cùng trạng thái: tham chiếu 
có thể thay đổi (mutable) và tham chiếu bất biến (immutable) trong cùng một phạm vi. 
Quy tắc đó áp dụng trong mục 8-6, trong đó chúng ta giữ một tham chiếu immutable đến 
phần tử đầu tiên trong một vector và cố gắng thêm một phần tử vào cuối. Chương trình 
này cũng sẽ không chạy được nếu chúng ta cố tham chiếu đến phần tử này sau đó trong hàm sau:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-06/src/main.rs:here}}
```

<span class="caption">Mục 8-6: Cố gắng thêm một phần tử vào vector trong khi đang 
giữ tham chiếu đến một phần tử </span>

Biên dịch đoạn mã này sẽ bị lỗi:

```console
{{#include ../listings/ch08-common-collections/listing-08-06/output.txt}}
```

Đoạn mã trong mục 8-6 có thể trông giống như: tại sao một tham chiếu đến phần tử 
đầu tiên phải quan tâm đến những thay đổi ở cuối vector? Lỗi này là do cách hoạt 
động của vector: vì vector đặt các giá trị cạnh nhau trong bộ nhớ, việc thêm một 
phần tử mới vào cuối vector có thể yêu cầu cấp phát bộ nhớ mới, nếu không có đủ 
chỗ để đặt tất cả các phần tử bên cạnh nhau tại nơi mà vector hiện đang được lưu 
trữ, thì nó sẽ sao chép tất cả các phần tử cũ vào không gian mới trong bộ nhớ. 
Trong trường hợp đó, tham chiếu đến phần tử đầu tiên sẽ trỏ đến bộ nhớ đã bị giải 
phóng. Các quy tắc mượn tham chiếu sẽ ngăn cản các chương trình kết thúc trong tình huống đó.

> Lưu ý: Để biết thêm về chi tiết triển khai của kiểu `Vec<T>`, hãy tham khảo [“The
> Rustonomicon”][nomicon].

### Duyệt qua những giá trị trong Vector

Để truy cập lần lượt từng phần tử trong một vector, chúng ta sẽ lặp qua tất cả các 
phần tử thay vì sử dụng các chỉ mục để truy cập từng phần tử một. Mục 8-7 thể hiện 
cách sử dụng vòng lặp `for` để nhận các tham chiếu immutable đến từng phần tử 
trong một vector có kiểu giá trị `i32` và in chúng ra.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-07/src/main.rs:here}}
```

<span class="caption">Mục 8-7: In từng phần tử trong một vector bằng cách lặp qua các 
phần tử sử dụng một vòng lặp `for`</span>

Chúng ta cũng có thể lặp qua các tham chiếu mutable đến từng phần tử trong một vector 
mutable để thực hiện thay đổi đối với tất cả các phần tử. Vòng lặp `for` trong mục 8-8 
sẽ thêm `50` vào mỗi phần tử.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-08/src/main.rs:here}}
```

<span class="caption">Mục 8-8: Lặp qua các tham chiếu mutable đến từng phần tử trong 
một vector</span>

Để thay đổi giá trị mà tham chiếu mutable đang tham chiếu đến, chúng ta phải sử dụng 
toán tử tham chiếu giá trị `*` để đi đến giá trị mà `i` đang trở tới trước khi chúng 
ta có thể sử dụng toán tử `+=`. Chúng ta sẽ nói thêm về toán tử tham chiếu giá trị 
trong phần [“Theo con trỏ đến giá trị với toán tử tham chiếu deref”][deref]<!-- ignore --> 
của Chương 15.

Duyệt qua một vector, cho dù là loại immutable hoặc mutable, đều an toàn với các quy 
tắc của trình kiểm tra mượn tham chiếu. Nếu chúng tôi cố thử chèn hoặc xóa các mục 
trong phần thân của vòng lặp `for` ở mục 8-7 và mục 8-8, chúng tôi sẽ gặp lỗi trình 
biên dịch tương tự như lỗi mà chúng tôi gặp phải với đoạn mã trong mục 8-6. Tham chiếu 
đến vector mà vòng lặp `for` đang nắm giữ sẽ ngăn cản việc sửa đổi đồng thời toàn bộ vector.

### Sử dụng Enum để lưu nhiều kiểu dữ liệu

Các vector chỉ có thể lưu trữ các giá trị có cùng kiểu dữ liệu. Điều này có thể gây bất tiện; 
chắc chắn có những trường hợp sử dụng cần lưu trữ một danh sách các hạng mục có kiểu dữ liệu 
khác loại nhau. May mắn thay, các biến thể (variants) của một enum được định nghĩa dưới cùng 
một kiểu enum, vì vậy khi chúng ta cần một cái nào đó để đại diện cho các phần tử có kiểu dữ 
liệu khác nhau, chúng ta có thể định nghĩa và sử dụng một enum!

Ví dụ: chúng ta muốn nhận các giá trị từ một dòng trong bảng tính, trong đó một số cột của 
dòng đó chứa số nguyên, số dấu chấm động và chuỗi. Chúng ta có thể định nghĩa một enum mà 
các biến thể của nó sẽ chứa các kiểu giá trị khác nhau và tất cả các biến thể enum sẽ được 
coi là cùng một kiểu: kiểu của enum. Sau đó, chúng ta có thể tạo một vector để chứa enum 
đó và cuối cùng, chứa các kiểu dữ liệu khác nhau. Chúng tôi đã biểu diễn điều này trong mục 8-9.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-09/src/main.rs:here}}
```

<span class="caption">Mục 8-9: Định nghĩa một `enum` lưu giá trị của 
những kiểu dữ liệu khác nhau trong một vector</span>

Rust cần biết những kiểu dữ liệu nào sẽ có trong vector tại thời điểm 
biên dịch để nó biết chính xác sẽ cần bao nhiêu bộ nhớ trên heap để lưu 
trữ mỗi phần tử. Chúng ta cũng phải rõ ràng về những kiểu dữ liệu được phép 
trong vector này. Nếu Rust cho phép một vector chứa bất kỳ kiểu dữ liệu nào, 
sẽ có khả năng một hoặc nhiều kiểu dữ liệu sẽ gây ra lỗi với các phép toán 
được thực hiện trên các phần tử của vector. Sử dụng một enum cộng với 
một biểu thức `match` có nghĩa là Rust sẽ đảm bảo tại thời điểm biên dịch rằng 
mọi trường hợp có khả năng xảy ra đều được xử lý, như đã thảo luận trong Chương 6.

Nếu bạn không biết đầy đủ tập hợp các loại dữ liệu mà một chương trình 
sẽ nhận được tại thời điểm thực thi để lưu trữ trong một vector, thì kỹ thuật enum 
sẽ không hoạt động tốt. Thay vào đó, bạn có thể sử dụng một đối tượng đặc tả (trait), 
mà chúng ta sẽ đề cập trong Chương 17.

Bây giờ chúng ta đã thảo luận về một số cách phổ biến nhất để sử dụng vector, hãy 
hớ xem lại [tài liệu API][vec-api]<!-- ignore --> để biết thêm nhiều phương thức 
hữu ích được định nghĩa trong `Vec<T>` bởi thư viện chuẩn. Ví dụ, ngoài `push`, 
một phương thức `pop` sẽ loại bỏ và trả về phần tử cuối cùng.

### Loại bỏ một vector cũng sẽ giải phóng luôn các phần tử của nó

Giống như bất kỳ `struct` nào khác, một vector được giải phóng khi nó ra khỏi phạm vi, như
được chú thích trong mục 8-10.

```rust
{{#rustdoc_include ../listings/ch08-common-collections/listing-08-10/src/main.rs:here}}
```

<span class="caption">Mục 8-10: Chỉ ra nơi mà vector và các phần tử của nó bị giải phóng</span>

Khi vector bị loại bỏ, tất cả nội dung của nó cũng bị loại bỏ, có nghĩa là các số nguyên mà nó giữ sẽ bị xóa sạch. 
Trình kiểm tra mượn tham chiếu sẽ đảm bảo rằng bất kỳ tham chiếu nào đến nội dung của vector chỉ 
được sử dụng khi chính vector đó còn hợp lệ.

Hãy chuyển sang loại tập hợp tiếp theo: `String`!

[data-types]: ch03-02-data-types.html#data-types
[nomicon]: ../nomicon/vec/vec.html
[vec-api]: ../std/vec/struct.Vec.html
[deref]: ch15-02-deref.html#following-the-pointer-to-the-value-with-the-dereference-operator
