## Paths for Referring to an Item in the Module Tree

Để chỉ cho Rust biết nơi tìm thấy một item trong cây module, chúng ta sử dụng
một đường dẫn (path) tương tự như chúng ta sử dụng một đường dẫn trong một hệ
thống tập tin. Để gọi một hàm, chúng ta cần biết đường dẫn của nó.

Một đường dẫn có thể có hai dạng:

* *Đường dẫn tuyệt đối (absolute path)* là đường dẫn đầy đủ bắt đầu từ một
crate root; đối với code từ một crate bên ngoài, đường dẫn tuyệt đối bắt đầu với
tên crate, và đối với code từ crate hiện tại, nó bắt đầu với từ khoá `crate`.
* *Đường dẫn tương đối (relative path)* bắt đầu từ module hiện tại và sử dụng
`self`, `super`, hoặc một định danh (identifier) trong module hiện tại.

Cả đường dẫn tuyệt đối và tương đối đều được theo sau bởi một hoặc nhiều định
danh (identifier) được phân tách bởi hai dấu hai chấm (`::`).

Trở lại Listing 7-1, giả sử chúng ta muốn gọi hàm `add_to_waitlist`. Điều này
tương đương với câu hỏi: đường dẫn của hàm `add_to_waitlist` là gì? Listing 7-3
chứa Listing 7-1 với một số module và hàm bị xóa. Chúng ta sẽ hiển thị hai cách
gọi hàm `add_to_waitlist` từ một hàm mới `eat_at_restaurant` được định nghĩa
trong crate root. Hàm `eat_at_restaurant` là một phần của API công khai của
library crate của chúng ta, vì vậy chúng ta đánh dấu nó với từ khoá `pub`.
Trong phần [“Exposing Paths with the `pub` Keyword”][pub]<!-- ignore -->, chúng
ta sẽ đi sâu vào chi tiết hơn về `pub`. Lưu ý rằng ví dụ này tạm thời sẽ không
được biên dịch; chúng ta sẽ giải thích tại trong ít phút nữa.

