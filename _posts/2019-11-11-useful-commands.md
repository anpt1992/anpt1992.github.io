---
layout: post
title: Useful commands
subtitle: for developer :D
bigimg: /img/code.jpg
tags: [tips, tricks]
---

**MongoDB**  
#1. Import large .json file

```
mongoimport -d <tên database> -c <tên collection> --file=<tên file json.json> --batchSize 1
```

{: .box-note}
**Note:** chỉ chạy trên cmd.exe
