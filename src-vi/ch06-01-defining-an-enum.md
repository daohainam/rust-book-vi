## Defining an Enum

Trong khi struct cho phép bạn nhóm các trường dữ liệu liên quan lại với nhau, ví dụ như `Rectangle` (hình chữ nhật) sẽ có `width` (chiều rộng) và `height` (chiều dài), enum cho phép bạn miêu tả một tập hợp chứa các giá trị có thể xảy ra. Ví dụ, chúng ta có thể muốn nói rằng `Rectangle` là một hình trong một tập hợp các hình, bao gồm `Circle` (hình tròn) và `Triangle` (tam giác). Rust cho phép chúng ta định nghĩa kiểu dữ liệu này bằng một enum.

Hãy xem xét một trường hợp mà chúng ta có thể muốn biểu diễn trong code và xem tại sao enum là hữu ích và phù hợp hơn struct trong trường hợp này. Hãy nói rằng chúng ta cần làm việc với địa chỉ IP. Hiện tại, có hai tiêu chuẩn chính được sử dụng cho địa chỉ IP: IPv4 và IPv6. Vì đây là những lựa chọn duy nhất cho địa chỉ IP mà chương trình của chúng ta sẽ gặp phải, chúng ta có thể *liệt kê* (enumerate) tất cả các biến thể có thể xảy ra, tên `enum` cũng được đặt ra từ enumerate.

Bất kỳ địa chỉ IP nào đều có thể là địa chỉ IPv4 hoặc địa chỉ IPv6, nhưng không thể là cả hai cùng một lúc. Đặc tính này của địa chỉ IP khiến cho kiểu dữ liệu enum phù hợp, vì một giá trị enum chỉ có thể là một trong các biến thể của nó. Cả địa chỉ IPv4 và địa chỉ IPv6 vẫn là địa chỉ cơ bản đều là địa chỉ IP, vì vậy chúng nên được xem như cùng một kiểu dữ liệu khi code cần xử lý địa chỉ IP.

Chúng ta có thể biểu diễn khái niệm này trong code bằng cách định nghĩa một enum `IpAddrKind` và liệt kê các loại có thể có của địa chỉ IP, `V4` và `V6`. Đây là các biến thể của enum:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:def}}
```

`IpAddrKind` là một kiểu dữ liệu tùy chỉnh mà chúng ta có thể sử dụng ở bất kỳ đâu trong code của mình.

---

### Enum Values

Chúng ta có thể tạo các thể hiện (instance) của mỗi biến thể của `IpAddrKind` như sau:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:instance}}
```

Lưu ý rằng các biến thể của enum được đặt trong namepsace của nó, và chúng ta sử dụng hai dấu hai chấm để phân tách chúng. Điều này rất hữu ích bởi vì giờ cả hai giá trị `IpAddrKind::V4` và `IpAddrKind::V6` đều là cùng một kiểu: `IpAddrKind`. Chúng ta có thể, ví dụ, định nghĩa một hàm nhận bất kỳ `IpAddrKind` nào:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:fn}}
```

Và chúng ta có thể gọi hàm này với bất kỳ biến thể nào:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-01-defining-enums/src/main.rs:fn_call}}
```

