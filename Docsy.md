---
title: "Cài đặt Docsy"
linkTitle: "Cài đặt Docsy"
date: 2022-07-18
description: "Cách cài đặt docsy trên window"
---
## **1.	Cài đặt.**
### **a.	Download Chocolatey.**
-	Mở PowerShell với quyền quản trị viên.
-	Run **Get-ExecutionPolicy**. If it returns **Restricted**, then run **Set-ExecutionPolicy AllSigned** or **Set-ExecutionPolicy Bypass -Scope Process**.
-	Sau đó chạy lệnh:
```go
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

### **b.	Install Hugo**
-	Mở PowerShell với quyền quản trị viên sau đó chạy lệnh sau: 
```
    choco install hugo-extended -confirm
```

### **c.	Install Git**
-	Vào link: https://git-scm.com/downloads 
-	Chọn Tải xuống Git phiên bản Windows > Mở file Git chọn Run > Chọn Next > Chọn Browse, chọn nơi cài đặt ấn Next > Chọn Next > Chọn vị trí lưu trên Start Menu, lần lượt chọn Next > Chọn Install > Nhấn Finish.

### **d.	Install ‘go’**
-	Click vào link: https://go.dev/dl/ , Sau đó cài đặt phiên bản phù hợp.

## **2.	Kiểm tra.**
### **a.	Tải Docsy Example Project**
-	Sau khi cài đặt Hugo, click link: https://github.com/google/docsy-example
-	Nhấp vào Use this template. 
-	Chọn tên cho dự án mới của bạn và nhấp vào Create repository from template.
-	Tạo bản sao làm việc cục bộ của riêng bạn cho repo mới của bạn bằng cách sử dụng git clone, thay thế https://github.com/me/example.git bằng URL web repo của bạn: 
```
    git clone --depth 1 https://github.com/me/example.git
```

### **b.	Kiểm tra**
-	Mở dự án bằng Visual Studio Code sau đó chạy lệnh sau trong terminal: hugo server
-	Mở trình duyệt web và nhập: http://localhost:1313/

## **3.	Viết, chỉnh sửa bài viết theo yêu cầu.**
### **Thiết lập ngôn ngữ tiếng Việt**
-	Tạo thư mục ‘vn’ trong content
-	Copy các thư mục trong ‘en’ dán vào ‘vn’. Dịch các nội dung cần thiết sang tiếng Việt.
-	Trong file config.toml, tạo đường dẫn tới file ‘vn’:
```
    [languages.vn] 
    title = "Goldydocs"
    description = "Docsy er operativsystem for skyen"
    languageName ="Vietnames"
    contentDir = "content/vn"
    time_format_default = "02.01.2006"
    time_format_blog = "02.01.2006"
```
-	Mở trình duyệt web để kiểm tra kết quả: http://localhost:1313/ 
