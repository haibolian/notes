# 1. browserslistrc

处理css、js 兼容性，

https://caniuse.com/ 统计了浏览器市场的使用情况

到底要兼容哪些平台

browserslistrc可以返回指定条件的浏览器版本情况

其他模块会根据这些浏览器版本生成兼容代码

# 2. postcss

postcss - 

postcss-cli 手动进行

autoprefiexer -  为样式代码根据browserslist 规则增加前缀

Postcss-preset-env



# 3. html-webpack-plugin

DefinePlugin webpack内置从 webpack 包导入



# 4. babel

babel-loader

@babel/preset-evn

babel.config.js

# 5. polyfill

core-js

regeneration-runtime

Babel-config-js 设置

useBuiltIns: 

false: 不对当前 js 做 polyfill 填充

usage：  根据用户源代码进行填充

entry：根据browseslist进行填充

exclude: node_modules

## 6. copy-webpack-plugin

把资源文件 copy 到打包后的文件夹中

```js
import CopyWebpackPlugin from "copy-webpack-plugin"

new CopyWebpackPlugin({
	patterns:[
    {
      // 不用写 to，自己会去找 output 的目录
      from: 'public',
      globOptions: {
        ignore: ['**/index.html']
      }
    }
  ]
})
```



# 7. webpack-dev-server

--watch

```
serve: webpack server
```

# 8.webpack-dev-middleware



# 9. HMR

```
index.js

if(module.hot) {
	module.hot.accept(['./title.js'], ()=>{
		console.log('title.js 模块更新了')
	})
}
```

# 10. output — path

```
output:{
	filename: '',
	path: '',
	// publicPath: 打包后 index.html 内部的引用路径 
	// 域名 +  publicPath + filename
	publicPath: ''
	
}
```

devServer：

​	publicPath：开发环境的地址

​	contentBase： 打包后的文件引用的静态资源，

​	watchContentBase：true // 热更新静态资源

devServer常用配置

```
devServer:{
	hot: true, 
	hotOnly: true, // 单个模块语法报错不影响其他模块的状态，页面不报错，只是控制台报错。解决报错之后，不刷新整个页面，只对报错模块刷新
	port: 4000,
	open: false, // 自动打开页面，但每次更新都会打开
	compress: true, // 服务端压缩打包文件
	historyApiFallback：// localhost:3000/about 刷新页面时，页面会以为是访问服务端的 about 资源，但其实此时 about 是前端路由，开启historyApiFallback后会指定被替代的 index.html 
}
```

proxy：

target: 

pathReWrite:

changeOrigin:



Resolve:

extent...

alias: @



source-map

mode: development/production

devtool: 控制是否生成 source-map 以及如何生成

source-map 映射的技术



