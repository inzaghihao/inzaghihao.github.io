---
title: jq插件-tab选项卡
date: 2017-04-19 18:57:19
tags: [javascript]
---

慕课网上的一个自定义封装的tab选项卡 jq插件，贴源码，学习

```
;(function($){
    var Tab = function(el,obj){
        var _this_ = this;

        //接收 自定义元素标签
        this.tab = $(el);

        //接收用户自定义参数
        this.userconfig = obj;
        //默认配置参数

        this.config = {
            "triggerType":"mouseover",
            "effect":"default",
            "invoke":1,
            "auto":false
        };

        //判断配置参数并扩展替换默认值
        if(this.getConfig()){
            $.extend(this.config,this.getConfig());
        };

        //获取标签列表
        this.tabItems = this.tab.find("ul.tab-nav li");
        this.contentItems = this.tab.find("div.content-wrap .content-item");
        var config = this.config;
        if(config.triggerType === "mouseover" || config.triggerType != "click"){
            this.tabItems.mouseover(function(){
                //加点延迟效果
                var self = $(this);
                this.timer1 = setTimeout(function(){
                    _this_.invoke(self);
                },300)
            }).mouseout(function(){
                clearTimeout(this.timer1);
            })
        }else{
            this.tabItems.click(function(){
                _this_.invoke($(this));
            })
        };
        if(config.auto){
            this.timer = null;
            this.loop = 0;
            this.autoPlay();
        }

        if(config.invoke>1){
            this.invoke(this.tabItems.eq(config.invoke-1));
        }

    };
    Tab.prototype = {
        // 获取配置参数
        getConfig:function(){
            // var config = this.tab.attr("data-config");
            var config = this.userconfig;

            if(config && config!= ""){
                // return $.parseJSON(config);
                return config;
            }else{
                return null;
            }
        },
        //事件驱动函数
        invoke:function(currentTab){
            var index = currentTab.index();        //保存当前item的索引值
            var conItems = this.contentItems;
            currentTab.addClass("active").siblings().removeClass("active");
            //根据配置参数effect效果切换内容
            var effect = this.config.effect;
            if(effect === "default" || effect=="" || effect!="fade"){
                conItems.eq(index).addClass("current").siblings().removeClass("current");
            }else if(effect === "fade"){
                conItems.eq(index).fadeIn().siblings().fadeOut();
            }
            //如果配置了自动切换，把当前的loop设置为index
            if(this.config.auto){
                this.loop = index;
            }
        },
        //自动切换
        autoPlay:function(){
            var _this_ = this;
            var tabItems = this.tabItems;
            var tabLen = tabItems.size();
            var config = this.config;


            this.timer = setInterval(function(){
                _this_.loop++;
                if(_this_.loop>= tabLen){
                    _this_.loop = 0;
                }
                tabItems.eq(_this_.loop).trigger(config.triggerType);
            },config.auto);

        }

    };
    //自定义方法
    $.fn.extend({
        tab:function(obj){
            this.each(function(){
                new Tab($(this),obj);
            })
            return this;
        }
    })
    window.Tab = Tab;
})(jQuery);


```
调用
```
<style>
    *{margin: 0;padding: 0;}
    ul,li{list-style: none;}
    body{padding: 100px;background-color: #323232;font-size: 14px;}
    .tab{width: 300px;float: left;}
    .tab-nav{height: 30px;}
    .tab-nav li{float: left;margin-right: 5px;background-color: #767676;}
    .tab-nav li a{display: block;height: 30px;padding: 0 20px;color: #fff;line-height: 30px;text-decoration: none;}
    .tab-nav li.active{background-color: #fff;}
    .tab-nav li.active a{color: #777;}
    .tab .content-wrap{background-color: #fff;padding: 5px;height: 200px;}
    .content-item{position: absolute; height: 200px;display: none;}
    .content-wrap .current{display: block;}
</style>

<div class="js-tab tab">
    <ul class="tab-nav">
        <li class="active"><a href="###">新闻</a></li>
        <li><a href="###">电影</a></li>
        <li><a href="###">娱乐</a></li>
        <li><a href="###">柯基</a></li>
    </ul>
    <div class="content-wrap">
        <div class="content-item current"><img src="a.jpg" alt="">1</div>
        <div class="content-item"><img src="b.png" alt="">2</div>
        <div class="content-item"><img src="a.jpg" alt="">3</div>
        <div class="content-item"><img src="b.png" alt="">4</div>
    </div>
</div>

$(function(){
    $(".js-tab").tab({
        "triggerType":"mouseover",
        "effect":"fade",
        "invoke":4,
        "auto":false
    });
})
```