<span class="filename">Filename: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-03/src/lib.rs:here}}
```

<span class="caption">Listing 7-3: Gọi hàm `add_to_waitlist` sử dụng đường dẫn
tuyệt đối và tương đối</span>

Lần đầu tiên chúng ta gọi hàm `add_to_waitlist` trong `eat_at_restaurant`, chúng
ta sử dụng đường dẫn tuyệt đối. Hàm `add_to_waitlist` được định nghĩa trong cùng
một crate với `eat_at_restaurant`, điều này có nghĩa là chúng ta có thể sử dụng
từ khoá `crate` để bắt đầu một đường dẫn tuyệt đối. Sau đó, chúng ta sẽ đi qua
từng tên của các module cho đến khi chúng ta bắt gặp `add_to_waitlist`.
Bạn có thể tưởng tượng một hệ thống tệp tin với cùng cấu trúc: chúng ta sẽ chỉ
định đường dẫn `/front_of_house/hosting/add_to_waitlist` để chạy chương trình
`add_to_waitlist`; sử dụng từ khoá `crate` để bắt đầu từ crate root giống như
sử dụng `/` để bắt đầu từ thư mục gốc của hệ thống tệp tin trong shell của bạn.

Lần thứ hai chúng ta gọi `add_to_waitlist` trong `eat_at_restaurant`, chúng ta
sử dụng đường dẫn tương đối. Đường dẫn bắt đầu với `front_of_house`, tên của
module được định nghĩa cùng cấp với module tree của `eat_at_restaurant`. Ở đây
đường dẫn tương đương với hệ thống tệp tin sẽ là sử dụng đường dẫn
`front_of_house/hosting/add_to_waitlist`. Bắt đầu với tên của module có nghĩa
rằng đường dẫn là tương đối.

Việc lựa chọn sử dụng đường dẫn tương đối hay tuyệt đối là một quyết định bạn
sẽ đưa ra dựa trên dự án, và phụ thuộc vào việc bạn có thể di chuyển
định nghĩa code của item riêng biệt hay cùng với code sử dụng item. Ví dụ, nếu
chúng ta di chuyển module `front_of_house` và hàm `eat_at_restaurant` vào một
module có tên `customer_experience`, chúng ta sẽ cần cập nhật đường dẫn tuyệt
đối đến `add_to_waitlist`, nhưng đường dẫn tương đối vẫn là hợp lệ. Tuy nhiên,
nếu chúng ta di chuyển hàm `eat_at_restaurant` riêng biệt vào một module có tên
`dining`, đường dẫn tuyệt đối đến `add_to_waitlist` vẫn giữ nguyên, nhưng đường
dẫn tương đối sẽ cần được cập nhật. Nhìn chung, chúng tôi thích dùng đường dẫn
tuyệt đối vì nó có thể  giúp chúng tôi di chuyển code định nghĩa và code gọi
item độc lập với nhau.

Hãy thử biên dịch Listing 7-3 và tìm hiểu tại sao nó vẫn chưa biên dịch được!
Lỗi mà chúng ta nhận được được hiển thị trong Listing 7-4.

```console
{{#include ../listings/ch07-managing-growing-projects/listing-07-03/output.txt}}
```

<span class="caption">Listing 7-4: Lỗi biên dịch từ việc biên dịch code trong
Listing 7-3</span>

Các thông báo lỗi nói rằng module `hosting` là riêng tư. Nói cách khác, chúng
ta có đúng đường dẫn cho module `hosting` và hàm `add_to_waitlist`, nhưng Rust
không cho phép chúng ta sử dụng chúng vì nó không có quyền truy cập vào các
phần riêng tư. Trong Rust, tất cả các item (hàm, phương thức, struct, enum,
module, và hằng số) là private đối với module cha theo mặc định. Nếu bạn muốn
tạo một item như một hàm hoặc struct private, bạn cần đặt nó trong một module.

Các item trong module cha không thể sử dụng các item private bên trong module
con, nhưng các item trong module con có thể sử dụng các item trong các module
cha của nó. Điều này là vì module đã gói các chi tiết code của nó và ẩn chúng đi
nhưng module con có thể thấy được ngữ cảnh mà nó được định nghĩa. Để tiếp tục
với ví dụ của chúng ta, hãy nghĩ về quy tắc riêng tư như là một phòng họp của
một nhà hàng: những gì xảy ra bên trong phòng họp là riêng tư đối với khách hàng
(tức là họ, khách hàng, không thể truy cập vào phòng họp), nhưng quản lý nhà
hàng có thể thấy và làm mọi thứ trong nhà hàng mà họ quản lý.

Rust đã chọn để hệ thống module hoạt động theo cách này để ẩn các chi tiết
code bên trong một cách mặc định. Theo cách này, bạn biết được những phần của
code bên trong mà bạn có thể thay đổi mà không làm hỏng code bên ngoài. Tuy
nhiên, Rust cũng cho phép bạn lựa chọn để tiết lộ các phần bên trong của module
con đến các module cha bên ngoài bằng cách sử dụng từ khóa `pub` để tạo một
item public.

### Exposing Paths with the `pub` Keyword

Cùng trở lại lỗi trong Listing 7-4 mà nói cho chúng ta rằng module `hosting`
là private. Chúng ta muốn hàm `eat_at_restaurant` trong module cha có thể
truy cập vào hàm `add_to_waitlist` trong module con, vì vậy chúng ta đánh dấu
module `hosting` với từ khóa `pub`, như trong Listing 7-5.

<span class="filename">Filename: src/lib.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-05/src/lib.rs}}
```

<span class="caption">Listing 7-5: Đánh dấu module `hosting` là `pub` để sử
dụng nó từ `eat_at_restaurant`</span>

Rất tiếc, code trong Listing 7-5 vẫn dẫn đến lỗi, như trong Listing 7-6.

```console
{{#include ../listings/ch07-managing-growing-projects/listing-07-05/output.txt}}
```

<span class="caption">Listing 7-6: Lỗi của compiler khi build code trong
Listing 7-5</span>

Điều gì đã xảy ra? Việc thêm từ khóa `pub` trước `mod hosting` làm cho module
public. Với thay đổi này, nếu chúng ta có thể truy cập `front_of_house`, chúng
ta có thể truy cập `hosting`. Nhưng *nội dung* của `hosting` vẫn là private;
việc làm module public không làm cho nội dung của nó public. Từ khóa `pub` trên
module chỉ cho phép code ở module cha tham chiếu đến nó, không cho phép
truy cập code bên trong. Vì module là container, việc làm cho module public;
không thực sự giúp chúng ta việc gì, chúng ta cần đi xa hơn và chọn để làm
public một hoặc nhiều item bên trong module.

Lỗi trong Listing 7-6 nói rằng hàm `add_to_waitlist` là private. Quy tắc
privacy áp dụng cho struct, enum, function, và method cũng như module.

Hãy cũng làm cho hàm `add_to_waitlist` public bằng cách thêm từ khóa `pub`
trước định nghĩa của nó, như trong Listing 7-7.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-07/src/lib.rs}}
```

<span class="caption">Listing 7-7: Thêm từ khóa `pub` vào `mod hosting` và
`fn add_to_waitlist` để chúng ta gọi hàm từ `eat_at_restaurant`</span>

Bây giờ code sẽ compile! Để biết tại sao thêm từ khóa `pub` cho phép chúng ta
sử dụng những đường dẫn này trong `add_to_waitlist` với quy tắc privacy, hãy
xem đường dẫn tuyệt đối và đường dẫn tương đối.

Trong đường dẫn tuyệt đối, chúng ta bắt đầu với `crate`, gốc của cây module
của crate. Module `front_of_house` được định nghĩa trong crate root. Khi
`front_of_house` không phải là public, vì hàm `eat_at_restaurant` được định
nghĩa trong cùng một module với `front_of_house` (tức là `eat_at_restaurant`
và `front_of_house` là anh em), chúng ta có thể tham chiếu đến `front_of_house`
từ `eat_at_restaurant`. Tiếp theo là module `hosting` được đánh dấu với `pub`.
Chúng ta có thể truy cập module cha của `hosting`, vì vậy chúng ta có thể truy
cập `hosting`. Cuối cùng, hàm `add_to_waitlist` được đánh dấu với `pub` và chúng
ta có thể truy cập module cha của nó, vì vậy cuộc gọi hàm này hoạt động!

Trong đường dẫn tương đối, logic là giống như đường dẫn tuyệt đối ngoại trừ
bước đầu tiên: thay vì bắt đầu từ crate root, đường dẫn bắt đầu từ
`front_of_house`. Module `front_of_house` được định nghĩa trong cùng một module
với `eat_at_restaurant`, vì vậy đường dẫn tương đối bắt đầu từ module mà
`eat_at_restaurant` được định nghĩa hoạt động. Sau đó, vì `hosting` và
`add_to_waitlist` được đánh dấu với `pub`, phần còn lại của đường dẫn hoạt
động, và cuộc gọi hàm này là hợp lệ!

Nếu bạn dự định chia sẻ thư viện crate để các dự án khác có thể sử dụng mã của
bạn, API công khai của bạn là interface mà người dùng của crate của bạn tương tác với mã của bạn. Có nhiều điều cần xem xét về việc quản lý các thay đổi
trong API công khai của bạn để làm cho việc phụ thuộc vào crate của bạn dễ dàng
hơn. Những điều này nằm ngoài phạm vi của cuốn sách này; nếu bạn quan tâm đến
chủ đề này, hãy xem [The Rust API Guidelines][api-guidelines].

> #### Best Practices for Packages with a Binary and a Library
>
> Chúng ta đã nói rằng một package có thể chứa cả *src/main.rs* binary crate
> root cũng như *src/lib.rs* library crate root, và cả hai crate sẽ có tên
> package mặc định. Thông thường, các package dạng này sẽ chứa đủ code trong
> binary crate để khởi động một chương trình thực thi, code này sẽ gọi đến
> code tron library crate. Điều này cho phép các dự án khác có thể tận dụng
> tối đa các chức năng mà package cung cấp, vì code trong library crate có thể
> được chia sẻ.
>
> Cây module nên được định nghĩa trong *src/lib.rs*. Sau đó, bất kỳ item công
> khai nào cũng có thể được sử dụng trong binary crate bằng cách bắt đầu đường
> dẫn với tên của package. Binary crate trở thành một người dùng của library
> crate giống như một crate hoàn toàn bên ngoài sẽ sử dụng library crate: nó
> chỉ có thể sử dụng API công khai. Điều này giúp bạn thiết kế một API tốt;
> không chỉ là bạn là người viết, bạn cũng là người dùng!
>
> Trong [Chapter 12][ch12]<!-- ignore -->, chúng ta sẽ minh họa cách tổ chức
> này với một chương trình CLI sẽ chứa cả binary crate và library crate.

---

### Starting Relative Paths with `super`

Chúng ta có thể khai báo đường dẫn tương đối bắt đầu từ module cha, thay vì
module hiện tại hoặc module gốc, bằng cách sử dụng `super` ở đầu đường dẫn.
Điều này giống như bắt đầu một đường dẫn hệ thống tập tin với cú pháp `..`.
Điều này cho phép chúng ta tham chiếu đến một item mà chúng ta biết nằm trong
module cha, điều này sẽ giúp chúng ta dễ dàng sắp xếp lại cây module khi module
đó liên quan gần với module cha, nhưng module cha có thể được di chuyển đến
một nơi khác trong cây module một lúc nào đó trong tương lai.

Xem code trong Listing 7-8 mô hình hóa tình huống trong đó một nhà bếp sửa
lỗi đơn hàng và mang đến cho khách hàng cá nhân. Hàm `fix_incorrect_order`
được định nghĩa trong module `back_of_house` gọi hàm `deliver_order` được định
nghĩa trong module cha bằng cách chỉ định đường dẫn đến `deliver_order` bắt đầu
bằng `super`:

<span class="filename">Tên tập tin: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-08/src/lib.rs}}
```

