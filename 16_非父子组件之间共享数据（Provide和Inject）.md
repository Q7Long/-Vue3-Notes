### Provide和Inject

```js
// 用于向子孙传数据，一般不用于兄弟之间传数据，对于简单的数据可以用于简单数据的传递，对于复杂的数据还是需要写成 Vuex
```

![image-20221206151729874](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C16_%E9%9D%9E%E7%88%B6%E5%AD%90%E7%BB%84%E4%BB%B6%E4%B9%8B%E9%97%B4%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE%EF%BC%88Provide%E5%92%8CInject%EF%BC%89.assets%5Cimage-20221206151729874.png)

- Provide/Inject用于非父子组件之间共享数据
  - 比如有<font color="red">一些深度嵌套的组件，子组件想要获取父组件的部分内容</font>
  - 在这种情况下，如果我们仍然<font color="red">将props沿着组件链逐级传递</font>下去，就会非常的麻烦
- 对于这种情况下，我们可以使用Provide的Inject
  - 无论层级结构有多深，父组件都可以作为其所有子组件的<font color="red">依赖提供者</font>
  - 父组件有<font color="red">一个 provide 选项</font>来提供数据
  - 子组件有<font color="red">一个 Inject 选项</font>来开始使用这些数据

![Provide和Inject](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CLEARN%20VUEJS%5C03_learn_component%5Csrc%5Cimg%5CProvide%E5%92%8CInject.jpg)

- 实际上，你可以将依赖注入看做是"ong range props"，除了：
  - 父组件不需要知道哪些子组件使用它provide的property
  - 子组件不需要知道 inject 的 property 来自哪里
  - 只需要知道引入数据就可以了，只需要知道引入数据就可以了

![image-20221206152734539](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C16_%E9%9D%9E%E7%88%B6%E5%AD%90%E7%BB%84%E4%BB%B6%E4%B9%8B%E9%97%B4%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE%EF%BC%88Provide%E5%92%8CInject%EF%BC%89.assets%5Cimage-20221206152734539.png)

![image-20221206153041750](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C16_%E9%9D%9E%E7%88%B6%E5%AD%90%E7%BB%84%E4%BB%B6%E4%B9%8B%E9%97%B4%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE%EF%BC%88Provide%E5%92%8CInject%EF%BC%89.assets%5Cimage-20221206153041750.png)

#### 总结

![image-20221206153431127](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C16_%E9%9D%9E%E7%88%B6%E5%AD%90%E7%BB%84%E4%BB%B6%E4%B9%8B%E9%97%B4%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE%EF%BC%88Provide%E5%92%8CInject%EF%BC%89.assets%5Cimage-20221206153431127.png)

![image-20221206153511880](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C16_%E9%9D%9E%E7%88%B6%E5%AD%90%E7%BB%84%E4%BB%B6%E4%B9%8B%E9%97%B4%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE%EF%BC%88Provide%E5%92%8CInject%EF%BC%89.assets%5Cimage-20221206153511880.png)

### 事件总线

#### 使用事件总线工具

```js
// ./utils/event-bus.js
import { HYEventBus } from "hy-event-store";

const eventBus = new HYEventBus()
		
// 这样就有了一个全局的 eventBus了
// 接下来就去 HomeBanner.vue里面导入
export default eventBus
```



![使用事件总线工具](D:/workspace/QiLongZhang/Vue/Q7Long/LEARN%2520VUEJS/03_learn_component/src/img/%25E4%25BA%258B%25E4%25BB%25B6%25E6%2580%25BB%25E7%25BA%25BF.png)
![取消事件总线](D:/workspace/QiLongZhang/Vue/Q7Long/LEARN%2520VUEJS/03_learn_component/src/img/%25E4%25BA%258B%25E4%25BB%25B6%25E5%258F%2596%25E6%25B6%2588.png)