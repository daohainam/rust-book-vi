## Bringing Paths into Scope with the `use` Keyword

Phải viết ra đường dẫn để gọi các hàm có thể cảm thấy không tiện lợi và lặp đi
lặp lại. Trong Listing 7-7, dù chúng ta đã chọn đường dẫn tuyệt đối hay tương
đối đến hàm `add_to_waitlist`, mỗi khi chúng ta muốn gọi `add_to_waitlist` chúng
ta phải chỉ định `front_of_house` và `hosting` nữa. May mắn thay, có một cách để
giảm bớt quá trình này: chúng ta có thể tạo một đường dẫn tắt với từ khóa `use`
một lần, và sau đó sử dụng tên ngắn hơn ở mọi nơi trong scope.

Trong Listing 7-11, chúng ta đưa module `crate::front_of_house::hosting` vào
scope của hàm `eat_at_restaurant` để chúng ta chỉ cần chỉ định
`hosting::add_to_waitlist` để gọi hàm `add_to_waitlist` trong
`eat_at_restaurant`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-11/src/lib.rs}}
```

<span class="caption">Listing 7-11: Mang một module vào scope với `use`</span>

Thêm `use` và đường dẫn trong một scope giống như tạo một liên kết tượng trưng
trong hệ thống tệp. Bằng cách thêm `use crate::front_of_house::hosting` trong
crate root, `hosting` là một tên hợp lệ trong scope đó, giống như module
`hosting` đã được định nghĩa trong crate root. Đường dẫn được đưa vào scope
với `use` cũng kiểm tra quyền riêng tư, giống như bất kỳ đường dẫn nào khác.

Lưu ý rằng `use` chỉ tạo đường dẫn tắt cho scope cụ thể mà `use` xảy ra. Listing
7-12 di chuyển hàm `eat_at_restaurant` vào một module con mới có tên
`customer`, đó là một scope khác với câu lệnh `use`, vì vậy nội dung hàm sẽ không được biên dịch:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness,does_not_compile,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-12/src/lib.rs:here}}
```

<span class="caption">Listing 7-12: Một câu lệnh `use` chỉ áp dụng trong scope
nó đang ở</span>

Lỗi biên dịch cho thấy đường dẫn tắt không còn áp dụng trong module `customer`:

```console
{{#include ../listings/ch07-managing-growing-projects/listing-07-12/output.txt}}
```

Lưu ý rằng còn có một cảnh báo rằng `use` không còn được sử dụng trong scope
của nó! Để sửa vấn đề này, di chuyển `use` trong module `customer` nữa, hoặc
tham chiếu đến đường dẫn tắt trong module cha với `super::hosting` trong module
con `customer`.

### Creating Idiomatic `use` Paths

Trong Listing 7-11, bạn có thể đã thắc mắc tại sao chúng ta chỉ định `use
crate::front_of_house::hosting` và sau đó gọi `hosting::add_to_waitlist` trong
`eat_at_restaurant` thay vì chỉ định đường dẫn `use` đến hàm `add_to_waitlist`
để đạt được kết quả giống như trong Listing 7-13.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-13/src/lib.rs}}
```

<span class="caption">Listing 7-13: Đưa hàm `add_to_waitlist` vào scope với
`use`, điều này không thường được dùng</span>

Mặc dù cả Listing 7-11 và 7-13 đều đạt được cùng một nhiệm vụ, Listing 7-11 là
cách hợp lệ để đưa một hàm vào scope với `use`. Đưa module cha của hàm vào scope
với `use` có nghĩa là chúng ta phải chỉ định module cha khi gọi hàm. Chỉ định
module cha khi gọi hàm làm rõ ràng rằng hàm không được định nghĩa cục bộ trong
lúc vẫn giảm thiểu sự lặp lại của đường dẫn đầy đủ. Mã trong Listing 7-13 không
rõ ràng về nơi `add_to_waitlist` được định nghĩa.

Mặt khác, khi đưa vào struct, enum, và các mục khác với `use`, hợp lý là chỉ
định đường dẫn đầy đủ. Listing 7-14 cho thấy cách hợp lý để đưa struct `HashMap`
của thư viện chuẩn vào scope của một binary crate.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-14/src/main.rs}}
```

<span class="caption">Listing 7-14: Đưa `HashMap` vào scope một cách hợp lý</span>

Không có lý do nào mạnh mẽ đằng sau quy tắc này: nó chỉ là quy ước đã xuất hiện
và mọi người đã quen với việc đọc và viết mã Rust theo cách này.

