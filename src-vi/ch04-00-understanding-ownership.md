# Tìm hiểu về Ownership (tính sở hữu)

Ownership là tính năng độc đáo nhất có trong Rust và có ảnh hưởng sâu rộng đến
những phần khác của ngôn ngữ. Nó cho phép Rust đảm bảo an toàn khi truy cập vào 
bộ nhớ mà không cần đến trình dọn rác, do vậy việc hiểu rõ về ownership là rất
quan trọng. Trong chương này, chúng ta sẽ nói về ownership cũng như một vài đặc 
tính liên quan: borrowing, slide, và cách Rust sắp xếp dữ liệu trong bộ nhớ.