Sử dụng enum có nhiều lợi ích khác. Nghĩ về kiểu địa chỉ IP của chúng ta, hiện tại chúng ta không có cách nào để lưu trữ *dữ liệu* thực sự của địa chỉ IP; chúng ta chỉ biết nó là *loại* gì. Vì bạn mới học về struct trong chương 5, bạn có thể muốn giải quyết vấn đề này bằng cách sử dụng struct như trong Listing 6-1.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-01/src/main.rs:here}}
```

<span class="caption">Listing 6-1: Lưu trữ dữ liệu và biến thể `IpAddrKind` của một địa chỉ IP bằng cách sử dụng một `struct`</span>

Ở đây, chúng ta đã định nghĩa một struct `IpAddr` có hai trường: một trường `kind` có kiểu `IpAddrKind` (enum mà chúng ta đã định nghĩa trước đó) và một trường `address` có kiểu `String`. Chúng ta có hai thể hiện của struct này. Thứ nhất là `home`, và nó có giá trị `IpAddrKind::V4` làm giá trị `kind` với dữ liệu địa chỉ liên quan là `127.0.0.1`. Thể hiện thứ hai là `loopback`. Nó có biến thể khác của `IpAddrKind` làm giá trị `kind`, `V6`, và có địa chỉ `::1`. Chúng ta đã sử dụng một struct để gói gọn các giá trị `kind` và `address` lại với nhau, vì vậy giờ biến thể được liên kết với giá trị.

Tuy nhiên, biểu diễn cùng một khái niệm bằng cách sử dụng chỉ một enum sẽ ngắn gọn hơn: thay vì một enum bên trong một struct, chúng ta có thể đặt dữ liệu trực tiếp vào mỗi biến thể enum. Định nghĩa mới của enum `IpAddr` nói rằng cả biến thể `V4` và `V6` sẽ có các giá trị `String` liên quan:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-02-enum-with-data/src/main.rs:here}}
```

Chúng ta gắn dữ liệu vào mỗi biến thể enum trực tiếp, vì vậy không cần một struct bổ sung. Ở đây cũng dễ dàng hơn để thấy một chi tiết khác về cách hoạt động của enum: tên của mỗi biến thể enum mà chúng ta định nghĩa cũng trở thành constructor tạo ra một instance của enum. Trong đó, `IpAddr::V4()` là constructor nhận một đối số `String` và trả về một instance của kiểu `IpAddr`. Constructor này được tự động định nghĩa khi định nghĩa enum.

Một lợi ích khác của việc sử dụng enum thay vì struct là mỗi biến thể có thể có các kiểu và số lượng dữ liệu liên quan khác nhau (ở đây nói về kiểủ của dữ liệu được gắn vào enum, không phải kiểu dữ liệu mà enum thể hiện). Địa chỉ IP phiên bản 4 sẽ luôn có 4 thành phần số có giá trị từ 0 đến 255. Nếu chúng ta muốn lưu trữ địa chỉ `V4` dưới dạng 4 giá trị `u8` nhưng vẫn muốn biểu diễn địa chỉ `V6` dưới dạng một giá trị `String`, chúng ta sẽ không thể làm được với một struct. Enum xử lý trường hợp này một cách dễ dàng:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-03-variants-with-different-data/src/main.rs:here}}
```

Chúng ta đã có thể dùng nhiều cách khác nhau để định nghĩa cấu trúc dữ liệu để lưu trữ địa chỉ IPv4 và IPv6. Tuy nhiên, việc lưu trữ địa chỉ IP và mã hóa loại địa chỉ đó là rất phổ biến nên [thư viện chuẩn có một định nghĩa cho chúng mà chúng ta có thể sử dụng!][IpAddr]<!-- ignore --> Hãy xem cách thư viện chuẩn định nghĩa `IpAddr`: nó có enum và các biến thể mà chúng ta đã định nghĩa và sử dụng, nhưng nó nhúng dữ liệu địa chỉ bên trong các biến thể dưới dạng hai struct khác nhau, được định nghĩa khác nhau cho mỗi biến thể:

```rust
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

Đoạn code này minh họa rằng bạn có thể đặt bất kỳ loại dữ liệu nào bên trong một biến thể enum: chuỗi, kiểu số, hoặc struct. Bạn cũng có thể bao gồm một enum khác! Ngoài ra, các loại thư viện chuẩn thường không phức tạp hơn những gì bạn có thể tạo ra.

Lưu ý rằng ngay cả khi thư viện chuẩn chứa một định nghĩa cho `IpAddr`, chúng ta vẫn có thể tạo và sử dụng định nghĩa của riêng mình mà không xung đột vì chúng ta chưa đưa định nghĩa của thư viện chuẩn vào phạm vi (scope) của mình. Chúng ta sẽ nói thêm về việc đưa các loại vào phạm vi trong Chương 7.

