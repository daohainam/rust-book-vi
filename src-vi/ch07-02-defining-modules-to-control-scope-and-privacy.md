## Defining Modules to Control Scope and Privacy

Trong phần này, chúng ta sẽ nói về các module và các phần khác của module
system, đặc biệt là *paths* nó cho phép bạn đặt tên các items; từ khóa `use` để
đưa một path vào scope; và từ khóa `pub` để làm các items public. Chúng ta cũng
sẽ thảo luận về từ khóa `as`, các packages bên ngoài, và toán tử glob.

Đầu tiên, chúng ta sẽ bắt đầu với một danh sách các quy tắc để tham khảo dễ dàng
khi bạn tổ chức code của bạn trong tương lai. Sau đó, chúng ta sẽ giải thích mỗi
quy tắc một cách chi tiết.

### Modules Cheat Sheet

Đây là một bảng tóm tắt về cách các module, paths, từ khóa `use` và từ khóa
`pub` hoạt động trong compiler, và cách hầu hết các lập trình viên tổ chức code
của họ. Chúng ta sẽ đi qua các ví dụ của mỗi quy tắc trong chương này, nhưng
đây là một nơi tuyệt vời để tham khảo như một lời nhắc về cách các module hoạt
động.

- **Bắt đầu từ crate root**: Khi biên dịch một crate, compiler sẽ đầu tiên tìm
  trong file crate root (thường là *src/lib.rs* cho một crate library hoặc
  *src/main.rs* cho một crate binary).
- **Định nghĩa module**: Trong file crate root, bạn có thể định nghĩa các module
  mới; nói cách khác, bạn định nghĩa một module “garden” với `mod garden;`. Compiler sẽ tìm kiếm code của module trong những nơi sau:
  - Trực tiếp, trực tiếp sau `mod garden`, trong dấu ngoặc nhọn thay vì dấu chấm phẩy
  - Trong file *src/garden.rs*
  - Trong file *src/garden/mod.rs*
- **Định nghĩa submodules**: Trong bất kỳ file nào khác file crate root, bạn có
  thể định nghĩa các submodules. Ví dụ, bạn có thể định nghĩa `mod vegetables;`
  trong *src/garden.rs*. Compiler sẽ tìm kiếm code của submodule trong thư mục
  có tên giống với module cha trong những nơi sau:
  - Trực tiếp, trực tiếp sau `mod vegetables`, trong dấu ngoặc nhọn thay vì dấu chấm phẩy
  - Trong file *src/garden/vegetables.rs*
  - Trong file *src/garden/vegetables/mod.rs*
- **Path dẫn đến code trong module**: Một khi một module là một phần của crate,
  bạn có thể tham chiếu đến code trong module đó từ bất kỳ nơi nào trong cùng
  crate, một khi các quy tắc bảo mật cho phép, bằng cách sử dụng đường dẫn (path) đến code. Ví dụ, một kiểu `Asparagus` trong module vegetables của
  garden sẽ được tìm thấy tại `crate::garden::vegetables::Asparagus`.
- **Private vs public**: Code trong một module mặc định là private với các module
  cha của nó. Để làm cho một module public, định nghĩa nó với `pub mod` thay vì
  `mod`. Để làm cho các item trong một module public, sử dụng `pub` trước các
  định nghĩa của chúng.
- **Từ khóa `use`**: Trong một scope, từ khóa `use` tạo các đường dẫn tắt đến các
  item để giảm thiểu sự lặp lại của các đường dẫn dài. Trong bất kỳ scope nào
  có thể tham chiếu đến `crate::garden::vegetables::Asparagus`, bạn có thể tạo
  một đường dẫn tắt với `use crate::garden::vegetables::Asparagus;` và từ đó
  bạn chỉ cần viết `Asparagus` để sử dụng kiểu đó trong scope.

Ở đây chúng ta tạo một crate nhị phân có tên `backyard` để minh họa các quy tắc
trên. Thư mục của crate, cũng được đặt tên là `backyard`, chứa các file và
thư mục sau:

```text
backyard
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs
```

File crate root trong trường hợp này là *src/main.rs*, và nó chứa:

<span class="filename">Filename: src/main.rs</span>

```rust,noplayground,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/quick-reference-example/src/main.rs}}
```

Dòng `pub mod garden;` nói với trình biên dịch rằng hãy thêm code mà nó tìm
thấy trong *src/garden.rs*, code là:

<span class="filename">Filename: src/garden.rs</span>

```rust,noplayground,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/quick-reference-example/src/garden.rs}}
```

Ở đây, `pub mod vegetables;` có nghĩa là code trong *src/garden/vegetables.rs*
cũng được bao gồm. Code đó là:

