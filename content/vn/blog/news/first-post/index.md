---
date: 2018-10-06
title: "Tài liệu dễ dàng với Docsy"
linkTitle: "Thông báo docsy"
description: "Chủ đề Docsy Hugo cho phép người duy trì dự án và người đóng góp tập trung vào nội dung, không phải để phát minh lại cơ sở hạ tầng trang web từ đầu"
author: Riona MacNamara ([@rionam](https://twitter.com/bepsays))
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
  params:
    byline: "Photo: Riona MacNamara / CC-BY-CA"
---

**Đây là một bài đăng trên blog điển hình bao gồm hình ảnh.**

Vật chất phía trước chỉ định ngày của bài đăng trên blog, tiêu đề của nó, một mô tả ngắn sẽ được hiển thị trên trang đích blog và tác giả của nó.

## Bao gồm cả hình ảnh

Đây là một hình ảnh (`featured-sunset-get.png`) điều đó bao gồm một biên giới và một chú thích.
{{< imgproc sunset Fill "600x300" >}}
Tìm nạp và mở rộng một hình ảnh trong Hugo 0,43 sắp tới.
{{< /imgproc >}}

Vấn đề phía trước của bài đăng này chỉ định các thuộc tính được gán cho tất cả các tài nguyên hình ảnh:

```
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
  params:
    byline: "Photo: Riona MacNamara / CC-BY-CA"
```

Để đưa hình ảnh vào một trang, chỉ định chi tiết của nó như sau:

```
{{< imgproc sunset Fill "600x300" >}}
Fetch and scale an image in the upcoming Hugo 0.43.
{{< /imgproc >}}
```

Hình ảnh sẽ được hiển thị ở kích thước và đường dây được chỉ định trong vấn đề phía trước.