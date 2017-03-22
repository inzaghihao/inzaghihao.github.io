---
title: js思维训练Object小记
date: 2017-03-20 18:29:55
tags: [javascript, es2015, 笔记]
---

业务类中编程思维

### 用Object.assgin() 方法把任意多源对象的方法和可枚举属性复制到目标对象中

> 将data方法中的属性和方法复制到methods中，使得可直接用this 指向访问data中定义的属性方法

```
var news = {
    data(){
        return {
            id:101,
            title:"标题1"
        }
    },
    methods:{
        show(){
            alert(this.id);
        }
    }
}
Object.assign(news.methods,news.data());
news.methods.show();
```

### 一个例子

假设我们创建一个业务主类，主类方法接受一些子类参数。主类中做一些特殊处理，大概的开发套路。在父类方法中传入子类
首先并判断子类当中固有的data和methods 是否为方法或者对象，然后进对象合并，并且在父类中新创建一个对应属性，名字通过参数赋值，之后父类就可以访问子类中的方法了

```
var God = {
    extends(name,obj){
        if("data" in obj && "methods" in obj && typeof obj.data == "function" && typeof obj.methods == "object"){
            var getDate = obj.data();
            if(typeof getDate == "object"){
                Object.assign(obj.methods,getDate);
                this[name] = obj;
            }
        }
    }
}

var news = {
    data(){
        return {
            id:101,
            title:"标题1"
        }
    },
    methods:{
        show(){
            alert(this.title);
        }
    }
}

God.extends("news",news);
God.news.methods.show();
```

### 子类中访问父类的属性，创建初始化函数，创建并通过$global 实现合并,并且指定（使其设置为不可枚举）子对象访问属性

> Object.keys(this);   是返回可枚举属性的数组
  Object.defineProperty(obj,prop,enumerable)   obj当前对象，prop当前属性名称，enumerable设置是否可枚举(默认false)
  多属性形式
```
  Object.defineProperties(obj, {
    'property1': {
      enumerable: false
    },
    'property2': {
      enumerable: false
    }
  });
```

```
var God = {
    version : "v1.0",
    extends(name,obj){
        this.init();
        if("data" in obj && "methods" in obj && typeof obj.data == "function" && typeof obj.methods == "object"){
            var getDate = obj.data();
            if(typeof getDate == "object"){
                Object.assign(obj.methods,getDate,this);
                this[name] = obj;
            }
        }
    },
    init(){
        Object.defineProperty(this,"init",{
            enumerable:false
        })
        var keys = Object.keys(this);
        this.$global = {};
        keys.forEach((item)=>{
            this.$global[item] = this[item];    //this[item] 是值
        })
    }
}

var news = {
    data(){
        return {
            id:101,
            title:"标题1"
        }
    },
    methods:{
        show(){
            //alert(this.title);
            alert(this.$global.version);		//v1.0
        }
    }
}

God.extends("news",news);
God.news.methods.show();
```
### 在子类中可能改变this.$global 对象的值，所以我们把全局$global对象设置为冻结对象Object.freeze(this.$global)，使其不可修改，删除

```
Object.freeze(this.$global);	//不能向这个对象添加新的属性，不能修改其已有属性的值，不能删除已有属性，以及不能修改该对象已有属性的可枚举性、可配置性、可写性

this.$global.version = "v2.0";
alert(this.$global.version);	// v1.0 
```

### 使用prototype原形 扩展父类方法或属性，实例可以访问，例如写一个初始化方法init()

```
var GodClass = function abc(obj){
    if(this instanceof abc){
        if(obj != undefined){
            Object.assign(this,obj)
        }
    }else{
        alert("请实例化对象进行调用")
    }
    
}
var god = new GodClass()
GodClass.init = function(obj){
    Object.assign(this.prototype,obj);
}
GodClass.prototype.init= function(obj){
    Object.assign(this.prototype,obj);
}

GodClass.init({version:"1.1"});        //仅仅只能父类调用
console.log(god.version)      //可用实例访问原形上合并的version

god.init()      //使用prototype 实例即可访问
```

