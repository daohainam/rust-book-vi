# Giới thiệu

> Ghi chú: Phiên bản này cũng chính là phiên bản in
[The Rust Programming
> Language][nsprust] và ebook [No Starch
> Press][nsp] của sách.

[nsprust]: https://nostarch.com/rust
[nsp]: https://nostarch.com/

Chào mừng bạn đến với *Ngôn ngữ lập trình Rust*, một cuốn sách giới thiệu về Rust.
"Ngôn ngữ lập trình Rust" sẽ giúp bạn viết các phần mềm nhanh và tin cậy hơn.
Việc thiết kế ngôn ngữ lập trình luôn phải giải quyết bài toán xung đột giữa
kiểm soát ở cấp thấp và việc hỗ trợ con người ở bậc cao; Rust thách thức sự xung đột
này. Thông qua khả năng cân bằng giữa khả năng tiếp thu công nghệ mạnh mẽ và kinh nghiệm
phát triển tuyệt vời, Rust cung cấp cho bạn khả năng kiểm soát các chi tiết ở cấp độ 
thấp (chẳng hạn việc sử dụng bộ nhớ) mà vẫn tránh được những phiền toái vốn hay gặp phải
khi phải làm việc ở cấp độ này.

## Rust được dành cho ai

Rust khá lý tưởng cho nhiều người vì nhiều lý do khác nhau. Hãy cũng xem qua một 
vài trong số những nhóm lý do quan trọng nhất.

### Các nhóm phát triển

Rust đang dần chứng minh như một công cụ hữu hiệu cho việc cộng tác giữa những nhóm 
lớn nhà phát triển với các mức độ kiến thức về lập trình hệ thống khác nhau. Mã lệnh cấp thấp
thường dính phải các loại lỗi ẩn, vốn chỉ có thể tìm thấy thông qua việc kiểm thử 
hoặc review cẩn thận bởi các nhà phát triển nhiều kinh nghiệm. Trong Rust, trình biên
dịch đóng vai trò như người gác cổng bằng cách từ chối các loại lỗi như vậy, bao gồm
cả các lỗi liên quan đến việc xử lý đồng thời. Bằng cách kết hợp với trình dịch, nhóm
phát triển có thể dành thời gian tập trung cho logic chương trình hơn là tìm kiếm
các lỗi.

Rust cũng đồng thời cung cấp các công cụ hỗ trợ phát triển đến cho thế giới lập trình hệ thống:

* Cargo, công cụ quản lý thư viện và build, cho phép thêm, dịch, quản lý 
các thư viện phụ thuộc dễ dàng và đồng bộ xuyên suốt hệ sinh thái Rust.
* Rustfmt đảm bảo sự đồng nhất về phong cách viết code giữa các nhà phát triển.
* The Rust Language Server cung cấp sức mạnh cho các Integrated Development Environment (IDE) để 
hỗ trợ các tính năng như code completion và các thông báo lỗi tại chỗ.

Bằng cách sử dụng các công cụ ở trên cũng như một số công cụ khác trong hệ sinh thái
Rust, các nhà phát triển có thể viết một cách hiệu quả các mã lệnh cấp hệ thống.

### Sinh viên

Rust được dành cho sinh viên và bất kỳ ai yêu thích việc học các khái niệm 
hệ thống. Nhiều người thông qua sử dụng Rust đã học về những chủ đề như phát
triển hệ điều hành. Cộng đông Rust cũng rất sẵn sàng chào đón và hỗ trợ trả 
các câu hỏi của sinh viên. Thông qua những nỗ lực tương tự như cuốn sách này,
các nhóm Rust muốn làm cho việc hiểu các khái niệm về hệ thống dễ dàng hơn với 
nhiều người, đặc biệt những người mới làm quen với lập trình.

### Các công ty

Hàng trăm công ty, cả lớn và nhỏ, sử dụng Rust cho nhiều nhiệm vụ khác nhau trong 
hoạt động của họ. Những nhiệm vụ đó bao gồm các công cụ dòng lệnh, dịch vụ web, 
các công cụ DevOps, các thiết bị nhúng, các trình phân tích và mã hóa âm thanh 
hình ảnh, tiền mã hóa, tin sinh học, các máy tìm kiếm, các ứng dụng IoT, học máy, 
và thậm chí các phần chính của trình duyệt web FireFox.

### Các nhà phát triển mã nguồn mở

Rust dành cho những người tạo nên ngôn ngữ, cộng đồng, công cụ phát triển
và các thư viện. Chúng tôi sẽ rất vui khi được bạn góp phần xây dựng ngôn ngữ Rust.

### Những người quan tâm đến tốc độ và ổn định

