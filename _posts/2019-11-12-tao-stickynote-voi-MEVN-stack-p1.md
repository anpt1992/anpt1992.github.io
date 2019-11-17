---
layout: post
title: Tạo sticky note với MEVN stack(P1)
category: "Tutorials"
tags: [Web, Tutorials]
date: 2019-11-12
---

![mevn](https://vlheww.by.files.1drv.com/y4mZ2nuEu4rL2dQU6_P1S53KPT2g1dO_-QGIViYGIPLgyEqQm--0Qa35hageMthH6oyZRZ5qVwP1sFfHGQVsIv3S2up8CTqWvp3ICJ8JgFujgsmVPfvOs-7y3Zqrh5b9yxkmYqDblVrf17Z-cUlnuBOz4p5X7USkVWjsRUnGLMjmZBnsrA9YRo4Xk9f18wsE0j86WkQKhTM_prpzKLOk2Lriw?width=1193&height=533&cropmode=none)  
Gần đây mình có nhận làm 1 project NodeJS, sau quá trình tìm hiểu cũng học được
nhiều điều mới lạ nên muốn ghi lại vừa để mình nhớ vừa có thể giúp được ai đó
đang cần :D. Ở bài viết này chúng mình sẽ cùng nhau tạo một webapp đơn giản (Sticky note) với MEVN stack.  
MEVN stack được hiểu là bộ combo các công nghệ (**M**ongoDB, **E**xpressJS, **V**ueJS, **N**odeJS) trong đó MongoDB đóng vai trò là database, ExpressJS là back-end, VueJS là front-end, NodeJS là môi trường
vận hành.

## Mục lục

[1. Cấu trúc project MEVN stack](#section1)  
[2. Thiết lập frontend với VueJS](#section2)  
[3. Thiết lập backend với ExpressJS](#section3)  
[4. Thiết lập liên kết (Routing)](#section4)  
[5. Dùng thư viện axios kết nối frontend backend](#section5)

## 1. Cấu trúc project MEVN stack<a name="section1"></a>

Cấu trúc project gồm 2 thư mục: client -> dành cho frontend và server -> dành cho backend.

## 2. Thiết lập frontend với VueJS<a name="section2"></a>

Cài đặt vue/cli và khởi tạo project trong thư mục client

```bash
# dùng npm hoặc yarn tùy sở thích nghe
npm install -g @vue/cli
# OR
yarn global add @vue/cli
# Lúc điền thông tin project nhớ chọn cài vue-router
vue init webpack client
```

Sau khi chạy lệnh **vue init webpack client** thì sẽ hiện một số câu hỏi thiết lập, lúc điền thông tin project nhớ chọn cài **vue-router**, nếu các bạn không có điều chỉnh gì khác thì cứ enter đến khi hiện progress bar tải các gói thư viện.  
Nếu cài đặt thành công sẽ hiện thông báo:

{: .box-note}
Project initialization finished!  
To get started:  
cd client  
npm run dev

Để kiểm tra vue chạy ổn hay chưa chúng ta làm theo hướng dẫn:

```bash
cd client
yarn start
```

Sau đó truy cập vào link: [http://localhost:8080/](http://localhost:8080/) sẽ thấy trang mặc định của VueJS  
![defaultpage](https://wvheww.by.files.1drv.com/y4m6EPrB40ZvACsNVI6H2b-5xWCUfITs7Adv3M-hhaIMdsnwon-3SbEL2yIki30-58NqpbCr1oRa3_IduEbnLLjaKy2AY_xBy86ON0vvYykCjWED2Pl4qEUc9NgwQ_f3NNA3cyKYYfgAI6qDzLM4c6CkxUPeIo5cPQz0jrqt7y6KMHr2KgeVkhHr9d9qNEklN08OptcI_OruhQvVC2_oI9MPg?width=1366&height=768&cropmode=none)  
Kế tiếp: vào file src/App.vue để sửa lại trang mặc định như đoạn code dưới đây:

```javascript
<template>
  <div id="app">
    <router-view/>
  </div>
</template>
<script>
export default {
  name: 'app',
  data () {
    return {

    }
  }
}
</script>
<style>
</style>
```

Bước tiếp theo chúng ta sẽ thiết lập backend.

## 3. Thiết lập backend với ExpressJS<a name="section3"></a>

Tại thư mục gốc của project, tạo thư mục server sau đó vào thư mục server vừa tạo để cài đặt ExpressJS.  
![structureBE](https://wfheww.by.files.1drv.com/y4mPx7ongzwYWADCv3rLACOHYdVHS9vc4PppTB-ZTgyPEpW0_6K-RpRbgRrGyDiOPLLff4akpH24CDZ1toVWQYvDZob1lCtscbVrPicPucfZEenSosGn9huxZf0psPl-69LiwTgapD7VgRhqrtLIKuh65MyEQpx9MfG_fXCKpyjMNeazE8kafTu779oJOj5RlTBajdnqXv3XjMUWF1dWQ0NCw?width=284&height=151&cropmode=none)

```bash
mkdir server
cd server
yarn init #khởi tạo thông tin cho project, nếu muốn mặc định cứ enter đến hết.
```

{: .box-note}
question entry point (index.js): #tùy chọn file js khởi động expressjs chúng ta chọn app.js

Sau khi điền xong thông tin backend, yarn sẽ tự động tạo file package.json để lưu danh sách các thư viện cần dùng.  
Ngoài ExpressJS, chúng ta cần cài thêm một số thư viện:

- nodemon(tự khởi động lại nodejs sau mỗi lần chỉnh sửa file)
- body-parser(xử lý JSON, text)
- cors(hỗ trợ truy cập api từ tên miền khác)
- morgan(tạo file log)

```bash
yarn add express body-parser cors morgan nodemon
```

Để cấu hình nodemon, mở file package.json tìm dòng "main": "app.js", thêm đoạn code sau vào bên dưới

```javascript
"scripts": {
    "start": "nodemon --watch src src/app.js",
    "test": "echo \"Error: no test specified\" && exit 1"
 },
```

Tạo thư mục src và file app.js bên trong có nội dung như sau:

```javascript
// gọi các thư viện (import dependencies)
const express = require("express");
const bodyParser = require("body-parser");
const cors = require("cors");
const morgan = require("morgan");

const app = express(); // khởi tạo express (create your express app)

// cấu hình express sử dụng các thư viện (make app use dependencies)
app.use(morgan("dev"));
app.use(bodyParser.json());
app.use(cors());

// tạo link localhost:8081/todo
app.get("/todo", (req, res) => {
 res.send(["Thing 1", "Thing 2"]); //kết quả trả về
});

console.log("Your backend locate at http://localhost:8081/");
app.listen(process.env.PORT || 8081); // backend sẽ chạy ở port 8081 (client is already running on 8080)
```

Chạy lệnh **yarn start** để khởi động nodemon. Truy cập vào link [http://localhost:8081/todo](http://localhost:8081/), sẽ thấy kết quả là ["Thing 1","Thing 2"].  
Tới đây chúng ta đã chuẩn bị xong về frontend với VueJS và backend với ExpressJS. Sau đây chúng ta sẽ thiết lập liên kết cho VueJS.

## 4. Thiết lập liên kết (Routing)<a name="section4"></a>

Quay trở lại thư mục **client/src** và tạo thư mục tên là **router** để lưu thiết lập các link của VueJS.
Vào thư mục **components** tạo file ToDo.vue với nội dung như sau:

```javascript
<template lang="html">
  <div>
    <ul>
      <li>
        Hello World
      </li>
    </ul>
  </div>
</template>
<script>
export default {};
</script>
<style lang="css">
</style>
```

Như vậy chúng ta đã có được trang ToDo.vue, tuy nhiên để truy cập vào trang này chúng ta cần phải thiết lập đường dẫn (route). Để chỉnh cấu hình route, chúng ta tạo file router/index.js có nội dung như sau:

```javascript
//Khai báo các thư viện và components cần dùng
import Vue from "vue";
import Router from "vue-router";
import HelloWorld from "@/components/HelloWorld";
import ToDo from "@/components/ToDo";

Vue.use(Router);

export default new Router({
 routes: [
  {
   path: "/",
   name: "HelloWorld",
   component: HelloWorld
  },
  {
   path: "/todo",
   name: "ToDo",
   component: ToDo
  }
 ]
});
```

Nếu mọi thiết lập đều chính xác thì khi truy cập vào [http://localhost:8080/#/todo](http://localhost:8080/#/todo) sẽ thấy dòng chữ Hello World.

## 5. Dùng thư viện axios kết nối frontend và backend<a name="section5"></a>

Để frontend giao tiếp với backend thì cần HTTP request từ frontend gọi vào backend và thư viện axios sẽ giúp chúng ta làm điều đó.

```bash
yarn add axios
```

Trước hết, chúng ta tạo thư mục src/services và tạo file API.js bên trong src/services có nội dung như sau:

```javascript
import axios from "axios";
export default () => {
 return axios.create({
  baseURL: `http://localhost:8081/` // địa chỉ của backend (node server)
 });
};
```

Tạo file ToDoAPI.js có nội dung như sau:

```javascript
import API from "@/services/API";
export default {
 getToDo() {
  return API().get("todo");
 }
};
```

Tới đây chúng ta cần điều chỉnh file ToDo.vue một số điểm như sau:

- Khai báo file ToDoAPI.js trong component ToDo.vue để trang này có quyền gọi hàm getToDo()
- Dùng "Vue hook" gọi hàm bất đồng bộ (async) bằng hàm mounted() để lấy dữ liệu từ hàm getToDo()
- Để load dữ liệu, chúng ta dùng thẻ v-for để tự điền vào todolist

Nội dung điều chỉnh ToDo.vue

```javascript
<template lang="html">
  <div>
    <ul>
      <li v-for='todo in todos'>
        <span>{{todo}}</span>
      </li>
    </ul>
  </div>
</template>
<script>
import ToDoAPI from '@/services/ToDoAPI.js'
export default {
  data () {
    return {
      todos: []
    }
  },
  mounted () {
    this.loadTodos()
  },
  methods: {
    async loadTodos () {
      const response = await ToDoAPI.getToDo()
      this.todos = response.data
    }
  }
}
</script>
<style lang="css">
</style>
```

Truy cập vào [http://localhost:8080/#/todo](http://localhost:8080/#/todo) nếu ra kết quả như hình bên dưới thì chúc mừng bạn đã thành công!  
![result](https://v1heww.by.files.1drv.com/y4m-Pqdc4xjDDHGzEpZZKj6r4lndeIJhT_MdMWnbg8Owih3nkvTeDpR1iyfxkKLveze3AgYV1zSZ2TdF-esot-D_Qc7ThGvl8IkGRmGHKqfRhfRNfHjiGcTHvhFcRU48d23sduDahnd-DhDzXEUZgt97zNxlyzuMyeiMTh-nNs-2dELnEAsDjXiDzHj5rK06-J9dr8Div_kMjUyltU2j0ulYg?width=1385&height=691&cropmode=none)  
Lỡ đen không ra đúng thì cũng đừng rối, hãy kiểm tra lại các hàm, các file, bấm F12 trên chrome, chọn console để xem thông báo lỗi và hãy chắc chắn rằng cả backend lẫn frontend đều đang được bật.  
Bài cũng dài rồi, chúng ta cùng điểm lại những gì đã làm được:

- Tạo frontend với VueJS
- Tạo backend với ExpressJS
- Thiết lập link cho các trang tại frontend
- Dùng axios để kết nối frontend và backend
- Bước đầu gọi được API đơn giản từ backend

[Github code mẫu cho bạn nào cần nhé](https://github.com/anpt1992/demo/tree/master/mevn_stickynote_p1)  
Ở phần kế chúng ta sẽ tìm hiểu về cơ sở dữ liệu MongoDB cũng như cách ứng dụng nó vào MEVN stack mà cụ thể hơn là ứng dụng Stickynote và dùng css để làm ứng dụng Stickynote thêm lung linh hơn :3  
Tạm biệt các bạn và hẹn gặp lại ở bài sau!
