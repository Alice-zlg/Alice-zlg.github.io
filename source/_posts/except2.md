---
title: "作业2,使用DOM简单操控页面"
date: 2024-10-28 12:00:00
tag: javascript
layout: "false"
---
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>判断用户名</title>
</head>
<style type="text/css">
        html, div, ul, li {
            margin: 0;
            padding: 0;
        }
        a {
            cursor: pointer;
        }
        li {
            list-style: none;
            cursor: pointer;
        }
        #cont_left {
            width: 300px;
            height: 500px;
            float: left;
        }
</style>

<div id="cont_left">
    <ul><a onclick="show('menu1')"><img src="https://t.tutu.to/img/tQO5"> 通过DOM获取信息 </a>
        <ul id="menu1" style="display: none">
            <li onclick="showImg()"><img src="https://t.tutu.to/img/t3Ir">获取原始图片路径</li>
            <li onclick="getFruit()"><img src="https://t.tutu.to/img/t3Ir">获取我喜欢的水果</li>
        </ul>
    </ul>
    <ul><img src="https://t.tutu.to/img/tQO5"><a onclick="show('menu2')"> 通过DOM操作元素 </a>
        <ul id="menu2" style="display: none">
            <li onclick="createImg()"><img src="https://t.tutu.to/img/t3Ir">创建图片</li>
            <li onclick="cloneImg()"><img src="https://t.tutu.to/img/t3Ir">克隆图片</li>
            <li onclick="changeImg()"><img src="https://t.tutu.to/img/t3Ir">改变图片</li>
            <li onclick="removeImg()"><img src="https://t.tutu.to/img/t3Ir">删除图片</li>
        </ul>
    </ul>
    <ul><img src="https://t.tutu.to/img/tQO5"><a onclick="show('menu3')"> 通过DOM操作样式 </a>
        <ul id="menu3" style="display: none">
            <li onclick="changeCss1()"><img src="https://t.tutu.to/img/t3Ir">为原始图片加上行间样式</li>
            <li onclick="changeCss2()"><img src="https://t.tutu.to/img/t3Ir">为所有的fieldset加上内部样式</li>
        </ul>
    </ul>
</div>
<fieldset>
    <legend>原始图片</legend>
    <img class="fruit" id="fruit" src="https://t.tutu.to/img/ttqG">
</fieldset>
<fieldset>
    <legend>图片路径</legend>
    <p id="msg1">在这里显示</p>
</fieldset>
<fieldset>
    <legend>选择你喜欢的水果</legend>
    <ul style="text-align: left;">
        <li style="float: left">
            <input name="enjoy" type="checkbox" value="苹果"/>苹果
        </li>
        <li style="float: left">
            <input name="enjoy" type="checkbox" value="香蕉" checked="checked"/>香蕉
        </li>
        <li style="float: left">
            <input name="enjoy" type="checkbox" value="葡萄"/>葡萄
        </li>
        <li style="float: left">
            <input name="enjoy" type="checkbox" value="梨" checked="checked"/>梨
        </li>
        <li style="float: left">
            <input name="enjoy" type="checkbox" value="西瓜"/>西瓜
        </li>
    </ul>
    <div id="msg2" style="margin-top: 10px;text-align: left;"></div>
</fieldset>
<fieldset>
    <legend>创建图片</legend>
    <div id="msg3"></div>
</fieldset>
<fieldset>
    <legend>克隆图片</legend>
    <div id="msg4"></div>
</fieldset>

{% raw %}
<script>
//菜单收缩与扩展
    function show(menuId) {
        const menu = document.getElementById(menuId);
        const allMenus = document.querySelectorAll('#cont_left ul ul');
        allMenus.forEach(function (m) {
            if (m !== menu) {
                m.style.display = 'none';
            }
        });
        menu.style.display = (menu.style.display === 'block' ? 'none' : 'block');
    }
//219971147 朱良桂
    //获取原始图片路径
    function showImg() {
        //获取路径
        //将路径在页面展示
        document.getElementById("msg1").innerHTML = document.getElementById("fruit").getAttribute("src");
    }

    //获取喜欢的水果
    function getFruit() {
        //用数组来存
        const enjoys = [];
        //获取复选框的被选中的选项
        const checkboxes = document.querySelectorAll('input[name="enjoy"]:checked');
        //遍历并去除选项的值
        checkboxes.forEach(function (checkbox) {
            enjoys.push(checkbox.value);
        });
        //将路径在页面展示
        document.getElementById("msg2").innerHTML = enjoys

    }

    //创建图片
    function createImg() {
        // 创建一个新的img元素
        const img = document.createElement('img');
        // 设置img元素的src属性
        img.src = 'https://t.tutu.to/img/tcQB';

        // 获取id为msg3的元素
        const msg3 = document.getElementById('msg3');
        // 用新创建的img元素替换id为msg3的元素
        msg3.parentNode.replaceChild(img, msg3);

    }

    //克隆图片
    function cloneImg() {
        // 克隆一个新的元素
        const img = document.getElementById('fruit');
        const newImg = img.cloneNode()

        // 获取id为msg3的元素
        const msg4 = document.getElementById('msg4');
        // 用克隆的元素替换id为msg3的元素
        msg4.parentNode.replaceChild(newImg, msg4);
    }

    //改变图片
    function changeImg() {
        // 创建一个新的img元素
        const img = document.createElement('img');
        // 设置img元素的src属性
        img.src = 'https://t.tutu.to/img/tcQB';

        // 获取id为fruit的元素
        const fruit = document.getElementById('fruit');
        // 用新创建的img元素替换id为msg3的元素
        fruit.parentNode.replaceChild(img, fruit);
    }

    //删除图片
    function removeImg() {
        // 获取id为fruit的元素
        const fruit = document.getElementById('fruit');
        //删除节点
        fruit.parentNode.removeChild(fruit)
    }

    //操作样式1
    function changeCss1() {
        // 获取id为fruit的元素
        const fruit = document.getElementById('fruit');
        fruit.style = "border: 5px solid aqua"
    }

    //操作样式2
    function changeCss2() {
        const fieldsets = document.getElementsByTagName("fieldset");
        for (let fieldset of fieldsets) {
            fieldset.style = "backGround: green"
        }

    }
</script>
{% endraw %}