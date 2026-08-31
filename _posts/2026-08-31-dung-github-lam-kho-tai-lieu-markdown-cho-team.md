---
layout: post
title: Dùng GitHub làm kho tài liệu Markdown cho team
category: "Tutorials"
tags: [Web, Github, Cloudflare, AI]
date: 2026-08-31
---

Dạo này nhờ có AI mà việc hiện thực hóa ý tưởng nhanh và ít tốn công hơn hẳn. Xuất phát từ nhu cầu cá nhân là muốn có một trang chia sẻ tài liệu kỹ thuật nội bộ cho team với chi phí rẻ nhất có thể, mình đã cùng AI "chốt đơn" một giải pháp khá gọn: **GitHub làm kho lưu trữ và phân quyền, Cloudflare làm nơi host app và lo luôn CI/CD**. Bài viết này ghi lại quá trình tư duy để đi đến thiết kế đó.

## Mục lục
[1. Bài toán](#section1)  
[2. Chọn kho lưu trữ: GitHub](#section2)  
[3. Bài toán xác thực](#section3)  
[4. Hosting và CI/CD với Cloudflare](#section4)  
[5. Thiết kế tổng thể](#section5)  

## 1. Bài toán<a name="section1"></a>

Yêu cầu mình đặt ra ban đầu khá đơn giản:

- Một trang web để team đọc tài liệu kỹ thuật viết bằng Markdown.
- Chỉ người trong team mới truy cập được (tài liệu nội bộ mà).
- Thêm/sửa tài liệu phải dễ, tốt nhất là không phải build/deploy lại.
- Và quan trọng nhất: **càng rẻ càng tốt**, lý tưởng là 0 đồng :D

## 2. Chọn kho lưu trữ: GitHub<a name="section2"></a>

Nghĩ đến "chỗ chứa Markdown miễn phí" thì mình nhớ ngay tới GitHub, vì trước đây đã từng [tạo blog cá nhân với GitHub Pages](/2019-10-24-tao-blog-voi-jekyll-p1/). Kho lưu trữ tài liệu thì chắc cũng không khác gì mấy: mỗi tài liệu là một file `.md` trong repo, ai muốn thêm bài thì cứ push lên là xong.

Vậy coi như chốt được phần lưu trữ. Nhưng đến phần hiển thị thì bài toán bắt đầu khác đi: blog thì đại chúng, ai vào đọc cũng được, còn tài liệu nội bộ thì phải giới hạn người truy cập — mà **GitHub Pages lại không hỗ trợ authentication**. Nghĩa là không thể render trực tiếp bằng GitHub Pages như blog, mà cần một con app riêng đứng giữa: app này render Markdown và có lớp auth chặn trước.

## 3. Bài toán xác thực<a name="section3"></a>

Có app rồi thì gắn auth nào cũng được, nên câu hỏi tiếp theo là chọn nhà cung cấp nào. Ban đầu mình phân vân với **Azure AD** vì đã quen dùng và tích hợp cũng đơn giản. Nhưng nghĩ kỹ lại thì hơi "cấn": user đăng nhập bằng Azure AD, còn tài liệu lại nằm trên GitHub, vậy phải tự quản lý thêm một lớp phân quyền ở giữa — ai được đọc repo nào, quản lý danh sách đó ở đâu — khá lằng nhằng.

May sao GitHub có sẵn **GitHub OAuth**, và điểm hay nhất là phân quyền nằm luôn trên GitHub: ai đọc được repo tài liệu thì người đó được xem trang, muốn cấp quyền cho thành viên mới chỉ cần add collaborator vào repo. Không cần database user, không cần allow-list riêng. Team kỹ thuật thì chắc chắn ai cũng có tài khoản GitHub rồi, nên chốt GitHub OAuth là lợi nhất.

## 4. Hosting và CI/CD với Cloudflare<a name="section4"></a>

Còn lại là tìm chỗ host con app render. Sau một hồi tìm hiểu thì AI đề xuất **Cloudflare Pages** — gói free đủ dùng, hỗ trợ chạy serverless functions cho phần OAuth, vậy là chốt hạ tầng.

Đến đoạn CI/CD, mình đang lọ mọ cấu hình GitHub Actions thì AI lại gợi ý tiếp: Cloudflare Pages có sẵn cơ chế **Git integration**, chỉ cần cấp quyền vào repo là mỗi lần push code nó tự pull về, build và deploy luôn, khỏi viết pipeline. Thơm quá, vậy cho Cloudflare ôm luôn phần CI/CD :D

## 5. Thiết kế tổng thể<a name="section5"></a>

Tổng kết lại, ý tưởng thiết kế gồm hai mảnh ghép chính:

- **GitHub**: vừa là kho lưu trữ tài liệu Markdown, vừa là nơi xác thực và phân quyền (quyền đọc repo chính là quyền xem tài liệu).
- **Cloudflare Pages**: host con app render và tự động CI/CD mỗi khi push code.

Con app thì đơn giản thôi — một markdown renderer có lớp GitHub OAuth chặn trước, đọc tài liệu trực tiếp từ repo qua API nên **file `.md` vừa push lên là thấy ngay trên trang, không cần build lại**. Sơ đồ thiết kế như bên dưới, còn code của app thì... AI code hộ :D

![Sơ đồ thiết kế](/img/md-reader-hld.svg)

Với cách làm này, chi phí vận hành đúng nghĩa là 0 đồng: không server, không database, chỉ có repo GitHub và một project Cloudflare Pages free. Nếu bạn cũng đang cần một trang tài liệu nội bộ gọn nhẹ cho team thì hy vọng bài viết gợi được chút ý tưởng, có gì cứ comment bên dưới để chúng ta cùng thảo luận nhé!

*Bài viết này có sự hỗ trợ từ AI.*
