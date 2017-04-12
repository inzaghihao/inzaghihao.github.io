---
title: vue萌新(包含自动化构建工具配置)
date: 2017-03-28 19:18:28
tags: [javascript, vue]
---

用自动化构建方式commenjs规范写vuejs

### 首先创建合理的项目开发目录结构

![目录结构](vue萌新/20170328193526.png "目录结构")

#### 初始化 `package.json` 文件和创建 `.babelrc` 编译es2015语法文件，以及npm 安装vue相关插件
```
	npm install vue webpack babel-cli babel-loader babel-preset-es2015 html-webpack-plugin --save-dev
```
先装这些，以后继续添加，webpack 不建议全局安装，版本更新问题

***package.json***
```
{
  "name": "vuepro",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack",
    "dev": "webpack-dev-server --inline --hot --content-base ./src/dist"
  },
  "author": "hao",
  "license": "ISC",
  "devDependencies": {
    "babel-cli": "^6.22.2",
    "babel-loader": "^6.2.10",
    "babel-preset-es2015": "^6.22.0",
    "babel-preset-stage-2": "^6.22.0",
    "css-loader": "^0.26.1",
    "element-ui": "^1.1.6",
    "file-loader": "^0.10.0",
    "html-webpack-plugin": "^2.26.0",
    "style-loader": "^0.13.1",
    "vue": "^2.1.10",
    "vue-loader": "^10.1.2",
    "vue-resource": "^1.1.1",
    "vue-router": "^2.2.0",
    "vue-template-compiler": "^2.1.10",
    "vuex": "^2.1.1",
    "webpack": "^1.14.0",
    "webpack-dev-server": "^1.16.2"
  }
}

//这仅仅是 package.json 样例
```
***webpack.config.js***
```
var HtmlWebpackPlugin = require("html-webpack-plugin");		//加载插件
var webpack = require("webpack");			//加载webpack

module.exports = {
    entry:{			
       // "index":__dirname+"/src/js/index.js"			//设置入口js文件
        "index":[__dirname+"/src/js/index.js",			//热加载配置
        "webpack-dev-server/client?http://127.0.0.1:8080"]
    },
    output:{
        publicPath:"http://127.0.0.1:8080/",		
        path:__dirname+"/src/dist/js",		//输出文件路径
        filename:"[name].js"			//输出的文件最终以入口文件的名称
    },
    resolve:{				//设置编译过后文件 vue指向vue.js 文件
        alias:{
            vue:"vue/dist/vue.js"
        }
    },
    module:{
        loaders:[
            {test: /\.css$/,loader:'style!css'},			//设置渲染css
            {test: /\.(png|jpe?g|gif|svg)(\?\S*)?$/,loader: 'file',query: {name: '[name].[ext]?[hash]'}},		//图片渲染
            {test:/\.js$/,loader:"babel",query:{compact:true}},			//es2015语法 babel转化
            {test:/\.vue$/,loader:"babel!vue", exclude:"/node_modules/"},
            {test:/\.(eot|woff|woff2|svg|ttf)([\?]?.*)$/,loader:"file" }

        ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            //filename:__dirname+"/src/dist/index.html",		//目标文件
            filename:"index.html",	
            template:__dirname+"/src/html/index.html",			//模板文件
            inject:"body",			//js注入位置
            hash:true,				//注入js增加随机hash 
            chunks:["index"] 		//注入哪一个js文件
        })
    ]
}

//这仅仅是 webpack.config.js配置文件 样例
```
***.babelrc***
```
{
    "presets":["es2015"]
}

// .babelrc 是固定文件名，设置es2015编译语法的配置文件
```

### 接下来就可以开始正式写页面代码了

#### 在`src/html/index.html` 里面写页面代码,在 `src/js/index.js` 里面写页面主逻辑
```
<div id="app">
    {{name}}
</div>
```
```
import Vue from "vue";			//首先导入vue
new Vue({
    el:"#app",			//绑定渲染元素
    data:{
       name:"hao"
    }
})

```
在 ` package.json` 里设置启动webpack

```
"scripts": {
    "build": "webpack -p"			//-p 是压缩生成的文件，会有警告信息
}
```
***运行项目***
```
$ npm run build 		
```