Hãy cùng nhìn vào một ví dụ khác về enum trong Listing 6-2: một enum `Message` có các biến thể lưu trữ các loại và số lượng khác nhau của giá trị.

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/listing-06-02/src/main.rs:here}}
```

Enum này có 4 biến thể với các loại khác nhau:

* `Quit` không có dữ liệu nào được gắn với nó.
* `Move` có các trường được đặt tên giống như một struct.
* `Write` bao gồm một chuỗi `String`.
* `ChangeColor` bao gồm 3 giá trị `i32`.

Định nghĩa một enum với các biến thể như trong Listing 6-2 tương tự như định nghĩa các loại struct khác, ngoại trừ enum không sử dụng từ khóa `struct` và tất cả các biến thể được nhóm lại dưới loại `Message`. Các struct sau có thể chứa cùng dữ liệu với các biến thể enum trước đó:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-04-structs-similar-to-message-enum/src/main.rs:here}}
```

Nhưng nếu chúng ta sử dụng các struct khác, mỗi struct có loại riêng, chúng ta sẽ không thể dễ dàng định nghĩa một hàm nhận bất kỳ loại tin nhắn nào như chúng ta có thể với enum `Message` được định nghĩa trong Listing 6-2, một loại duy nhất.

Có một điểm tương đồng nữa giữa enum và struct: giống như chúng ta có thể định nghĩa các phương thức trên struct sử dụng `impl`, chúng ta cũng có thể định nghĩa các phương thức trên enum. Đây là một phương thức có tên `call` mà chúng ta có thể định nghĩa trên enum `Message` của chúng ta:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-05-methods-on-enums/src/main.rs:here}}
```

Phần thân của phương thức sẽ sử dụng `self` để lấy giá trị mà chúng ta khởi tạo. Trong ví dụ này, chúng ta đã tạo một biến `m` có giá trị `Message::Write(String::from("hello"))`, và giá trị này cũng sẽ là giá trị của `self` mà phương thức `call` sẽ lấy ra khi `m.call()` chạy.

Hãy xem một enum khác trong thư viện chuẩn rất phổ biến và hữu ích: `Option`.

---

### The `Option` Enum and Its Advantages Over Null Values

Phần này sẽ khám phá một trường hợp sử dụng của `Option`, một enum được định nghĩa bởi thư viện chuẩn. Kiểu `Option` có thể được dùng trong nhiều tình huống mà dữ liệu có thể là một giá trị hoặc không có giá trị.

Ví dụ, nếu bạn yêu cầu phần tử đầu tiên của một danh sách, bạn sẽ nhận được một giá trị. Nếu bạn yêu cầu phần tử đầu tiên của một danh sách rỗng, bạn sẽ không nhận được gì. Biểu diễn khái niệm này trong hệ thống kiểu có nghĩa là trình biên dịch có thể kiểm tra xem bạn có xử lý tất cả các trường hợp mà bạn nên xử lý hay không; tính năng này có thể ngăn chặn các lỗi rất phổ biến trong các ngôn ngữ lập trình khác.

Thiết kế ngôn ngữ lập trình thường được xem như là việc bạn sẽ thêm vào các tính năng nào, nhưng các tính năng bạn loại bỏ cũng quan trọng. Rust không có tính năng null mà nhiều ngôn ngữ khác có. *Null* là một giá trị có nghĩa là không có giá trị nào ở đó. Trong các ngôn ngữ có null, các biến luôn có thể ở một trong hai trạng thái: null hoặc không null.

Vào năm 2009, trong bài trình bày "Null References: The Billion Dollar Mistake", Tony Hoare, người sáng chế null, nói như sau:

> Tôi gọi nó là sai lầm đắt đỏ hàng tỷ đô la của tôi. Tại thời điểm đó, tôi đang thiết kế hệ thống kiểu tham chiếu đầu tiên cho một ngôn ngữ lập trình hướng đối tượng. Mục tiêu của tôi là đảm bảo rằng tất cả các tham chiếu sẽ được an toàn, với kiểm tra được thực hiện tự động bởi trình biên dịch. Nhưng tôi không thể cưỡng lại nỗi nản lòng của tôi để thêm vào một tham chiếu null, chỉ vì nó rất dễ để triển khai. Điều này đã dẫn đến hàng ngàn lỗi, thiệt hại và hỏng hóc hệ thống, có thể đã gây ra hàng tỷ đô la sự cố trong 40 năm qua.

Vấn đề với các giá trị null là nếu bạn cố gắng sử dụng một giá trị null như một giá trị not-null, bạn sẽ nhận được một loại lỗi nào đó. Bởi vì tính chất này null hoặc not-null rất phổ biến, nó rất dễ để gây ra loại lỗi này.

Tuy nhiên, khái niệm mà null đang cố gắng biểu thị vẫn là một khái niệm hữu ích: một null là một giá trị hiện tại không hợp lệ hoặc vắng mặt vì một lý do nào đó.

Vấn đề thực sự không phải là với khái niệm mà là với cách hiện thực cụ thể của null. Vì vậy, Rust không có null, nhưng nó có một dạng enum có thể  thể hiện rằng giá trị đang vắng mặt. Enum này là `Option<T>`, và nó được [định nghĩa bởi thư viện chuẩn][option]<!-- ignore --> như sau:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

Enum `Option<T>` rất hữu ích nên nó được bao gồm trong prelude; bạn không cần phải đưa nó vào scope một cách tường minh. Các biến thể của nó cũng được bao gồm trong prelude: bạn có thể sử dụng `Some` và `None` trực tiếp mà không cần tiền tố `Option::`. Enum `Option<T>` vẫn chỉ là một enum thông thường, và `Some(T)` và `None` vẫn là các biến thể của kiểu `Option<T>`.

`<T>` là syntax của Rust mà chúng ta chưa nói đến. Nó là một tham số kiểu generic, và chúng ta sẽ tìm hiểu về generic chi tiết hơn trong chương 10. Hiện tại, bạn chỉ cần biết rằng `<T>` có nghĩa là biến thể `Some` của enum `Option` có thể chứa một phần dữ liệu của bất kỳ kiểu nào, và mỗi kiểu cụ thể được sử dụng thay thế cho `T` làm cho kiểu `Option<T>` tổng thể trở thành một kiểu khác. Đây là một số ví dụ về việc sử dụng giá trị `Option` để chứa các kiểu số và các kiểu chuỗi:

```rust
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-06-option-examples/src/main.rs:here}}
```

Kiểu của `some_number` là `Option<i32>`. Kiểu của `some_char` là `Option<char>`, đây là một kiểu khác. Rust có thể suy ra các kiểu này vì chúng ta đã chỉ định một giá trị bên trong biến thể `Some`. Đối với `absent_number`, Rust yêu cầu chúng ta phải gắn nhãn kiểu tổng thể `Option`: trình biên dịch không thể suy ra kiểu mà biến thể `Some` sẽ chứa bằng cách chỉ xem một giá trị `None`. Ở đây, chúng ta nói với Rust rằng chúng ta muốn `absent_number` có kiểu `Option<i32>`.

Khi chúng ta có một giá trị `Some`, chúng ta biết rằng có một giá trị hiện có và giá trị đó được giữ bên trong `Some`. Khi chúng ta có một giá trị `None`, một cách nào đó, nó có nghĩa giống như null: chúng ta không có một giá trị hợp lệ. Vậy tại sao  `Option<T>` tốt hơn null?

Ngắn gọn mà nói, vì `Option<T>` và `T` (với `T` có thể là bất kỳ kiểu nào) là các kiểu khác nhau, trình biên dịch sẽ không cho phép chúng ta sử dụng một giá trị `Option<T>` như là một giá trị hợp lệ. Ví dụ, đoạn mã này sẽ không biên dịch được vì nó đang cố gắng cộng một `i8` với một `Option<i8>`:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch06-enums-and-pattern-matching/no-listing-07-cant-use-option-directly/src/main.rs:here}}
```