Ngoại lệ cho quy tắc này là nếu chúng ta đưa hai mục cùng tên vào scope với
câu lệnh `use`, vì Rust không cho phép điều đó. Listing 7-15 cho thấy cách để
đưa hai loại `Result` vào scope mà cùng tên nhưng module cha khác nhau và cách
tham chiếu đến chúng.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-15/src/lib.rs:here}}
```

<span class="caption">Listing 7-15: Đưa hai loại cùng tên vào scope cùng một
lúc yêu cầu sử dụng module cha của chúng.</span>

Như bạn có thể thấy, sử dụng module cha để phân biệt hai loại `Result`. Nếu
thay vì đó chúng ta chỉ định `use std::fmt::Result` và `use std::io::Result`,
chúng ta sẽ có hai loại `Result` trong cùng một scope và Rust sẽ không biết
chúng ta muốn loại nào khi chúng ta sử dụng `Result`.

---

### Providing New Names with the `as` Keyword

Có một cách khác để giải quyết vấn đề đưa hai loại cùng tên vào cùng một scope
với `use`: sau đường dẫn, chúng ta có thể chỉ định `as` và một tên cục bộ mới,
hoặc *bí danh*, cho loại. Listing 7-16 cho thấy một cách khác để viết code trong
Listing 7-15 bằng cách đổi tên một trong hai loại `Result` sử dụng `as`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-16/src/lib.rs:here}}
```

<span class="caption">Listing 7-16: Đổi tên một loại khi nó được đưa vào scope
sử dụng từ khóa `as`</span>

Trong câu lệnh `use` thứ hai, chúng ta đã chọn tên mới `IoResult` cho loại
`std::io::Result`, nó sẽ không xung đột với `Result` từ `std::fmt` mà chúng ta
cũng đã đưa vào scope. Listing 7-15 và Listing 7-16 được xem là tiêu chuẩn, vì
vậy bạn có thể chọn một trong hai!

### Re-exporting Names with `pub use`

Khi chúng ta dùng từ khóa `use`, tên mới được đưa vào chỉ hiện diện bên
trong scope mà `use` được gọi. Để cho phép code bên ngoài có thể gọi đến tên đó
chúng ta có thể kết hợp `pub` và `use`. Kỹ thuật này được gọi là
*export lại(re-exporting)* vì chúng ta đang đưa một item vào scope nhưng cũng
làm cho item đó có sẵn cho người khác để đưa vào scope của họ.

Listing 7-17 cho thấy code trong Listing 7-11 với `use` trong module gốc được
đổi thành `pub use`.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground,test_harness
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-17/src/lib.rs}}
```

<span class="caption">Listing 7-17: Làm cho một tên có sẵn cho bất kỳ code nào
để sử dụng từ một scope mới với `pub use`</span>

Trước khi thay đổi này diễn ra, code bên ngoài phải gọi hàm `add_to_waitlist`
bằng cách sử dụng đường dẫn
`restaurant::front_of_house::hosting::add_to_waitlist()`. Bây giờ vì `pub use`
đã export lại module `hosting` từ module gốc, code bên ngoài có thể sử dụng
đường dẫn `restaurant::hosting::add_to_waitlist()` thay vì đường dẫn cũ.

Re-exporting rất hữu ích khi cấu trúc bên trong code của bạn khác với cách
lập trình viên gọi code của bạn sẽ nghĩ về domain. Ví dụ, trong thí dụ nhà hàng
ở trên, người điều hành nhà hàng nghĩ về “front of house” và “back of house”.
Nhưng khách hàng đến nhà hàng có thể không nghĩ về các phần của nhà hàng theo
cách đó. Với `pub use`, chúng ta có thể viết code của mình với một cấu trúc
nhưng sẽ cho thấy một cấu trúc khác. Việc làm như vậy giúp code của chúng ta
được tổ chức tốt cho cả các lập trình viên làm việc trên code của chúng ta và
các lập trình viên gọi code của chúng ta. Chúng ta sẽ xem một ví dụ khác về
`pub use` và cách nó ảnh hưởng đến tài liệu của crate của bạn trong phần
[“Exporting a Convenient Public API with `pub use`”][ch14-pub-use]<!-- ignore --> của Chapter 14.

### Using External Packages

Trong chương 2, chúng ta đã viết một project đoán số sử dụng một package bên
ngoài gọi là `rand` để lấy ra các số ngẫu nhiên. Để sử dụng `rand` trong
project của chúng ta, chúng ta đã thêm dòng này vào *Cargo.toml*:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch02-00-guessing-game-tutorial.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:9:}}
```

