# Sử dụng Struct để cấu trúc dữ liệu liên quan

*struct* hoặc *structure*, là một kiểu dữ liệu tùy chỉnh cho phép bạn đóng gói
cùng nhau và đặt tên cho nhiều giá trị có liên quan tạo nên một nhóm có ý nghĩa. Nếu như
bạn đã quen thuộc với một ngôn ngữ hướng đối tượng, *struct* giống như một
thuộc tính dữ liệu của đối tượng. Trong chương này, chúng ta sẽ so sánh và đối chiếu các tuple
với struct để xây dựng dựa trên những gì bạn đã biết và minh họa khi nào các struct
được sử dụng tốt hơn để nhóm dữ liệu.

Chúng ta sẽ trình bày cách định nghĩa và khởi tạo struct. Chúng ta sẽ thảo luận làm thế nào để
xác định các function liên quan, đặc biệt là các function được gọi là
*method*, để chỉ định hành vi được liên kết với kiểu struct. Struct và enums
(được thảo luận trong Chương 6) là các khối xây dựng để tạo các kiểu dữ liệu mới trong
chương trình để tận dụng tối đa khả năng kiểm tra kiểu trong thời gian biên dịch của Rust.