Nếu chúng ta chạy đoạn code này, chúng ta sẽ nhận được một thông báo lỗi như sau:

```console
{{#include ../listings/ch06-enums-and-pattern-matching/no-listing-07-cant-use-option-directly/output.txt}}
```

Thông báo lỗi này có nghĩa là Rust không hiểu cách cộng một `i8` và một `Option<i8>`, vì chúng là các kiểu khác nhau. Khi chúng ta có một giá trị của một kiểu như `i8` trong Rust, trình biên dịch sẽ đảm bảo rằng chúng ta luôn có một giá trị hợp lệ. Chúng ta có thể tiến hành một cách tự tin mà không cần kiểm tra null trước khi sử dụng giá trị đó. Chỉ khi chúng ta có một `Option<i8>` (hoặc bất kỳ kiểu giá trị nào chúng ta đang làm việc với nó) thì chúng ta mới phải lo lắng về việc có thể không có một giá trị, và trình biên dịch sẽ đảm bảo rằng chúng ta xử lý trường hợp đó trước khi sử dụng giá trị.

Nói cách khác, bạn phải chuyển đổi một `Option<T>` thành một `T` trước khi bạn có thể thực hiện các thao tác trên `T`. Thông thường, điều này giúp phát hiện một trong những vấn đề phổ biến nhất với null: giả định rằng một thứ gì đó không phải là null khi nó thực chất là null.