Rust được dành cho những người đam mê tốc độ và sự ổn định trong một ngôn ngữ. Với
tốc độ, chúng tôi nó về cả tốc độ thực thi của các chương trình do bạn viết, đồng thời 
tốc độ mà Rust cho phép bạn tạo ra chúng. Các phép kiểm tra của trình dịch Rust đảm
bảo sự ổn định thông qua các đặc tính thêm vào và việc tái cấu trúc (refactoring). 
Điều này ngược lại với các mã lệnh dễ-mắc-lỗi khi viết bằng các ngôn ngữ không có 
các phép tra đó, vốn làm cho các nhà phát triển ngại việc chỉnh sửa. Bằng việc đánh 
vào sự trừu tượng, hoặc các đặc tính ở cấp độ cao không gây phát sinh ra thêm chi phí 
khi dịch ra mã lệnh cấp thấp, Rust cho phép các mã lệnh này thực thi nhanh và an toàn 
như khi viết bằng tay. 

Ngôn ngữ Rust cũng hy vọng hỗ trợ thêm nhiều người dùng khác; những người được nhắc
đến ở đây chỉ đơn thuần là một trong số những người tham gia nhiều nhất. Tổng thể
lại, tham vọng lớn nhất của Rust là loại bỏ những sự đánh đổi mà lập trình viên phải
chấp nhận trong hàng thập kỷ qua, để cung cấp sự an toàn *và* năng suất, tốc 
độ *và* sự hỗ trợ bậc cao của ngôn ngữ. Hãy thử và xem Rust liệu có thể trở thành 
lựa chọn cho công việc của bạn.

## Cuốn sách này dành cho ai

Cuốn sách này cho rằng bạn đã viết code bằng một ngôn ngữ khác nhưng không chỉ 
rõ là ngôn ngữ nào. Chúng tôi đã cố gắng tạo ra các tài liệu có thể dùng được 
bởi nhiều người với nhiều nền tảng lập trình khác nhau. Chúng tôi không dành nhiều
thời gian để nói về những thứ như lập trình *là gì* và bạn nghĩ về nó như thế nào.
Nếu bạn là người hoàn toàn mới với việc lập trình, có lẽ tốt hơn là bạn nên đọc 
trước một quyển sách chuyên về nhập môn lập trình.

## Sử dụng quyển sách này như thế nào

Về tổng thể, quyển sách này cho là bạn đang đọc tuần tự từ đầu đến cuối. Các chương
sau được xây dựng dựa trên các khái niệm được giới thiệu trong các chương trước,
và các chương trước có lẽ sẽ không đi vào chi tiết về một chủ đề; chúng ta thường
sẽ quay lại chủ đề đó trong các chương sau.

Bạn sẽ tìm thấy hai loại chương trong cuốn sách này: các chương khái niệm và các chương
dự án. Trong các chương khái niệm, bạn sẽ học về một khía cạnh nào đó của Rust. Trong 
các chương dự án, chúng ta sẽ cùng với nhau xây dựng các chương trình nhỏ, áp dụng 
những gì các bạn đã học. Các chương 2, 12 và 20 là các chương dự án; còn lại là các
chương khái niệm.

Chương 1 hướng dẫn cách cài đặt Rust, làm sao để viết một chương trình "Hello, world!",
và làm sao sử dung Cargo, công cụ quản lý các gói thư viện và build chương trình. 
Chương 2 là một phần giới thiệu kiểu "trên tay" về ngôn ngữ Rust. Chúng ta sẽ xem sơ
các khái niệm ở cấp cao, rồi các chương sau đó sẽ cung cấp thêm các chi tiết. Nếu
bạn muốn vọc vạch ngay, chương lại là dành cho bạn. Đầu tiên, có lẽ bạn sẽ muốn 
bỏ qua chương 3, vốn giới thiệu các đặc tính của Rust tương tự trong các ngôn
ngữ khác, và nhảy ngay đến chương 4 để học về hệ thống ownership của Rust. Tuy nhiên,
nếu bạn là một người học cẩn trọng muốn học tất cả các chi tiết trước khi chuyển đến
phần kế tiếp, có lẽ bạn sẽ bỏ qua chương 2 và đi thẳng đến chương 3, trở về lại chương 2
khi bạn muốn áp dụng những chi  tiết mà bạn đã học.

Chương 5 thảo luận về struct và method, chương 6 giới thiệu về enum, các biểu thức `match`,
và cấu trúc điều khiển `if let`. Bạn sẽ dùng struct và enum để tạo ra các kiểu tùy biến
trong Rust.

Trong chương 7, bạn sẽ học về hệ thống module của Rust và về các quy tắc riêng tư
khi tổ chức mã nguồn và hệ thống Application Programming Interface (API) của nó.
Chương 8 thảo luận về một số kiểu tập hơn (collection) mà các thư viện chuẩn cung
cấp, như vector, string và hash map. Chương 9 khám phá các triết lý cũng như kỹ thuật của
Rust trong việc xử lý lỗi. 

