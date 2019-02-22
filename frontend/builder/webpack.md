# Webpack

---

> 构建工具

## install
```shell
npm install -g webpack
npm install --save-dev webpack
```

## build
```shell
webpack ./app/main.js -o ./public/build.js
```

## webpack-dev-server
```shell
npm install --save-dev webpack-dev-server
```

## babel
```shell
# babel 核心组件
npm install --save-dev babel-core babel-loader css-loader style-loader
# preset env:es6转换 react:jsx转换
npm install --save-dev babel-preset-env babel-preset-react
# react
npm install --save-dev react react-dom
```


### .babelrc
```
{
  "presets": ["react", "env"]
}
```

### webpack.config.js

```javascript
const webpack = require('webpack')

module.exports = {
  devtool: 'eval-source-map',

  entry: __dirname + '/app/app.js',
  output: {
    path: __dirname + '/public',
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /(\.jsx|\.js)$/,
        use: {
          loader: 'babel-loader'
        },
        exclude: /node_modules/
      },
      {
        test: /\.css$/,
        use: [
          {
            loader: 'style-loader'
          },
          {
            loader: 'css-loader',
            options: {
              modules: true,
              localIdentName: '[name]__[local]--[hash:base64:5]'
            }
          },
          {
            loader: 'postcss-loader'
          }
        ]
      }
    ]
  },

  plugins:[
    new webpack.BannerPlugin('版权所有，翻版必究')
  ],

  devServer: {
    contentBase: './public',
    historyApiFallback: true,
    inline: true
  }
}
```

### postcss.config.js
```javascript
module.exports = {
  plugins: [require('autoprefixer')]
}
```