### 继承

团队协作开发，组长开发一个基类，组员开发相关业务子类，实现继承基类

```
var BaseNews = function(){
    this.display= function(){
        alert("父类")
    }
}

var sportNews = function(){
    BaseNews.call(this);
}

var n = new sportNews();
n.display()
```

使用Function 扩展方法，使所有的方法都可以调用

```
var BaseNews = function(){
    this.display= function(){
        alert("父类")
    }
}

Function.prototype.extends = function(base){
    base.call(this.prototype);
}
var sportNews = function(){
    this.version="1.0";
}
sportNews.extends(BaseNews);
var n = new sportNews();
n.display()
```

使子类可以通过一个类似php的parent方法调用父类的方法，增加一个抵挡函数，判断是否是父类的实例才可以调用父类的构造函数，避免重复调用

```
var BaseNews = function bb(){
    if(this instanceof bb){             //抵挡函数，判断this是否是父类的实例，调用构造函数
        alert("父类构造");
    }
    
    this.display= function(){
        alert("父类")
    }
}

Function.prototype.extends = function(base){
    base.call(this.prototype);
    this.prototype.father = new base();         //new父类的实例并绑定在子类的原型链上，子类的实例就可以访问父类的方法
}
var sportNews = function(){
    this.version="1.0";
    this.display= function(){
        alert("子类")
    }
}
sportNews.extends(BaseNews);
var n = new sportNews();
n.father.display();         //通过原型链father访问父类的方法    
n.display()
```

### 使用模块化开发方式

相关阅读链接   [走进node (初级)](https://inzaghihao.github.io/2017/03/13/%E8%B5%B0%E8%BF%9Bnode/)

一个例子 设计一个订单模块，主订单包含订单号，下单id，订单数量，和下单时间，子订单（一对多），使用工厂模式开发

基类 BaseOrder.js
```
const BaseOrder = {
    orderId : 100,
    orderUser : "zhangsan",
    orderTime : "0316",
    orderPro : "bag",
    orderDetail:[]
}

const DetailOrder = {
    proId:0,
    proName:0,
    proPrice:0.00
}

export default {
    create(name,user,time,pro){
        if(name == "main"){
            if(user!=undefined){BaseOrder.orderUser = user};
            if(time!=undefined){BaseOrder.orderTime = time};
            if(pro!=undefined){BaseOrder.orderPro = pro};
            return BaseOrder;
        }else if(name == "detail"){
            return DetailOrder;
        }
    }
}

```
***使用延展操作符 对参数进行赋值***
index.js (入口文件)
```
import Factory from "./BaseNews";

var initdate = ["hao","0333","pig"];        //定义参数数组
var BaseOrder = Factory.create("main",...initdate);         //自动解构initdate数组各个元素作为参数传入，参数不传默认为undefined

console.log(BaseOrder);
```

***另一种写法，使用`rest`运算符对参数进行解构***
```
export default {
    create(name,...args){
        if(name == "main"){
            BaseOrder.orderUser = args[0];
            return BaseOrder;
        }else if(name == "detail"){
            return DetailOrder;
        }
    }
}

/**** index.js ****/
var BaseOrder = Factory.create("main","arg1","arg2","arg3");        //直接赋值 从第二个参数开始自动合并成数组
```

***另一种写法，用对象进行解构（兼容性不好，但是推荐使用）***

```
export default {
    create(name,{user,time,pro}){           //这里的第二个参数是对象，传入的时候做对应
        if(name == "main"){
            if(user!=undefined){BaseOrder.orderUser = user};
            if(time!=undefined){BaseOrder.orderTime = time};
            if(pro!=undefined){BaseOrder.orderPro = pro};
            return BaseOrder;
        }else if(name == "detail"){
            return DetailOrder;
        }
    }
}

/**** index.js ****/

var BaseOrder = Factory.create("main",{             //对象一一对应进行赋值
    user:"username",
    time:"today",
    pro:"shit"
});
```

*未完待续...*
