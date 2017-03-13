---
title: 走进node (初级)
date: 2017-03-13 19:44:10
tags: [node,笔记]
---

### 安装nodejs

下载地址：[nodejs官网](https://nodejs.org/en/)

### babel 相关

#### 在一个新建的文件夹底下初始化配置文件`package.json`

``npm init``

```
{
  "name": "testjs",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "hao",
  "license": "ISC",
  "devDependencies": {}
}
```

#### 使用npm安装babel 客户端工具

``npm install babel-cli --save-dev``

*建议不要全局安装，以免更新导致版本不一*

#### 安装“编译”es2015 相关babel 插件 babel-preset-es2015

``npm install babel-preset-es2015 --save-dev``

#### 在根目录下创建.babelrc 的配置文件

```
{
	"presets":["es2015"]
}
```

#### 配置`package.json`文件运行es2015 文件

```
"scripts": {
    "text": "babel-node index.js"
 }
```

#### 使用npm运行

```
npm run text
```

### 使用CommonJs规范 es2015 语法

####  export / import

##### 导出的写法

```
export const BaseNews = function(){
  this.display = function(){
    console.log("我是父类");
  }
}
```

导入的写法

```
import {BaseNews} from "./BaseNews.js"

let n = new BaseNews();
n.display();

```

##### 方式2

```
const BaseNews = function(){
  this.display = function(){
    console.log("我是父类");
  }
}
const version = "1.0";

export {BaseNews,version}

```

```
import {BaseNews,version as v} from "./BaseNews.js"

let n = new BaseNews();
n.display();
console.log(v);
```

##### 方式3(常用)

每个模块只能有一次default导出

```
export default function(){
  this.display = function(){
    console.log("我是父类");
  }
}
const version = "1.0";

export {version}
```

```
import BaseNews,{version as v} from "./BaseNews.js"

let n = new BaseNews();
n.display();
console.log(v);
```

```
import {default as BaseNews,version as v} from "./BaseNews.js"

let n = new BaseNews();
n.display();
console.log(v);
```

还可以这样将所有导出

```
import * as a from "./BaseNews.js"

let n = new a.default();
n.display();
console.log(v);
```







***真是感觉脑子被绕住了，索性记下来***
