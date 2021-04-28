---
layout: post
title: Hosting Nodejs trên Windows với IIS reverse proxy
category: "Tutorials"
tags: [Web, Tutorials]
date: 2019-11-17
---

![mevn](https://vlheww.by.files.1drv.com/y4mZ2nuEu4rL2dQU6_P1S53KPT2g1dO_-QGIViYGIPLgyEqQm--0Qa35hageMthH6oyZRZ5qVwP1sFfHGQVsIv3S2up8CTqWvp3ICJ8JgFujgsmVPfvOs-7y3Zqrh5b9yxkmYqDblVrf17Z-cUlnuBOz4p5X7USkVWjsRUnGLMjmZBnsrA9YRo4Xk9f18wsE0j86WkQKhTM_prpzKLOk2Lriw?width=1193&height=533&cropmode=none)  
Mừng các bạn đã quay trở lại với series Tạo Sticky note với MEVN stack. Ở bài trước, chúng ta đã làm xong bộ khung của ứng dụng Sticky note gồm có frontend,backend và axios để kết nối chúng lại với nhau. Hôm nay chúng ta tiếp tục hoàn thiện Sticky note với các bước sau:

## Mục lục

[1. Tạo project Nodejs](#section1)  
[2. Cấu hình reverse proxy cho IIS](#section2)
[3. Cài pm2 để Nodejs chạy production](#section3) 

## 1. Tạo project Nodejs<a name="section1"></a>

Tạo project Nodejs với nội dung như sau:

```javascript
// gọi các thư viện (import dependencies)
const express = require("express");
const app = express(); // khởi tạo express (create your express app)
app.get('/', function (req, res) {
  res.send('Hello World!');
});
app.listen(3000, function () {
  console.log('Example app listening on port 3000!');// backend sẽ chạy ở port 3000 (http://localhost:3000/)
});
```

## 2. Cấu hình reverse proxy cho IIS<a name="section2"></a>

Để cấu hình reverse proxy cho IIS chúng ta cần cài các phần mở rộng [URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) và [Application Request Routing](https://www.iis.net/downloads/microsoft/application-request-routing) cho IIS.
Sau khi cài xong URL Rewrite và Application Request Routing chúng ta dùng tổ hợp phím Win + R sau đó chạy lệnh:

```bash
inetmgr
```

Sau khi Internet Information Services (IIS) Manager hiện lên, chúng ta chọn trang web cần tạo proxy


![connection string](https://vlfrqg.by.files.1drv.com/y4mm3U6VxUkYHVRQhqzbFD6aq5_ZQs5QtR9amxGwbwbv1u5F8OzQqvrwQlD3A7ilvIn-wl-m4RfxsjVGgXb-bVUD-sSkK1TgquNTL9BMvvWawgN6x55qAhyf068oLMGnuWYax7oC0SF9F4nb8UcokpRpA3Yh7CBxfJkgOBG-_9ttcr18lBNBTLads9o4HOaI3nSJ6Xp8G4kfZXYXsPQKQxqZQ?width=644&height=358&cropmode=none)  
Trở lại file **server/src/app.js** dán chuỗi kết nối vừa copy vào vị trí của YOUR_CONNECTION_STRING là xong bước thiết lập kết nối MongoDB.

## 3. Tạo bảng (Collection) với MongoDB<a name="section3"></a>



Bài viết được lược dịch và sử dụng tài nguyên từ các bài viết:  
[Hosting a Node.js application on Windows with IIS as reverse proxy](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)  