Chương 10 đào sâu và generic, strait và vòng đời, thứ mang lại sức mạnh để tạo 
ra các mã lệnh có thể dùng được với nhiều kiểu dữ liệu khác nhau. Chương 11 nói hoàn toàn 
về kiểm thử, thứ cần có để đảm bảo logic chương trình của bạn là chính xác, ngay cả khi 
đã có hệ thống an toàn của Rust. Trong chương 12, chúng ta sẽ xây dựng một tập con các 
tính năng từ câu lệnh `grep`, cho phép tìm kiếm các đoạn văn bản bên trong các file.
Để làm điều này, chúng ta sẽ dùng nhiều các khái niệm đã thảo luận trong các chương trước.

Chương 13 khám phá closure và iterator: các tính năng của Rust vốn đến từ các ngôn
ngữ lập trình hàm (functional programming). Trong chương 14, chúng ta sẽ khảo sát lại
Cargo sâu hơn và nói về các phương pháp hay nhất để chia sẻ thư viện của bạn với những người 
khác. Chương 15 thảo luận về các con trỏ thông minh mà các thư viện chuẩn cung cấp, đồng thời
nói về các trait cho phép chúng hoạt động.

Trong chương 16, chúng ta sẽ dạo qua các mô hình khác nhau của lập trình song song và nói 
về cách Rust hỗ trợ bạn viết đa luồng một cách dễ dàng. Chương 17 so sánh một số thành phần trong 
Rust với các khái niệm hướng đối tượng mà có lẽ bạn đã quen thuộc.

Chương 18 là phần tham khảo về mẫu và khớp mẫu, vốn là những cách thức mạnh mẽ để
biểu đạt các ý tưởng trong suốt các chương trình Rust. Chương 19 là một bữa đại tiệc với
nhiều chủ đề nâng cao khác nhau, bao gồm unsafe Rust, macro, và nói thêm về vòng đời, trait,
type, function và closure.

Trong chương 20, chúng ta sẽ hoàn thành một máy chủ web đa luồng cấp thấp.

Cuối cùng, các phụ lục sẽ chứa nhiều thông tin hữu ích về ngôn ngữ theo dạng liệt kê dễ dàng để
tham khảo. Phụ lục A chứa các từ khóa của Rust, phụ lục B chứa các toán từ và ký hiệu, phụ lục C 
chứa các trait có thể dẫn xuất lại được cung cấp bởi thư viện chuẩn, phụ lục D nói về các công cụ
phát triển hữu ích, và phụ lục E nói về các phiên bản của Rust.

Sẽ không có một cách sai để đọc quyển sách này: nếu bạn muốn nhảy qua một vài phần, cứ việc! Bạn 
cũng có thể nhảy ngược lại các chương trước khi gặp phải điều gì khó hiểu. Cứ đơn giản làm cái 
gì bạn cảm thấy hiệu quả.

<span id="ferris"></span>

Một phần quan trọng khi học Rust là học cách đọc các thông báo lỗi mà trình biên dịch hiển thị: 
Chúng sẽ giúp bạn tạo ra các code làm việc được. Do đó, chúng tôi sẽ cung cấp nhiều ví dụ không
thể dịch được cùng với thông báo lỗi trình dịch sẽ hiển thị trong trường hợp tương ứng. Hãy nhớ 
nếu bạn nhập và chạy một ví dụ ngẫu nhiên, nó có thể bị lỗi! Hãy đảm bảo bạn đã đọc những nội dung 
xung quanh để xem liệu code bạn đang thử chạy có tạo ra lỗi hay không. Ferris cũng sẽ giúp bạn phân 
biệt những code nào sẽ không chạy:

| Ferris                                                                                                           | Meaning                                          |
|------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| <img src="img/ferris/does_not_compile.svg" class="ferris-explain" alt="Ferris with a question mark"/>            | Code này không dịch được!                        |
| <img src="img/ferris/panics.svg" class="ferris-explain" alt="Ferris throwing up their hands"/>                   | Code này trông phát ớn!                          |
| <img src="img/ferris/not_desired_behavior.svg" class="ferris-explain" alt="Ferris with one claw up, shrugging"/> | Code này sẽ không tạo ra kết quả mong muốn.      |

Trong hầu hết trường hợp, chúng tôi sẽ dẫn bạn đến phiên bản đúng của bất kỳ code nào không dịch được.

## Mã nguồn

Các file nguồn của để tạo ra cuốn sách này có thể tìm thấy tại
[GitHub][book].

[book]: https://github.com/rust-lang/book/tree/main/src
