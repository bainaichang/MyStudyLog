通过类选择器来获取DOM元素:

```js
document.querySelector('.classname')
```

通过id获取DOM

```js
document.getElementById('id')
```

计时器:

```js
//可以通过timer关闭计时器
timer = setInterval(()=>{语句},时间间隔)
clearInterval(timer)
//一次性计时器
setTimeout(function(),1000)

```

一些常用事件:

鼠标:click, mouseenter 鼠标经过, mouseleave 鼠标离开

焦点: focus 获得焦点, blur 失去焦点

键盘: Keydown 按下, Keyup 抬起

表单: input 用户输入

```js
document.getElementById('input').addEventListener('keydown',e =>{
            document.getElementById('head').innerHTML = document.getElementById('input').value
})
```

过度效果

```css
transition: all .3s;
//文本域默认值
placeholder="a"
```

查找节点

```js
//父类
li.parentNode
//子类
ul.children
//兄弟节点
li.previousElementSibling//上一个
li.nextElementSibling//下一个
```

创建节点

```js
const div = document.createElement('div')
document.body.appendChild(div)
//选择插入到谁前面
父元素.insertBefore(插入的元素, 哪个元素的前面)
//克隆节点
元素.cloneNode(是否克隆子元素)
```

删除节点

```js
父元素.removeChild(被删除的元素)

```

fetch发送请求

```js
//普通版
fetch('http://ajax-base-api-t.itheima.net/api/getbooks').then(resp =>{
      console.log(resp);
      return resp.json()
    }).then(json =>{
      console.log(json);
      resArr = json.data
      resArr.forEach(e => {
        s = JSON.stringify(e)
        console.log(s);
      });
    })
//选择方法和设置请求头
fetch('http://ajax-base-api-t.itheima.net/api/getbooks',{
    method: 'post',
    headers: {
        'Content-Type': 'application/json'
    },
    body: 请求体数据(可以放json数据)
}).then(resp =>{
      console.log(resp);
      return resp.json()
    }).then(json =>{
      console.log(json);
      resArr = json.data
      resArr.forEach(e => {
        s = JSON.stringify(e)
        console.log(s);
      });
    })
```

![image-20250119151327579](C:\Users\白乃常\AppData\Roaming\Typora\typora-user-images\image-20250119151327579.png)

![image-20250119151753837](C:\Users\白乃常\AppData\Roaming\Typora\typora-user-images\image-20250119151753837.png)

js 原生dom修改css样式

```js
dom.style
如果是 长一点的属性名字的话 是通过 驼峰的写法
比如。“backgroun d-color” => backgroundColor

var bottom = document.querySelector('#bottom');
bottom.style.backgroundColor = '#aaa'

setAttribute的方式
 bottom.setAttribute('style', 'background-color: #0f0;')

dom.style.cssText
bottom.style.cssText = 'background-color: #00f;color: #f00;'

重写className的方式 修改样式
bottom.className = 'red'

这种方式怎么说 很有可能会覆盖之前写好的样式。最好的方式还是添加上去比较好

这个时候可以通过 classList的方式追加上去。
dom.classList.add(className); // 添加一个类名
dom.classList.remove(className); // 移除一个类名
bottom.classList.add('red')

```

AJAX

```javascript
$.ajax({
	url: "",
    data: JSON.stringify(data),
    
})
```

```
typedf(Storage)!=="uundefined"
	localStorage.setItem("key",value)
localStroage.getItem("key")
```

