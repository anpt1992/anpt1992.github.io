---
layout: post
title: Tạo blog với Jekyll(P2)
category: "Tutorials"
tags: [Web, Tutorials]
date: 2019-10-24
comments: true
---

Ở [phần một](https://anpt1992.github.io/) các bạn đã biết được cách tạo ra 1 trang web tĩnh mà cụ thể là trang blog. Ở phần hai này chúng ta sẽ cùng tìm hiểu về những thiết lập nâng cao hơn.

## Gắn khung comments

Đối với blog, việc tương tác giữa người đọc và người viết là vô cùng quan trọng. Sẽ có người đồng tình với quan điểm, cũng có một số người khác không đồng tình và một số người khác nữa thì có những thắc mắc, chính điều này làm cho bài viết có ý nghĩa hơn. Wordpress làm rất tốt điều này nhưng lúc mới tại blog từ Jekyll thì mình không thấy có khung comments. Vậy có cách nào gắn khung comment vào Jekyll không?  
**Có chứ**, để gắn khung comments các bạn cần có tài khoản Disqus, bạn nào chưa có thì tạo tài khoản tại [đây](https://disqus.com/profile/login/) bạn có thể điền thông tin vào để tạo tài khoản hoặc đăng nhập từ Facebook, Twitter, Google. Ở đây mình đăng nhập bằng Facebook.  
![login](https://v1eurq.by.files.1drv.com/y4mJNC3A0xBDB5-brQzv7wdZ53kHFLTlTVzl-D1EAQLyRZtv-jnUNH8RxTWP7br0ML7NC9Z0v7xu30_FWJNSbzArZ0CG5UTfSOAtn8MNGU8dd6kUOnkAJYrYNBsCxp_BNLQF4lUjztvBBthmk0uoct24igEDU-m8C1DMtxC6ahmWbwPET3SVA1a5Qp6YeGvs0QovnW0rg3d4OYt190ERTotvw?width=399&height=535&cropmode=none){: .center-block :}  
Sau khi đăng nhập xong các bạn vào trang [Admin](https://disqus.com/admin/) chọn **New** để đăng ký blog với Disqus.  
![new](https://xfeurq.by.files.1drv.com/y4mXqFInaheV1COVei_-vyib-w7lKhsTowHSlTVs8ll_iNxROFVJda5D_6jhADNuNoMDkVjM04FfEIU9pUGcR_3TjZRt9Bax8DaRhQ0bcRyh2_qZjrr_aPZwDXT92MBPJMRBGk9U-seRZzrc_spLwsHH-rcoOEIE4jC1nSWRnI-hTlpQJFFYDU2swKyphhePiuT7C9Q6wQ4CZDf5zaChY8B5g?width=733&height=155&cropmode=none){: .center-block :}  
Kế tiếp các bạn điền Tên blog vào mục **Website Name** và để ý dòng chữ nhỏ bên dưới có dạng xxxxxx.disqus.com với xxxxxx là shortcode. Shortcode sẽ là mã nhận diện của trang blog với disqus nên các bạn phải **ghi nhớ**. Các bạn cũng có thể thay đổi dc shortcode bằng cách chọn vào **Customize Your URL** và sửa lại Shortname.  
![shortcode](https://vleurq.by.files.1drv.com/y4mbt70c9TuCdUJt2i7Uip8En2kp2ReOCfkExa28EEB40Tj-tXFtfpHaBJxiCglVCkUdfUam9xXAQmWH886DmCTumNdL5Z-ZLwc2OcYZV0g4c5jlI7JqQsuUuCBKxJSqoJd0RzcEnJIUWiaNwMeve3L4xT8iHtspGaZVJGCBCOsDBm7E-ReIJuOa687nQMi16MCE82f--oKNrMigDPvJqnzKg?width=665&height=609&cropmode=none){: .center-block :}  
Sau khi tạo xong, các bạn vào file **\_config.yml** trong mã nguồn của Blog, uncomment và sửa dòng disqus thành disqus: "shortcode" với shortcode là shortcode của bạn như hình sau và lưu lại.  
![config](https://xveurq.by.files.1drv.com/y4mcg4PR6EcVt1URUh3KBmYGfV-QMd7RB4N9ZUaCj5nV0EfpvcLW4hkwCijxkSTAVp8GkIX3RPZibB0VMGBsDUbovZqiahvCRoSGEC0CvN1z_TRavHnDLPz3i4QvD8gWo04wmK2PDTMZ7wz8R80VIXLC4-hdSZKYVa0GePEVm8Whrm_rID9Nu-hj80c_w723bLCjtU2fUh4QI8am0QGKncJsQ?width=743&height=213&cropmode=none){: .center-block :}
Cuối cùng, các bạn vào một bài viết thêm vào tham số **comments:true** như bên dưới rồi lưu bài viết.  
![comment](https://w1eurq.by.files.1drv.com/y4mzKnyAdZ_YqhfZfOZXF4AjSwyJnP4hwg2Z-rW26DfxTU7iwEgMkgIbkSEZrxjCnAvb89tu3SkAoMEE0Ohn89Y8SH0ZCrVL7HcZc6dpedtktWxp2Yob4EtQpFErim-oJJN0AP-o5NfCkgsXDJt45VwZGSGcfTzx-_lK3LpYG9HXB5OLUlm42cFRMwmMKiZ25InG8OanyGfe2BVq4hKEPubDw?width=310&height=157&cropmode=none)  
Và đây là kết quả nếu bạn thao tác đúng :D  
![result](https://wleurq.by.files.1drv.com/y4moGnb1vIM35GGiUlseuNrD1XfiXM2Ek_0vEd2pOgvdGb8LHWv4ApJXRYxK6XRNpR9kGjNpgHDkbmaZw1TPSaQ0oC2fDWQjzyplMxFpJqlEoHEcEuSIUyHWMPwaseSKO-3ZmwGLtQQFwFsaJPse3pZqQQFPDy3w5eHU7j1g4uPyj81vtzgSWNwn5g10sOCLXuSzwq0jL0OhYA_hvnkUTfD9w?width=1331&height=619&cropmode=none)  
Nếu các bạn gặp vấn đề hoặc có gì thắc mắc thì cứ tự nhiên comment bên dưới, chúng ta cùng nhau thảo luận :3 !
