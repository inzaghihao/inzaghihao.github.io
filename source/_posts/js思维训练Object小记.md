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

假设我们创建一个业务主类，主类接受一些子类参数。主类中做一些特殊处理，大概的开发套路。在父类方法中传入子类
首先并判断子类当中固有的data和methods 是否为方法或者对象，然后进行合并，并且在父类中新创建一个对应属性，名字通过参数赋值，之后父类就可以访问子类中的方法了

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

### 子类中访问父类的属性，通过$global 实现合并,并且指定（使其设置为不可枚举）子对象访问属性

> Object.keys(this);   是返回可枚举属性的数组
  Object.defineProperty(obj,prop,enumerable)   obj当前对象，prop当前属性名称，enumerable设置是否可枚举(默认false)


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
