## Các kiểu dữ liệu Generic

Chúng ta sử dụng generics để tạo định nghĩa cho các mục như chữ ký hàm hoặc 
các struct, mà chúng ta sau đó có thể sử dụng với nhiều loại dữ liệu cụ thể 
khác nhau. Hãy trước hết xem cách định nghĩa hàm, struct, enum và các phương 
thức bằng generics. Sau đó, chúng ta sẽ thảo luận về cách generics ảnh hưởng 
đến hiệu suất của code.

### Trong Định nghĩa Hàm

Khi định nghĩa một hàm sử dụng generics, chúng ta đặt generics trong chữ ký 
của hàm nơi chúng ta thường chỉ định loại dữ liệu của các tham số và giá trị 
trả về. Việc này làm cho code của chúng ta linh hoạt hơn và cung cấp thêm chức 
năng cho người gọi hàm của chúng ta trong khi ngăn chặn việc trùng lặp code.

Tiếp tục với hàm `largest` của chúng ta, Listing 10-4 hiển thị hai hàm cả hai 
đều tìm giá trị lớn nhất trong một slice. Sau đó, chúng ta sẽ kết hợp chúng 
thành một hàm duy nhất sử dụng generics.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-04/src/main.rs:here}}
```

<span class="caption">Listing 10-4: Two functions that differ only in their
names and the types in their signatures</span>

Hàm `largest_i32` là hàm chúng ta đã trích xuất ở Listing 10-3 để tìm giá trị 
lớn nhất của `i32` trong một slice. Hàm `largest_char` tìm giá trị lớn nhất của `char` 
trong một slice. Cả hai hàm có cùng code nguồn, vì vậy hãy loại bỏ sự trùng lặp 
bằng cách giới thiệu một tham số kiểu generic trong một hàm duy nhất.

Để tham số hóa các loại trong một hàm mới, chúng ta cần đặt tên tham số kiểu, 
giống như chúng ta làm với các tham số giá trị của một hàm. Bạn có thể sử dụng 
bất kỳ định danh nào làm tên tham số kiểu. Nhưng chúng ta sẽ sử dụng `T` vì theo 
quy ước, tên tham số kiểu trong Rust ngắn gọn, thường chỉ là một chữ cái, và 
quy ước đặt tên kiểu của Rust là CamelCase. Rút gọn từ “type,” `T` là lựa chọn 
mặc định của hầu hết các lập trình viên Rust.

Khi chúng ta sử dụng một tham số trong thân của hàm, chúng ta phải khai báo tên 
tham số trong chữ ký để trình biên dịch biết nghĩa của tên đó là gì. Tương tự, khi 
chúng ta sử dụng tên tham số kiểu trong chữ ký hàm, chúng ta phải khai báo tên 
tham số kiểu trước khi sử dụng nó. Để định nghĩa hàm generic `largest`, đặt khai 
báo tên kiểu bên trong dấu ngoặc nhọn, `<>`, giữa tên hàm và danh sách tham số, như sau:

```rust,ignore
fn largest<T>(list: &[T]) -> &T {
```

Chúng ta đọc định nghĩa này như sau: hàm `largest` là generic qua một loại T 
nào đó. Hàm này có một tham số có tên là `list`, là một slice của các giá trị 
kiểu `T`. Hàm `largest` sẽ trả về một tham chiếu đến một giá trị cùng kiểu `T`.

Listing 10-5 cho thấy định nghĩa hàm `largest` kết hợp sử dụng kiểu dữ liệu 
generic trong chữ ký của nó. Listing cũng cho thấy cách chúng ta có thể gọi 
hàm với một slice của giá trị `i32` hoặc giá trị `char`. Lưu ý rằng đoạn code này 
chưa thể biên dịch được, nhưng chúng ta sẽ sửa nó sau trong chương này.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-05/src/main.rs}}
```

<span class="caption">Listing 10-5: Hàm `largest` sử dụng các tham số kiểu 
generic; code này hiện chưa thể biên dịch được</span>

Nếu chúng ta biên dịch đoạn code này ngay bây giờ, chúng ta sẽ nhận được lỗi này:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-05/output.txt}}
```

Phần trợ giúp chỉ đến `std::cmp::PartialOrd`, đó là một *trait*, và chúng ta 
sẽ nói về các traits trong phần tiếp theo. Hiện tại, hãy biết rằng lỗi này 
nói rằng thân của `largest` không hoạt động với tất cả các loại `T` có thể có. 
Vì chúng ta muốn so sánh giá trị của kiểu `T` trong thân hàm, chúng ta chỉ có 
thể sử dụng các loại mà giá trị của chúng có thể được so sánh. Để kích hoạt so 
sánh, thư viện chuẩn có trait `std::cmp::PartialOrd` mà bạn có thể triển khai cho 
các loại (xem Phụ lục C để biết thêm về trait này). Bằng cách tuân theo gợi ý 
của văn bản trợ giúp, chúng ta giới hạn các loại hợp lệ cho `T` chỉ đến những 
loại triển khai PartialOrd, và ví dụ này sẽ biên dịch được, vì thư viện chuẩn 
triển khai `PartialOrd` cho cả `i32` và `char`.

### Trong Các Định Nghĩa Struct

Chúng ta cũng có thể định nghĩa các structs để sử dụng một tham số kiểu generic 
trong một hoặc nhiều trường bằng cú pháp `<>`. Listing 10-6 định nghĩa một 
struct `Point<T>` để chứa các giá trị tọa độ `x` và `y` của bất kỳ kiểu nào.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-06/src/main.rs}}
```

