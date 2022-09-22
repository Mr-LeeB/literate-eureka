---
title: "Cài đặt Docker"
linkTitle: "Cài đặt Docker"
date: 2022-07-18
description: "Cách cài đặt Docker Desktop"
---
## **1.	Cài đặt.**
### **a.	Cài đặt Docker desktop**
-	Tải [Docker Desktop](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe) và cài đặt.

### **b.	Cài đặt WSL 2:**
####	Khởi động Windows Subsystem cho Linux
-	Mở PowerShell với quyền quản trị viên sau đó chạy lệnh sau: 
```go
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

####	Kiểm tra các yêu cầu để chạy WSL 2
-	Để cập nhật lên WSL 2, bạn phải chạy Windows 10 ... 
    +	Đối với hệ thống x64: Phiên bản 1903 trở lên, với Build 18362 trở lên. 
    +	Đối với hệ thống ARM64: Phiên bản 2004 trở lên, với Build 19041 trở lên. 
-	Hoặc Windows 11.

####	Khởi động tính năng máy ảo
-	Mở PowerShell với quyền quản trị viên và chạy lệnh sau: 
```go
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

####	Tải Linux kernel
-	Tải [Linux kernel](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi).
-	Chạy gói vừa tải xuống và cài đặt

####	Đặt WSL 2 làm phiên bản mặc định
-	Trong PowerShell, thi hành lệnh: 
>wsl --set-default-version 2

####	Cài đặt bản phân phối Linux
-	Trong Microsoft Store, cài đặt bản phân phối Linux bất kì, VD: **Ubuntu 18.04 LTS**
-	Tạo tài khoản người dùng trong bản phân phối Linux.

### **c.	Cài đặt Windows Terminal:**
-	 Nên cài đặt Windows Terminal từ Microsoft Store để dễ dàng thao tác và sử dụng các bản phân phối Linux.


## **2.	Docker compose.**
### **a.	Thiết lập**
-	Tạo một thư mục: 
    +	mkdir composetest 
    +	cd composetest
-	Tạo một file app.py:
```go
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
``` 
-	Tạo một file requirements.txt:
```
    flask
    redis
```

### **b.	Tạo Dockerfile**
-	Tạo một file tên là ‘Dockerfile’
```go # syntax=docker/dockerfile:1
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```
-	Điều này yêu cầu Docker: 
    +	Xây dựng một hình ảnh bắt đầu bằng Python 3.7 image. 
    +	Đặt thư mục làm việc thành /code. 
    +	Đặt các biến môi trường được sử dụng bởi flask command. 
    +	Cài đặt gcc và các phụ thuộc khác 
    +	Sao chép requirements.txt và cài đặt các phụ thuộc Python. 
    +	Thêm siêu dữ liệu vào hình ảnh để mô tả rằng container đang lắng nghe trên cổng 5000 
    +	Sao chép thư mục hiện tại . trong dự án vào workdir . trong image.
    +	Đặt lệnh mặc định cho container thành flask run.

### **c.	Compose file**
-	Tạo một file docker-compose.yml
```go
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

### **d.	Build và run với docker compose**
-	Chạy lệnh: docker compose up
-	Nhập http: // localhost: 8000 / vào trình duyệt.
-	Trình duyệt sẽ hiển thị một thông báo: Hello World! I have been seen 1 times.
-	Làm mới trang thì số lượng sẽ tăng lên.
-	Dừng ứng dụng bằng cách chạy: docker compose down
