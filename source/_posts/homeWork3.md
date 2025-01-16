---
title: "练习3,用键盘操纵坦克移动"
date: 2024-10-28 12:00:00
tags: ["javascript","html","css"]
---

```javascript
    // 获取坦克标签
let tank = document.getElementById("mytank");
// 设置监听器监视键盘行为
document.addEventListener('keypress', function (e) {
    // 获取坦克当前位置
    //获取坦克的css样式
    let style = window.getComputedStyle(tank);
    //获取坦克top和left，并强转为number类型
    let top = parseInt(style.getPropertyValue("top"));
    let left = parseInt(style.getPropertyValue("left"));
    // 根据按键移动坦克
    //通过图片相对位置来限制坦克的移动
    if (e.key === "w") {
        //修改坦克的图片达到旋转的效果
        tank.src = "images/up.png";
        //通过对样式的修改达到移动的效果
        tank.style.top = (top - 10) + "px";
    } else if (e.key === "s") {
        tank.src = "images/down.png";
        tank.style.top = (top + 10) + "px";
    } else if (e.key === "a") {
        tank.src = "images/left.png";
        tank.style.left = (left - 10) + "px";
    } else if (e.key === "d") {
        tank.src = "images/right.png";
        tank.style.left = (left + 10) + "px";
    }
});
```

## 图片加载缓慢，没加载完成时坦克不会转向
### 指导手册，wsad 移动

<style type="text/css">
input {
    font-size: 26px;
    margin-top: 20px;
}
#map {
    height: 640px;
    width: 640px;
    background-image: url(https://t.tutu.to/img/ibDm);
}
    #mytank {
        position: relative;
        left: 10px;
        top: 100px;
}
</style>
<div id="map">
    <img id="mytank" src="https://t.tutu.to/img/tvzP"/>
</div>
{% raw %}
<script>
    // 获取坦克标签
    let tank = document.getElementById("mytank");
    // 设置监听器监视键盘行为
    document.addEventListener('keypress', function (e) {
        // 获取坦克当前位置
        //获取坦克的css样式
        let style = window.getComputedStyle(tank);
        //获取坦克top和left，并强转为number类型
        let top = parseInt(style.getPropertyValue("top"));
        let left = parseInt(style.getPropertyValue("left"));
        // 根据按键移动坦克
        if (e.key === "w" && top > 0 ) {
            //修改坦克的图片达到旋转的效果
            tank.src = "https://t.tutu.to/img/iwsX";
            //通过对样式的修改达到移动的效果
            tank.style.top = (top - 10) + "px";
        } else if (e.key === "s" && top < 560 ) {
            tank.src = "https://t.tutu.to/img/ixhj";
            tank.style.top = (top + 10) + "px";
        } else if (e.key === "a" && left > -280 ) {
            tank.src = "https://t.tutu.to/img/tdFA";
            tank.style.left = (left - 10) + "px";
        } else if (e.key === "d" && left < 280 ) {
            tank.src = "https://t.tutu.to/img/tvzP";
            tank.style.left = (left + 10) + "px";
        }
    });
</script>
{% endraw %}    