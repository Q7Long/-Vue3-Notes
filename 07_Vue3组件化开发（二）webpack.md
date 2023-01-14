### 认识webpack

![image-20221205165944344](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C07_Vue3%E7%BB%84%E4%BB%B6%E5%8C%96%E5%BC%80%E5%8F%91%EF%BC%88%E4%BA%8C%EF%BC%89webpack.assets%5Cimage-20221205165944344.png)

```js
npm init -y
npm install webpack webpack-cli -D   // webpack 只有在开发的过程中才能使用 -D说明是开发依赖
// 对项目打包第一种方式 npx webpack 说明首先会在node_modules中查找webpack
npx webpack
// 第二种方式，在开发中我们在 package.json 中进行配置
"script":{
    "build":"webpack"   // 这里不需要添加 npx 因为在 script 脚本里面首先就会去 node_modules中查找
}
```

但是上面第二种方式的脚本里面没有指定路径，对 webpack 地址进行配置 webpack.config.js

```js
// webpack.config.js
const path = require('path');

modules.exports = {
    entry:"./src/main.js",
    output:{
        path:path.resolve(__dirname,"./build"),
        filename:"bundle.js"
    }
}
```

![image-20221205213350966](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C07_Vue3%E7%BB%84%E4%BB%B6%E5%8C%96%E5%BC%80%E5%8F%91%EF%BC%88%E4%BA%8C%EF%BC%89webpack.assets%5Cimage-20221205213350966.png)

![image-20221205213151816](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C07_Vue3%E7%BB%84%E4%BB%B6%E5%8C%96%E5%BC%80%E5%8F%91%EF%BC%88%E4%BA%8C%EF%BC%89webpack.assets%5Cimage-20221205213151816.png)

![image-20221205212915991](D:%5Cworkspace%5CQiLongZhang%5CVue%5CQ7Long%5CVue3%5C%E7%AC%94%E8%AE%B0%5C07_Vue3%E7%BB%84%E4%BB%B6%E5%8C%96%E5%BC%80%E5%8F%91%EF%BC%88%E4%BA%8C%EF%BC%89webpack.assets%5Cimage-20221205212915991.png)

