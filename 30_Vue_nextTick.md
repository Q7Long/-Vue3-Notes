# 🍔什么是Vue.nextTick()？？

> ​                                                                                                                     -  made by Q7Long

**定义：**在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

所以就衍生出了这个获取更新后的 DOM 的 Vue 方法。所以放在 Vue.nextTick() 回调函数中的执行的应该是会对 DOM 进行操作的 js代码；

**理解：**nextTick()，是将回调函数延迟在下一次 dom 更新数据后调用，简单的理解是：当数据更新了，在 dom 中渲染后，自动执行该函数

## 🥨案例

![image-20230113142303907](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C30_nexttick.assets%5Cimage-20230113142303907.png)

### 🍟案例一

1. 代码部分

```vue
<template>
  <div>
    <h2 class="title" ref="titleRef">{{ message }}</h2>
    <button @click="getHeight">获取高度</button>
  </div>
</template>
<script>
import { ref } from "vue";
export default {
  setup() {
    const titleRef = ref(null);
    const message = ref("");
    const getHeight = () => {
      message.value += "Q7Long";
      console.log(titleRef.value.offsetHeight);
    };
    return {
      titleRef,
      getHeight,
      message,
    };
  },
};
</script>
<style>
.title {
  width: 100px;
}
</style>
```

2. 页面结果

![nexttick_获取内容高度](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C30_nexttick.assets%5Cnexttick_%E8%8E%B7%E5%8F%96%E5%86%85%E5%AE%B9%E9%AB%98%E5%BA%A6.gif)

```js
// 当我们点击按钮时，会立即给 message 增加一串文本，然后获取此时 h2 的高度
// 但是此时的 vue 还没有更新 dom，我们根本无法获取 dom 更新后的高度，只能获取此时 h2 的高度
// 我们可以看到，第一次我们 dom 已经更新了，但是获取的高度为 0
```

#### 🍿解决方案一：将回调函数放进 onUpdated 中

```vue
<template>
  <div>
    <h2 class="title" ref="titleRef">{{ message }}</h2>
    <button @click="getHeight">获取高度</button>
  </div>
</template>
<script>
import { ref, onUpdated } from "vue";
export default {
  setup() {
    const titleRef = ref(null);
    const message = ref("");
    const getHeight = () => {
      message.value += "Q7Long";
    };
    onUpdated(() => {
      console.log(titleRef.value.offsetHeight);
    });
    return {
      titleRef,
      getHeight,
      message,
    };
  },
};
</script>
<style>
.title {
  width: 100px;
}
</style>
```

页面效果:

![nexttick_获取内容高度放入onUpdated中](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C30_nexttick.assets%5Cnexttick_%E8%8E%B7%E5%8F%96%E5%86%85%E5%AE%B9%E9%AB%98%E5%BA%A6%E6%94%BE%E5%85%A5onUpdated%E4%B8%AD.gif)

这种方式虽然也能解决问题，但却不合适，在开发中会有很多个数据，那他们就会相互影响，比如增加一个 counter++ 的操作

```vue
<template>
  <div>
    <h2 class="title" ref="titleRef">{{ message }}</h2>
    <button @click="getHeight">获取高度</button>
    <h2>{{ counter }}</h2>
    <button @click="increment">+1</button>
  </div>
</template>
<script>
import { ref, onUpdated } from "vue";
export default {
  setup() {
    const counter = ref(0);
    const titleRef = ref(null);
    const message = ref("");
    const getHeight = () => {
      message.value += "Q7Long";
    };
    const increment = () => {
      counter.value++;
    };
    onUpdated(() => {
      console.log(titleRef.value.offsetHeight);
    });
    return {
      titleRef,
      getHeight,
      message,
      counter,
      increment,
    };
  },
};
</script>
<style>
.title {
  width: 100px;
}
</style>
```

虽然没有点击获取 message 的高度，但是由于 counter++ 引起的页面更新，获取高度的函数也会执行一遍

![nexttick_counter++引起的页面更新](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C30_nexttick.assets%5Cnexttick_counter++%E5%BC%95%E8%B5%B7%E7%9A%84%E9%A1%B5%E9%9D%A2%E6%9B%B4%E6%96%B0.gif)

#### 🍿解决方案二：使用 nexttick

```js
// 经过上述，希望达到的是，操作完数据之后，需要更新 DOM 再去获取高度即可

message.value += "Q7Long";
// 更新 DOM
console.log("获取message高度" + titleRef.value.offsetHeight);
```

1. 代码部分

