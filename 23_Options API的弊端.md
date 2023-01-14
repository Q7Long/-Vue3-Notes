### Options API的弊端

![image-20221207204542597](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C23_Options%20API%E7%9A%84%E5%BC%8A%E7%AB%AF.assets%5Cimage-20221207204542597.png)

```javascript
export default{
	// options api
	data(){
		return {
			counter:100	
		}
	},
	props:{

	},
	components:{

	},
	methods:{
		changeCounter(){
			counter +=100
		}
	},
	computed:{
		doubleCounter(){
			return this.counter *2
		}
	},
	watch(){
		counter(newValue,oldValue){

		}
	},
	created(){

	},
	mixin:{}
}
```
**options api 有个缺点就是，我们这里都是对counter的操作，但是这所有的操作又被分的太散了，如果下面我们需要对message操作，那么也是同样的办法**

- 在 Vu3中是依然支持 options api的，但是在Vue3中推出了一个新的东西，
Compositions api 其实就是一个setup函数
- Compositions API 想做到一件事就是，只有一个setup函数，甚至可以使用语法糖 <script setup></script>
```javascript
setup(){
	// 对counter所有操作，都在我这里给我完成
	const counter = ref(100)
	const doubleCounter = computed(()=>counter.value * 2)
	const changeCounter = ()=>{}
	watch()
}
// 好处
//1. 方便对操作进行维护
//2. 这里面其实就是JavaScript代码，那么我们就可以对这些代码进行抽取，放进一个函数里面
function userCounter(){
const counter = ref(100)
	const doubleCounter = computed(()=>counter.value * 2)
	const changeCounter = ()=>{}
	watch()
}
//3. 用的时候直接调用函数
setup(){
	userCounter()
}
```