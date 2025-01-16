---
title: "作业1,使用循环打印基础图形"
tag: "javascript"
date: "2024-4-6"
---

# 作业1

### 使用循环打印基础图形

```javascript
let number = 5;
number = window.prompt("请输入生成的行数")

// 生成n个空格
function generateSpaces(n) {
    let spaces = "";
    for (let i = 0; i < n; i++) {
        spaces += " ";
    }
    return spaces;
}

// 生成n个星号
function generateStars(n) {
    let stars = "";
    for (let i = 0; i < n; i++) {
        stars += "* ";
    }
    return stars;
}

// 打印等腰三角形
function printTriangle(rows) {
    let triangle = "";

    for (let i = 1; i <= rows; i++) {
        let spaces = generateSpaces(rows - i);
        let stars = generateStars(i);
        triangle += spaces + stars + "\n";
    }
    document.write("<pre>" + triangle + "</pre>");
}

// 打印平行四边形
function printParallelogram(rows) {
    let parallelogram = "";
    for (let i = 1; i <= rows; i++) {
        let spaces = generateSpaces(rows - i);
        let stars = generateStars(rows);
        parallelogram += spaces + stars + "\n";
    }
    document.write("<pre>" + parallelogram + "</pre>");
}

// 打印菱形
function printDiamond(rows) {
    let midRow = Math.floor(rows / 2);
    let diamond = "";
    // 打印上半部分
    for (let i = 0; i < midRow; i++) {
        let spaces = generateSpaces(2 * midRow - 2 * i);
        let stars = generateStars(i * 2 + 1);
        diamond += spaces + stars + "\n";
    }
    // 打印中间行
    if(rows%2 === 0){
        rows++;
    }
    diamond += generateStars(rows) + "\n";

    // 打印下半部分
    for (let i = midRow - 1; i >= 0; i--) {
        let spaces = generateSpaces(2 * midRow - 2 * i);
        let stars = generateStars(i * 2 + 1);
        diamond += spaces + stars + "\n";
    }
    document.write("<pre>" + diamond + "</pre>");
}

// 调用打印函数
printTriangle(number);
printParallelogram(number);
printDiamond(number);

```
{% raw %}
<script>
let number = 5;
number = window.prompt("请输入生成的行数")

// 生成n个空格
function generateSpaces(n) {
    let spaces = "";
    for (let i = 0; i < n; i++) {
        spaces += " ";
    }
    return spaces;
}

// 生成n个星号
function generateStars(n) {
    let stars = "";
    for (let i = 0; i < n; i++) {
        stars += "* ";
    }
    return stars;
}

// 打印等腰三角形
function printTriangle(rows) {
    let triangle = "";

    for (let i = 1; i <= rows; i++) {
        let spaces = generateSpaces(rows - i);
        let stars = generateStars(i);
        triangle += spaces + stars + "\n";
    }
    document.write("<pre>" + triangle + "</pre>");
}

// 打印平行四边形
function printParallelogram(rows) {
let parallelogram = "";
for (let i = 1; i <= rows; i++) {
let spaces = generateSpaces(rows - i);
let stars = generateStars(rows);
parallelogram += spaces + stars + "\n";
}
document.write("<pre>" + parallelogram + "</pre>");
}

// 打印菱形
function printDiamond(rows) {
let midRow = Math.floor(rows / 2);
let diamond = "";
// 打印上半部分
for (let i = 0; i < midRow; i++) {
let spaces = generateSpaces(2 * midRow - 2 * i);
let stars = generateStars(i * 2 + 1);
diamond += spaces + stars + "\n";
}
// 打印中间行
if(rows%2 === 0){
rows++;
}
diamond += generateStars(rows) + "\n";

    // 打印下半部分
    for (let i = midRow - 1; i >= 0; i--) {
        let spaces = generateSpaces(2 * midRow - 2 * i);
        let stars = generateStars(i * 2 + 1);
        diamond += spaces + stars + "\n";
    }
    document.write("<pre>" + diamond + "</pre>");
}

// 调用打印函数
printTriangle(number);
printParallelogram(number);
printDiamond(number);
</script>
{% endraw %}