```rust,noplayground,ignore
{{#rustdoc_include ../listings/ch07-managing-growing-projects/quick-reference-example/src/garden/vegetables.rs}}
```

Bây giờ hãy đi vào chi tiết của các quy tắc này và minh họa chúng trong thực tế!

### Grouping Related Code in Modules

*Modules* cho phép chúng ta tổ chức code trong một crate để dễ đọc và tái sử
dụng. Modules cũng cho phép chúng ta kiểm soát *bí mật* của các item, vì code
trong một module là riêng tư theo mặc định. Các item riêng tư là nội bộ và
không có sẵn cho việc sử dụng bên ngoài. Chúng ta có thể để các modules và các
item bên trong chúng là public, nó cho phép chúng ta sử dụng chúng.

Ví dụ, hãy cùng viết một library crate cung cấp các tính năng của một nhà hàng.
Chúng ta sẽ định nghĩa các hàm nhưng để trống các thân hàm để tập trung vào
cấu trúc của code, thay vì việc triển khai một nhà hàng.

Trong ngành ẩm thực, một số bộ phận của một nhà hàng được gọi là *front of
house* và một số khác là *back of house*. Front of house là nơi khách hàng đến;
bao gồm nơi chủ nhà hàng tiếp khách, nhân viên phục vụ nhận đơn đặt
hàng và thanh toán, và nhân viên pha chế làm các loại đồ uống. Back of house là
nơi nấu ăn và làm bếp, nhân viên vệ sinh dọn dẹp, và quản lý làm việc hành
chính.

Cấu trúc crate của chúng ta theo cách này, chúng ta có thể tổ chức các hàm
của nó thành các modules lồng nhau. Tạo một library mới tên `restaurant` bằng
cách chạy `cargo new --lib restaurant`; sau đó nhập code trong Listing 7-1 vào
*src/lib.rs* để định nghĩa một số modules và chữ ký hàm. Đây là phần front of
house:

<span class="filename">Filename: src/lib.rs</span>

```rust,noplayground
{{#rustdoc_include ../listings/ch07-managing-growing-projects/listing-07-01/src/lib.rs}}
```

<span class="caption">Listing 7-1: Một module `front_of_house` chứa các module
khác sau đó chứa các hàm</span>

Chúng ta định nghĩa một module với từ khóa `mod` theo sau bởi tên của module
(trong trường hợp này, `front_of_house`). Thân của module sau đó đi vào trong
dấu ngoặc nhọn. Trong các module, chúng ta có thể đặt các module khác, như trong
trường hợp này với các module `hosting` và `serving`. Các module cũng có thể
chứa các định nghĩa cho các mục khác, chẳng hạn như các cấu trúc, các enum,
các hằng số, các trait, như trong Listing 7-1—functions.

Bằng cách sử dụng các module, chúng ta có thể nhóm các định nghĩa liên quan nhau
vào cùng một nhóm và đặt tên cho lý do đại diện của chúng. Các lập trình viên
sử dụng code này có thể điều hướng code dựa trên các nhóm thay vì phải đọc qua
tất cả các định nghĩa, làm cho việc tìm kiếm các định nghĩa liên quan đến dễ
dàng hơn. Các lập trình viên thêm tính năng mới vào code này sẽ biết nơi để
đặt giúp cho chương trình được tổ chức tốt.

Trước đây, chúng ta đã nói rằng *src/main.rs* và *src/lib.rs* được gọi là
crate roots. Chúng tên vậy vì nội dung của bất kỳ một trong hai tệp này tạo
ra một module được gọi là `crate` ở gốc của cấu trúc module của crate, được
gọi là *module tree*.

Listing 7-2 cho thấy module tree cho cấu trúc trong Listing 7-1.

```text
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```

<span class="caption">Listing 7-2: Module tree cho code trong Listing 7-1</span>

Cây này cho thấy cách các module lồng nhau; ví dụ, `hosting` lồng trong
`front_of_house`. Cây cũng cho thấy rằng một số module là *anh em (siblings)* với nhau, nghĩa là chúng được định nghĩa trong cùng một module; `hosting` và
`serving` là anh em cùng được định nghĩa trong `front_of_house`. Nếu module A
được chứa bên trong module B, chúng ta nói module A là *con (child)* của module
B và module B là *cha (parent)* của module A. Lưu ý rằng toàn bộ module tree
được gốc dưới module ngầm định được gọi là `crate`.

Cây module có thể liên tưởng đến cây thư mục của hệ thống tệp trên máy tính của
bạn; đây là một so sánh rất chính xác! Giống như các thư mục trong hệ thống
tệp, bạn sử dụng module để tổ chức code của bạn. Cũng giống như các tệp trong
thư mục, chúng ta cần một cách để tìm module của chúng ta.