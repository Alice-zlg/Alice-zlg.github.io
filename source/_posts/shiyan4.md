---
title: "实验4_web页面广告制作"
date: 2024-04-27 14:30:00
tags: ["javascript","css","html"]
---

```javascript    
function closeCouplet(coupletId) {
    // 获取标签
    let couplet = document.getElementById(coupletId);
    // 将标签隐藏,设置为空
    couplet.style.visibility = 'hidden';
}

// 定义变量用于保存定时器
let intervalId;

// 首次调用 setInterval 定时器
startInterval();

function startInterval() {
    intervalId = setInterval(down, 1);
}

function down() {
    // 获取广告标签
    let down = document.getElementById('couplet-down');
    // 获取广告标签样式
    let style = window.getComputedStyle(down);
    // 广告显现
    down.style.display = 'flex';
    // 获取广告当前位置
    let top = parseInt(style.getPropertyValue("top"));

    if (top > 320) {
        // 设置图片上移
        down.style.top = (top - 0.1) + "px";
    } else {
        // 当 top 小于等于 320 时，清除定时器
        clearInterval(intervalId);
    }
}
```

{% raw %}
<style>
    /* 所有元素都采用弹性布局，水平分布 */
    .all {
        display: flex;
        justify-content: space-between;
    }

    /* 左侧和右侧对联样式 */
    #couplet-left, #couplet-right {
        height: 300px;
        position: sticky; /* 让页面标签在滚动时固定 */
        top: 0;
        z-index: 1000; /* 确保在其他内容之上 */
    }

    /* 左侧和右侧对联的图片样式 */
    #img-left, #img-right {
        width: 200px;
        height: 300px;
    }

    /* 内容区域样式 */
    #content {
        width: 800px;
        height: 1500px;
        background-color: aqua;
    }

    /* 关闭按钮样式 */
    #close-couplet-left, #close-couplet-right,#close-couplet-down {
        background-color: black;
        color: white;
        position: absolute; /* 绝对定位 */
        top: 5px;
        right: 5px;
        border: none; /* 去除边框 */
        padding: 5px 10px; /* 上下 5px，左右 10px 的内边距 */
        cursor: pointer; /* 设置鼠标样式为手型 */
    }

    /* 下方对联样式 */
    #couplet-down {
        position: absolute; /* 绝对定位 */
        top: 2300px;
        left: 150px;
        z-index: 1000; /* 确保在其他内容之上 */
    }

    /* 下方对联的图片样式 */
    #img-down {
        width: 200px;
        height: 200px;
    }
</style>

<div class="all">
    <div id="couplet-left">
        <img id="img-left" src="https://t.tutu.to/img/t1KQ" alt="">
        <button onclick="closeCouplet('couplet-left')" id="close-couplet-left">关闭</button>
    </div>
    <div id="content">1</div>
    <div id="couplet-right">
        <img id="img-right" src="https://t.tutu.to/img/trE6" alt=""/>
        <button onclick="closeCouplet('couplet-right')" id="close-couplet-right">关闭</button>
    </div>
    <div id="couplet-down" style="display: none">
        <img src="https://t.tutu.to/img/tywq" id="img-down" alt="" />
        <button onclick="closeCouplet('couplet-down')" id="close-couplet-down">关闭</button>
    </div>
</div>

<script>
    function closeCouplet(coupletId) {
        let couplet = document.getElementById(coupletId);
        couplet.style.visibility = 'hidden';
    }
    let intervalId;

    startInterval();

    function startInterval() {
        intervalId = setInterval(down, 1);
    }

    function down() {
        let down = document.getElementById('couplet-down');
        let style = window.getComputedStyle(down);
        down.style.display = 'flex';
        let top = parseInt(style.getPropertyValue("top"));

        if (top > 1000) {
            down.style.top = (top - 0.1) + "px";
        } else {
            clearInterval(intervalId);
        }
    }
</script>
{% endraw %}