```vue
<template>
  <div>
    <h2 class="title" ref="titleRef">{{ message }}</h2>
    <button @click="getHeight">获取高度</button>
    <h2>{{ counter }}</h2>
    <button @click="increment">+1</button>
  </div>
</template>
<script>
import { ref, onUpdated, nextTick } from "vue";
export default {
  setup() {
    const counter = ref(0);
    const titleRef = ref(null);
    const message = ref("");
    const getHeight = () => {
      message.value += "Q7Long";
      nextTick(() => {
        console.log("获取message高度" + titleRef.value.offsetHeight);
      });
    };
    const increment = () => {
      counter.value++;
    };
    return {
      titleRef,
      getHeight,
      message,
      counter,
      increment,
    };
  },
};
</script>
<style>
.title {
  width: 100px;
}
</style>
```

2. 页面效果

![nexttick_使用nextTick之后页面效果](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C30_nexttick.assets%5Cnexttick_%E4%BD%BF%E7%94%A8nextTick%E4%B9%8B%E5%90%8E%E9%A1%B5%E9%9D%A2%E6%95%88%E6%9E%9C.gif)

这时候再理解官方的解释就一目了然了：

**定义：**在下次 **DOM 更新循环结束之后执行延迟回调**。在修改数据之后立即使用这个方法，获取更新后的 DOM。

### 🍟什么时候需要用的Vue.nextTick()？？

1、Vue生命周期的 **created() 钩子函数进行的 DOM 操作一定要放在 Vue.nextTick() 的回调函数中**，原因是在 created() 钩子函数执行的时候 DOM 其实并未进行任何渲染，而此时进行 DOM 操作无异于徒劳，所以此处一定要将 DOM 操作的 js 代码放进 Vue.nextTick() 的回调函数中。与之对应的就是 mounted 钩子函数，因为该钩子函数执行时所有的 DOM 挂载已完成。

```vue
  created(){
    let that=this;
    that.$nextTick(function(){  //不使用this.$nextTick()方法会报错
        that.$refs.titleRef.innerHTML="created中更改了按钮内容";  //写入到DOM元素
    });
  },
```

2、当项目中你想在**改变DOM元素的数据后**基于新的dom做点什么，**对新DOM一系列的js操作都需要放进Vue.nextTick()的回调函数中；**通俗的理解是：更改数据后当你想立即使用js操作新的视图的时候需要使用它

## 🥨nextTick原理

### 🍟简述

1. *vue* 实现响应式并不是数据发生变化后 *DOM* 立即变化，而是按照一定策略异步执行 *DOM* 更新的

2. *vue* 在修改数据后，视图不会立刻进行更新，而是要等**同一事件循环机制**内所有数据变化完成后，再统一进行DOM更新

3. `nextTick` 可以让我们在下次 DOM 更新循环结束之后执行延迟回调，用于获得更新后的 DOM。

### 🍟事件循环机制

我们都知道 `JS引擎线程` 是专门用来解析 `JavaScript` 脚本的，所有的 `JavaScript` 代码都由这一个线程来解析。然而这个 `JS引擎` 是单线程的，也就意味着 `JavaScript` 程序在执行时，前面的必须处理好，后面的才会执行。

但是 `JavaScript` 中除了一些 `顺序执行` 的逻辑代码，还有很多 `异步任务`，比如 `Ajax请求`、`定时器` 等。如果 `JS引擎` 在单线程解析 `JavaScript` 时遇到了一个 `Ajax请求` ，那就必须等 `Ajax请求` 返回结果才能继续执行后续的代码，很显然这样的行为是非常低效的。

那为了解决这样的问题，`事件循环机制` 这样的技术就显得尤为重要：

```js
"JS引擎"在顺序执行"JavaScript"代码时，如果遇到"同步代码"立即执行；

如果遇到一些"异步任务"就会将这个"异步任务"交给对应的模块处理，然后继续执行后续代码；

当一个"异步任务"到达触发条件时就将该"异步任务"的回调放入"任务队列"中；

当"JS引擎"空闲以后，就会从"任务队列"读取和执行异步任务；
```

>**补充内容:**
>
>1. `JS引擎线程` 也被称为执行 `JS` 代码的 `主线程`，后续如果出现 `主线程` 这样的描述，指的就是 `JS引擎线程`。
>2. `任务队列` 属于数据结构中的队列，特性是 `先进先出`。

#### 🌭JavaScript任务的分类

![image-20230113191112790](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C30_nexttick.assets%5Cimage-20230113191112790.png)

在浏览器环境中，我们可以将我们的执行任务分为宏任务和微任务

