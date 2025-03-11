环境准备:

```javascript
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:'#id',
        data:{
            v: 1,
        }
        methods:{},
    })
```

使用/取值

```js
{{name}}
```

v-html

```javascript
<div v-html='msg'></div>
...
data:{
	msg:'<a href='#'>addr</a>'
}
```

v-show,v-if

控制元素显示隐藏

v-show='表达式'

v-show 通过display: none;来实现的 频繁切换的场景

v-if 直接删除元素 (条件渲染)

```html
<div v-show='flag'>XXX</div>
<div v-if='flag'>XXX</div>
```

v-else

v-else-if

```html
<span v-if="x >= 90">A</span>
<span v-else-if="x >= 50">B</span>
<span v-else>Cc</span>
app = new Vue({
    el:'#id',
    data:{
        
    }
})
```

v-on

```html
<button v-on:click="count++">
    按钮
</button>
简写:
<button @click="count++">
    按钮
</button>
链接函数:
<button @click="fun()">
    按钮
</button>
```

v-bind

动态设置属性

```html
<img v-bind:src="变量名">
简写:
<img :src="变量名">
```

v-for

多次渲染元素

```html
<ul>
    <li v-for="(item, index) in list">
        {{item}}, {{index}}
    </li>
</ul>

<ul>
    <li v-for="item in list" :key="item.id">
        {{item}}
    </li>
</ul>

data:{
	list:['1','2','3',]
}
```

js

```javascript
//根据条件保留数组元素
list = this.list.filter(item=> item.id!==id)
```

v-model

实现数据双向更新

```html
<input type="text" v-model="变量名">
```

指令修饰符

@keyup.enter -> 判断回车

v-bind对于样式控制的增强

```html
<div :class="{class1: 布尔值, }"></div>
<div :class="[class1, class2, ]"></div>
```

v-model应用于其他表单元素

input:text, textarea, input:checkbox, input:radio, select

单选框的name属性可以分组

value绑定单选框发送的值

计算属性

```vue
<p>{{count}}</p>
data:{
	list:[1,2,3,4]
}
computed:{
	count(){
	//计算和
		let NUM = this.list.reduce((sum, tiem)=>sum+item.num, 0)
		return NUM
	}
}
```

完整写法

```
computed:{
	//当被获取时运行
	get(){
		return valuse
	},
	//被赋值时运行
	set(value){
		this.name = value
	}
}
```

watch 监视器

简单写法:

```javascript
watch:{
	变量名 (newV, oldV){
		//变化执行的代码
		
	}
}
watch:{
	'类名.变量名'(newV, oldV){
		//变化执行的代码
		
	}
}
```