### 用组件化开发

#### 可复用的结构html代码可以封装成组件的形式，挂载在非body的根节点上

##### 安装vue-loader 并配置可识别.vue 文件

分为全局注册和局部注册

```
$ npm install vue-loader --save-dev

//配置vue-loader
{test:/\.vue$/,loader:"babel!vue", exclude:"/node_modules/"},
```
html 结构
```
<div class="container">         //不要直接挂载在body上
   <myname></myname> 
</div>
```
全局注册

```
Vue.component("myname",{
    props:["count"],            //组件中不可以改变从父组件中传过来的数据
    data(){
        return {
            inCount:this.count
        }
    },
    template:`
        <div><h1 @click:"change">我是一个全局注册的组件{{inCount}}</h1></div>
    `,
    methods:{
        change(){
            this.inCount ++;
        }
    }
})

new Vue({
    el:".container",
})
```

局部注册

方式一

```
import Vue from "vue";
let myname = {
    template:"<div id='me'>my name is {{name}}</div>",
    data:()=>{
        return {
            name :"hao"    
        } 
    }
}
new Vue({
    el:".container",
    components:{
        "myname":myname
    }
})

```

方式二 在src目录下创建 components 组件文件夹,新建mycomponents.vue 文件

```
<style>
    #myage{color: green;}
</style>
<template>
    <div id="myage">
        我的年龄是{{age}}
    </div>
</template>
<script>
    export default {
        data:()=>{
            return {age:"18"}
        }
    }
</script>

```

```
import mynameCom from "./../components/mycomponents.vue";
new Vue({
    el:".container",          //绑定渲染元素
    components:{"myname",mynameCom}
})
```
#### 父子组件之间的通信

每一个组件都有独立的作用域，通过props 值传递参数
```
<div class="container">       
   <myname value="25"></myname> 
</div>

——————————————————————————————————————
<style>
    #myage{color: green;}
</style>
<template>
    <div id="myage">
        我的年龄是{{value}}
    </div>
</template>
<script>
    export default {
        props:['value']
        data:()=>{
            return {age:"18"}
        }
    }
</script>
```

Props验证
```
// 对象语法，提供校验
Vue.component('props-demo-advanced', {
  props: {
    // 只检测类型
    height: Number,
    // 检测类型 + 其他验证
    age: {
      type: Number,         //验证类型
      default: 0,           //默认值
      required: true,       //必填
      validator: function (value) {         //自定义验证规则函数 
        return value >= 0
      }
    }
  }
})
```

子组件访问其相邻的兄弟组件

通过this.$parent.$children 获取其父组件下所有的子组件(数组);可通过数组索引,也可以给子组件增加一个索引id ref:name 访问

```
<myname ref="uname"></myname>

this.$parent.$refs.uname.$data.username         //获取到相邻兄弟组件的属性
```

父子组件嵌套

```
<div class="container">       
   <me age="25" name="hao"></myname> 
</div>

————————————————vue——————————————————————
<template>
    <div id="me">
        <myname :name="name"></myname>      //使用v-bind: 指令动态绑定属性值,简写为:
        <myage :age="age"></myage>
    </div>
</template>

<script>
    import myname from "./myname.vue";
    import myage from "./myage.vue";

    export default {
        props:["name","age"],
        components:{
            "myname":myname,
            "myage":myage
        }
    }

</script>

————————————————index.js——————————————————————

import me from "./../components/me.vue";

new Vue({
    el:".container",          //绑定渲染元素
    components:{"me",me}
})

```




### webpack配置热加载免刷新

使用webpack-dev-server 是一个小型静态服务器搭建热运行环境，代码修改无需编译和页面刷新进行预览，并不产生任何编译文件，在内存当中运行

#### 安装 `webpack-dev-server` 并配置

