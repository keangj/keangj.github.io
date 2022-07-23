# VUE 3



## 组合 API

### setup

`setup` 选项是一个接收 `props` 和 `context` 的函数

### ref



### reactive

### readonly

Readonly 会返回原生对象的只读代理（它依然是一个 Proxy，这是一个 proxy 的 set 方法被劫持，并且不能对其进行修改）

``` vue 
setup {
	const info = { name: 'jay' }	// or const info = reactive({ name: 'jay' })
	const readonlyInfo = readonly(info)
	const update = () => {
		info.name = 'kang'	// warning
	}
}
```



### shallowRef



### shallowReactive



### toRaw



### markRaw



### toRef



### toRefs

### customRef



## 生命周期