<span class="caption">Listing 10-6: Một struct `Point<T>` chứa các giá trị x 
và y của kiểu `T`</span>

Cú pháp sử dụng generics trong định nghĩa struct tương tự như cú pháp 
được sử dụng trong định nghĩa hàm. Trước tiên, chúng ta khai báo tên 
của tham số kiểu bên trong dấu ngoặc nhọn ngay sau tên của struct. Sau 
đó, chúng ta sử dụng kiểu generic trong định nghĩa struct ở những nơi 
chúng ta thông thường sẽ chỉ định kiểu dữ liệu cụ thể.

Lưu ý rằng vì chúng ta đã sử dụng chỉ một kiểu generic để định nghĩa `Point<T>`, 
định nghĩa này nói rằng struct `Point<T>` là generic trên một loại `T`, và các 
trường `x` và `y` đều là cùng một kiểu đó, bất kể kiểu đó là gì. Nếu chúng ta 
tạo một thể hiện của `Point<T>` có giá trị của các kiểu khác nhau, như trong 
Listing 10-7, code nguồn của chúng ta sẽ không biên dịch được.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-07/src/main.rs}}
```

<span class="caption">Listing 10-7: Các trường `x` and `y` phải có cùng kiểu vì chúng 
sử dụng cùng kiểu generic T</span>

Trong ví dụ này, khi chúng ta gán giá trị số nguyên 5 cho `x`, chúng ta thông 
báo cho trình biên dịch biết rằng kiểu generic T sẽ là một số nguyên cho 
instance này của `Point<T>`. Sau đó, khi chúng ta chỉ định giá trị 4.0 cho 
`y`, mà chúng ta đã định nghĩa có cùng kiểu với x, chúng ta sẽ nhận được một 
lỗi không khớp kiểu như sau:

```console
{{#include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-07/output.txt}}
```

To define a `Point` struct where `x` and `y` are both generics but could have
different types, we can use multiple generic type parameters. For example, in
Listing 10-8, we change the definition of `Point` to be generic over types `T`
and `U` where `x` is of type `T` and `y` is of type `U`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-08/src/main.rs}}
```

<span class="caption">Listing 10-8: Một struct `Point<T, U>` generic trên hai 
kiểu để `x` và `y` có thể là giá trị của các kiểu khác nhau</span>

Bây giờ tất cả các thể hiện của `Point` được hiển thị đều được chấp nhận! 
Bạn có thể sử dụng nhiều tham số kiểu generic trong định nghĩa càng nhiều 
càng tốt, nhưng sử dụng quá nhiều có thể làm cho code nguồn của bạn khó đọc. 
Nếu bạn phát hiện bạn cần nhiều kiểu generic trong code nguồn của mình, 
điều này có thể là dấu hiệu cho thấy code nguồn của bạn cần được tổ chức 
lại thành các phần nhỏ hơn.

### Trong Định Nghĩa Enum

Như chúng ta đã làm với các struct, chúng ta có thể định nghĩa các enum để giữ 
các kiểu dữ liệu chung trong các biến khác nhau của chúng. Hãy xem xét 
lại enum `Option<T>` mà thư viện chuẩn cung cấp, mà chúng ta đã sử dụng trong Chương 6:

```rust
enum Option<T> {
    Some(T),
    None,
}
```

Bây giờ, định nghĩa này sẽ rõ hơn đối với bạn. Như bạn có thể thấy, enum `Option<T>` 
là chung cho kiểu T và có hai biến thể: Some, giữ một giá trị của kiểu T, và 
một biến thể None không giữ bất kỳ giá trị nào. Bằng cách sử dụng enum `Option<T>`, 
chúng ta có thể diễn đạt khái niệm trừu tượng của giá trị tùy chọn, và do enum 
Option<T> là chung, chúng ta có thể sử dụng trừu tượng này không phụ thuộc vào 
kiểu giá trị tùy chọn là gì.

Enum cũng có thể sử dụng nhiều kiểu chung. Định nghĩa enum Result mà chúng ta sử 
dụng trong Chương 9 là một ví dụ:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Enum Result là chung cho hai kiểu T và E, và có hai biến thể: Ok, giữ một giá 
trị của kiểu T, và Err, giữ một giá trị của kiểu E. Định nghĩa này làm cho 
việc sử dụng enum Result thuận lợi ở bất kỳ nơi nào chúng ta có một hoạt động có 
thể thành công (trả về một giá trị của một kiểu T) hoặc thất bại (trả về một lỗi 
của một kiểu E). Trong thực tế, đây là điều chúng ta đã sử dụng để mở một tập tin 
trong Listing 9-3, trong đó T được điền bằng kiểu std::fs::File khi tập tin được 
mở thành công và E được điền bằng kiểu std::io::Error khi có vấn đề khi mở tập tin.

Khi bạn nhận diện các tình huống trong code của bạn với nhiều định nghĩa struct hoặc 
enum khác nhau chỉ khác nhau ở các kiểu giá trị chúng giữ, bạn có thể tránh sự 
trùng lặp bằng cách sử dụng các kiểu chung.

### Trong Định Nghĩa Phương Thức

Chúng ta có thể triển khai các phương thức trên các struct và enums (như chúng 
ta đã làm trong Chương 5) và sử dụng các kiểu generic trong định nghĩa của chúng 
cũng. Listing 10-9 hiển thị struct `Point<T>` mà chúng ta đã định nghĩa trong 
Listing 10-6 với một phương thức có tên là `x`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-09/src/main.rs}}
```

<span clas	s="caption">Listing 10-9: Triển khai một phương thức có tên `x` trên cấu trúc 
`Point<T>` sẽ trả về một tham chiếu đến trường `x` kiểu `T`.</span>

Ở đây, chúng ta đã định nghĩa một phương thức có tên x trên `Point<T>` trả về một 
tham chiếu đến dữ liệu trong trường `x`.

Lưu ý rằng chúng ta phải khai báo `T` ngay sau `impl` để chúng ta có thể sử dụng `T` 
để chỉ định rằng chúng ta đang triển khai các phương thức trên kiểu `Point<T>`. Bằng 
cách khai báo `T` làm một loại generic sau impl, Rust có thể xác định rằng kiểu 
trong ngoặc nhọn ở `Point` là một kiểu generic thay vì một kiểu cụ thể. Chúng ta 
có thể chọn một tên khác cho tham số generic này so với tham số generic được 
khai báo trong định nghĩa struct, nhưng việc sử dụng cùng một tên là phổ biến. 
Các phương thức được viết trong một `impl` khai báo tham số generic sẽ được định 
nghĩa cho bất kỳ thể hiện nào của kiểu đó, không phụ thuộc vào kiểu cụ thể nào 
thay thế cho kiểu generic.

Chúng ta cũng có thể chỉ định ràng buộc trên các kiểu generic khi định nghĩa 
các phương thức trên kiểu. Ví dụ, chúng ta có thể triển khai các phương thức 
chỉ trên các thể hiện `Point<f32>` thay vì trên các thể hiện `Point<T>` với bất kỳ 
kiểu generic nào. Trong Listing 10-10, chúng ta sử dụng kiểu cụ thể `f32`, có 
nghĩa là chúng ta không khai báo bất kỳ kiểu nào sau impl.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-10/src/main.rs:here}}
```

