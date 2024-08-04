



# DOM

class

``` js
var element = document.getElementById('myDiv')
element.className	// 获取元素节点的class属性，返回 'class1 class2 class3'
element.className += 'class4'	// 添加 calss
element.className += element.className.replace(/^class4$/, '')	// 移除 calss

element.classList	// 返回一个类似数组的对象 { 0: 'class1', 1: 'class2', 2: 'class3', length: 3}
// classList 还有以下方法
element.classList.add('newClass')	// 添加一个 class
element.classList.add('newClass1', 'newClass2')	// 添加多个 class
element.classList.remove('newClass')	// 移除 一个class
element.classList.toggle('newClass') // 如果 newClass 不存在就加入，否则移除。接收第二个参数，bool 为 true 添加，false 删除
element.classList.contains('newClass') // 是否包含某个 class，返回 true 或者 false
element.classList.item(0) // 返回第一个 Class
element.classList.toString()	// 返回 class 列表的字符串
```



``` js
element.clientHeight	// 返回元素的高度（只包含 padding），只对块级元素生效，对于行内元素返回0。如果块级元素没有设置 CSS 高度，则返回实际高度。
element.clientWidth	// 同 clientHeight 类似，返回元素宽度
document.documentElement.clientHeight	// 返回浏览器窗口高度（可视部分）
document.body.clientHeight	// 网页总高度

element.clientLeft	// 左 border 的宽度
element.clientTop	// 顶部 border 的宽度

element.scrollHeight	// 元素总高度，包括 padding 溢出容器、当前不可见的部分，但不包含 border、margin 以及水平滚动条的高度。只读
element.scrollWidth	// 同 element.scrollHeight，返回元素总宽度
// 返回网页的总高度
document.documentElement.scrollHeight
document.body.scrollHeight

element.scrollLeft	//
element.scrollTop	//	


```

