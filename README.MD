# 手动搭建React项目

## 第一步：创建并项目文件夹

### 进入命令行工具依次执行以下命令

```bash
mkdir reactdemo // 建立目录
cd reactdemo    // 进入目录
npm init -y     // 初始化package.json
git init        // 初始化git
```

* 现在 reactdemo 目录下只有package.json一个文件。接着为git创建忽略文件，在根目录下新建.gitignore文件，内容如下：

```txt
node_modules
```

## 第二步：安装webpack

```bash
 npm i webpack -D   //使用了简写
```

* 安装后的版本：webpack@4.41.6

```bash
 npm i webpack-cli -D
```

* webpack-cli@3.3.11

## 第三步：安装Babel

* 详情参考：https://www.babeljs.cn/docs/

```bash

npm i babel-upgrade -g
babel-upgrade --write
npm i -D @babel/core @babel/preset-env babel-loader

```

## 第四步：安装 React 支持

### 安装React

```bash
npm i react react-dom -D
```

* react-dom@16.12.0
* react@16.12.0

### 配置Babel以解析React的JSX语法

```bash
npm i @babel/preset-react -D
```

* @babel/preset-react@7.8.3

### 配置webpack自动生成html页面

```bash
npm i html-webpack-plugin -D
```

* html-webpack-plugin@3.2.0

## 第五步：安装  webpack-dev-server

```bash
npm i webpack-dev-server -D
```

* webpack-dev-server@3.10.3

## 第六步：配置 webpack.json.js ，写页面文件

* 在项目目录下新建 webpack.config.js 文件

* 配置信息参考：https://www.webpackjs.com/concepts/

```javascript
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');
module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'autoPackageJs.js',
        path: path.resolve(__dirname, 'public')
    },
    module: {
        rules: [{
            test: /\.js$/,
            exclude: /node_modules/,
            use: {
                loader: "babel-loader",
                options: {
                    presets: ["@babel/preset-env","@babel/preset-react"]
                }
            }

        }]
    },
    plugins: [
        new webpack.HotModuleReplacementPlugin(),
        new HtmlWebpackPlugin({ template: './public/index.html' })
    ],
    devServer: {
        hot: true,
        contentBase: path.join(__dirname, './public'),
        host: 'localhost',
        port: 3030,
        https: false,
        open: true,
        stats: 'errors-only'
    }
};

```

* 在项目目录下新建 public 目录并新建 index.html

* public/index.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>手动配置React开发环境</title>
</head>
<body>
    <div id="root"></div>
</body>
</html>
```

* 在项目目录下新建 src 目录并新建 index.js

* src/index.js

```javascript
import React, { Component } from 'react';
import ReactDOM from 'react-dom';

class App extends Component {
    constructor(props){
        super(props)
        this.state={
            count:0
        }
        this.handerClick = this.handerClick.bind(this);
    }

    handerClick(){
        let count = this.state.count++;
        this.setState({
            count
        })
    }

    render(){
        return(
            <div>
                <h1>Hello,{this.props.name}</h1>
                <button onClick={this.handerClick()}>你点击了{this.state.count}次</button>
            </div>
        )
    }

}

const mountNode = document.getElementById('root');
ReactDOM.render(<App name='pavasun' />,mountNode);
```

## 第七步：测试

### 配置启动脚本 webpack.json

```javascript
"scripts": {
    "start": "webpack-dev-server --mode development",
    "build": "webpack --mode production",
    "test": "echo \"Error: no test specified\" && exit 1"
},

```

### 此时的项目目录文件如下

```text
├─node_modules
├─.gitignore
├─package.json
├─package-lock.json
├─webpack.config.js
├─public
│      index.html
├─src
├─     index.js
```

### 测试一下

```bash
npm run start
```

* 自动打开网页