```
entry:{
    "index":[__dirname+"/src/js/index.js",
    "webpack-dev-server/client?http://127.0.0.1:8080"]          //配置编译到入口文件当中
},
output:{
    publicPath:"http://127.0.0.1:8080/",        // 设置访问网站和端口
    path:__dirname+"/src/dist/js",
    filename:"[name].js"
},
// 可单独配置webpack-dev-server
devServer:{
    hot:true,
    inline:true,
    progress:true,
    port:8080
},
plugins:[
        new HtmlWebpackPlugin({
            //filename:__dirname+"/src/dist/index.html",
            filename:"index.html",          //更改生成路径，热运行直接加载
            template:__dirname+"/src/html/index.html",
            inject:"body",
            hash:true,
            chunks:["index"]
        })
    ]
```
运行服务
```
webpack-dev-server --inline --hot --content-base ./src/dist"

// 使用inline模式，--hot 热模块替换 --content-base 指定网站目录路径
```

使用 webpack --config webpack.dev.config.js 指定要运行的webpack 配置文件

```
"webpack": "webpack --config webpack.config.js --progress --display-modules --colors --display-reasons"

配置参数，看到打包过程
```

### 路由插件

简单理解路由就是单页面应用跳转链接的管理

#### 安装

使用cdn `https://unpkg.com/vue-router/dist/vue-router.js` 方式加载在vue后面

```
npm install vue-router --save-dev
```

如果在一个模块化工程中使用它，必须要通过 Vue.use() 明确地安装路由功能：

```
//加载路由
import VueRouter from 'vue-router';

//使用路由
Vue.use(VueRouter);
```

#### 配置路由映射

```
//设置路由
const routes = [
  { path: '/', component: userlogin },      
  { path: '/news', component: newslist,name:"newslist" },
  { path: '/newsdetail/:newsid', component: newsdetail,name:"newsdetail" },
  { path: '/login', component: userlogin,name:"userlogin" },
  { path: '/eltable', component: eltable,name:"eltable" },
  { path: '/navbar', component: navbar,name:"navbar" }
]
//初始化路由
const router = new VueRouter({
   routes 
})


var mv = new Vue({
    el:".container",
    router:router,      //配置router key为固定写法
})
```

#### 模板中使用

```
<li class="active"><a href="#/login">登录</a></li>
<li><a href="#/news">热点新闻</a></li>
<li><a href="#/eltable">element 测试组件</a></li>
<li><a href="#/navbar">导航 测试组件</a></li>

```

使用 `<router-link>` 组件支持用户在具有路由功能的应用中（点击）导航。 通过 to 属性指定目标地址，默认渲染成带有正确链接的 `<a>` 标签，可以通过配置 tag 属性生成别的标签.。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名 .router-link-active
 
 ```
<router-link to="/login">Go to login</router-link>
<router-link :to="{name:'newsdetail',params:{newsid:news.newsid}}"> {{news.title}}</router-link>
 ```

使用 `<router-view></router-view>`  组件是一个 functional 组件，渲染路径匹配到的视图组件。如果 `<router-view>`设置了名称，则会渲染对应的路由配置中 components 下的相应组件。


*** 任何插件使用先引入然后用Vue.use() ***

使用 `vue-resource` ajax请求插件,引入加载之后会在全局注册一个$http 的变量

```
// 在生命周期mounted 中设置post请求带参数。
mounted(){
    this.$http.post("http://localhost/vue/text.php",{name:"hao"},{emulateJSON:true}).then(function(res){
        // alert(res.body);
    },function(res){
        //error
    })
}
```
ajax获取后台新闻内容展示到页面,在php后端接口数据有newsid 字段，根据这个字段进行传值，用 `<router-link>` :to 根据命名路由，通过params带有newsid 参数传值。在detail页面初始化生命周期函数里 用get方式接收地址后带有 this.$route.params 的参数，回调对应的条目，赋值到newsdetail 数据中。然后渲染到页面
```
 <h2> <router-link :to="{name:'newsdetail',params:{newsid:news.newsid}}"> {{news.title}}</router-link> <small>{{news.pubtime}}</small></h2>
```
```
created(){
    this.$http.get("http://localhost/vue/newslist.php?newsid="+this.$route.params.newsid)
    .then(function(res){
        this.newsdetail =res.body;
    },function(res){
        //error
    })
},
data(){
    return{
        newsdetail:{}
    }
}

<h2>标题： {{newsdetail.title}}<small>发布时间：{{newsdetail.pubtime}}</small></h2>
<p> {{newsdetail.desc}} </p>
```
