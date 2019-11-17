---
layout: post
title: Tạo sticky note với MEVN stack(P2)
category: "Tutorials"
tags: [Web, Tutorials]
date: 2019-11-17
---

![mevn](https://vlheww.by.files.1drv.com/y4mZ2nuEu4rL2dQU6_P1S53KPT2g1dO_-QGIViYGIPLgyEqQm--0Qa35hageMthH6oyZRZ5qVwP1sFfHGQVsIv3S2up8CTqWvp3ICJ8JgFujgsmVPfvOs-7y3Zqrh5b9yxkmYqDblVrf17Z-cUlnuBOz4p5X7USkVWjsRUnGLMjmZBnsrA9YRo4Xk9f18wsE0j86WkQKhTM_prpzKLOk2Lriw?width=1193&height=533&cropmode=none)  
Mừng các bạn đã quay trở lại với series Tạo Sticky note với MEVN stack. Ở bài trước, chúng ta đã làm xong bộ khung của ứng dụng Sticky note gồm có frontend,backend và axios để kết nối chúng lại với nhau. Hôm nay chúng ta tiếp tục hoàn thiện Sticky note với các bước sau:

## Mục lục

[1. Tạo MongoDB trên MongoAtlas](#section1)  
[2. Thiết lập kết nối MongoDB](#section2)  
[3. Tạo bảng (Collection) với MongoDB](#section3)  
[4. Truy cập Collection trong NodeJS](#section4)  
[5. Ghi dữ liệu xuống database](#section5)  
[6. Xóa những note đã làm xong](#section7)  
[7. Màu mè hoa lá hẹ cho đẹp mắt](#section8)

## 1. Tạo MongoDB trên MongoAtlas<a name="section1"></a>

MongoAtlas là dịch vụ lưu trữ dữ liệu đám mây của MongoDB. Dịch vụ này cho phép tạo một tài khoản miễn phí với dung lượng lưu trữ là 500MB (quá ngon cho ứng dụng Sticky note).  
Trước tiên, chúng ta sẽ tạo tài khoản Atlas tại [đây](https://www.mongodb.com/cloud/atlas/register). Sau khi đã có tài khoản, chúng ta đăng nhập vào Atlas sẽ hiện ra giao diện như bên dưới.  
![first](https://wlheww.by.files.1drv.com/y4mWETWJuRVmqOWcANGWCvYY3ZXYiUqSzg3Xwcc8knj9-FnkN5ZzTNJY5-P6dK97ugfcmjnwid2W2Rfo6l9OK-l6YpuST1_ySz3ZmHHFSvMgQoNXV8S9VeHMIhA6amc8qOpoO4iP3en3T0sOZ7kTG3QCaEjRyV0VcYhSGZSfbP8fnL5unt4Fc9KTwyqb57GwNM3NUZNdNwsvIq2fBnnuOiGfg?width=1366&height=553&cropmode=none)  
Mặc định Atlas sẽ chặn truy cập với mọi địa chỉ IP trừ địa chỉ IP trong Whitelist, vì thế chúng ta phải thêm địa chỉ IP của máy cần kết nối Atlas vào Whitelist. Trong mục Security có mục Network Access, sau khi chọn Network Access thì chọn tiếp ADD IP WHITELIST trong tab IP Whitelist. Click vào ADD CURRENT IP ADDRESS để thêm ip của máy đang dùng vào Whitelist sau đó Confirm để xác nhận.  
![ipwhitelist](https://xfheww.by.files.1drv.com/y4mw5qXz4J03lFGzgA6jjyTZVEthxzT8EpdQaLkQXFjLHFR2b4ZfYjUUpA6Wk1VqCdWLXmZhSnlT3CZCXnW1ib3bM8Uw5VUGj850aYvv062ILmm5ci-YcqMap0z39PQW0MXgVGiI_9WcfFSmFZ0Kgan59f1yHWdrFQQ2TCYnoiQDy34w9VM3W1uufmwoostVTa1ILthVSBQb1rzKF_ihs9tmQ?width=698&height=406&cropmode=none)  
Lưu ý nhỏ là nếu bạn sử dụng địa chỉ IP động như kiểu gói Hộ gia đình thì sau một thời gian sẽ bị đổi IP, lúc đó phải vào thêm lại địa chỉ IP mới truy cập được.  
Như mình đã nói ở trên, Atlas có gói dùng miễn phí với dung lượng 500MB (gói M0), để tạo Cluster với gói M0 chúng ta click vào Build a Cluster như hình bên dưới.  
![build](https://yvhqnq.by.files.1drv.com/y4mWSIhnRnnLPyhyKeCtjy0xtVsenNbk6e8I8HjWWZXnFiPHc0ZXDiQ022RlBIyHY9NfS2UuNEx3h4I3exq4SGSCgBydeX85VSp7CCe8U_uhL5-uLVcYQIuore0PhVkkhFtNoAJyaBdhr2TV8czjre9PBmaX0wjFdSWeLpZ3ZbeD4HOYyabk_uZuRFSaMYALtLhqtGvZJMQTOp-YQ-5CFoZJQ?width=1141&height=393&cropmode=none)  
Kế tiếp chúng ta click vào Create a Cluster của khung Starter Cluster ở phía bên trái màn hình  
![starter](https://wffrqg.by.files.1drv.com/y4mKACAxzgwHS5OCmaEjZp7iTmEB3-iK_f-rRIQ0rDRO-bLEDV_hvL3FIHIjg2CKy7xUXzN4EheGuZaWlweZuAyB5LGanBOAHYODiL_F6vygDEcFkvr1o0pn6dqWYwWUpfDXFukrb55gp0hIKN4-4Sc9x0KfjZT6Lvd-ViYebw0trtbQ9gIWNkLv_L-WhWOAKfX-k_qHYryD8z8eN4WCSdhQA?width=289&height=622&cropmode=none)  
Tiếp theo sẽ hiện lên giao diện chọn máy chủ triển khai MongoDB, cả Azure, AWS và Google đều hỗ trợ gói miễn phí (M0). Do chúng ta ở Việt Nam nên chúng ta sẽ chọn Singapore như trong hình rồi click vào Create Cluster.  
![choose](https://yfheww.by.files.1drv.com/y4mbQmBQt1UOSrnDREqhFbKU8D_W3jAbcRccqqqRKjhYA2xJ7nOlwvyCtN6_mKU2xtY5MnP0TayAQzOB1zafyCoTPg1BmpfUVvZXh4iL_bcEUAD9euMFdOs87RBYySRmAkxfTk28TjAZtz5QJr42xt1OdwBikqKNoTRsYgKYe3Od-VCVkO2cquB_hqPKBBVYk_SLOthRtMI-bxpq1OFOIGHnA?width=1366&height=768&cropmode=none)  
Quá trình khởi tạo mất khoảng 1 phút, sau khi khởi tạo xong sẽ hiện thị giao diện Dashboard của Cluster như hình bên dưới.  
![dashboard](https://wvfrqg.by.files.1drv.com/y4m1_bVR3riAb4GX6dQp1cQG52AkurD7yx_Rx4yJvxt2vamRk5qSOCdUqiaCp9GD9FEcQTapXwzio48qPSXTuWl4ejUTzaD_MAMwQcEoKCSHM3eVOwMCynMgHDyR9ROlsAwtEs0HCxj8yP0FfSIVNdfk51RRTVgVXmBEVZNOJ3KlwNqRp9IG4dHunDKAxa5Bg82H9GAWWCW9zN6QuBN2uChcQ?width=1141&height=368&cropmode=none)

## 2. Thiết lập kết nối MongoDB<a name="section2"></a>

Để sử dụng MongoDB ở Backend chúng ta cần thư viện mongodb để kết nối tới database. Tại thư mục server cài mongodb

```bash
yarn add mongodb
```

Sau khi cài xong, chúng ta sẽ thiết lập kết nối ở file **server/src/app.js** với nội dung như sau:

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

Trong đoạn code trên có tham số YOUR*CONNECTION_STRING là chuỗi kết nối đến MongoDB của Atlas. Để lấy được chuối kết nối này chúng ta sẽ làm như sau:  
Đăng nhập vào Atlas, tại màn hình Dashboard click vào nút Connect, sau đó chọn Connect Your Application, sau đó chọn ngôn ngữ lập trình cũng như phiên bản tương ứng, ở đây chúng ta chọn Nodejs và 3.0 or later cuối cùng là click vào nút copy.  
![connect](https://w1heww.by.files.1drv.com/y4mt7ln7X_YQh4qsclg7E7AP2lvaD51hPBTD4gkpE0WOjCfzLkkNCNdvNVJoe1WhpEWRubkaaLUL00JdKakThUMsThubtsZoUiLiLe2av-6UGGECJKOfi5xtlWHes4mufmVChroSbGhcFPaNaItaYJnFqZFXwQGA_gw-I8VJu9hm103yBssFkMuq5EigNx6D8JRgL7YjILRIEGojcqoqUKSxg?width=1190&height=619&cropmode=none)  
![connect app](https://v1frqg.by.files.1drv.com/y4mL0kTIL_hLwXFStZTwuaQzTKR-mk0NVWpmvwBFZv9qOQwvPCCahpba7vk89OYOqwMJM8gnGfwQ0D_4*-GDKXwBSJjkthGFYRxiy11JddMe_XE2EwOPFEPR_0ixJrw99h_wMzPsD2MwuIzwLjW-8Zx6gsWapbJkEG7sSmakWbXx1yAkFmaRqrnhzGruvydth00m5Xf6gVJqIKjt-Uu0MSuLA?width=659&height=299&cropmode=none)
![connection string](https://vlfrqg.by.files.1drv.com/y4mm3U6VxUkYHVRQhqzbFD6aq5_ZQs5QtR9amxGwbwbv1u5F8OzQqvrwQlD3A7ilvIn-wl-m4RfxsjVGgXb-bVUD-sSkK1TgquNTL9BMvvWawgN6x55qAhyf068oLMGnuWYax7oC0SF9F4nb8UcokpRpA3Yh7CBxfJkgOBG-_9ttcr18lBNBTLads9o4HOaI3nSJ6Xp8G4kfZXYXsPQKQxqZQ?width=644&height=358&cropmode=none)  
Trở lại file **server/src/app.js** dán chuỗi kết nối vừa copy vào vị trí của YOUR_CONNECTION_STRING là xong bước thiết lập kết nối MongoDB.

## 3. Tạo bảng (Collection) với MongoDB<a name="section3"></a>

Quay trở lại Mongo Atlas, tại khung Dashboard chọn COLLECTIONS, trong tab Collections vừa hiện ra click vào nút Add my own data, điền tên database và tên collections rồi click nút Create. Để thêm một dòng, click vào nút Insert Document, mặc định trường \_id được sinh tự động, chúng ta sẽ thêm trường title để lưu tiêu đề công việc sau khi điền xong click vào Insert để ghi vào collection.  
![collection](https://xvfrqg.by.files.1drv.com/y4m7savErr6EiSKtwpatxOunYwQqGDNb4xIcsWy9Z1E-pmdCZa1-p-w0cpjW4nUhPe-uNCymBv_jfLq-JHInWrVZJmQUvSR7KO2DcZDqKEi5uDCJYVHJB1XywE0NeC83VPfanPR9kKxezRYgK3cOQPkw37swwxHqJhGmAmO95xe1G-C9QWoUZBh3ONdttE8HfD9a-xqorDvi2JY-ttFBQt_yg?width=1141&height=368&cropmode=none)  
![add](https://xffrqg.by.files.1drv.com/y4mtKp2YyVtywLztnbUIy0zqZTm_7krg4yvsPcQPjfQMsnpdKNSttCR0IFaxAYDZYC96KF2Q5_8yO19UoEU0ZRc9-BORLfpQl5ZcmBpFzVYHxmhCr08_-R-UzUEOIG14IKnj1gJTXhHnaTyMk71jVPXu0RmYYwNVU1o7SS2MMajIIltKBZSLO9ECImlpV5tdiloMPNAMI7rMJN79Lr1UV-Uzg?width=1132&height=375&cropmode=none)  
![create](https://w1frqg.by.files.1drv.com/y4mPkx_AlatZoJnWPLXDIFq-fkjj4v3kaN-aB1sKDgJbs4GjQTYY4DiQ6n1GGNmkasB3pMYMDN0eLjdsGE2mz-HaBjYxE9wkcGFwnrcc3ZSKKnH_-TsqLgsqx3i5ka_8qqEaQVPydYxLJyr7xfllmy9hCWc2ZRnSHbb6TOv_lsRkZsC6sRyrVMlr7UwvXO0MME0V-Sb6t2sOP7KNCvMuLgtdQ?width=411&height=438&cropmode=none)  
![insert](https://wlfrqg.by.files.1drv.com/y4mQM4WWjclB-pl_MiE3w_IHNkGwrgJThwbRcOW05t90nQc1PcuJW1HNWKx1-VRyVscfM8yhKv-bdY-UIwsbFHgYwDqXrVCJz4xAba5_CGqEOJOpBM9sfY9etpCHMX7giYyBt05KuiKEROUwssD1wxMfLOs_ovoMAJJ3I9mhxCvqSN5g9APnwr7CdNiYlON0ZfLHOEzLXSFblINoZMuTf5f1Q?width=833&height=250&cropmode=none)  
![insert2](https://yvfrqg.by.files.1drv.com/y4mGrlDl7sqSNqqZNjF21xv36b7FnlQX8r-5OIMEGSff4TFhHOAM4PefjAdhIk86fbMB9pZqMiLwQQyG_mIGpRmzahBK6xEhjaJIMAmwBaLEXHtub6wospmtqPtb5QZ7-amOWR5tRYFrERStAsyizz3YWkDHiVl1RJoLQmY4IQDVdvDefmuu7dEvClN6RO9wv7JA-n_iU68JUjzoqn2TOM6bA?width=580&height=249&cropmode=none)

## 4. Truy cập Collection trong NodeJS<a name="section4"></a>

Quay lại file **server/src/app.js** sửa nội dung của đường dẫn /todo theo đoạn code dưới đây:

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
![rs](https://wvhqnq.by.files.1drv.com/y4mi_T9jRIMVjDGTeFeNxDS_RUCaVlcC1TkcgnS6gMO_Szp4LMFMk9J3SyXqnS_xsE7BSzRkABjvDgLot7DTY24aM72bIETvIbis7sY9ifld6ll5Q34slOitnXloBfvTYoF2m1R6iZSxJqhRtkMVZaYtyASzatpcHLddcI7Ps5y8Wua7-_CbBoFEQEnrIZeoRTSZBXkPMMIALDNjAWyG4CRpA?width=576&height=99&cropmode=none)

## 5. Ghi dữ liệu xuống database<a name="section5"></a>

Để ghi dữ liệu xuống database, trước tiên cần thêm input component vào file HTML, sau đó dùng axios đẩy nội dung trong input component về file **server/src/app.js** , cuối cùng là ghi dữ liệu xuống database. Giờ thì bắt tay vào làm thôi :3  
Để thêm input component chúng ta sửa file **client/src/components/ToDo.vue** theo nội dung như sau:

```javascript
<template lang="html">
  <div>
    <form v-on:submit='addTodo($event)'>
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

Sau khi submit VueJS sẽ truyền newToDo vào hàm addTodo để gọi hàm ToDoAPI.addTodo sau khi có kết quả trả về thì xóa chữ trong input. Kế tới, chúng ta sẽ tạo thêm addTodo(todo) trong **client/src/services/ToDoAPI.js**

```javascript
addTodo(todo) {
  return API().post("addTodo", {
   todo: todo // add our data to the request body
  });
},

```

Tiếp theo chúng ta cần bổ sung đường dẫn addTodo với giao thức post vào backend với nội dung file **server/src/app.js**

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
![rs1](https://wvhqnq.by.files.1drv.com/y4mi_T9jRIMVjDGTeFeNxDS_RUCaVlcC1TkcgnS6gMO_Szp4LMFMk9J3SyXqnS_xsE7BSzRkABjvDgLot7DTY24aM72bIETvIbis7sY9ifld6ll5Q34slOitnXloBfvTYoF2m1R6iZSxJqhRtkMVZaYtyASzatpcHLddcI7Ps5y8Wua7-_CbBoFEQEnrIZeoRTSZBXkPMMIALDNjAWyG4CRpA?width=576&height=99&cropmode=none)  
![rs2](https://wfhqnq.by.files.1drv.com/y4mFnk5l36u4NriWNRJZsaQpZrrqD0pNZskGhTSbskwDwhwmwaudcm8yguhqcnLhLDQmXJNrct0GBYwx1gORINVcWLYvrK18dee8MDM-QYQ7t-TfrVjw5LQ9Yuknk16KH10jNIWH7KOBbHGrMEtEqC-rlqKlZMeOydzY4wALtMhDSnzdmy7YtEu67zpVzCtXapfI934K71d1oNdCdCm8uzXkg?width=523&height=117&cropmode=none)

## 6. Xóa những note đã làm xong<a name="section6"></a>

Xóa những note đã làm xong là một trong những chức năn cơ bản của Sticky note. Để thực hiện chức năng này chúng ta cần làm các bước sau:  
Đầu tiên, tại file **client/src/components/Todo.vue** chúng ta đổi tag span thành tag input checkbox và bắt sự kiện click.

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

bước kế tiếp là định nghĩa hàm deleteTodo ở file **client/src/services/ToDoAPI.js** để điều phối đến backend

```javascript
deleteTodo (todoID) {
  return API().post('deleteTodo', {
    todoID: todoID // add our data to the request body
  })
}
```

và dĩ nhiên bước còn lại là định nghĩa đường dẫn /deleteTodo với giao thức post tại file **server/src/app.js**

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

Giờ thì chúng ta cùng xem kết quả đạt được có giống hình bên dưới không :3  
![del1](https://vlhqnq.by.files.1drv.com/y4mNixOcXhmJ_P9ucU1kAzPZpRG4vj-lSWZPnL30VaWKLreddCSKeY549-rEOUi81MWpTvW-wS0rTJRdVFGiTAl94ZzTBzoNEqNAHZGhI74gc47ants6hUvUuR3d0HRiqlDIxyqtv6Wd13u5lB2p0Br2fNnfAjmZHBUPWGWdY8063JIDLKENLm9S8U4r0v6Ym4By-kd2uBmd83qBHgv8vA4bA?width=254&height=130&cropmode=none)  
![del2](https://xvhqnq.by.files.1drv.com/y4mCBZPO2o2X5ZPzNIZTHlDBKgw3HMmZfgZJdyV9QBYFTaqCfUImJdhfs3tEB9Y2AQr95WaTcXjqE1kncb9PB_Ay9fL92ViVkhucm7yN6Wy9oNuk2e2E2i4kAHG9X-p5kF0-8C5AlAt-4vSp3k1Tzywcqbd2YAx0D-7mmjbrOMXhKO3WC5PaTUkZopw73LRJs3I68r-qDNggGWl32yYajyXpA?width=266&height=117&cropmode=none)

Yeah! tới đây chúng ta về cơ bản đã xong về mặt chức năng của Sticky note rồi nhưng bây giờ nhìn nó giống to do list nhiều hơn mà cũng xấu òm nữa. Vậy thì ở bước sau cũng là bước cuối cùng, chúng ta cùng trang trí màu mè hoa lá hẹ cho app được hoàn chỉnh nhé.

## 7. Màu mè hoa lá hẹ cho đẹp mắt<a name="section7"></a>

Search tìm trên mạng template HTML sticky note hên sao ra được 1 bài mà mình thấy cũng ưng ý, các bạn có thể xem chi tiết tại đây.
Sau khi chọn dc template thì gắn vào thôi.
Tạo hình sticky note:
Tại file **client/src/components/ToDo.vue** thay tag input check box thành tag a như dưới đây:

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
![sticknote](https://xfhqnq.by.files.1drv.com/y4mk-5f8zLK3UJlwWriZhLc_EVLo-BEy1Mt2DfZRILHPDRir5xE39UEPELdjzqf5SrNeE_t5ZvE2wUzDMUcVWRd2SHUuo1Y9bakbkiwHGYfKcUZHYA4EEj6r4XmTv9ReO7IOgySc20KChppNIt2lVfhBqOf8o7tnVdHNh5YFDxAShywZvmUKPZZE9s5MCkOiNw8dYkg0erqaV_DaQ-ruO8Opw?width=1366&height=576&cropmode=none)  
Kế tới là khung nhập nội dung, thêm đoạn css sau đây vào  
**client/src/components/ToDo.vue**

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

Tiếp đó chúng ta sẽ điều chỉnh lại form nhập theo đoạn code dưới đây:

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

Nếu để ý thì bạn sẽ thấy có tag font-awesome-icon cái này dùng để chèn icon viết note và hiển nhiên mặc định HTML không hỗ trợ tag này, để sử dụng chúng ta sẽ cài đặt thư viện  
**@fortawesome/fontawesome-svg-core**  
**fortawesome/free-solid-svg-icon**  
**@fortawesome/vue-fontawesome**

```bash
yarn add @fortawesome/fontawesome-svg-core fortawesome/free-solid-svg-icon @fortawesome/vue-fontawesome
```

sau khi cài xong thì chúng ta cần phải khai báo với VueJS bằng cách sửa lại file **client/src/main.js**

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
![text](https://w1hqnq.by.files.1drv.com/y4mf4GkBXGk7kYeXubuN271YnkMdw8xPFax-t69trO-J0Rp8V4cU_Dv9hd7iLLwW5jHWDKEytHWmr7ARIMQSMNG2yxu6cY8aiFfah1B_UZafSMq3-l_qh1cFkvwpI6cxy3A2cfXBiqCFxowmdGld0sWHAG8_hs2XC5c9_c-N2eHUjxiycho5prIB9UqbT43LJ1yRB0BNS3ZjwSFevZFWNNqpQ?width=1366&height=576&cropmode=none)  
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

![final](https://wlhqnq.by.files.1drv.com/y4maTZ1VPDeXqNiBcnYc5FyWdvuVFqStxi7cpxbOeodhDOpC12FQF65-ADUF0Ywl0LP_o66xHlyMq363LJMzJJktPvHRVRuSHTdjczPUwQqWdo_AM2tfQAjRYKDguuuxmuGgSzCL-1aynacb7_E_jKKnJ2kdlF_RtKxDsrUQ7_fN1u3f5vKlKHNIhwFbS_w8cp99Iznc1G8mBzQtUsc78ebOA?width=1366&height=576&cropmode=none)  
Tèn ten, như vậy chúng ta đã hoàn thành xong một web app Sticky note bằng stack MEVN hi vọng demo nhỏ này có thể tạo truyền cảm hứng để các bạn tạo ra những sản phẩm đẹp mắt và sáng tạo hơn.  
[Github code mẫu cho bạn nào cần nhé](https://github.com/anpt1992/demo/tree/master/mevn_stickynote_p2)  
Bài viết được lược dịch và sử dụng tài nguyên từ các bài viết:  
[MEVN Todo app](https://medium.com/@mattmaribojoc/creating-a-todo-app-with-a-mevn-full-stack-part-2-8180d944233a)  
[Html + css sticky note](https://code.tutsplus.com/tutorials/create-a-sticky-note-effect-in-5-easy-steps-with-css3-and-html5--net-13934)  
[Html + css form](https://codepen.io/huange/pen/rbqsD)