Giảm thiểu rủi ro của những giả định không đúng về giá trị not-null giúp bạn tin tưởng hơn vào code của mình. Để có một giá trị có thể là null, bạn phải chọn lựa một cách rõ ràng bằng cách đặt kiểu của giá trị đó là `Option<T>`. Sau đó, khi bạn sử dụng giá trị đó, bạn sẽ buộc phải xử lý trường hợp khi giá trị là null. Mọi nơi mà một giá trị có một kiểu không phải là `Option<T>`, bạn *có thể* an toàn giả định rằng giá trị đó không phải là null. Đây là một quyết định thiết kế có chủ đích của Rust để giới hạn sự lan truyền của null và tăng tính an toàn của code Rust.

Do đó, làm thế nào để bạn lấy giá trị `T` ra khỏi một biến thể `Some` khi bạn có một giá trị của kiểu `Option<T>` để bạn có thể sử dụng giá trị đó? Enum `Option<T>` có một số phương thức rất hữu ích trong một số tình huống; bạn có thể kiểm tra chúng trong [tài liệu của nó][docs]<!-- ignore -->. Quen thuộc với các phương thức trên `Option<T>` sẽ rất hữu ích trong hành trình của bạn với Rust.

Tổng quan mà nói, để sử dụng một giá trị `Option<T>`, bạn cần có code để xử lý mỗi biến thể. Bạn cần một code để chạy chỉ khi bạn có một giá trị `Some(T)`, và code này được phép sử dụng `T` bên trong. Bạn cần một code khác để chạy nếu bạn có một giá trị `None` và code đó không có một giá trị `T` nào. Biểu thức `match` là một cấu trúc điều khiển sẽ giúp bạn làm điều này khi được sử dụng với enum: nó sẽ chạy code khác nhau tùy thuộc vào biến thể của enum mà nó có, và code đó có thể sử dụng dữ liệu bên trong giá trị khớp với nó.

---

[IpAddr]: ../std/net/enum.IpAddr.html
[option]: ../std/option/enum.Option.html
[docs]: ../std/option/enum.Option.html
