关于form表单

```html
<form action="/login" method="get" enctype="类型">
        username:<input type="text"><br>
        password:<input type="password"><br>
        <input type="submit" id="submit" value="提交">
</form>
```

关于table表格

一个好看的表格

```css
body {  
            font-family: Arial, sans-serif;  
            margin: 20px;  
        }  
  
        table {  
            width: 100%;  
            border-collapse: collapse;  
            margin: 20px 0;  
            font-size: 1em;  
            text-align: left;  
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);  
        }  
  
        table thead tr {  
            background-color: #009879;  
            color: #ffffff;  
            text-align: left;  
        }  
  
        table th, table td {  
            padding: 12px 15px;  
            border: 1px solid #dddddd;  
        }  
  
        table tbody tr {  
            border-bottom: 1px solid #dddddd;  
        }  
  
        table tbody tr:nth-of-type(even) {  
            background-color: #f3f3f3;  
        }  
  
        table tbody tr:last-of-type {  
            border-bottom: 2px solid #009879;  
        }  
  
        table tbody tr.active-row {  
            font-weight: bold;  
            color: #009879;  
        }  
  
        table tbody tr:hover {  
            background-color: #f1f1f1;  
        } 
```

thead => 第一行

tr->一行

th->表头的一列

td->表体的一列

```html
<table>
        <thead>
            <tr>
                <th>id</th>
                <th>name</th>
                <th>age</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td>Mark</td>
                <td>18</td>
            </tr>
            <tr>
                <td>2</td>
                <td>White</td>
                <td>35</td>
            </tr>
        </tbody>
    </table>
```

CSS

选择器

```css
p{
   color: red;
}
标签{
    属性: 值;
}
-------------
.类名{
    ...
}
.MyRed{
    ...
}
->如何使用
<div class="MyRed">
	红色字体
</div>
-------------
->使用多个类
<div class="classname1 clnm2">
	字
</div>
-------------
id选择
只能用一次
#idName{
    ...
}
->使用
<div id="idName"></div>
*{
    
}
选择所有
```

字体

```css
p{
	font-family:"微软雅黑";//字体
	font-size: 20px;//文字大小
	font-weight: bold;//粗细
}
p{
    font: font-style font-weight font-size/line-height font-family;
}
div {
    color: #ffaaff;
    color: rgb(255,255,255);
}
```

引入方式

```html
<style>
    ...
</style>
---------------
<div style="color: red; font-size: 12px;">
    testxxxxxxxxx
</div>
---------------
<link rel="stylesheet" href="./demo-1.css">
```

复合选择器

```css
ol li {
    color: pink;
}
--------
只会选ol下的a
ol > a{
    ...
}
----------
一块变化
a, span {
    ...
}
-----------
伪类选择器
a:link {
    
}
a:....

```

元素显示模式

```css
a {
    display: block;
    display: inline;
    display: inline-block;
}
```

一个好看的表格

```css
<style>
        table,
        td,
        th {
            text-align: center;
            font-style: italic;
            border: 3px solid pink;
            border-collapse: collapse;
        }

        th {
            font-style: normal;
            background-color: aquamarine;
            font-weight: 700;
        }
        table {
            height: 100px;
            width: 500px;
        }

/*
*** 以下是一个美观且带有交互效果的蓝色按钮CSS样式
	.blue-btn {
    padding: 12px 24px;
    border: none;
    border-radius: 8px;
    background: linear-gradient(135deg, #4A90E2, #2B6CD4);
    color: white;
    font-size: 16px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 0 4px 12px rgba(43, 108, 212, 0.3);
    position: relative;
    overflow: hidden;
    user-select: none;
}
// 好看的加载动画
.load {
  width: 50px;
  height: 50px;
  margin: 0 auto;
  position: relative;
  border-radius: 50%;
  overflow: hidden;
  background-color: rgba(0, 169, 178,.2);;
  &::before {
    content: "";
    width: 70px; // 50 * √2
    height: 70px; // 50 * √2
    background-color: #00adb5;
    position: absolute;
    left: 50%;
    bottom: 50%;
    z-index: 1;
    transform-origin: left bottom;
    animation: rotate 1.5s infinite linear;
  }
  &::after {
    content: "";
    width: 40px;
    height: 40px;
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    margin: auto;
    background-color: #fff;
    z-index: 2;
    border-radius: 50%;
  }
}
@keyframes rotate {
  0% {
    transform: rotate(0);
  }
  50% {
    transform: rotate(180deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

//
/* 悬停效果 */
.blue-btn:hover {
    background: linear-gradient(135deg, #2B6CD4, #4A90E2);
    box-shadow: 0 6px 16px rgba(43, 108, 212, 0.4);
    transform: translateY(-1px);
}

/* 点击效果 */
.blue-btn:active {
    transform: translateY(2px);
    box-shadow: 0 2px 6px rgba(43, 108, 212, 0.3);
}

/* 可选 - 添加点击涟漪效果 */
.blue-btn::after {
	width: 80px;
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    width: 5px;
    height: 5px;
    background: rgba(255, 255, 255, 0.5);
    border-radius: 50%;
    transform: translate(-50%, -50%) scale(0);
    animation: ripple 0.6s ease-out;
}

@keyframes ripple {
    from {
        transform: translate(-50%, -50%) scale(0);
        opacity: 1;
    }
    to {
        transform: translate(-50%, -50%) scale(20);
        opacity: 0;
    }
}
*/
    </style>
```

```html
<table>
        <thead>
            <tr>
                <th>id</th>
                <th>name</th>
                <th>age</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td>Mark</td>
                <td>18</td>
            </tr>
            <tr>
                <td>2</td>
                <td>White</td>
                <td>35</td>
            </tr>
        </tbody>
    </table>
```

float

清除浮动

1.添加标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>clearFloat</title>
    <style>
        .box {
            border-style: dashed;
            width: 500px;

        }
        .in {
            background-color: pink;
            width: 50px;
            height: 50px;
            float: left;
        }
        .clear{
            clear: left;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="in">1</div>
        <div class="in">2</div>
        <div class="in">3</div>
        <div class="clear"></div>
    </div>
</body>
</html>
```

2.给父级元素添加属性

```css
.box {
            overflow: hidden;
            border-style: dashed;
            width: 300px;
        }
```

Flex 技术

```css
display: flex;
/*这是y轴, 默认为x轴*/
flex-direction: column;
/*设置主轴上的子元素排列方式*/
justify-content: ;
/*设置子元素是否换行,默认不换行*/
flex-wrap: wrap;
/*设置子元素在侧轴上的排列方式(单行的情况)*/
align-items: ;
/*设置子元素在侧轴上的排列方式(多行的情况)*/
align-content: ;
/*排列和换行的组合*/
flex-flow: row wrap;
```

