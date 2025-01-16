---
title: "作业5，使用jQuery修改表格"
date: 2024-05-08 22:47:00
tags: [ "javascript","css","html" ]
---

<table border="0" cellspacing="0" cellpadding="0" id="myTable">
    <tr id="row1">
        <td>书名</td>
        <td>价格</td>
    </tr>
    <tr id="row2">
        <td>看得见风景的房间</td>
        <td class="center">&yen;30.00</td>
    </tr>
    <tr id="row3">
        <td>60个瞬间</td>
        <td class="center">&yen;32.00</td>
    </tr>
</table>
<input name="b1" type="button" value="增加一行" onclick="addRow()" />
<input name="b2" type="button" value="删除第2行" onclick="delRow()" />
<input name="b3" type="button" value="修改标题样式" onclick="updateRow()" />
<input name="b4" type="button" value="复制最后一行" onclick="copyRow()" />

<style type="text/css">
        body {
            font-size: 13px;
            line-height: 25px;
        }

        table {
            border-top: 1px solid #333;
            border-left: 1px solid #333;
            width: 300px;
        }

        td {
            border-right: 1px solid #333;
            border-bottom: 1px solid #333;
        }

        .center {
            text-align: center;
        }
</style>

```javascript
function addRow() {
    // 创建新节点
    let newRow = $("<tr><td>幸福从天而降</td><td align='center'>&yen;18.50</td></tr>");
    // 将节点添加到指定标签后
    $("#row1").parent().append(newRow);
}

// 219971147朱良桂
function updateRow() {
    // 获取标签后，设置css
    $("#row1").css({"font-weight": "bold", "text-align": "center", "background-color": "#cccccc"});
}

function delRow() {
    let dRow = document.getElementsByTagName("tr"); //访问被删除的行
    if (dRow[2] != null) {
        // 删除指定节点
        dRow[2].remove();
    }
}

function copyRow() {
    // 获取table标签的所有子节点
    let lastRow = $("#myTable tr:last-child");
    // 克隆节点
    let newRow = lastRow.clone();
    // 将节点添加到指定标签之后
    $("#myTable").append(newRow);
}
```

{% raw %}

<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.slim.min.js"></script>

<script>
        function addRow() {
            // 创建新节点
            let newRow = $("<tr><td>幸福从天而降</td><td align='center'>&yen;18.50</td></tr>");
            // 将节点添加到指定标签后
            $("#row1").parent().append(newRow);
        }
        // 219971147朱良桂
        function updateRow() {
            // 获取标签后，设置css
            $("#row1").css({"font-weight": "bold", "text-align": "center", "background-color": "#cccccc"});
        }

        function delRow() {
            let dRow = document.getElementsByTagName("tr"); //访问被删除的行
            if(dRow[2]!=null){
                // 删除指定节点
                dRow[2].remove();
            }
        }

        function copyRow() {
            // 获取table标签的所有子节点
            let lastRow = $("#myTable tr:last-child");
            // 克隆节点
            let newRow = lastRow.clone();
            // 将节点添加到指定标签之后
            $("#myTable").append(newRow);
        }
</script>
{% endraw %}