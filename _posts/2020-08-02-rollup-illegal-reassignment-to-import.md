---
layout: post
title:  "rollup error illegal reassignment to import"
date:   2020-05-09 10:18:50 +0800
categories: frontend
---

### Rollup报错
> Error: Illegal reassignment to import 'XXX'
解释: 非法重新给从"XXX"包import的变量赋值
### 报错例子
Entry
```javascript
import {bar} from "./bar.js";

export var Foo = {
    setBar: function (value) {
        bar = value;
    },
    logBar: function () {
        console.log(bar);
    }
}
```
bar.js
```javascript
export var bar = 42;
```

在Entry文件中对从bar.js中import来的bar进行赋值(setBar函数), 所以报错.

### 原因解释
imports是只读的, 使用imports导入东西就好象用const声明它一样, 因此无法修改.
### 参考
https://exploringjs.com/es6/ch_modules.html#sec_imports-as-views-on-exports