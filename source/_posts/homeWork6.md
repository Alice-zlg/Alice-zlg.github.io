---
title: "练习5,使用jQuery制作游戏角色"
date: 2024-05-12 23:25:00
tags: [ "javascript","html","css" ]
layout: "false"
---

<style>
        .map {
            background-color: black;
            height: 80em;
            width: 50em;
        }
        #role {
            height: 10em;
            width: 10em;
            position: relative; /* 将图片设置为相对定位 */
            z-index: 1; /* 将角色置于最上层 */
        }
</style>
<meta charset="UTF-8">
<div>
<span>操作手册：左键双击变身，右键移动</span>
<span>图片加载缓慢，效果展示第一次有延迟</span>
<div class="map">
    <img id="role" src="https://t.tutu.to/img/tEJj" alt="">
</div>
</div>
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
{% raw %}
<script>
$(document).ready(function () {
        let map = $(".map");
        let role = $("#role");
        // 禁用游戏区域中的右键开启菜单
        $(document).contextmenu(function (e) {
            // 如果右键点击的区域为游戏区域则禁用右键菜单
            return !$(e.target).hasClass("map");
        });
        // 标记双击事件是否已触发
        let doubleClickTriggered = false;
        // 设置鼠标点击事件
        map.mousedown(function (e) {
            // 当鼠标右键点击时
            if (3 === e.which) {
                // 获取鼠标此时坐标
                let x = e.pageX;
                let y = e.pageY;
                let roleX = role.offset().left + 80;
                if (x <= roleX) {
                    // 修改人物动画
                    role.attr("src", "https://t.tutu.to/img/tEJj");
                } else {
                    role.attr("src", "https://t.tutu.to/img/tXqP");
                }
                // 如果双击事件已触发，则统一使用双击事件的图片
                if (doubleClickTriggered) {
                    if (x <= roleX) {
                        role.attr("src", "https://t.tutu.to/img/tZ54");
                    } else {
                        role.attr("src", "https://t.tutu.to/img/tGTS");
                    }
                }
                role.stop().animate({
                    marginLeft: x,
                    marginTop: y
                }, 3000);
            }
        });
        // 设置鼠标双击事件
        map.on("dblclick", function (e) {
            // 当鼠标左键双击时
            if (1 === e.which) {
                // 修改角色图片
                if (doubleClickTriggered) {
                    role.attr("src", "https://t.tutu.to/img/tGTS");
                } else {
                    role.attr("src", "https://t.tutu.to/img/tZ54");
                }
// 标记双击事件已触发
doubleClickTriggered = true;
}
});
});
</script>
{% endraw %}