<span class="caption">Listing 10-10: Một khối `impl` chỉ áp dụng cho một struct 
với một kiểu cụ thể cho tham số kiểu generic `T`.</span>

Mã này có nghĩa là kiểu `Point<f32>` sẽ có một phương thức `distance_from_origin`; 
các phiên bản khác của `Point<T>` nơi `T` không phải là kiểu f32 sẽ không có phương thức 
này được định nghĩa. Phương thức này đo lường khoảng cách từ điểm của chúng ta 
đến điểm tại tọa độ (0.0, 0.0) và sử dụng các phép toán toán học chỉ có sẵn cho 
các kiểu số thực.

Các tham số kiểu generic trong định nghĩa struct không luôn giống nhau so với các 
tham số kiểu bạn sử dụng trong các chữ ký phương thức của cùng struct đó. Mã ở 
Listing 10-11 sử dụng các kiểu generic `X1` và `Y1` cho struct `Point` và `X2` `Y2` cho 
chữ ký phương thức mixup để làm cho ví dụ trở nên rõ ràng hơn. Phương thức này 
tạo một thể hiện mới của `Point` với giá trị x từ `self` `Point` (kiểu `X1`) và giá trị 
`y` từ `Point` được chuyển vào (kiểu `Y2`).

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch10-generic-types-traits-and-lifetimes/listing-10-11/src/main.rs}}
```

<span class="caption">Listing 10-11: Một phương thức sử dụng các kiểu generic 
khác so với định nghĩa struct của nó.</span>

Trong `main`, chúng ta đã định nghĩa một `Point` có kiểu `i32` cho `x` (giá trị là `5`) 
và kiểu `f64` cho `y` (giá trị là `10.4`). Biến `p2` là một struct `Point` có một chuỗi 
("Hello") cho `x` và một ký tự (c) cho `y`. Gọi mixup trên p1 với đối số là `p2` 
cho chúng ta `p3`, nơi có kiểu `i32` cho `x`, vì `x` đến từ `p1`. Biến `p3` sẽ có kiểu 
char cho `y`, vì `y` đến từ `p2`. Cuộc gọi macro println! sẽ in ra `p3.x = 5, p3.y = c`.

Mục đích của ví dụ này là để minh họa một tình huống trong đó một số tham 
số generic được khai báo với `impl` và một số được khai báo với định nghĩa 
phương thức. Ở đây, các tham số generic `X1` và `Y1` được khai báo sau impl vì 
chúng đi kèm với định nghĩa `struct`. Các tham số generic `X2` và `Y2` được 
khai báo sau `fn mixup`, vì chúng chỉ liên quan đến phương thức.

### Hiệu năng của code sử dụng Generic

Bạn có thể tự hỏi liệu có chi phí thời gian chạy khi sử dụng các tham số kiểu 
generic hay không. Tin tốt là việc sử dụng các kiểu generic sẽ không làm 
chương trình của bạn chạy chậm hơn so với việc sử dụng các kiểu cụ thể.

Rust đạt được điều này bằng cách thực hiện *monomorphization* của mã nguồn 
sử dụng generics trong quá trình biên dịch. *Monomorphization* là quá trình 
chuyển đổi mã nguồn generic thành mã nguồn cụ thể bằng cách điền vào các kiểu cụ 
thể được sử dụng khi biên dịch. Trong quá trình này, trình biên dịch thực hiện 
theo chiều ngược lại so với các bước chúng ta đã sử dụng để tạo hàm generic 
trong Listing 10-5: trình biên dịch xem xét tất cả các điểm mà mã nguồn generic 
được gọi và tạo mã nguồn cho các kiểu cụ thể mà mã nguồn generic được gọi với.

Hãy xem cách điều này hoạt động bằng cách sử dụng generic `Option<T>` enum từ 
thư viện chuẩn:

```rust
let integer = Some(5);
let float = Some(5.0);
```

Khi Rust biên dịch mã nguồn này, nó thực hiện monomorphization. 
Trong quá trình đó, trình biên dịch đọc các giá trị đã được sử dụng 
trong các trường hợp của `Option<T>` và xác định hai loại `Option<T>`: một 
là `i32` và một là `f64`. Do đó, nó mở rộng định nghĩa generic của `Option<T>` 
thành hai định nghĩa được tối ưu hóa cho `i32` và `f64`, thay thế định nghĩa 
generic bằng những định nghĩa cụ thể này.

Phiên bản đã được tối ưu hóa bằng monomorphization của mã nguồn trông 
giống như sau (trình biên dịch sử dụng tên khác với những gì chúng ta sử 
dụng ở đây cho mục đích minh họa):

<span class="filename">Filename: src/main.rs</span>

```rust
enum Option_i32 {
    Some(i32),
    None,
}

enum Option_f64 {
    Some(f64),
    None,
}

fn main() {
    let integer = Option_i32::Some(5);
    let float = Option_f64::Some(5.0);
}
```

Cấu trúc tổng quát Option<T> được thay thế bằng các định nghĩa cụ thể được 
tạo ra bởi trình biên dịch. Vì Rust biên dịch mã tổng quát thành mã xác định 
kiểu trong từng trường hợp cụ thể, nên chúng ta không phải chịu chi phí 
thực thi tại thời gian chạy khi sử dụng generics. Khi mã được thực thi, 
nó hoạt động giống hệt như thể chúng ta đã sao chép từng định nghĩa thủ công. 
Quá trình đơn hình hóa (monomorphization) khiến generics của Rust cực kỳ 
hiệu quả khi chạy.