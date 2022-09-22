---
title: "Hướng dẫn sử dụng Container"
linkTitle: "Container"
date: 2022-07-18
description: "Hướng dẫn cài đặt Container"
---

## **1.	Cài đặt container trên máy ảo linux.**
### **a.	Yêu cầu hệ điều hành.**
-	Để cài đặt Docker Engine, bạn cần phiên bản 64-bit của một trong các phiên bản Ubuntu sau:
    +	Ubuntu Jammy 22.04 (LTS)
    +	Ubuntu Impish 21.10
    +	Ubuntu Focal 20.04 (LTS)
    +	Ubuntu Bionic 18.04 (LTS)

### **b.	Gỡ cài đặt các phiên bản cũ.**
```
    $ sudo apt-get remove docker docker-engine docker.io containerd runc
```

### **c.	Thiết lập kho lưu trữ.**
-	Cập nhật apt chỉ mục gói và cài đặt các gói để cho phép apt sử dụng kho lưu trữ qua HTTPS:
```
$ sudo apt-get update
$ sudo apt-get install \ 
    ca-certificates \ 
    curl \ 
    gnupg \ 
    lsb-release
```
-	Thêm khóa GPG chính thức của Docker:
```go
$ sudo mkdir -p /etc/apt/keyrings 
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
-	Sử dụng lệnh sau để thiết lập kho lưu trữ:
```go
    echo \ 
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \ 
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### **d.	Cài đặt Docker Engine.**
-	Cập nhật aptchỉ mục gói và cài đặt phiên bản mới nhất của Docker Engine, containerd và Docker Compose hoặc chuyển sang bước tiếp theo để cài đặt một phiên bản cụ thể:
```go
    $ sudo apt-get update 
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
-	_**Node:**_ Nhận lỗi GPG khi chạy apt-get update? Umask mặc định của bạn có thể không được đặt chính xác, khiến tệp khóa công khai cho repo không được phát hiện. Chạy lệnh sau và sau đó thử cập nhật lại repo của bạn
```
    $ sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
-	Để cài đặt một phiên bản cụ thể của Docker Engine, hãy liệt kê các phiên bản có sẵn trong repo, sau đó chọn và cài đặt. Liệt kê các phiên bản có sẵn trong repo của bạn:
```
    $ apt-cache madison docker-ce
```
-	Ví dụ: cài đặt một phiên bản cụ thể bằng cách sử dụng chuỗi phiên bản từ cột thứ hai 5:20.10.16~3-0~ubuntu-jammy.
```go
    $ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io docker-compose-plugin
```
-	Xác minh rằng Docker Engine được cài đặt chính xác bằng cách chạy hello-world hình ảnh.
```
    $ sudo docker run hello-world
```
-	Nếu nó không hoạt động, có thể bạn chưa khởi động docker
( Usage: service docker {start|stop|restart|status} )
```
    $ sudo service docker start
```
or
```
    $ export DOCKER_HOST=unix:///var/run/docker.sock
```

## **2.	Cấu hình docker chạy với user thường.**
### **a.	Tạo group docker:** 
    $ sudo groupadd docker

### **b.	Thêm user:** 
    $ sudo usermod -aG docker $USER

### **c.	Đăng xuất và đăng nhập lại để thành viên nhóm của bạn được đánh giá lại. Trên Linux, bạn cũng có thể chạy lệnh sau để kích hoạt các thay đổi đối với nhóm:**
    $ newgrp docker

### **d.	Xác minh rằng bạn có thể chạy dockercác lệnh mà không cần sudo:**
    $ docker run hello-world

## **3.	Tương tác với container.**
### **a.	Các lệnh chính:**
-	create : Tạo container từ một image.
-	start : Khởi động một container hiện có.
-	run : Tạo một  container mới và khởi chạy nó.
-	ls : Liệt kê các container đang chạy.
-	inspect : Thông tin chi tiết về container.
-	logs : Hiển thị nhật ký thực thi của container.
-	stop : Dừng chạy container một cách tinh tế.
-	kill : Dừng quá trình chính trong container một cách đột ngột.
-	rm : Xóa một container đã dừng.

