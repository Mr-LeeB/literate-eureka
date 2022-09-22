---
title: "Hướng dẫn cài đặt môi trường sử dụng laradock"
linkTitle: "Cài đặt môi trường sử dụng laradock"
date: 2022-08-18
description: >
  Hướng dẫn cài đặt môi trường sử dụng laradock
---
## **Download file cấu hình**
Tạo thư mục chứa file cấu hình:
```
    mkdir data
    cd data
    mkdir working
    cd working
    git clone https://github.com/laradock/laradock
```

## **Chỉnh sửa file cấu hình**
Sau đó vào thư mục laradock vừa clone xong. Copy file .env sau đó chỉnh sửa các nội dung sau:
```
    cp .env.example .env
    nano .env
```

```
APP_CODE_PATH_HOST=../ ==> APP_CODE_PATH_HOST=../www
	==> Giả sử em làm việc trong thư mục /data/working. Thì cấu trúc sẽ là:
		- /data/working/laradock ==> đứng ở đây để chạy các lệnh docker-compose
		- /data/working/www là thư mục chứa source web sẽ được mount vào trong thư mục /var/www của nginx và workspace

DATA_PATH_HOST=~/.laradock/data ==> Ghi nhớ thư mục này để mai mốt biết ở đâu mà tìm để backup

PHP_VERSION=7.4 ==> Mình đang xài php 7.4 nên giữ nguyên. Nếu em muốn test php 8.1, 8.2 gì đó thì sửa lại.

WORKSPACE_INSTALL_MYSQL_CLIENT=false ==> WORKSPACE_INSTALL_MYSQL_CLIENT=true
WORKSPACE_INSTALL_PING=false ==> WORKSPACE_INSTALL_PING=true


PHP_FPM_INSTALL_MYSQL_CLIENT=false ==> PHP_FPM_INSTALL_MYSQL_CLIENT=true
PHP_FPM_INSTALL_PING=false ==> PHP_FPM_INSTALL_PING=true

PHP_WORKER_INSTALL_MYSQL_CLIENT=false ==> true
	==> Cái này chưa xài tới nhưng để sẵn.

NGINX_HOST_HTTP_PORT=80
NGINX_HOST_HTTPS_PORT=443
	--> 2 cái này để mặc định cho tiện. Nếu sợ tranh chấp port thì đổi thành port khác.
	
MYSQL_VERSION=latest ==> MYSQL_VERSION=8.0.27 để tránh breaking change khi thay đổi version

MYSQL_USER=default
MYSQL_PASSWORD=secret
	==> User quyền thường, đổi mật khẩu theo ý của em.
	
MYSQL_ROOT_PASSWORD=root
	==> mật khẩu user root. Đổi lại theo ý của em
```

Sửa xong rồi thì start nó lên bằng lệnh: `docker-compose up -d nginx mysql`
Chú ý là phải tắt hết **nginx, apache, php**... trên máy em để không bị tranh chấp port.