<span class="caption">Listing 7-8: Gọi một hàm sử dụng đường dẫn tương đối
bắt đầu bằng `super`</span>

Hàm `fix_incorrect_order` nằm trong module `back_of_house`, vì vậy chúng ta
có thể sử dụng `super` để đi đến module cha của `back_of_house`, trong trường
hợp này là `crate`, gốc. Từ đó, chúng ta tìm kiếm `deliver_order` và tìm thấy
nó. Chúng ta cho rằng module `back_of_house` và hàm `deliver_order`
có thể ở trong mối quan hệ tương tự và được di chuyển cùng nhau dù
chúng ta quyết định tổ chức lại cây module của crate. Do đó, chúng ta đã sử
dụng `super` để chúng ta sẽ có ít chỗ cần cập nhật code trong tương lai nếu code
này được di chuyển đến một module khác.

---

### Making Structs and Enums Public

Chúng ta cũng có thể sử dụng `pub` để đánh dấu struct và enum là public, nhưng
có một số chi tiết khác về cách sử dụng `pub` với struct và enum. Nếu chúng ta
sử dụng `pub` trước một định nghĩa struct, chúng ta sẽ làm public struct, nhưng
các trường của struct vẫn sẽ là private. Chúng ta có thể làm public hoặc không
cho mỗi trường một cách riêng biệt. Trong Listing 7-9, chúng ta đã định nghĩa một struct `back_of_house::Breakfast` public với một trường `toast` public nhưng một trường `seasonal_fruit` private. Điều này mô hình trường hợp trong một nhà hàng nơi khách hàng có thể chọn loại bánh mì mà họ muốn kèm theo một bữa ăn, nhưng bếp trưởng quyết định loại quả tươi sẽ đi kèm theo bữa ăn dựa trên mùa và hàng tồn kho. Các loại quả tươi thay đổi nhanh chóng, vì vậy khách hàng không thể chọn loại quả tươi hoặc thậm chí xem được loại quả tươi mà họ sẽ nhận được.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-09/src/lib.rs}}
```

<span class="caption">Listing 7-9: Một struct với một số trường public và một
số trường private</span>

Bởi vì trường `toast` trong struct `back_of_house::Breakfast` là public, trong
`eat_at_restaurant` chúng ta có thể viết và đọc trường `toast` sử dụng dấu chấm
. Lưu ý rằng chúng ta không thể sử dụng trường `seasonal_fruit` trong
`eat_at_restaurant` vì `seasonal_fruit` là private. Hãy thử bỏ comment dòng
sửa đổi giá trị trường `seasonal_fruit` để xem có lỗi gì xảy ra!

Ngoài ra, lưu ý rằng vì `back_of_house::Breakfast` có một trường private, struct
cần phải cung cấp một hàm liên quan public để tạo một instance của `Breakfast`
(chúng ta đã đặt tên là `summer`). Nếu `Breakfast` không có hàm như vậy, chúng
ta không thể tạo một instance của `Breakfast` trong `eat_at_restaurant` vì chúng
ta không thể thiết lập giá trị của trường private `seasonal_fruit` trong
`eat_at_restaurant`.

Trái ngược với struct, nếu chúng ta đặt một enum là public, tất cả các variant
của nó sẽ là public. Chúng ta chỉ cần đặt `pub` trước từ khóa `enum`, như trong
Listing 7-10.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-10/src/lib.rs}}
```

<span class="caption">Listing 7-10: Đánh dấu một enum là public sẽ làm cho tất
cả các variant của nó là public</span>

Bởi vì chúng ta đã đánh dấu enum `Appetizer` là public, chúng ta có thể sử dụng
variant `Soup` và `Salad` trong `eat_at_restaurant`.

Enum không có ích gì nếu các variant của nó không phải là public; sẽ rất phiền
phức phải đánh dấu tất cả các variant của enum với `pub` trong mọi trường hợp,
vì vậy mặc định cho variant của enum là public. Struct thường có ích mà không
cần các trường của nó là public, vì vậy các trường của struct theo quy tắc chung
của mọi thứ là private trừ khi được đánh dấu với `pub`.

Có một trường hợp nữa liên quan đến `pub` mà chúng ta chưa bàn đến, đó là tính
năng cuối cùng của module system: từ khóa `use`. Chúng ta sẽ bàn `use` một mình
trước, và sau đó chúng ta sẽ hiển thị cách kết hợp `pub` và `use`.

[pub]: ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#exposing-paths-with-the-pub-keyword
[api-guidelines]: https://rust-lang.github.io/api-guidelines/
[ch12]: ch12-00-an-io-project.html