### **b.	Xem các container đang chạy:**
    $ docker container ls

### **c.	Xem các container đã chạy và ngừng lại:**
    $ docker container ls -a

## **4.	Map một thư mục ở ngoài vào container**
### **a.	Chia sẻ dữ liệu giữa máy host và container**
-	Tạo thư mục ở máy host, ví dụ:
```
        $ mkdir Desktop
        $ cd Desktop
        $ mkdir data
        $ cd data
        $ touch hello.txt
        $ pwd
```
-	Lấy đường dẫn đến thư mục data.
-	Ta sẽ tạo một container từ image ubuntu và nhận dữ liệu chia sẻ là thư mục data của máy host bằng lệnh: 
```
docker run -it -v <đường dẫn đến thư mục data>:/home/data_container ubuntu
```
-	_**Node:**_
    +	Để chia sẻ dữ liệu giữa máy host và container thì ta thêm vào tham số -v.
    +	Để container có thể nhận tương tác và có kết nối với terminal thì thêm tham số -it.
    +   /home/data_container là đường dẫn đến thư mục chứa dữ liệu trên container.
    +	ubuntu là image ubuntu dùng để tạo ra container.
    +	Sau khi chạy lệnh trên ta đã tạo ra 1 container từ image ubuntu và đang đứng trong terminal của container đó, cd vào thư mục home sẽ thấy thư mục data_container và có file hello.txt => dữ liệu đã được chia sẻ từ máy host vào container.
    +	Dữ liệu giữa 2 thư mục này được map với nhau, nếu container lưu dữ liệu vào thư mục này thì cũng lưu trên host, khi xóa container thì dữ liệu vẫn được lưu trên máy host.

### **b.	Chia sẻ dữ liệu giữa 2 container**
_**Bài toán:**_ ta có thư mục data ở máy host và muốn chia sẻ thư mục này cho container_1 sử dụng, đồng thời ta cũng chia sẻ thư mục này cho container_2 sử dụng.

Ở đây ta tiếp tục sự dụng phần vừa thực hành trên, ta đã có thư mục data ở máy host và đã chia sẻ cho 1 container như trên.

Tiếp theo ta sẽ tạo một container nữa và được chia sẻ dữ liệu từ container cũ 
Vẫn ở trong container cũ ta nhấn tổ hợp phím _Ctrl + p_ và _Ctrl + q_ để thoát ra ngoài container nhưng vẫn chạy container đó.

Chạy lệnh **_docker ps_** sẽ thấy container đó vẫn đang chạy.

Giờ ta sẽ tạo một container mới được chia sẻ dữ liệu từ thư mục data_container của container tạo trước đó bằng lệnh:
```
    $ docker run -it --name C2 --volumes-from <container-ID> ubuntu
```
Lệnh trên ta tạo container có tên là C2 từ image ubuntu và được chia sẻ dữ liệu từ container có Id là ID của container tạo trước đó

Chạy xong lệnh trên thì ta đang đứng trong container C2 vừa tạo, cd vào thư mục home sẽ thấy thư mục data_container được chia sẻ

## **5.	Copy file vào khi container đang chạy**
### **a.	Cú pháp:**
```
	docker cp <container_id>:/path/file /path/host 
	docker cp /path/host <container_id>:/path/file
```

### **b.	Ví dụ:**
-	Khởi động một container nginx đơn giản lên: 
```
    $ docker run -id nginx:latest
    $ docker ps
```
-	Copy file cấu hình /etc/nginx/nginx.conf trong container ra ngoài local:
```
    docker cp container-ID:/etc/nginx/nginx.conf 
    ls -la nginx.conf
```
-	Copy một file text linh tinh vào trong Docker Container Nginx đang chạy: 

```
    touch example-text.txt
    docker cp example-text.txt container-ID:/etc/nginx/
```
-	Chạy lệnh liệt kê thư mục /etc/nginx trong Docker Container Nginx thử xem có file example-text.txt không: 
```
    docker exec -it <container-ID> ls /etc/nginx
```