- 宏任务： 包括`整体代码script`，`setTimeout`，`setInterval` 、`setImmediate`、 I/O 操作、UI 渲染

- 微任务： `Promise.then`、`MuationObserver`

![image-20230113180800518](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C30_nexttick.assets%5Cimage-20230113180800518.png)

#### 🌭任务的执行顺序

接着事件循环机制中 `JS` 引擎对这些任务的 `执行顺序` 描述如下：

- 步骤一： 从 `代码开始，到` 代码结束，按顺序执行所有的代码。
- 步骤二： 在 `步骤一` 顺序执行代码的过程中，如果遇到 `同步任务` ，立即执行，然后继续执行后续代码；如果遇到 `异步任务`，将 `异步任务` 交给对应的模块处理（ `事件` 交给 `事件处理线程` ，`ajax` 交给 `异步HTTP请求线程`），当 `异步任务` 到达触发条件以后将 `异步任务` 的 `回调函数` 推入 `任务队列`（ `宏任务` 推入 `宏任务队列`，`微任务` 推入 `微任务队列`）。
- 步骤三：`步骤一 `结束后，说明同步代码执行完毕。此时读取并执行 `微任务队列` 中保存的所有的`微任务`。
- 步骤四： `步骤三` 完成后读取并执行 `宏任务队列` 中的`宏任务`，每执行完一个 `宏任务 `就去查看 `微任务队列` 中是否有新增的 `微任务`，如果存在则重复 `步骤三`；如果不存在，继续执行下一个 `宏任务`。

![image-20230113185706086](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C30_nexttick.assets%5Cimage-20230113185706086.png)

![image-20230113200008615](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C30_nexttick.assets%5Cimage-20230113200008615.png)

>**补充说明 ！！！**
>
>1. 步骤四中描述的 `新增的微任务` 和步骤三中描述的 `微任务` 是一样的，因为 `异步任务` 只有满足条件以后才会被推入 `任务队列`， `步骤三` 在执行时，不一定所有的 `微任务` 都 `到达触发条件` 而被推入 `任务队列`；
>
>2. 所谓的 `到达触发条件` 指的是下面这几种情况：
>
>     ① 定时器：定时器设置的时间到达，才会将定时器的回调函数推入任务队列中
>
>     ② DOM事件：DOM绑定的事件被触发以后，才会将事件的回调函数推入任务队列中
>
>     ③ 异步请求：异步请求返回结果以后，才会将异步请求的回调函数推入任务队列中
>
>     ④ 异步任务之间的互相嵌套：比如 `宏任务A` 嵌套 `微任务X`，当 `宏任务A` 对应的回调函数代码没有被执行到的时候，很显然根本不存在 `微任务X`；只有 `宏任务A`对应的 `回调函数` 代码被执行以后，`JS` 引擎才会解析到 `微任务X`，此时依然是将该 `微任务X`交给对应的线程去处理，当 `微任务X` 满足前面描述的①、②、③的条件，才会将`微任务X`对应的回调推入 `任务队列`，等待 `JS引擎` 去执行。
>
>3. 所有的 `异步任务`都在 `JS引擎` 遇到 `</script>` 以后才会开始执行。
>
>4. `宏任务` 对应的英文描述为 `task`，`微任务` 对应的英文描述为 `micro task`；`宏任务队列 `描述为 `task quene`，`微任务队列 `描述为 `micro task quene`。

### 🍟关于为什么执行console.log() 高度的时候 nextTick() 就可以解决问题

![image-20230113212403925](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C30_nexttick.assets%5Cimage-20230113212403925.png)

### 🍟总结

Vue 是异步执行 dom 更新的，一旦观察到数据变化，Vue 就会开启一个队列，然后把在同一个事件循环 (event loop)  当中观察到数据变化的 watcher 推送进这个队列。如果这个 watcher 被触发多次，只会被推送到队列一次。这种缓冲行为可以有效的去掉重复数据造成的不必要的计算和 DOM操作。而在下一个事件循环时，Vue会清空队列，并进行必要的DOM更新。
当你设置 `vm.someData = 'new value'`，DOM 并不会马上更新，而是在异步队列被清除，也就是下一个事件循环开始时执行更新时才会进行必要的 DOM 更新。如果此时你想要根据更新的 DOM 状态去做某些事情，就会出现问题。。为了在数据变化之后等待 Vue 完成更新 DOM ，可以在数据变化之后立即使用 Vue.nextTick(callback) 。这样回调函数在 DOM 更新完成后就会调用。