Thêm `rand` làm một dependency trong *Cargo.toml* cho biết Cargo sẽ tải về
package `rand` và bất kỳ dependencies nào từ [crates.io](https://crates.io/) và
cho phép `rand` có sẵn để dùng trong project của chúng ta.

Sau đó, để cho phép `rand` có thể được sử dụng trong scope của package,
chúng ta đã thêm một dòng `use` bắt đầu với tên của crate, `rand`, và liệt kê
những item mà chúng ta muốn cho phép sử dụng trong scope. Nhớ lại trong phần
[“Generating a Random Number”][rand]<!-- ignore --> của chương 2, chúng ta đã
import trait `Rng` vào scope và gọi hàm `rand::thread_rng`:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:ch07-04}}
```

Thành viên của cộng đồng Rust đã làm rất nhiều package có sẵn tại
[crates.io](https://crates.io/), và để sử dụng bất kỳ package nào trong
package của bạn, bạn chỉ cần làm những bước tương tự như trên: liệt kê chúng
trong file *Cargo.toml* của package và sử dụng `use` để import các item từ
package đó vào scope.

Lưu ý rằng thư viện chuẩn `std` cũng là một crate bên ngoài package của chúng
ta. Vì thư viện chuẩn được cung cấp cùng với ngôn ngữ Rust, chúng ta không cần
phải thay đổi *Cargo.toml* để bao gồm `std`. Nhưng chúng ta cần phải tham chiếu
đến nó với `use` để import các item từ đó vào scope của package của chúng ta.
Ví dụ, với `HashMap` chúng ta sẽ sử dụng dòng này:


```rust
use std::collections::HashMap;
```

Đây là một đường dẫn tuyệt đối bắt đầu với `std`, tên của crate thư viện chuẩn.

### Using Nested Paths to Clean Up Large `use` Lists

Nếu chúng ta sử dụng nhiều item được định nghĩa trong cùng một crate hoặc cùng
một module, việc liệt kê mỗi item trên một dòng riêng sẽ chiếm rất nhiều không
gian dọc trong file của chúng ta. Ví dụ, hai dòng `use` này chúng ta có trong
game đoán số ở Listing 2-4 import các item từ `std` vào scope:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/no-listing-01-use-std-unnested/src/main.rs:here}}
```

Thay vì đó, chúng ta có thể sử dụng đường dẫn lồng nhau để import các item
cùng vào scope trong một dòng. Chúng ta làm điều này bằng cách chỉ định phần
chung của đường dẫn, theo sau bởi hai dấu hai chấm, và sau đó là dấu ngoặc nhọn
xung quanh một danh sách các phần của đường dẫn khác nhau, như trong Listing
7-18.

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-18/src/main.rs:here}}
```

<span class="caption">Listing 7-18: Khai báo một đường dẫn lồng nhau để import
nhiều item cùng với cùng một tiền tố vào scope</span>

Trong các chương trình lớn hơn, việc import nhiều item từ cùng một crate hoặc
module sử dụng đường dẫn lồng nhau có thể giảm số lượng các dòng `use` riêng
biệt rất nhiều.

Chúng ta có thể sử dụng một đường dẫn lồng nhau ở bất kỳ mức độ nào trong một
đường dẫn, điều này rất hữu ích khi kết hợp hai dòng `use` chia sẻ một đường
dẫn con. Ví dụ, Listing 7-19 cho thấy hai dòng `use`: một trong đó import
`std::io` vào scope và một trong đó import `std::io::Write` vào scope.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-19/src/lib.rs}}
```

<span class="caption">Listing 7-19: Hai dòng `use` trong đó một là một đường
dẫn con của đường dẫn khác</span>

Phần chung của hai đường dẫn này là `std::io`, và đó là đường dẫn đầu tiên
hoàn chỉnh. Để kết hợp hai đường dẫn này thành một dòng `use`, chúng ta có thể
sử dụng `self` trong đường dẫn lồng nhau, như trong Listing 7-20.

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-20/src/lib.rs}}
```

<span class="caption">Listing 7-20: Kết hợp đường dẫn trong Listing 7-19 thành
một dòng `use`</span>

Dòng này import `std::io` và `std::io::Write` vào scope.

### The Glob Operator

Nếu chúng ta muốn import *tất cả* các mục công khai được định nghĩa trong một
đường dẫn vào scope, chúng ta có thể chỉ định đường dẫn đó theo sau bởi toán
tử `*` glob:

```rust
use std::collections::*;
```

Dòng `use` này import tất cả các mục công khai được định nghĩa trong
`std::collections` vào scope hiện tại. Hãy cẩn thận khi sử dụng toán tử glob!
Toán tử glob có thể làm cho việc biết được tên nào đang trong scope và nơi một
tên được sử dụng trong chương trình của bạn được định nghĩa trở nên khó khăn
hơn.

Toán tử glob thường được sử dụng khi kiểm thử để import tất cả mọi thứ dưới
kiểm thử vào module `tests`; chúng ta sẽ nói về điều đó trong phần
[“How to Write Tests”][writing-tests]<!-- ignore --> trong Chương 11. Toán tử
glob cũng được sử dụng đôi khi là một phần của pattern prelude: xem
[documentation của thư viện chuẩn](../std/prelude/index.html#other-preludes)<!-- ignore --> để biết thêm thông tin về pattern đó.

[ch14-pub-use]: ch14-02-publishing-to-crates-io.html#exporting-a-convenient-public-api-with-pub-use
[rand]: ch02-00-guessing-game-tutorial.html#generating-a-random-number
[writing-tests]: ch11-01-writing-tests.html#how-to-write-tests
