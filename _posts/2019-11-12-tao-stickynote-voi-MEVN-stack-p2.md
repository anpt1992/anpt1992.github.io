---
layout: post
title: Tạo sticky note với MEVN stack(P2)
category: "Tutorials"
tags: [Web, Tutorials]
date: 2019-11-14
---

![mevn](https://vlheww.by.files.1drv.com/y4mZ2nuEu4rL2dQU6_P1S53KPT2g1dO_-QGIViYGIPLgyEqQm--0Qa35hageMthH6oyZRZ5qVwP1sFfHGQVsIv3S2up8CTqWvp3ICJ8JgFujgsmVPfvOs-7y3Zqrh5b9yxkmYqDblVrf17Z-cUlnuBOz4p5X7USkVWjsRUnGLMjmZBnsrA9YRo4Xk9f18wsE0j86WkQKhTM_prpzKLOk2Lriw?width=1193&height=533&cropmode=none)  
Mừng các bạn đã quay trở lại với series Tạo sticky note với MEVN stack. Ở bài trước, chúng ta đã làm xong bộ khung của ứng dụng Sticky note gồm có frontend,backend và axios để kết nối chúng lại với nhau. Hôm nay chúng ta tiếp tục hoàn thiện Sticky note với các bước sau:

## Mục lục

[1. Tạo MongoDB trên MongoAtlas](#section1)  
[2. Thiết lập kết nối MongoDB](#section2)  
[3. Tạo bảng (Collection) với MongoDB](#section3)  
[4. Truy cập Collection trong NodeJS](#section4)  
[5. Ghi dữ liệu xuống database](#section5)
[6. Xóa những note đã làm xong](#section7)
[7. Màu mè hoa lá hẹ cho đẹp mắt](#section8)

## 1. Tạo MongoDB trên MongoAtlas<a name="section1"></a>

MongoAtlas là dịch vụ lưu trữ dữ liệu đám mây của MongoDB. Dịch vụ này cho phép tạo một tài khoản miễn phí với dung lượng lưu trữ là 500mb (quá ngon cho ứng dụng Sticky note).  
Trước tiên, chúng ta sẽ tạo tài khoản Atlas tại đây. Sau khi đã có tài khoản, chúng ta đăng nhập vào Atlas sẽ hiện ra giao diện như bên dưới. Mặc định Atlas sẽ chặn truy cập với mọi địa chỉ IP trừ địa chỉ IP trong Whitelist, vì thế chúng ta phải thêm địa chỉ IP của máy cần kết nối Atlas vào Whitelist. Trong mục Security có mục Network Access, sau khi chọn Network Access thì chọn tiếp ADD IP WHITELIST trong tab IP Whitelist. Click vào ADD CURRENT IP ADDRESS để thêm ip của máy đang dùng vào Whitelist sau đó Confirm để xác nhận. Lưu ý nhỏ là nếu bạn sử dụng địa chỉ IP động như kiểu gói Hộ gia đình thì sau một thời gian sẽ bị đổi IP, lúc đó phải vào thêm lại địa chỉ IP mới truy cập được. Như mình đã nói ở trên, Atlas có gói dùng miễn phí với dung lượng 500MB (gói M0), để tạo Cluster với gói M0 chúng ta click vào Build a Cluster như hình bên dưới.  
Kế tiếp chúng ta click vào Create a Cluster của khung Starter Cluster ở phía bên trái màn hình  
Tiếp theo sẽ hiện lên giao diện chọn máy chủ triển khai MongoDB, cả Azure, AWS và Google đều hỗ trợ gói miễn phí (M0). Do chúng ta ở Việt Nam nên chúng ta sẽ chọn Singapore như trong hình rồi click vào Create Cluster. Quá trình khởi tạo mất khoảng 1 phút, sau khi khởi tạo xong sẽ hiện thị giao diện Dashboard của Cluster như hình bên dưới.

## 2. Thiết lập kết nối MongoDB<a name="section2"></a>

Để sử dụng MongoDB ở Backend chúng ta cần thư viện mongodb để kết nối tới database. Tại thư mục server cài mongodb

```bash
yarn add mongodb
```

Sau khi cài xong, chúng ta sẽ thiết lập kết nối ở file src/app.js với nội dung như sau:

```javascript
// gọi các thư viện (import dependencies)
const express = require("express");
const bodyParser = require("body-parser");
const cors = require("cors");
const morgan = require("morgan");

const app = express(); // khởi tạo express (create your express app)

const mongo = require("mongodb");
const MongoClient = mongo.MongoClient;
const uri = "YOUR_CONNECTION_STRING";
var client;

var mongoClient = new MongoClient(uri, {
 reconnectTries: Number.MAX_VALUE,
 autoReconnect: true,
 useNewUrlParser: true,
 useUnifiedTopology: true
}); // allows for connection to the db

mongoClient.connect((err, db) => {
 // returns a connection to the mongodb
 if (err != null) {
  console.log(err);
  return;
 }
 console.log("success");
 client = db;
});

// cấu hình express sử dụng các thư viện (make app use dependencies)
app.use(morgan("dev"));
app.use(bodyParser.json());
app.use(cors());

app.get("/todo", (req, res) => {
 res.send(["Thing 1", "Thing 2"]);
});

console.log("Your backend locate at http://localhost:8081/");
app.listen(process.env.PORT || 8081); // backend sẽ chạy ở port 8081 (client is already running on 8080)
```

trong đoạn code trên có tham số YOUR_CONNECTION_STRING là chuỗi kết nối đến MongoDB của Atlas. Để lấy được chuối kết nối này chúng ta sẽ làm như sau:
Đăng nhập vào Atlas, tại màn hình Dashboard click vào nút Connect, sau đó chọn Connect Your Application, sau đó chọn ngôn ngữ lập trình cũng như phiên bản tương ứng, ở đây chúng ta chọn Nodejs và 3.0 or later cuối cùng là click vào nút copy. Trở lại file server/src/app.js dán chuỗi kết nối vừa copy vào vị trí của YOUR_CONNECTION_STRING là xong bước thiết lập kết nối MongoDB.

## 3. Tạo bảng (Collection) với MongoDB<a name="section3"></a>

Quay trở lại Mongo Atlas, tại khung Dashboard chọn COLLECTIONS, trong tab Collections vừa hiện ra click vào nút Add my own data, điền tên database và tên collections rồi click nút Create. Để thêm một dòng, click vào nút Insert Document, mặc định trường \_id được sinh tự động, chúng ta sẽ thêm trường title để lưu tiêu đề công việc sau khi điền xong click vào Insert để ghi vào collection

## 4. Truy cập Collection trong NodeJS<a name="section4"></a>

Quay lại file server/src/app.js sửa nội dung của đường dẫn /todo theo đoạn code dưới đây:

```javascript
app.get("/todo", (req, res) => {
 const collection = client.db("test").collection("todos");
 collection.find().toArray(function(err, results) {
  if (err) {
   console.log(err);
   res.send([]);
   return;
  }
  res.send(results);
 });
});
```

truy cập vào [http://localhost:8080/#/todo](http://localhost:8080/#/todo) để xem kết quả.

## 5. Ghi dữ liệu xuống database<a name="section5"></a>

Để ghi dữ liệu xuống database, trước tiên cần thêm input component vào file HTML, sau đó dùng axios đẩy nội dung trong input component về file app.js , cuối cùng là ghi dữ liệu xuống database. Giờ thì bắt tay vào làm thôi :3  
Để thêm input component chúng ta sửa file ToDo.vue theo nội dung như sau:

```javascript
<template lang="html">
  <div>
    <form v-on:submit='addTodo($event)'> //form input để submit tiêu đề
      <input type='text' placeholder='Enter Todo' v-model='newTodo'/>
      <input type='submit' />
    </form>
    <ul>
      <li v-for='todo in todos' :key='todo._id'>
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
      newTodo: '',
      todos: []
    }
  },
  mounted () {
    this.loadTodos()
  },
  methods: {
    async addTodo (evt) {
      evt.preventDefault()
      const response = await ToDoAPI.addTodo(this.newTodo) //truyền tiêu đề vừa submit đến backend thông qua file ToDoAPI.js
      this.todos.push(response.data)
      this.newTodo = '' // clear the input field
    },
    async loadTodos () {
      const response = await ToDoAPI.getToDos()// lấy danh sách todo
      this.todos = response.data
    }
  }
}
</script>

<style lang="css">
</style>
```

Sau khi submit VueJS sẽ truyền newToDo vào hàm addTodo để gọi hàm ToDoAPI.addTodo sau khi có kết quả trả về thì xóa nội dung input. Kế tới, chúng ta sẽ tạo thêm addTodo(todo) trong services/ToDoAPI.js

```javascript
addTodo(todo) {
  return API().post("addTodo", {
   todo: todo // add our data to the request body
  });
},

```

Tiếp theo chúng ta cần bổ sung đường dẫn addTodo với giao thức post vào backend với nội dung file server/src/app/js

```javascript
app.post("/addTodo", (req, res) => {
 const collection = client.db("test").collection("todos");
 var todo = req.body.todo; // parse the data from the request's body
 collection.insertOne({ title: todo }, function(err, results) {
  if (err) {
   console.log(err);
   res.send("");
   return;
  }
  res.send(results.ops[0]); // returns the new document
 });
});
```

Giờ thì vào [http://localhost:8080/#/todo](http://localhost:8080/#/todo) để xem kết quả nào, như hình bên dưới là dc :3

## 6. Xóa những note đã làm xong<a name="section6"></a>

Xóa những note đã làm xong là một trong những chức năn cơ bản của Sticky note. Để thực hiện chức năng này chúng ta cần làm các bước sau:  
Đầu tiên, tại file Todo.vue chúng ta đổi tag span thành tag input checkbox và bắt sự kiện click.

```javascript
<li v-for='todo in todos' :key='todo._id'>
  <input type='checkbox' @click='deleteTodo(todo._id)'> {{todo.title}}
</li>
```

Kế tiếp, chúng ta định nghĩa hàm deleteTodo để xóa note bằng cách gửi request đến backend

```javascript
deleteTodo (todoID) {
  ToDoAPI.deleteTodo(todoID)
  // remove the array element with the matching id
  this.todos = this.todos.filter(function (obj) {
    return obj._id !== todoID
  })
}
```

bước kế tiếp là định nghĩa hàm deleteTodo ở file src/services/ToDoAPI.js để điều phối đến backend

```javascript
deleteTodo (todoID) {
  return API().post('deleteTodo', {
    todoID: todoID // add our data to the request body
  })
}
```

và dĩ nhiên bước còn lại là định nghĩa đường dẫn /deleteTodo với giao thức post tại file server/src/app.js

```javascript
app.post("/deleteTodo", (req, res) => {
 const collection = client.db("test").collection("todos");
 // remove document by its unique _id
 collection.removeOne({ _id: mongo.ObjectID(req.body.todoID) }, function(
  err,
  results
 ) {
  if (err) {
   console.log(err);
   res.send("");
   return;
  }
  res.send(); // return
 });
});
```

giờ thì chúng ta cùng xem kết quả đạt được có giống hình bên dưới không :3

Yeah! tới đây chúng ta về cơ bản đã xong về mặt chức năng của Sticky note rồi nhưng bây giờ nhìn nó giống to do list nhiều hơn mà cũng xấu òm nữa. Vậy thì ở bước sau cũng là bước cuối cùng, chúng ta cùng trang trí màu mè hoa lá hẹ cho app được hoàn chỉnh nhé.

## 7. Màu mè hoa lá hẹ cho đẹp mắt<a name="section7"></a>

Search tìm trên mạng template HTML sticky note hên sao ra được 1 bài mà mình thấy cũng ưng ý, các bạn có thể xem chi tiết tại đây.
Sau khi chọn dc template thì gắn vào thôi.
Tạo hình sticky note:
Tại file ToDo.vue thay tag input check box thành tag a như dưới đây:

```javascript
<a href="#" class ='notes' >
    <button v-on:click="deleteTodo(todo._id)"/>
    <h2>{{todo.title}}</h2>
</a>
```

thêm đoạn css sau vào tag style

```javascript
/* css cho từng note */
ul,
li {
  list-style: none;
}
ul {
  overflow: hidden;
  padding: 3em;
}

ul li a {
  text-decoration: none;
  color: #000;

  background: #ffc;
  display: block;
  height: 10em;
  width: 10em;
  padding: 1em;
  background-image: url(https://rob.kilnhg.com/Content/Images/kiln_focus.gif);
  background-repeat: no-repeat;
  -moz-box-shadow: 5px 5px 7px rgba(33, 33, 33, 1);
  -webkit-box-shadow: 5px 5px 7px rgba(33, 33, 33, 0.7);
  box-shadow: 5px 5px 7px rgba(33, 33, 33, 0.7);
  -moz-transition: -moz-transform 0.15s linear;
  -o-transition: -o-transform 0.15s linear;
  -webkit-transition: -webkit-transform 0.15s linear;
}
ul li {
  margin: 1em;
  float: left;
}
ul li h2 {
  margin-top: 30px;
  font-size: 140%;
  font-weight: bold;
  padding-bottom: 10px;
}
ul li input {
  float: right;
}
ul li a {
  -webkit-transform: rotate(-6deg);
  -o-transform: rotate(-6deg);
  -moz-transform: rotate(-6deg);
}
ul li:nth-child(even) a {
  -o-transform: rotate(4deg);
  -webkit-transform: rotate(4deg);
  -moz-transform: rotate(4deg);
  position: relative;
  top: 5px;
  background: #cfc;
}
ul li:nth-child(3n) a {
  -o-transform: rotate(-3deg);
  -webkit-transform: rotate(-3deg);
  -moz-transform: rotate(-3deg);
  position: relative;
  top: -5px;
  background: #ccf;
}
ul li:nth-child(5n) a {
  -o-transform: rotate(5deg);
  -webkit-transform: rotate(5deg);
  -moz-transform: rotate(5deg);
  position: relative;
  top: -10px;
}
ul li a:hover,
ul li a:focus {
  box-shadow: 10px 10px 7px rgba(0, 0, 0, 0.7);
  -moz-box-shadow: 10px 10px 7px rgba(0, 0, 0, 0.7);
  -webkit-box-shadow: 10px 10px 7px rgba(0, 0, 0, 0.7);
  -webkit-transform: scale(1.25);
  -moz-transform: scale(1.25);
  -o-transform: scale(1.25);
  position: relative;
  z-index: 5;
}
ol {
  text-align: center;
}
ol li {
  display: inline;
  padding-right: 1em;
}
ol li a {
  color: #fff;
}

.notes button {
  border: none;
  background-color: Transparent;
  background-image: url("https://i.ibb.co/vYJqwVV/x.png");
  padding: 18px;
  float: right;
  cursor: pointer;
}
```

Dưới đây là kết quả, nhìn cũng được quá hen :3
kế tới là khung nhập nội dung, thêm đoạn css sau đây vào ToDo.vue

```javascript
/* css cho khung nhập nội dung*/
.titleForm {
  width: 50%;
  margin: 0 auto;
  position: relative;
  display: flex;
}

.title {
  width: 100%;
  border: 3px solid #00b4cc;
  border-right: none;
  padding: 5px;
  height: 20px;
  border-radius: 5px 0 0 5px;
  outline: none;
  color: #9dbfaf;
}

.title:focus {
  color: #00b4cc;
}

.Button {
  width: 40px;
  height: 36px;
  border: 1px solid #00b4cc;
  background: #00b4cc;
  text-align: center;
  color: #fff;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
  font-size: 20px;
}
```

kế đó chúng ta sẽ điều chỉnh lại form nhập theo đoạn code dưới đây:

```javascript
<form v-on:submit='addTodo($event)'>
    <div class="titleForm">
        <input type="text" class="title" placeholder="Write what you need to remember!" v-model='newTodo'>
        <button type="submit" class="Button">
            <font-awesome-icon :icon="['fas', 'edit']" class="icon alt"/>
        </button>
    </div>
</form>
```

Nếu để ý thì bạn sẽ thấy có tag font-awesome-icon cái này dùng để chèn icon viết note và hiển nhiên mặc định HTML không hỗ trợ tag này, để sử dụng chúng ta sẽ cài đặt thư viện @fortawesome/fontawesome-svg-core, fortawesome/free-solid-svg-icon, @fortawesome/vue-fontawesome.

```bash
yarn add @fortawesome/fontawesome-svg-core fortawesome/free-solid-svg-icon @fortawesome/vue-fontawesome
```

sau khi cài xong thì chúng ta cần phải khai báo với VueJS bằng cách sửa lại file client/src/main.js

```javascript
import Vue from "vue";
import App from "./App";
import router from "./router";
import { library } from "@fortawesome/fontawesome-svg-core";
import { fas } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

library.add(fas);

Vue.component("font-awesome-icon", FontAwesomeIcon);
Vue.config.productionTip = false;

/* eslint-disable no-new */
new Vue({
 el: "#app",
 router,
 components: { App },
 template: "<App/>"
});
```

giờ chạy coi sao he ^^!
Nhìn ngon lành rồi đó mà sao thiếu thiếu he, đúng rồi cái nền trắng, kiếm hình chèn vào thôi :3, cách chèn như đoạn css bên dưới nghe.

```javascript
* {
  margin: 0;
  padding: 0;
}
body {
  font-family: arial, sans-serif;
  font-size: 100%;
  margin: 3em;
  background: #ccf;
  background-image: url("https://wallpaperbro.com/img/311300.jpg");//hình nền ở đây nghe ^^!
  color: #fff;
}
h2,
p {
  font-size: 100%;
  font-weight: normal;
}
```

Tèn ten, như vậy chúng ta đã hoàn thành xong một web app Sticky note bằng stack MEVN hi vọng demo nhỏ này có thể tạo truyền cảm hứng để các bạn tạo ra những sản phẩm đẹp mắt và sáng tạo hơn.
Bài viết được lược dịch và sử dụng tài nguyên từ các bài viết:
[MEVN Todo app](https://medium.com/@mattmaribojoc/creating-a-todo-app-with-a-mevn-full-stack-part-2-8180d944233a)  
[Html + css sticky note](https://code.tutsplus.com/tutorials/create-a-sticky-note-effect-in-5-easy-steps-with-css3-and-html5--net-13934)  
[Html + css form](https://codepen.io/huange/pen/rbqsD)
