---
title: "Hướng dẫn cài đặt MySQL"

date: 2022-08-18
description: >
  Hướng dẫn cài đặt và bảo mật MySQL trên Windows
---
## **Cài đặt MySQL**
MySQL là một hệ thống quản lý cơ sở dữ liệu mã nguồn mở phổ biến trên thế giới. Bạn có thể dễ dàng cài đặt và bảo mật MySQL trên Windows, CentOS, Ubuntu.

Nếu bạn đang có một trang web được viết bằng ngôn ngữ PHP, chắc chắn bạn không còn xa lạ gì với cái được gọi là MySQL. Bài viết này sẽ hướng dẫn bạn cài đặt và bảo mật MySQL trên Windows 7, 8, 10.

Có 2 cách để bạn có thể cài đặt MySQL lên một máy tính chạy hệ điều hành Windows đó là cài đặt bằng gói MySQL Installer MSI và từ source ZIP Archive.

## **Cài đặt MySQL từ source**
Mục đích của việc cài bản này là để bạn có thể dễ dàng quản lý với các source khác như Nginx, Apache và PHP. Nếu như bạn đã cài Nginx hoặc Apache và PHP từ source thì việc cài MySQL từ source là hợp lý nhất.

Đầu tiên bạn cần tạo một thư mục để lưu trữ tất cả source của Nginx, Apache và PHP ở một nơi, đương nhiên là MySQL cũng ở đây luôn. Ở đây mình sẽ tạo thư mục **Web** trên ổ đĩa **D** nhé.

- Tải bản [MySQL ZIP Archive](https://dev.mysql.com/downloads/mysql/) cho 64 bit hoặc 32 bit phù hợp với máy tính của bạn (Nên cài bản **MySQL5.6**).
- Sau khi tải về, bạn giải nén sẽ được một thư mục như **mysql-xxx**. Đổi tên thư mục này thành **mysql** cho nó đẹp rồi copy vào thư mục **D:\Web** vừa tạo ở trên.
- Trong thư mục **D:\Web\mysql** có một file là my-default.ini, bạn nên đổi lại thành __my.ini__.

Bây giờ bạn cần đi đến thư mục bin trong D:\Web\mysql để cài đặt MySQL, bao gồm cả việc thực hiện một số lệnh bảo mật.

Mở **Command Prompt** hoặc run **cmd** (Run as administrator):
>cd /d D:\Web\mysql\bin

Ở đây mình sẽ hướng dẫn bạn cài đặt MySQL qua 2 lệnh `mysqld` và `mysql_install_db`.

Cài đặt MySQL bằng lệnh `mysqld`:
```
mysqld --install MySQL5.6 --defaults-file=D:\Web\mysql\my.ini
```
Nếu bạn thấy “**Service successfully installed.**“ là bạn đã cài MySQL thành công rồi.

Với dòng lệnh trên, MySQL sẽ cài đặt một Service với tên là **MySQL5.6** và với file cấu hình là **D:\Web\mysql\my.ini**.

Hoặc bạn có thể cài đặt MySQL bằng lệnh **mysql_install_db**:
```
mysql_install_db.exe --datadir=D:\Web\Database --service=MySQL5.6 --password=mat-khau-cua-tui --socket=MySQL
```
Khi sử dụng lệnh **mysql_install_db**, bạn có rất nhiều tùy chọn như thiết lập mật khẩu, tên Services. Quan trọng là bạn sẽ tạo được tên socket để có thể sử dụng PHP để kết nối đến MySQL thông qua gói mở rộng `php_mysqli.dll`. Bạn cũng có thể chỉ định thư mục chứ các tập tin cơ sở dữ liệu của bạn ở bất cứ đâu mà bạn muốn.

Bạn có thể dễ dàng khởi động MySQL, hoặc restart, hoặc stop MySQL bằng cách start, restart hay stop service này trong **Control Panel > Administrative Tools > Services**. Service MySQL5.6 này cũng có thể dễ dàng start hoặc stop bằng lệnh, mở **Command Prompt** hoặc **run** cmd (Run as administrator):
```
net start MySQL5.6
net stop MySQL5.6
```
Mặc định thì MySQL cài đặt service này sẽ tự động start cùng với máy tính. Nếu bạn muốn khởi động MySQL thủ công, nhấp đôi chuột vào `MySQL5.6` trong **Services** và chọn **Manual** tại mục **Startup type**.

Vậy là bạn đã cài đặt MySQL xong và bước tiếp theo là cần thực hiện một số việc để bảo mật MySQL.

## **Bảo mật MySQL trên Windows**
Để bảo mật cho MySQL, theo khuyến nghị từ MySQL thì bạn nên thiết lập mật khẩu cho user root, loại bỏ anonymous users, test database.

Trong **D:\Web\mysql\bin** có file **mysql_secure_installation.pl**, bạn có thể bảo mật bằng cách chạy file này bằng Perl. Nếu bạn không muốn cài đặt Perl vào máy, bạn có thể sử dụng các lệnh dưới đây để bảo mật cho MySQL cũng được.

Bạn cần khởi động MySQL và sử dụng lệnh sau để login vào MySQL với user root:
>mysql -u root -p

Nó sẽ hiện “**Enter password:**” đòi bạn nhập mật khẩu để login vào root, nhưng bạn chưa có nên Enter luôn là được.

### **Để thiết lập mật khẩu cho root**
Có nhiều cách để thiết lập mật khẩu cho user root của MySQL, ở đây mình sẽ dùng lệnh SET PASSWORD
```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('matkhau-moi');
SET PASSWORD FOR 'root'@'127.0.0.1' = PASSWORD('matkhau-moi');
SET PASSWORD FOR 'root'@'::1' = PASSWORD('matkhau-moi');
```

Nhớ đổi lại `matkhau-moi` theo ý của bạn nhé. Bạn có thể kiểm tra mật khẩu đã được thiết lập cho user root của MySQL chưa bằng cách exit và restart MySQL và đăng nhập lại.
```
exit
net stop MySQL5.6
net start MySQL5.6
mysql -u root -p
```
Nhập mật khẩu vừa thiết lập vào mục “**Enter password:**” và enter, nếu bạn login được như sau là bạn đã thiết lập mật khẩu thành công:
```
mysql: Unknown OS character set ‘cp1258’.
mysql: Switching to the default character set ‘latin1’.
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 5.6.26 MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type ‘help;’ or ‘\h’ for help. Type ‘\c’ to clear the current input statement.

mysql>
```

### **Loại bỏ các user ẩn danh (anonymous users)**
Bạn có thể thiết lập mật khẩu cho các user ẩn danh, nhưng mình nghĩ các user này cũng chẳng để làm gì. Nên loại bỏ hết ra cho nó an toàn.

Để loại bỏ các user ẩn danh (anonymous users), sử dụng lệnh sau:
>DELETE FROM mysql.user WHERE User='';

Nếu bạn thấy “**Query OK, 1 row affected (0.00 sec)**” thì đã có 1 user bị loại bỏ.

### **Loại bỏ các database thử nghiệm**
Mặc định thì MySQL có các database để thử nghiệm, các database này có tên bắt đầu là “test”. Bạn có thể dễ dàng loại bỏ các database này với lệnh sau:
```
DELETE FROM mysql.db WHERE Db LIKE 'test%';
DROP DATABASE test;
FLUSH PRIVILEGES;
```
Nếu bạn thấy “**Query OK, 2 rows affected (0.00 sec)**” thì đã có 2 cái bị loại bỏ.

Vậy là bạn đã hoàn thành việc cài đặt và bảo mật MySQL trên Windows.