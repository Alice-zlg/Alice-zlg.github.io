---
title: "作业2，闭包"
tag: javascript
date: 2024-4-6
---

## 作业2

### 简单的闭包的实现

```javascript
function fn() {
    var num = 0;
    var c = function () {
        return ++num;
    };
    return c;
}

var count = fn();
console.log(count())
console.log(count())
console.log(count())
```


