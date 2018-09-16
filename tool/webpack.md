# Webpack

---
[原文](https://www.jianshu.com/p/42e11515c10f)
> **webpack** *用于打包*
> **babel** *编译js的平台*

## 安装

```cpp
//webpack
npm install -g webpack//全局安装
npm install --save-dev webpack webpack-cli//安装到项目目录
npm init//初始化package.json文件
npm install --save-dev webpack-dev-server//webpack-dev-server

//babel
npm install --save-dev babel-core babel-loader@7.1.5 babel-preset-env//babel-loader^8.*会报@babel/core的错
npm install --save-dev babel-react react react-dom//主流框架解析

//css
npm install --save-dev css-loader style-loader//css-loader实现类似require()，style-loader将样式引入页面

```

## 打包

### 命令打包

```cpp
webpack ./app/main.js ./public/build.js//打包webpackv.0<4
webpack --entry ./app/main.js --output ./public/build.js//打包 webpack4.0
```

### webpack.config.js

```javascript
module.exports = {
  devtool: "eval-source-map",
  entry: __dirname + '/app/main.js',
  output: {
    path: __dirname + '/public',
    filename: 'build.js'
  },
  devServer: {
    contentBase: './public',
    port: 8000,//请求http://localhost:8000/build.js即可
    inline: true,
    historyApiFallback: true
  },
  module:{
  rules:[{
  test:/(\.js|\.jst)$/,
  use:{
    loader:'babel-loader',
    options:{
    presets:['env','vue']
    }
},
exclude:/node_modules/
  }]
  }
}
```

## package.json

```json
{
  "name": "webpackcode",
  "version": "1.0.0",
  "description": "",
  "main": "app/main.js",
  "dependencies": {},
  "devDependencies": {
    "webpack": "^4.17.1",
    "webpack-cli": "^3.1.0"
  },
  "scripts": {
    "start": "webpack",
    "server": "webpack-dev-server --open"
  },
  "author": "",
  "license": "ISC"
}
```